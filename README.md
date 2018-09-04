# grass-ing

#include<conio.h>

#include<stdlib.h>

#include<stdio.h>

#include<windows.h>

#include<process.h>

#include<time.h>


//***************************************************************

void changedirection(char key);  //to change direction

int    makefood();                       //to generate food

void printborder();                   //to print border

void speedupdate();                   //to increase speed

int    instruct();                         //instructions

void gotoxy(int,int);                //assign coordinates

void  movesnake();                  //to make snake move

int    document();                    //documentation

int    mainintro();                    //main screen, links to actual game(move snake)

void createborder();                   //creates some 30-30 border 

void hitborder();                        

void hit();

void cord();

void change(char);

//***********************************************************

//limit xaxis=0-20 yaxis=10-30

///global variables

clock_t start;

int canvas[70][70]={0,0};

int x=25,y=35;

int direction=6;

COORD snake[10]={{15,15},{14,15},{13,15},{12,15},{11,15}};

COORD foodpos={20,15};

int length=1;

int score=0;

int gameover=0;

///********************************************************************

int instruct()///instructions here

{   system("cls");

    gotoxy(20,10);printf("HOW TO PLAY? ");

    gotoxy(10,15);printf(" 4: left");

    gotoxy(10,20);printf(" 6: right");

    gotoxy(10,25);printf(" 8: up");

    gotoxy(10,30);printf(" 2:down");

    gotoxy(20,40);printf("  the objective of the game is to eat as many");

    gotoxy(20,42);printf("  dollars($) as possible.");

    gotoxy(20,44);printf("1 $ = 10 points");

    gotoxy(20,46);printf("snake grows everytime you earn a $");

    gotoxy(20,48);printf("speed of the snake increases automatically...");

    gotoxy(20,50);printf("So just keep a watch on the SPEED. make the snake legacy proud.");

    gotoxy(20,52);printf("press any key to continue");

    gotoxy(20,54);printf(" ");getch();

    return 0;

}

///********************************************************************

int document()///documentation: mostly inactive

{

    system("cls");

    gotoxy(5,10);

    printf("SYSTEM documentation:");cout<<endl;

    gotoxy(30,12);

    printf("language :c++n IDE:CODEBLOCKS 13.12 (windows 32-bit)");cout<<endl;cout<<endl;

    gotoxy(5,14);

    printf("USER documentation:");cout<<endl;

    gotoxy(30,16);

    printf("creators: SHREYA SAHAY & SHASHWATI JHA ");cout<<endl;cout<<endl;

    gotoxy(10,20);printf(" press m to return to main menu");

    gotoxy(10,21);printf("press p to play");

    if(getch()=='m')

        mainintro();

    else if(getch()=='p')

        movesnake();

    return 0;

}

///********************************************************************

int mainintro()///main intro screen,menu driven

{

        int ch;

    do

    {

    system("cls");

    gotoxy(20,10);      printf("SNAKE SHASHAY!");cout<<endl;cout<<endl;

    gotoxy(15,14);      printf("1. START");cout<<endl;

    gotoxy(15,18);      printf("2. INSTRUCTIONS");cout<<endl;

    gotoxy(15,22);      printf("3. DOCUMENTATION");cout<<endl;

    gotoxy(15,26);      printf("press 0 to exit");cout<<endl;

    gotoxy(20,32);      printf("enter choice: ");cin>>ch;

        switch(ch)

        {

            case 1:   movesnake();

                      system("cls");

                      gameover=0;

                       break;

            case 2:instruct();break;

            case 3:document();break;

            case 0: cout<<"QUITTING";_sleep(100);cout<<"#";_sleep(100);cout<<"#";_sleep(100);cout<<"#";_sleep(100);cout<<"#";_sleep(100);cout<<"#";

            exit(0);

        }

        ch=getch();

    }

    while(ch!=0);

}

///********************************************************************


void printborder()

{

                for(int i=11;i<=51;i++)

                {

                   gotoxy(i,11);

                   cout<<"!";

                   gotoxy(i,51);

                   cout<<"!";

                }

                for(int i=11;i<=51;i++)

                {

                    gotoxy(11,i);

                     cout<<"!";

                     gotoxy(51,i);

                     cout<<"!";

                }

}

///*******************************************************************

void movesnake()//game opens here

{  start= clock();

snake[0].X=15;

snake[0].Y=15;

  makefood();

  while(gameover==0)

     {

       while(!kbhit())//make food

       {

           speedupdate();

           system("cls");

           printborder();

           gotoxy(foodpos.X,foodpos.Y);

           cout<<"$";

           gotoxy(25,8);

           cout<<"SCORE :"<<(score)*10;cout<<"food:"<<foodpos.X<<","<<foodpos.Y;

           if(direction==6)

            {

             for(int i=0;i<length;i++)

              {

                  snake[i].X++;

               gotoxy(snake[i].X,snake[i].Y); cout<<"#";

               hit();

              }

           }

           else if(direction==4)

           {

               for(int i=0;i<length;i++)

              {

                  snake[i].X--;

               gotoxy(snake[i].X,snake[i].Y);cout<<"#";

               hit();

              }

           }

           else if(direction==8)

           {

                  for(int i=0;i<length;i++)

              {

                   snake[i].Y--;

               gotoxy(snake[i].X,snake[i].Y);

               cout<<"#";hit();

              }

           }

           else if(direction==2)

           {

                for(int i=0;i<length;i++)

               {snake[i].Y++;

               gotoxy(snake[i].X,snake[i].Y);

               cout<<"#";hit();

               }

           }

           if(snake[0].X==foodpos.X&&snake[0].Y==foodpos.Y)

           {

                score++;

               cout<<"level "<<score;

               _sleep(500);

               makefood();

           }

    }

       char key=getch();

     change(key);

 }

}

///*******************************************************************

int makefood()

    
{    system("cls");

      cout<<"level "<<score;_sleep(200);

           if(length<9)

               length++;

              int midx=25,midy=25;

              for(int i=0;i<length;i++)

              {

                  snake[i].X=midx-i;

                  snake[i].Y=midy;

                  direction=6;

              }

        srand(clock());

        foodpos.X=rand()%(50-length);

         if(foodpos.X<=snake[0].X)

            foodpos.X+=snake[0].X;

            //if(foodpos.X%2==0)

                //foodpos.X++;

        foodpos.Y=rand()%(50-length);

        if(foodpos.Y<=snake[0].Y)

            foodpos.Y+=snake[0].Y;;

            //if(foodpos.Y%2==0)

               // foodpos.Y++;

    }

///********************************************************************

int main()

{

cout<<"*******  *        *           *         *      *    *******"<<endl;_sleep(250);

cout<<"*        * *      *         *   *       *    *      *      "<<endl;_sleep(250);

cout<<"*        *   *    *        *     *      *  *        *       "<<endl;_sleep(250);

cout<<"*******  *    *   *       *       *     **          *******"<<endl;_sleep(250);

cout<<"      *  *     *  *      **********     *  *        *       "<<endl;_sleep(250);

cout<<"      *  *      * *     *           *   *    *      *       "<<endl;_sleep(250);

cout<<"*******  *       *     *             *  *       *   *******"<<endl;_sleep(250);

cout<<"============================================================"<<endl;_sleep(1000);

    mainintro();

}

///*********************************************************************

void speedupdate()

{

    if(clock()-start<=9000)

    {

        _sleep(300);

    }

    else if(clock()-start>9000&&clock()-start<16000)

    {

        _sleep(200);

    }

    else

    {

        _sleep(100);

    }

}

void hit()

{

    if(snake[0].X>=11&&snake[0].X<=51&&snake[0].Y==11)

    {

           gotoxy(35,25);

           system("cls");

           cout<<"GAME OVER!";

           gotoxy(35,27);

           cout<<"your SCORE is "<<(score)*10<<endl;

           gotoxy(35,28);

           cout<<"PRESS p TO PLAY AGAIN OR PRESS m TO GOTO MAIN MENU";

           if(getch()=='m')

                mainintro();

           else if(getch()=='p')

           {

               gameover=0;

               movesnake();

           }

    }

    if(snake[0].X>=11&&snake[0].X<=51&&snake[0].Y==51)

    {

           gotoxy(35,45);

           cout<<"GAME OVER!";

           gotoxy(35,47);

           cout<<"your SCORE is "<<(score)*10<<endl;

           getch();;

           gameover=1;

           mainintro();

    }

    if(snake[0].Y>=11&&snake[0].Y<=51&&snake[0].X==11)

    {

         gotoxy(35,45);

           cout<<"GAME OVER!";

           gotoxy(35,47);

           cout<<"your SCORE is "<<(score)*10<<endl;

           gameover=1;

           mainintro();

    }

    if(snake[0].Y>=11&&snake[0].Y<=51&&snake[0].X==51)

    {

           gotoxy(35,45);

           cout<<"GAME OVER!";

           gotoxy(35,47);

           cout<<"your SCORE is "<<(score)*10<<endl;

           gameover=1;

          mainintro();

    }

}

    void change(char key)

{

    if(key=='a'&&(direction==8||direction==2))

    {   snake[0].Y=snake[0].Y;

        snake[0].X=snake[0].X-1;

        //gotoxy(snake[0].X,snake[0].Y);

         // cout<<"#";

        for (int i=1;i<length;i++)

        {

          snake[i].X=snake[0].X+i;

          snake[i].Y=snake[0].Y;

         // gotoxy(snake[i].X,snake[i].Y);

         // cout<<"#";

        }

        direction=4;

    }

    if(key=='d'&&(direction==8||direction==2))

    {

        snake[0].Y=snake[0].Y;

        snake[0].X=snake[0].X+1;

        //gotoxy(snake[0].X,snake[0].Y);

          //cout<<"#";

        for (int i=1;i<length;i++)

        {

          snake[i].X=snake[0].X-i;

          snake[i].Y=snake[0].Y;

         // gotoxy(snake[i].X,snake[i].Y);

         // cout<<"#";

        }

        direction=6;

    }

    if(key=='w'&&(direction==4||direction==6))

    {

        snake[0].X=snake[0].X;

        snake[0].Y=snake[0].Y-1;

        //gotoxy(snake[0].X,snake[0].Y);

         // cout<<"#";

        for (int i=1;i<length;i++)

        {

          snake[i].Y=snake[0].Y-i;

          snake[i].X=snake[0].X;

         // gotoxy(snake[i].X,snake[i].Y);

         // cout<<"#";

        }

        direction=8;

    }

    if(key=='s'&&(direction==4||direction==6))

    {

       snake[0].X=snake[0].X;

        snake[0].Y=snake[0].Y+1;

        //gotoxy(snake[0].X,snake[0].Y);

          //cout<<"#";

        for (int i=1;i<length;i++)

        {

          snake[i].Y=snake[0].Y-i;

          snake[i].X=snake[0].X;

          //gotoxy(snake[i].X,snake[i].Y);

         // cout<<"#";

        }

       direction=2;

    }

}
