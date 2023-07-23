# Ping-Pong-CPP
Ping Pong game using procedural paradigm with C++

## Note:
1st player's buttons a, d and s  
2nd player's buttons j, l and k  
to begin the movement of the ball press g  
to exit the game press x  

  
## Code:

using namespace std;  
#include <iostream>  
#include <conio.h>  
enum Direction {Stop = 0 ,Up ,Down ,Left ,Right ,UpLeft ,DownLeft ,UpRight ,DownRight};  
struct Map { int Height, Width, BallX, BallY; Direction Dir; };  
struct Player1 { int Player1X[3], Player1Y; Direction D1; };  
struct Player2 { int Player2X[3], Player2Y; Direction D2; };  
struct Move { int Score1 ,Score2 ; bool Lose; };  
Map m; Player1 p1; Player2 p2; Move v;  
void setup()  
{  
	m.Height = 15;  
	m.Width = 20;  
	m.BallX = m.Width / 2;  
	m.BallY = m.Height / 2;  
	p1.Player1X[0] = (m.Width / 2) - 1;  
	p1.Player1X[1] = (m.Width / 2);  
	p1.Player1X[2] = (m.Width / 2) + 1;  
	p1.Player1Y = 1;  
	p2.Player2X[0] = (m.Width / 2) - 1;   
	p2.Player2X[1] = (m.Width / 2) ;  
	p2.Player2X[2] = (m.Width / 2) + 1;  
	p2.Player2Y = m.Height - 2;  
	v.Score1 = 0;  
	v.Score2 = 0;  
	v.Lose = false;  
}  
void draw()  
{  
	system("cls");  
	for (int i = 0; i < m.Height; i++)  
	{  
		for (int j = 0; j < m.Width; j++)  
		{  
			if ( i == 0 || i == m.Height - 1) cout << "■";  
			else if (j == 0 || j == m.Width - 1 ) cout << "█";  
			else if (j == m.BallX && i == m.BallY) cout << "o";  
			else if (j == p1.Player1X[0] && i == p1.Player1Y || j == p1.Player1X[1] && i == p1.Player1Y || j == p1.Player1X[2] && i == p1.Player1Y) cout << "▀";  
			else if (j == p2.Player2X[0] && i == p2.Player2Y || j == p2.Player2X[1] && i == p2.Player2Y || j == p2.Player2X[2] && i == p2.Player2Y) cout << "▀";  
			else cout << " ";  
		}  
		cout << "\n";  
	}  
	cout << "Player1 Score : " << v.Score1 << "\n";  
	cout << "Player2 Score : " << v.Score2 << "\n";  
}  
void input()  
{  
	if (_kbhit())  
	{  
		char c = _getch();  
		switch (c)  
		{  
		case 'a': p1.D1 = Left; break;  
		case 's': p1.D1 = Stop; break;  
		case 'd': p1.D1 = Right; break;  
		case 'j': p2.D2 = Left; break;  
		case 'k': p2.D2 = Stop; break;  
		case 'l': p2.D2 = Right; break;  
		case 'g': m.Dir = Down; break;  
		case 'x': exit(0);  
		default:  
			break;  
		}  
	}  
}  
void move()  
{  
	//for (int k = 0; k < 3; k++)  
		switch (p1.D1)  
		{  
		case Left: p1.Player1X[0] -- , p1.Player1X[1] -- , p1.Player1X[2] --; break;  
		case Right: p1.Player1X[0] ++ , p1.Player1X[1] ++ , p1.Player1X[2] ++; break;  
		case Stop:; break;  
		default:  
			break;  
		}  
		switch (p2.D2)  
		{  
		case Left: p2.Player2X[0] --, p2.Player2X[1] --, p2.Player2X[2] --; break;  
		case Right: p2.Player2X[0] ++, p2.Player2X[1] ++, p2.Player2X[2] ++; break;  
		case Stop:; break;  
		default:  
			break;  
		}  
		switch (m.Dir)  
		{  
		case Down: m.BallY++; break;  
		case Up: m.BallY--; break;  
		case UpRight: m.BallY--, m.BallX++; break;  
		case UpLeft: m.BallY--, m.BallX--; break;  
		case DownRight: m.BallY++, m.BallX++; break;  
		case DownLeft: m.BallY++, m.BallX--; break;  
		case Stop: m.BallX, m.BallY; break;  
		default:  
			break;  
		}  
		if (p1.Player1X[0] == 1 || p1.Player1X[2] == m.Width - 2) p1.D1 = Stop;  
		if (p2.Player2X[0] == 1 || p2.Player2X[2] == m.Width - 2) p2.D2 = Stop;  
		if (m.BallY == 0)  
		{  
			v.Score2++;  
			m.BallX = m.Width / 2;  
			m.BallY = m.Height / 2;  
			m.Dir = Stop;  
		}  
		else if (m.BallY >= m.Height - 1)  
		{  
			v.Score1++;  
			m.BallX = m.Width / 2;  
			m.BallY = m.Height / 2;  
			m.Dir = Stop;  
		}  
		else if (m.BallY == p1.Player1Y + 1 && m.BallX == p1.Player1X[0])  
		{  
			if (m.Dir == Up) m.Dir = DownLeft;  
			else if (m.Dir == UpLeft) m.Dir = DownLeft;  
			else if (m.Dir == UpRight) m.Dir = Down;  
		}  
		else if (m.BallY == p1.Player1Y + 1 && m.BallX == p1.Player1X[1])  
		{  
			if (m.Dir == Up) m.Dir = Down;  
			else if (m.Dir == UpLeft) m.Dir = DownLeft;  
			else if (m.Dir == UpRight) m.Dir = DownRight;  
		}  
		else if (m.BallY == p1.Player1Y + 1 && m.BallX == p1.Player1X[2])  
		{  
			if (m.Dir == Up) m.Dir = DownRight;  
			else if (m.Dir == UpLeft) m.Dir = Down;  
			else if (m.Dir == UpRight) m.Dir = DownRight;  
		}  
		else if (m.BallY == p2.Player2Y - 1 && m.BallX == p2.Player2X[0])  
		{  
			if (m.Dir == Down) m.Dir = UpLeft;  
			else if (m.Dir == DownLeft) m.Dir = UpLeft;  
			else if (m.Dir == DownRight) m.Dir = Up;  
		}  
		else if (m.BallY == p2.Player2Y - 1 && m.BallX == p2.Player2X[1])  
		{  
			if (m.Dir == Down) m.Dir = Up;  
			else if (m.Dir == DownLeft) m.Dir = UpLeft;  
			else if (m.Dir == DownRight) m.Dir = UpRight;  
		}  
		else if (m.BallY == p2.Player2Y - 1 && m.BallX == p2.Player2X[2])  
		{  
			if (m.Dir == Down) m.Dir = UpRight;  
			else if (m.Dir == DownLeft) m.Dir = Up;  
			else if (m.Dir == DownRight) m.Dir = UpRight;  
		}  
		else if (m.BallX == 1)  
		{  
			if (m.Dir == UpLeft) m.Dir = UpRight;  
			else if (m.Dir == DownLeft) m.Dir = DownRight;  
		}  
		else if (m.BallX == m.Width - 2)  
		{  
			if (m.Dir == UpRight) m.Dir = UpLeft;  
			else if (m.Dir == DownRight) m.Dir = DownLeft;  
		}  
		else if (v.Score1 >= 5)  
		{  
			cout << "Player 1 Win!";  
			exit(0);  
		}  
		else if (v.Score2 >= 5)  
		{  
			cout << "Player 2 win!";  
			exit(0);  
		}  
}  
void main()  
{  
	setup();  
	while (!v.Lose)  
	{  
		draw();  
		input();  
		move();  
	}  
}    

## Screens:

![4--Ping-pro-1](https://github.com/Elzubair-Dev/Ping-Pong-CPP/assets/104657152/f4d9c894-c489-4507-88ef-9d148b3df333)
  
![4--Ping-pro-2](https://github.com/Elzubair-Dev/Ping-Pong-CPP/assets/104657152/253a2ec0-3dac-4ed1-aa90-25e8b812ab15)
  
![4--Ping-pro-3](https://github.com/Elzubair-Dev/Ping-Pong-CPP/assets/104657152/24f61b40-45bc-4d51-85b7-a22dc0084a2c)
  
![4--Ping-pro-4](https://github.com/Elzubair-Dev/Ping-Pong-CPP/assets/104657152/c1eae1a5-78d2-4e03-9003-99dbb8ed252d)
  
![4--Ping-pro-5](https://github.com/Elzubair-Dev/Ping-Pong-CPP/assets/104657152/2a2b27e2-08d3-49ef-91d2-63b5193051d1)

## Buy me a Coffee:
if you want to support me
(https://www.buymeacoffee.com/zu698air)

## Done  
