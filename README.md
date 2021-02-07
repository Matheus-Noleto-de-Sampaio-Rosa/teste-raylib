# teste-raylib
Testando alguns detalhes no raylib


### testando um "Pega pega"

```c
#include "raylib.h"
#include <stdlib.h>
#include <time.h>
#include <math.h>

int main(){

    int const ScreenWidth = 800;
    int const ScreenHeight = 450;

    srand(time(NULL));

    //variavel para poder mover o quadrado
    int x = ScreenWidth/2, y = ScreenHeight/2;

    //inicializo o texto em posicao aleatoria
    int pontox = rand()% ScreenWidth+1;
    int pontoy = rand() % ScreenHeight+1;

    //inicializo as variaveis para checar a distancia entre texto e quadrado
    int colisaox = x - pontox;
    if(colisaox < 0) colisaox*-1;
    int colisaoy = y - pontoy;
    if(colisaoy < 0) colisaoy*-1;


    //inicilizo a distancia total entre os objetos como a potencia e par pouco importa
    //se a colisaox e y sao negativos
    int distancia = sqrt(pow(colisaox, 2) + pow(colisaoy, 2));

    InitWindow(ScreenWidth, ScreenHeight, "Pega Pega");

    SetTargetFPS(60);

    while(!WindowShouldClose()){
        //movimentacao
        if(IsKeyPressed(KEY_W) || IsKeyPressed(KEY_UP)){
                y -= 10;
            }else if(IsKeyPressed(KEY_S) || IsKeyPressed(KEY_DOWN)){
                y += 10;
            }else if(IsKeyPressed(KEY_A) || IsKeyPressed(KEY_LEFT)){
                x -= 10;
            }else if(IsKeyPressed(KEY_D) || IsKeyPressed(KEY_RIGHT)){
                x += 10;
            }

        //apos printar na tela irei verificar a distancia para mudar a posicao do texto
        colisaox = x - pontox;
        if(colisaox < 0) colisaox*-1;
        colisaoy = y - pontoy;
        if(colisaoy < 0) colisaoy*-1;

        distancia = sqrt(pow(colisaox,2) + pow(colisaoy, 2));

        if(distancia < 175){
            pontox = GetRandomValue(0, 800);
            pontoy = GetRandomValue(0, 450);
        }

        BeginDrawing();

            ClearBackground(RAYWHITE);

            DrawText("kkj nao me pega", pontox, pontoy, 20, BLACK);
            DrawRectangle(x, y, 100, 100, RED);


        EndDrawing();
    }

 return 0;
}
```

### tesntando um algoritmo de colisao com circulos

```c
#include <stdio.h>
#include <math.h>
#include "raylib.h"

//teste de colisao
bool colisao(int* x1, int* x2, int* y1, int* y2){
    int distancia = sqrt(pow(*x1-*x2, 2) + pow(*y1-*y2, 2));
    if(distancia <= 60){
        return 1;
    }else{
        return 0;
    }
}

int main(){
    srand(time(NULL));
    int const ScreenWidth = 800, ScreenHeight = 450;
    //inicializo os pontos
    int x1 = 300, x2 = 400, y1 = 225, y2 = 225;
    //inicializo as variaveis de distancia
    int dx = x1 - x2, dy = y1 - y2;
    //inicializo as variaveis de previsao de distancia
    int prevx1 = x1+10, prevx2 = x2+10, prevy1 = y1+10, prevy2 = y2+10;

    InitWindow(ScreenWidth, ScreenHeight, "Colisao");

    SetTargetFPS(60);

    while(!WindowShouldClose()){
        dx = x1 - x2;
        dx = y1 - y2;
        //condicionais de colisao extremamente mal otimizado
        if(!colisao(&x1, &x2, &y1, &y2)){
            if(IsKeyPressed(KEY_UP)) y1 -= 10;
            else if(IsKeyPressed(KEY_DOWN))y1 += 10;
            else if(IsKeyPressed(KEY_RIGHT))x1 += 10;
            else if(IsKeyPressed(KEY_LEFT))x1 -= 10;
        }else{
           if(x1 > x2 && y1 == y2){
                if(IsKeyPressed(KEY_UP)) y1 -= 10;
                else if(IsKeyPressed(KEY_DOWN)) y1 += 10;
                else if(IsKeyPressed(KEY_RIGHT)) x1 += 10;
           }else if(x1 < x2 && y1 == y2){
                if(IsKeyPressed(KEY_UP)) y1 -= 10;
                else if(IsKeyPressed(KEY_DOWN)) y1 += 10;
                else if(IsKeyPressed(KEY_LEFT)) x1 -= 10;
           }else if(x1 == x2 && y1 > y2-70){
                if(IsKeyPressed(KEY_DOWN)) y1 -= 10;
                else if(IsKeyPressed(KEY_RIGHT)) x1 += 10;
                else if(IsKeyPressed(KEY_LEFT)) x1 -= 10;
           }else if(x1 == x2 && y1+60 < y2){
                if(IsKeyPressed(KEY_DOWN)) y1 += 10;
                else if(IsKeyPressed(KEY_RIGHT)) x1 += 10;
                else if(IsKeyPressed(KEY_LEFT)) x1 -= 10;
           }else if(x1 < x2 && y1 > y2-35){
                if(IsKeyPressed(KEY_UP)) y1 -= 10;
                else if(IsKeyPressed(KEY_LEFT)) x1 -= 10;
           }else if(x1 < x2 && y1 < y2){
                if(IsKeyPressed(KEY_DOWN)) y1 += 10;
                else if(IsKeyPressed(KEY_LEFT)) x1 -= 10;
           }else if(x1 > x2 && y1 > y2-35){
                if(IsKeyPressed(KEY_UP)) y1 -= 10;
                else if(IsKeyPressed(KEY_RIGHT)) x1 += 10;
           }else if(x1 > x2 <= 60 && y1 < y2){
                if(IsKeyPressed(KEY_DOWN)) y1 += 10;
                else if(IsKeyPressed(KEY_RIGHT)) x1 += 10;
           }
        }
        BeginDrawing();
            ClearBackground(RAYWHITE);

            DrawCircle(x1, y1, 30, RED);
            DrawCircle(x2, y2, 30, BLUE);

        EndDrawing();
        printf("%i, %i, %i, %i, %i, %i\n", x1 - x2, y1 - y2, x1, x2, y1, y2);
    }

return 0;
}
```

### Teste de "tiro"

```c
#include "raylib.h"

void tiro(int* posx, int* posy){
   return DrawRectangle(posx, posy, 10, 10, RED);
}

int velocidade(int* posx){
    return posx+7;
}



int main(){
    int const ScreenWidth = 800, ScreenHeight = 450;

    InitWindow(ScreenWidth, ScreenHeight, "Teste de tiros");

    int x = ScreenWidth/2 , y = ScreenHeight/2, posx = (ScreenWidth/2)+100, posy = (ScreenHeight/2)+50;

    bool tiro = 0;
    int vel = 7;

    SetTargetFPS(60);

    while(!WindowShouldClose()){

    if(IsKeyDown(KEY_UP))y -= 5;
    else if(IsKeyDown(KEY_DOWN))y += 5;
    else if(IsKeyDown(KEY_RIGHT))x += 5;
    else if(IsKeyDown(KEY_LEFT))x -= 5;

    if(IsKeyPressed(KEY_SPACE)){
        tiro = 1;
    }

    BeginDrawing();
    ClearBackground(RAYWHITE);

        DrawRectangle(x, y, 100, 100, PURPLE);
        if(tiro)DrawRectangle(posx, posy, 10, 10, RED);

    EndDrawing();

    printf("%i, %i, %i, %i\n", x, posx, y, posy);

    if(tiro)posx += vel;
    else posx = x+100, posy = y+50;

    if(posx >= 800){
            posx = x+100, posy = y+50, tiro = 0;
            tiro = 0;
        }
    }
}
```
