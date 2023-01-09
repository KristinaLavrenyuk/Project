
#include <iostream>
#include <conio.h>
#include <Windows.h>
#include <string>
using namespace std;

const int SIZE_Y = 35, SIZE_X = 74;

char scene[SIZE_Y][SIZE_X]{};

void SetCursor(int x, int y)
{
    HANDLE hStdOut = GetStdHandle(STD_OUTPUT_HANDLE);
    COORD cursorPos;
    cursorPos.X = x;
    cursorPos.Y = y;
    SetConsoleCursorPosition(hStdOut, cursorPos);
}

void SetConsoleSize(int x, int y)
{
    HANDLE wHnd = GetStdHandle(STD_OUTPUT_HANDLE);
    SMALL_RECT windowSize = { 0, 0, x - 1, y - 1 };
    SetConsoleWindowInfo(wHnd, TRUE, &windowSize);
}

struct laser
{
    char view[1] = { char(179) };

    int positionXLaser = 5;     // Значение последней координаты лазера по X
    int positionYLaser = 5;     // Значение последней координаты лазера по Y
    int oldXLaser = 10;         // Предыдущее значение координаты лазера по X
    int oldYLaser = 10;         // Предыдущее значение координаты лазера по Y
    int flagFire = 0;

    void Fire(int x, int y)
    {
        positionXLaser = x;
        positionYLaser = y;
        scene[oldYLaser][oldXLaser] = ' ';
        scene[y][x] = view[0];
        oldXLaser = x;
        oldYLaser = y;
    }

    void ClearFire(int x, int y)
    {
        scene[y][x] = ' ';
    }
};

struct
{
    int positionY = 0, positionX = 1, time = 0, choose, lifes = 3;

    char view[3][5] = {
        {(char)186, ' ', (char)203, ' ', (char)186},
        {(char)204, (char)205, (char)206, (char)205, (char)185},
        {(char)186, ' ', ' ', ' ', (char)186}
    };

    int old_X = SIZE_X / 2;     // Предыдущее значение координаты по X
    int old_Y = 5;              // Предыдущее значение координаты по Y
    laser laser;

    void MoveTo(int x, int y)
    {
        positionY = y;
        positionX = x;

        for (int i = 0; i < 3; i++)
        {
            for (int j = 0; j < 5; j++)
            {
                scene[old_Y + i][old_X + j] = ' ';
            }
        }

        for (int i = 0; i < 3; i++)
        {
            for (int j = 0; j < 5; j++)
            {
                scene[y + i][x + j] = view[i][j];
            }
        }

        old_X = x;
        old_Y = y;
    }

    void StepUp()
    {
        if (positionY - 1 > 3) MoveTo(positionX, --positionY);
    }

    void StepDown()
    {
        if (positionY + 3 < SIZE_Y - 5) MoveTo(positionX, ++positionY);
    }

    void StepRight()
    {
        if (positionX + 1 + 5 < SIZE_X - 2) MoveTo(++positionX, positionY);
    }

    void StepLeft()
    {
        if (positionX - 1 > 1) MoveTo(--positionX, positionY);
    }

    void Effect(int x)
    {
        choose = rand() % 8 + 1;

        if (positionX + 2 != x && positionX + 5 < SIZE_X - 1 && positionX > 1)
        {
            if (x > positionX + 2)
            {
                MoveTo(++positionX, positionY);
            }
            if (x < positionX + 2)
            {
                MoveTo(--positionX, positionY);
            }
        }

        switch (choose)
        {
        case 1:
            StepUp();
            break;
        case 2:
            StepDown();
            break;
        case 3:
            StepRight();
            break;
        case 4:
            StepLeft();
            break;
        case 5:
            StepUp();
            StepRight();
            break;
        case 6:
            StepUp();
            StepLeft();
            break;
        case 7:
            StepDown();
            StepRight();
            break;
        case 8:
            StepDown();
            StepLeft();
            break;
        }
    }

    void VragDead(int x, int y)
    {
        for (int i = 0; i < 3; i++)
        {
            for (int j = 0; j < 5; j++)
            {
                scene[y + i][x + j] = ' ';
            }
        }

        old_X = 5;
        old_Y = 5;
    }

    void VragSpawn(int x, int y)
    {
        positionY = y;
        positionX = x;

        for (int i = 0; i < 3; i++)
        {
            for (int j = 0; j < 5; j++)
            {
                scene[y + i][x + j] = view[i][j];
            }
        }

        old_X = x;
        old_Y = y;
    }

} vrag;

struct
{
    int positionX = 0;          // Текущее значение координаты по X
    int positionY = 0;          // Текущее значение координаты по Y
    int old_X = 10;             // Предыдущее значение координаты по X
    int old_Y = 10;             // Предыдущее значение координаты по Y
    int points = 0;             // Количество очков
    laser laser;
    
    char we[1] = { 30 };  // Наш внешний вид

    void MoveTo(int x, int y)
    {
        positionY = y;
        positionX = x;
        scene[old_Y][old_X] = ' ';
        scene[y][x] = we[0];
        old_X = x;
        old_Y = y;
    }

    void StepToRight() 
    {
        if (positionX < SIZE_X - 4) MoveTo(++positionX, positionY);
    }

    void StepToLeft()
    {
        if (positionX > 3) MoveTo(--positionX, positionY);
    }
} we;

int main()
{
    char level[6] = "level", points[6] = "point", name[10] = "Star Wars", lifes[6] = { 3, 3, 3, 3, 3 };
    int positionLifes = 64;

    SetConsoleSize(SIZE_X + 1, SIZE_Y + 1);

    //Создание рамки
    for (int i = 0; i < SIZE_Y; i++)
    {
        for (int j = 0; j < SIZE_X; j++)
        {
            if ((i == 0 || i == SIZE_Y - 1) && (j == 0 || j == SIZE_X - 1)) //Углы рамки
            {
                if (i == 0 && j == 0) scene[i][j] = 201; // ╔
                else if (i == 0 && j == SIZE_X - 1) scene[i][j] = 187; // ╗
                else if (i == SIZE_Y - 1 && j == 0) scene[i][j] = 200; // ╚
                else if (i == SIZE_Y - 1 && j == SIZE_X - 1) scene[i][j] = 188; // ╝
            }
            else //Стороны рамки
            {
                if (i == 0 || i == SIZE_Y - 1) scene[i][j] = 205; // ═
                if (i == 2 && j > 0 && j < SIZE_X - 1) scene[i][j] = 205;
                if (j == 0 || j == SIZE_X - 1) scene[i][j] = 186; // ║
            }
        }
    }

    //Заголовок (название игры, уровень, очки, жизни)
    for (int i = 0; i < 21; i++)
    {
        if (i < 10) scene[1][i + 2] = name[i];
        else if (i == 20)
        {
            scene[1][i + 2] = 186; // ║
            scene[0][i + 2] = 203; // ╦
            scene[2][i + 2] = 202; // ╩
        }
    }
    for (int i = 0; i < 17; i++)
    {
        if (i < 6) scene[1][i + 23] = level[i];
        else if (i == 16)
        {
            scene[1][i + 23] = 186;
            scene[0][i + 23] = 203;
            scene[2][i + 23] = 202;
        }
    }
    for (int i = 0; i < 17; i++)
    {
        if (i < 6) scene[1][i + 41] = points[i];
        else if (i == 16)
        {
            scene[1][i + 41] = 186;
            scene[0][i + 41] = 203;
            scene[2][i + 41] = 202;
        }
    }
    for (int i = 0; i < 6; i++) scene[1][i + 59] = lifes[i];

    vrag.positionX = rand() % 68 + 1; 

    int gh = 0;
    string str;

    we.MoveTo(SIZE_X / 2, SIZE_Y - 2);
    vrag.MoveTo(vrag.positionX, 4);

    while (true)
    {
        SetCursor(0, 0);

        // Движение врага
        if (vrag.time == 5)
        {
            vrag.Effect(we.positionX);
            vrag.time = 0;
        }

        if (_kbhit())
        {
            int simbol = _getch();

            if (simbol == 224)
            {
                switch (_getch())
                {
                case 75: // Влево
                    we.StepToLeft();
                    break;
                case 77: // Вправо
                    we.StepToRight();
                    break;
                }
            }
            if (simbol == 32) // Стрельба
            {
                if (we.laser.flagFire == 0)
                {
                    we.laser.Fire(we.positionX, we.positionY - 1);
                    we.laser.flagFire = 1;
                }
            }
        }

        // Стрельба врага
        if (vrag.positionX + 2 <= we.positionX + 2 && vrag.positionX + 2 >= we.positionX - 2)
        {
            if (vrag.laser.flagFire == 0)
            {
                vrag.laser.flagFire = 1;
                vrag.laser.Fire(vrag.positionX + 2, vrag.positionY + 3);
            }
        }

        if (vrag.laser.flagFire == 1) vrag.laser.Fire(vrag.laser.positionXLaser, ++vrag.laser.positionYLaser);

        if (vrag.laser.positionYLaser - 1 == SIZE_Y - 3)
        {
            vrag.laser.ClearFire(vrag.laser.positionXLaser, vrag.laser.positionYLaser);
            vrag.laser.flagFire = 0;
            if (vrag.laser.positionXLaser == we.positionX)
            {
                positionLifes--;
                scene[1][positionLifes] = ' ';
            }
            vrag.laser.positionXLaser = 0;
            vrag.laser.positionYLaser = 0;
        }

        if (positionLifes == 59) break;

        // Наша стрельба
        if (we.laser.flagFire == 1) we.laser.Fire(we.laser.positionXLaser, --we.laser.positionYLaser);

        if (we.laser.positionYLaser - 1 < 4 || (we.laser.positionYLaser == vrag.positionY + 2 && we.laser.positionXLaser >= vrag.positionX && we.laser.positionXLaser <= vrag.positionX + 5))
        {
            we.laser.ClearFire(we.laser.positionXLaser, we.laser.positionYLaser);
            we.laser.flagFire = 0;
            if (we.laser.positionYLaser == vrag.positionY + 2 && we.laser.positionXLaser >= vrag.positionX && we.laser.positionXLaser <= vrag.positionX + 5)
            {
                vrag.lifes--;
            }
            we.laser.positionXLaser = 0;
            we.laser.positionYLaser = 0;
        }

        // Смерть врага и пояление нового
        if (vrag.lifes == 0)
        {
            we.points++;

            vrag.VragDead(vrag.positionX, vrag.positionY);
            vrag.positionX = rand() % 68 + 1;
            vrag.positionY = 3;
            vrag.lifes = 3;
            vrag.VragSpawn(vrag.positionX, vrag.positionY);
        }

        // Вывод сцены
        for (int i = 0; i < SIZE_Y; i++) {
            for (int j = 0; j < SIZE_X; j++) {
                cout << scene[i][j];
            }
            cout << '\n';
        }

        vrag.time++;

        //Sleep(100);
    }
    system("pause");
}
