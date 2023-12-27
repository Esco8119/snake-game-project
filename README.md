# snake-game-project
#include <stdio.h>
#include <conio.h> // For getch() function
#include <stdlib.h> // For system() function

int length;
int gameover;
int x, y, fruitX, fruitY, score;
int tailX[100], tailY[100];

// Function to set up the initial state of the game
void setup() {
    gameover = 0;
    x = 10; // Initial X position of the snake
    y = 12; // Initial Y position of the snake
    length = 1; // Initial length of the snake
    score = 0; // Initial score

    // Placing the initial fruit randomly on the screen
    fruitX = rand() % 20;
    fruitY = rand() % 20;
}

// Function to draw the game board
void draw() {
    system("cls"); // Clear the console screen
    for (int i = 0; i < 20; i++) {
        for (int j = 0; j < 20; j++) {
            if (i == 0 || i == 19 || j == 0 || j == 19) {
                printf("#"); // Draw walls
            } else if (i == x && j == y) {
                printf("O"); // Draw snake head
            } else if (i == fruitX && j == fruitY) {
                printf("F"); // Draw fruit
            } else {
                int isTail = 0;
                for (int k = 0; k < length; k++) {
                    if (tailX[k] == i && tailY[k] == j) {
                        printf("o"); // Draw snake body
                        isTail = 1;
                    }
                }
                if (!isTail) {
                    printf(" ");
                }
            }
        }
        printf("\n");
    }
    printf("Score:%d", score);
}

// Function to take user input
void input() {
    if (_kbhit()) {
        switch (_getch()) {
            case 'a':
                y--;
                break;
            case 'd':
                y++;
                break;
            case 'w':
                x--;
                break;
            case 's':
                x++;
                break;
            case 'x':
                gameover = 1;
                break;
        }
    }
}

// Function to move the snake
void algorithm() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;

    for (int i = 1; i < length; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }

    switch (_getch()) {
        case 'a':
            y--;
            break;
        case 'd':
            y++;
            break;
        case 'w':
            x--;
            break;
        case 's':
            x++;
            break;
        case 'x':
            gameover = 1;
            break;
    }

    if (x < 0 || x > 19 || y < 0 || y > 19) {
        gameover = 1; // If snake goes out of bounds, the game is over
    }

    for (int i = 0; i < length; i++) {
        if (tailX[i] == x && tailY[i] == y) {
            gameover = 1; // If snake collides with itself, the game is over
        }
    }

    if (x == fruitX && y == fruitY) {
        score += 10; // If the snake eats the fruit, increase the score
        length++;
        fruitX = rand() % 20;
        fruitY = rand() % 20; // Place a new fruit randomly on the screen
    }
}

int main() {
    setup();
    while (!gameover) {
        draw();
        input();
        algorithm();
    }
    return 0;
}
