/*
---ANALYSIS--

user input table
[1] [2] [3] 
[4] [5] [6]
[7] [8] [9]

coord table
[0,0] [0,1] [0,2]
[1,0] [1,1] [1,2]
[2,0] [2,1] [2,2]

win properties diagonal
- must be symmetrical difference from middle
- both win types must have center coord [1,1]
- middle must be odd

win properties horizontal
- all rows have same x cord
- arithmetic sequence

win properties vertical 
- arithmetic sequence
- all rows have same y cord

*/

// setup

bool win = false;

char[,] grid_2d = {
    {'1', '2', '3' },
    {'4', '5', '6' },
    {'7', '8', '9' }
};

// MAPPING INPUT TO COORDINATES
Dictionary<char, int[]> input_to_grid = new Dictionary<char, int[]>()
{
    { '1', new int[] {0,0}},
    { '2', new int[] {0,1}},
    { '3', new int[] {0,2}},
    { '4', new int[] {1,0}},
    { '5', new int[] {1,1}},
    { '6', new int[] {1,2}},
    { '7', new int[] {2,0}},
    { '8', new int[] {2,1}},
    { '9', new int[] {2,2}}
};

ConsoleKeyInfo input;
 
// - DISPLAY GRID -
void RefreshGrid()
{
    Console.Clear();
    Console.WriteLine(@$"  

              {grid_2d[0,0]}  |  {grid_2d[0,1]}  |  {grid_2d[0,2]}  
            -----|-----|-----
              {grid_2d[1, 0]}  |  {grid_2d[1,1]}  |  {grid_2d[1,2]} 
            -----|-----|-----
              {grid_2d[2,0]}  |  {grid_2d[2,1]}  |  {grid_2d[2,2]} 
    ");
}

// - AI move -
Random rand= new();
int ai_move_x = 0;
int ai_move_y = 0;
int ai_moves_count = 0;
// AI strategy : first 2 moves are random based, due to more free space, finds random places from 1st try for both 2 turns with 59% probability
// later, the strategy selects incomplete player columns and rows, taking advantage of winchecking algorithm
void AIMove()
{
    if (ai_moves_count < 2)
    {
        do
        {
            ai_move_x = rand.Next(0, 3);
            ai_move_y = rand.Next(0, 3);
        }
        while (!char.IsDigit(grid_2d[ai_move_x, ai_move_y])); // && grid_2d[ai_move_x, ai_move_y] == 'X'
    }
    // else ai move coords will be set during WinCheck

    ai_moves_count++;
    Thread.Sleep(700);
    grid_2d[ai_move_x, ai_move_y] = 'O';

    RefreshGrid();
    WinCheck('O', [ai_move_x,ai_move_y]);
}


// - WIN CHECK -
void WinCheck(char player_mark, int[] coords)
{
    // scan horizontal row
    for (int r = 0; r < 3; r++)
    {
        if (grid_2d[r,coords[1]] != player_mark) 
        {
            // check if this slot is free to defend
            if (char.IsDigit(grid_2d[r, coords[1]]))
            {
                ai_move_x = r;
                ai_move_y = coords[1];
            }
            else ai_moves_count = 1;
            break; // other sign present in row
        }
        else if (r == 2)
        {
            Console.WriteLine($"\n\n--- PLAYER {player_mark} WINS VERTICALLY! ---");
            win = true;
        }
    }
    // scan vertical column
    for (int j = 0; j < 3; j++)
    {
        if (grid_2d[coords[0], j] != player_mark) 
        {
            // check if this slot is free to defend
            if (char.IsDigit(grid_2d[coords[0], j]))
            {
                ai_move_x = coords[0];
                ai_move_y = j;
            }
            else ai_moves_count = 1;
            break; // other sign present in column (due to indexing for 2d array being row column, which is [y,x] the graph has to be at 90 degrees which isnt conventient)
        }
        else if (j == 2)
        {
            Console.WriteLine($"\n\n--- PLAYER {player_mark} WINS HORIZONTALLY! ---");
            win = true;
        }
    }
    // scan diagonal
    if (grid_2d[1,1] == player_mark) // if middle taken by player, else dont even check
    {
        if (grid_2d[2,0] == player_mark && grid_2d[0,2] == player_mark || grid_2d[0,0] == player_mark && grid_2d[2,2] == player_mark)
        {
            Console.WriteLine($"\n\n--- PLAYER {player_mark} WINS DIAGONALLY! ---");
            win = true;
        }
    }
}

// ---------- MAIN ----------

// INTRO
Console.WriteLine(@"
       | TIC TAC TOE |
         X  | O |  X
        ----|---|----
         >  | X |  <
        ----|---|----
         X  | O |  X
                         __
--- PRESS ENTER TO START    \
                           \|/
");
Console.ReadLine();

RefreshGrid();
for (int i = 0; i < 5; i++)
{
    if (win)
    {
        Console.WriteLine("GAME WIN");
        break;
    }

    // PLAYER INPUT
    bool valid_flag = false;
    int[]? coords;
    do
    {       
        Console.WriteLine("PRESS A NUMBER ON THE GRID TO PUT A CROSS");
        input = Console.ReadKey();

        if (char.IsDigit(input.KeyChar))
        {
            // if such number exists on grid
            if (input_to_grid.TryGetValue(input.KeyChar, out coords) && char.IsDigit(grid_2d[coords[0], coords[1]]))
            {
                grid_2d[coords[0], coords[1]] = 'X';  
                RefreshGrid();

                WinCheck('X', [coords[0], coords[1]]); 
                valid_flag = true;
            } 
            else Console.WriteLine(" - x - That place is unavailable, try again");
        }
        else Console.WriteLine(" - x - Not a number, try again");

    } while (!valid_flag);

    // game over check & AI move
    if (win)
    {
        Console.Write("--- GAME OVER ---");
        break;
    }

    if (i == 4)
    {
        Console.WriteLine( "\n\n--- GAME OVER DRAW ---");
        break;
    }
    else AIMove();
}
Console.ReadLine();
