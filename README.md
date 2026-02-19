# RoboticsA1
Assessment 1 

**Project Overview**
this project implemenst a maze-solving rovot in VEX V using a tremaux-inspired exploration stargey and bread-first search (BFS) for shortest-path return. 

Aims:
1. Explores a dynamic wall maze using edge marking (Trémaux method).
2. Detects the goal using Location Sensor coordinates.
3. Builds a graph of explored corridors while moving.
4. Computes the shortest path back to the start using BFS.
5. Returns to the start using the computed shortest route.
6. Displays an ASCII grid of the explored maze and shortest return path on the Brain screen, print ASCII grid on the brain screen:
   - S = Start
   - G = Goal
   - * = Fastest route back (shortet path)
   - . = Explored cells
  
**Sensors used:**
location sensor: 
- Detect when the robot has reached the goal using coordinates
- Convert robot position into a grid cell '(cx, cy)'
using the code,
cx = round(x_mm / STEP)
cy = round(y_mm / STEP)

The constants used are:
TARGET_X = -100 
TARGET_Y = 900 
TOL = 80 

**Distance Sensor**
the disatnce sensor determins whether a corrider is open or blocked.
- if distance > WALL, this means the corridor is open.
- if distance <= WALL, wall is detected.

Constants used:
- WALL = 70

**Movement System**
The maze is treated as a grid, 
STEP = 250, represents one grid movement.
Headings are encoded as:
- 0 = North
- 1 = East
- 2 = South
- 3 = West


**How it works**
1) Exploration Phase
The robot treats the maze like a grid of cells. Each move is one "title" foward ('STEP' mm), it uses the edge-marking stragey
At each cell, it tests possible diections and chooses the corrider with the lowest "mark count".
For each cell:
1. it check four directions in proirity order:
   - right
   - forward
   - left
   - back
    for each open corridor, it checks how many times that edge has been used(marks[(cell_x, cell_y, direction)]
3. it selects the direction with the lowest mark count

Treamux algorithm: This algorithm was a good strategy as it keeps track of how many times you've used each corrider so you dont waste time repeadtedly exploring the same routes.
while exploring, the robot also builds:
- explors every cell it has stepped on, making anote of it 
- graph = adjaceny list of corriders between cells

**Goal Detection**
the robot continusoly checks its location using the location sensor.
when the co-ords match with thegoal co-ords:
- the exploration stops
- the goal of the grid is stored
- the shortest path back to the computation begins 

**Fastest Return (BFS Shorest Path)**
After reaching the goal, we find the shortest path bath using BFS;
Process:
- BFS starts from the goal
- it spreads through the discovered graph
- it creates parent pointers so we can reconstrust the shorest route
This shortest route is then used to:
- Create the route set (ASCII )
- Drive the robot back to start using computed remebered steps.

**ASCII grrid display**
The program prints a simple ASCII frid to the brain screen using:
- brain.clear()
- brain.print()
- brain.new_line()

It prints:
- a small pathway 
- the explored region

To prevent screen over these limits are set:
- MAX_ROWS = 18
- MAX_COLS = 30
Having these limits stops the too many lines and crashing the brain screen output. 

**Return Movement**
If BPS succesfuly follows the computed a shortest path:
- The robot follows the computed route from goal to start
- It turns based on heading differences.
- It moves forward one STEP per grid cell
if he BFS is not available the robot retraces using the stored direction stack (path)

**Output/ what should happen** 
Robot behaviour 
- robot explores the maze
- when it reaches (TARGET_X, TARGET_Y) range, it stops exploing.
- it prints the ASCII grid to the brain screen
- it return to the start using the BFS shortcut route (fastest back).

**Algorithm Summary**
Exploration stragety: Tremaux-inspired edge marking, prioritises corriders with fewer marks to reduce repeated exploration.
Optimised Return: Builds a graph exploration, uses BFS to compute the shortest path back, Displays that path clearly ASCII. 

**Troubleshooting**
Robot reaches goal but does not return 
- Increased TOL so the goal is detected cleanly.
- Changing the step measurement from 200 to 250 to make sure it matches the grid

**Evaluation:**
Overall, I believe the robot successfully met the aims of the assignment. It was able to explore the dynamic maze using a Trémaux-inspired marking system, detect the goal using the Location Sensor coordinates, build a graph of explored corridors, compute the shortest path using BFS, display the ASCII grid on the Brain screen, and return to the start using the computed route.
The exploration strategy worked well. By marking corridors with a count and always choosing the direction with the lowest mark, the robot avoided repeatedly exploring the same paths unnecessarily. Although it sometimes revisited cells, this behaviour was controlled and logical rather than random. The marking system helped reduce inefficient looping compared to a simple wall-following approach.
The BFS implementation was effective in computing the shortest path back to the start. Because BFS guarantees the shortest path in terms of number of moves (in an unweighted graph), it ensured the return journey was optimal based on the discovered maze structure. One important detail is that BFS only works on the explored graph, meaning it cannot consider corridors that were never discovered. However, since the graph was built during exploration, this approach was suitable for the task. The fallback retrace method also made the system more robust in case the start was not reachable in the BFS tree.
There were some limitations. The ASCII display does not show walls, only explored cells and the shortest route, so the visualisation is simplified. The system also relies on rounding position values using STEP, which means small inaccuracies in the Location Sensor could slightly affect grid mapping. Additionally, the performance would likely decrease in a significantly larger maze because the graph and explored structures would grow in size.
If I were to improve the project further, I would enhance the ASCII display to include walls for clearer visualisation. I could also experiment with alternative exploration strategies to compare efficiency. Finally, I would refine the grid conversion process to reduce potential rounding inconsistencies.
Overall, the project demonstrates successful implementation of exploration, graph construction, shortest path computation, and visual output within the constraints of the VEX VR environment.


AI Statments: 
I can confirm no AI tools were used in preparation or completion of this assessment. 
