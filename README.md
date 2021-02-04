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
