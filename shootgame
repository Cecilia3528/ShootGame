#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <termios.h>
#include <fcntl.h>

#define WIDTH 50
#define HEIGHT 20

int kbhit(void) {
    struct termios oldt, newt;
    int ch;
    int oldf;

    tcgetattr(STDIN_FILENO, &oldt);
    newt = oldt;
    newt.c_lflag &= ~(ICANON | ECHO);
    tcsetattr(STDIN_FILENO, TCSANOW, &newt);
    oldf = fcntl(STDIN_FILENO, F_GETFL, 0);
    fcntl(STDIN_FILENO, F_SETFL, oldf | O_NONBLOCK);

    ch = getchar();

    tcsetattr(STDIN_FILENO, TCSANOW, &oldt);
    fcntl(STDIN_FILENO, F_SETFL, oldf);

    if(ch != EOF) {
        ungetc(ch, stdin);
        return 1;
    }

    return 0;
}

void clearScreen() {
    printf("\033[2J\033[1;1H"); // 清屏
}

int main() {
    int x = WIDTH / 2;
    int y = HEIGHT - 2;
    int enemyX = rand() % WIDTH;
    int enemyY = 0;
    int score = 0;
    int gameOver = 0;
    int speed = 100000;

    while (!gameOver) {
        clearScreen();
        printf("Score: %d\n", score);
        for (int i = 0; i < WIDTH + 2; i++) {
            printf("#");
        }
        printf("\n");

        for (int i = 0; i < HEIGHT; i++) {
            for (int j = 0; j < WIDTH; j++) {
                if (i == y && j == x) {
                    printf(">");
                } else if (i == enemyY && j == enemyX) {
                    printf("X");
                } else {
                    printf(" ");
                }
            }
            printf("\n");
        }

        for (int i = 0; i < WIDTH + 2; i++) {
            printf("#");
        }
        printf("\n");

        if (kbhit()) {
            char key = getchar();
            if (key == 'a' && x > 0) {
                x--;
            } else if (key == 'd' && x < WIDTH - 1) {
                x++;
            }
        }

        enemyY++;

        if (enemyY == y && enemyX == x) {
            gameOver = 1;
        }

        if (enemyY == HEIGHT) {
            enemyX = rand() % WIDTH;
            enemyY = 0;
            score++;
            if (speed > 50000) {
                speed -= 5000;
            }
        }

        usleep(speed);
    }

    clearScreen();
    printf("Game Over!\n");
    printf("Score: %d\n", score);

    return 0;
}
