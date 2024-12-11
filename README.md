java cENGR1010J FA2024 Lab6: Pac-Man
Due date: 23:59, December 9, 2024
Background
Pac-Man is a classic video game released in 1980. In this lab, we just need to create a basic gaming
interfac e so that we humans can play with it.
This is a screenshot from a Pac-Man game. A typical Pac-Man world contains a cute Pacman, walls,
small foods, big energy capsules, and lovely ghosts.
To make our lives easier, our version of "Pac-Man" does not feature such fancy graphics. Instead, it is
composed of all ASCII characters. Pacman is represented by a similar-looking  C , walls by  # , foods
by  . , capsules by  o , and ghosts by  @ . (The provided templates print boundaries, so you do not
need to consider them.) The picture below shows the game interface in the console.We have provided compiled executables for Windows, MacOS, and Linux. We recommend you run
the executable and play the game to get a basic idea of what you should do. Also, when you have
any doubt about whatever part you write, if your behavior matches our example program, it is
(almost) sure to say you are safe.
On MacOS or Linux, in order to run the provided executable, you may need to first run the command
 chmod +x Pacman_MacOS_arm64/x86  or  chmod +x Pacman_Linux  under the path of the text file in your
terminal. Contact TAs if you have any problems.
Unlike in the real-time video game Pac-Man, we control our Pacman frame by frame. The game will
pause every frame and wait for your input. You can type " w/a/s/d " into the game to move Pacman,
or " i " for it to stand still. Confirm your input by “Enter”, and the game will show you the next frame.
PartA: Make a Game From Scratch
Files
We have provided you with code templates, which contains  lab6.h ,  io.cpp ,  game.cpp ,  main.cpp .
The contents of each file are listed below:
File Content
lab6.h All the structs and variables as well as the function prototypes.
io.cpp Functions dealing with user inputs and output the game interface in the console.
game.cpp To realize all the functions given in lab6.h.
main.cpp The main function.
In this lab, what you need to do is to fill up the functions in game.cpp according to the comments and
instructions. Then you can run the main.cpp to test your codes with code runner. Use the command
line to compile  io.cpp ,  main.cpp ,  game.cpp  together:
 g++ -std=c++1z game.cpp main.cpp -o lab6 -I -lm . If you have any questions regarding compile
issues, feel free to ask!
However, if you think the functions in  game.cpp  are not enough, you are allowed to write your
own functions in  game.cpp , and do not forget to declare it in  lab6.h .
Variables and Functions
The game operates by a  struct game  structure. We have already specified some components of it:typedef struct game {
//Part A
 char** grid; // a 2-dimensional array of characters to display the game;
 int rows; // number of rows of the grid;
 int columns; // number of columns of the grid;
 int foodCount; // number of remaining food in the game;
 int score; // current score;
 GameState state; // the state of the game, one of losing, onGoing, or winning.
} Game;
Feel free to add more components to this structure if you would like to.
Apart from the structure, there are many functions you need to write for this game to operate. When
implementing provided function prototypes in the templates, you should follow the instructions below,
or see the comments. You MUST NOT modify the function names, or add/remove parameters. You
can also add more functions if you like. We will not check any function that are not provided.
A game of given rows and columns is created by calling the function
 Game* NewGame(int rows, int columns) . In this function, You should:
dynamically allocate space for a Game pointer
initialize all member variables of your Game structure. (For example,  foodCount  and  score 
should be initialized to 0.)
create the member grid by dynamically allocating a 2-dimensional array of given size.
Boundary is not included in either rows or columns, and the cell at top-left corner is at row 0 and
column 0.
When the game ends, the function  void EndGame(Game* game)  is automatically called. In this function,
you should:
free any memory you dynamically allocated, such as grid.
free the parameter game, as it is also dynamically created.
Walls, foods and Pacman are added to the game by functions  AddWall ,  AddFood , and  AddPacman . In
these functions you should:modify the grid in your  Game  structure to make sure whatever item you add displays correctly.
make sure all of these game components can only be added to an empty cell.
make sure Pacman cannot be added to the game if there is already a Pacman.
Finally, you can write the function  void MovePacman(Game* game, Direction direction)  to control your
Pacman.  Direction  is an enum of  {up, down, left, right, idle}  . The rule to move your Pacman
is as follows:
On  idle , Pacman will stay still.
If Pacman would move to an empty cell, Pacman will do so successfully;
If Pacman would move to a food cell, Pacman will move to it and eat the food. Your score will
increase by  FOOD_SCORE = 10 . If Pacman eats the last food, you win the game. You should mark
the state of this game as winning.
If Pacman would bump into a wall or a boundary, Pacman will stay still.
In any of the cases above, your score should decrease by 1, for one turn you have played.
You can have a better understanding of the whole process by reading the sample  main()  function in
 main.cpp  and function  void PlayGame(Game* game)  in  io.cpp .
Time to play!
You can initialize your custom game in your  main()  function by calling  NewGame . After that, you can
add walls and foods to any specific location by  AddWall  and  AddFood . Don’t forget to add a Pacman
by AddPacman to the game.
When your game is prepared, you can call the provided  PlayGame  function. When you win or lose,
 PlayGame  will terminate by calling  EndGame .
If your game runs... Congratula代 写ENGR1010J、C/C++
代做程序编程语言tions! You now have a "complete" Pacman game. You can submit it to
JOJ for Part A, and the first three testcases are for Part A, so don't worry if you cannot pass the last
cases now. However, the game seems a little boring, let’s go to Part B...
Part B: Here come the ghosts!
Your game is missing a part of the greatest fun - the ghosts. In this part, you will add ghosts and
energy capsules to your game so that it will become more playable.
We do not force any restrictions on how you should store your data for ghosts and capsules. Do you
think you need to write a structure, especially for ghosts? If so, what do you need to store in it? Your
design can be in any way you like (but still try not to use global variables), as long as it meets the
requirements below:Requirements for ghosts:
There are at most  MAX GHOSTS = 30  ghosts.
Ghosts are added to the game by the function
 bool AddGhost(Game* game, int r, int c, Direction direction) .
This function is slightly different, as ghosts can be added on a cell with food or a capsule.
Ghosts cover foods and capsules in display, so their cells (originally  .  or  o ) will be
displayed in  @ . However, those food or capsules must still exist, and should be displayed
again when ghosts leave their cells.
 Direction  defines how a ghost moves. Ghosts move either in a horizontal line or a vertical
line. The parameter  direction  in this function is the ghost’s initial direction.
Ghosts are moved by the function  void MoveGhosts(Game* game) .
This function will move all ghosts in the game by one step to their own directions.
Ghosts should be moved in the order they were added.
If a ghost would move onto a cell with food or a capsule, it will cover the food or capsule in
display, so that cell (originally  .  or  o ) will be displayed in  @ . However, that food or capsule
must still exist, and should be displayed again when this ghost leaves that cell.
If a ghost would bump into a wall, another ghost, or a boundary, its direction will reverse,
and it will try to move in the new direction immediately this turn. If then it would bump into
another wall/ghost/boundary, it will stop and won’t move for this turn, with its direction
reversed.
Now it is possible to lose the game. By rules, Pacman always moves first. There are three situations
that need to be specified:
If Pacman directly bumps into a ghost, Pacman will move to that cell, and get killed. You should
mark the game state as losing.
If a food or a capsule is below that ghost, Pacman cannot eat it.
If Pacman moves to a cell that a ghost also attempts to move to, Pacman will perform a
successful move, and the ghost then moves onto Pacman’s cell. You will also lose the game.
Requirements for capsules:
Capsules are large foods that give Pacman superpower. Therefore, capsules are counted as the
number of foods in the game. Pacman must eat all food and capsules to win.
Capsules are added by the function  bool AddCapsule(Game* game, int r, int c) . Like food, a
capsule cannot be added to a cell with a wall, a Pacman, or a ghost. However, a capsule can be
added to a cell with a food, resulting in that food being upgraded to a capsule.
When Pacman eats a capsule, your score will increase by  CAPSULE SCORE = 50 , and Pacman will
gain superpower for its next  CAPSULE DURATION = 10  moves. Its superpower is that:
All ghosts will be scared, and their display change from  @  to  X . When Pacman’s
superpower expires, they change back to their cute evil faces  @ .Scared ghosts are slowed down by 50%, which is shown by that they cannot move every
other turn.(We cannot move ghosts by half a cell, after all) They will be able to move on the
same turn when Pacman eats a capsule, but cannot move the next turn. This goes on until
Pacman’s superpower expires.
When with superpower, Pacman can eat ghosts! When Pacman moves onto a grid with a
scared ghost, it eats the ghost, earning a score of  GHOST SCORE = 200 . If there is a food or a
capsule below that ghost, Pacman eats it as well. That ghost will not respawn and can be
removed from the game. The same goes for the case when a scared ghost bumps into
Pacman.
Pacman’s superpower activates immediately when it eats a capsule, and counts down right after
Pacman’s turn, starting from its next turn. For example, if Pacman and a scared ghost attempt to
move onto the same grid on Pacman’s 10th turn of superpower, Pacman will move first, but its
superpower will immediately expire, and that ghost, not scared anymore, can kill Pacman. In
other words, Pacman’s superpower ends after 10 turns at the same moment of eating a capsule.
If Pacman eats another capsule while it has superpower, the duration of superpower will be
refreshed to 10 turns, rather than stack. In this case, it is possible that a scared ghost has
already moved on the turn(the new 9th turn) right before the turn when Pacman’s superpower
expires(10th). That ghost can still move on its next turn(10th), because it will not be a scared
ghost then.
Finally, you can add ghosts and capsules to your game in your main function, and enjoy the finished
game of Pacman. The submission is the same as how you did for part A. Good luck and have fun!
Rubric
Tasks
Part A: 30 pts
Part B: 90 pts
Oral Explanation: 30 pts
Total: 150 pts
Deduction
Late submission on Canvas: -20 pts per day. JOJ will be closed on the day of the lab, so no late
submission will be accepted on JOJ.
Global variables: Using global variables in such a big project could be dangerous and you will
lose 10 points for each global variable.Submission
You need to compress your  game.cpp  and  lab6.h  file into a single zip file and submit it to JOJ.
After the lab, remember to compress your 2 files into a single zip file in the following name format:
lab6-[Your Last Name]-[Your First Name]-[Your SJTU ID].zip. For example, lab6-Gao-Yunluo521111910151.zip
and submit it to the corresponding homework page on canvas.

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
