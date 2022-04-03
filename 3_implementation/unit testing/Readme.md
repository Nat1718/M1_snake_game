using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Threading;

namespace Snake
{
    /// <summary>
    /// Start class, everything goes from here.
    /// </summary>
   public class Starter : InterF
    { 
        public static object Key { get; private set; }

        static void Main(string[] args)
        {
            
            Console.ForegroundColor = ConsoleColor.Green;
            bool loopC = true;
            while (loopC)
            {
                string mystring = null; Console.WriteLine("Welcome to Snake.\nPress S+Enter to start or press Q+Enter to exit.");
                mystring = Console.ReadLine();
                switch (mystring)
                {
                    case "Q":
                        Environment.Exit(0);
                        break;
                    case "S":
                        Console.WriteLine("Starting game!");
                        System.Threading.Thread.Sleep(4000);
                        loopC = false;
                        break;
                    default:
                        Console.WriteLine("Invalid Entry.. Try again.");
                        break;
                }

            }

            //Call for GameSense if
            //they want to play.
            GameSense(); 

        }


      
        /// <summary>
        /// Game object!
        /// </summary>
        public static void GameSense()

        {
            //Game console height/width.
            Console.WindowHeight = 30;
            Console.WindowWidth = 70;
            int screenwidth = Console.WindowWidth;
            int screenheight = Console.WindowHeight;
            Random randomnummer = new Random();
            //Lenght of tail == current score.
            int score = 2;
            int gameover = 0;
            //Gather positions from pixel class.
            pixel positions = new pixel();
            positions.xpos = screenwidth / 2;
            positions.ypos = screenheight / 2;
            positions.Black = ConsoleColor.Red;
            string movement = "RIGHT";
            //Position manegment.
            List<int> xpos = new List<int>();
            List<int> ypos = new List<int>();
            int berryx = randomnummer.Next(0, screenwidth);
            int berryy = randomnummer.Next(0, screenheight);
            //Time manegment.
            DateTime time1 = DateTime.Now;
            DateTime time2 = DateTime.Now;
            string buttonpressed = "no";

            
            //Draw world from GameWorld.cs.
            GameWorld.DrawBorder(screenwidth, screenheight);

            while (true)
            {
                GameWorld.ClearConsole(screenwidth, screenheight);
                if (positions.xpos == screenwidth - 1 || positions.xpos == 0 || positions.ypos == screenheight - 1 || positions.ypos == 0)
                {
                    gameover = 1;
                }

                Console.ForegroundColor = ConsoleColor.Green;
                if (berryx == positions.xpos && berryy == positions.ypos)
                {
                    score++;
                    berryx = randomnummer.Next(1, screenwidth - 2);
                    berryy = randomnummer.Next(1, screenheight - 2);
                }
                for (int i = 0; i < xpos.Count(); i++)
                {
                    Console.SetCursorPosition(xpos[i], ypos[i]);
                    Console.Write("*");
                    if (xpos[i] == positions.xpos && ypos[i] == positions.ypos)
                    {
                        gameover = 1;
                    }
                }
                if (gameover == 1)
                {
                    break;
                }
                Console.SetCursorPosition(positions.xpos, positions.ypos);
                Console.ForegroundColor = positions.Black;
                Console.Write("*");
                //Food color & position.
                Console.SetCursorPosition(berryx, berryy);
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.Write("*");
                Console.CursorVisible = false;
                time1 = DateTime.Now;
                buttonpressed = "no";
                while (true)
                {
                    time2 = DateTime.Now;
                    if (time2.Subtract(time1).TotalMilliseconds > 500) { break; }
                    if (Console.KeyAvailable)
                    {
                        ConsoleKeyInfo info = Console.ReadKey(true);
                        
                        if (info.Key.Equals(ConsoleKey.UpArrow) && movement != "DOWN" && buttonpressed == "no")
                        {
                            movement = "UP";
                            buttonpressed = "yes";
                        }
                        if (info.Key.Equals(ConsoleKey.DownArrow) && movement != "UP" && buttonpressed == "no")
                        {
                            movement = "DOWN";
                            buttonpressed = "yes";
                        }
                        if (info.Key.Equals(ConsoleKey.LeftArrow) && movement != "RIGHT" && buttonpressed == "no")
                        {
                            movement = "LEFT";
                            buttonpressed = "yes";
                        }
                        if (info.Key.Equals(ConsoleKey.RightArrow) && movement != "LEFT" && buttonpressed == "no")
                        {
                            movement = "RIGHT";
                            buttonpressed = "yes";
                        }
                    }
                }
                xpos.Add(positions.xpos);
                ypos.Add(positions.ypos);
                switch (movement)
                {
                    case "UP":
                        positions.ypos--;
                        break;
                    case "DOWN":
                        positions.ypos++;
                        break;
                    case "LEFT":
                        positions.xpos--;
                        break;
                    case "RIGHT":
                        positions.xpos++;
                        break;
                }
                if (xpos.Count() > score)
                {
                    xpos.RemoveAt(0);
                    ypos.RemoveAt(0);
                }
            }
            Console.SetCursorPosition(screenwidth / 5, screenheight / 2);
            Console.WriteLine("Game over, Score: " + score);
            Console.SetCursorPosition(screenwidth / 5, screenheight / 2 + 1);
            System.Threading.Thread.Sleep(1000);
            restart();
            
        }

        /// <summary>
        /// Restarter.
        /// </summary>
        public static void restart()
        {
        
            string Over = null; Console.WriteLine("\nWould you like to start over? Y/N");
            bool O = true;

            while (O)
            {
                Over = Console.ReadLine();
                switch (Over)
                {
                    case "Y":
                        Console.WriteLine("\nRestarting!");
                        System.Threading.Thread.Sleep(2000);
                        break;
                    case "N":
                        Console.WriteLine("\nThank you for playing!");
                        Environment.Exit(0);
                        break;
                    default:
                        Console.WriteLine("Invalid Entry.. Try again.");
                        break;
                }
            }

        }
        /// <summary>
        /// Set/get pixel position.
        /// </summary>
        class pixel
        {
            public int xpos { get; set; }
            public int ypos { get; set; }
            public ConsoleColor Black { get; set; }
        }
    }
