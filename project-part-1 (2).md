# Heat Equation Simulation Project, Part 1
### CMDA 3634 Fall 2020
##### Planning Document Due:
11:59 PM Eastern, Fri. Sep. 18, 2020 to Canvas
##### First Push Due:
11:59 PM Eastern, Fri. Sep. 25, 2020 in GitHub repository (with at least 30 lines of code)
##### Code and Final Writeup Due:
11:59 PM Eastern, Fri. Oct. 2, 2020 in GitHub repository

### Collaboration and References
All work you submit must be your own. You are allowed to seek help from teaching staff. You may discuss concepts and share references with classmates, but may not share code (or detailed pseudocode that closely resembles real syntax). You must list every classmate you collaborate with in your references.md file, along with all references you looked up. All submitted code will be run through the MOSS structural similarity detection system, and suspiciously similar codes will be submitted to the Office of Undergraduate Academic Integrity for investigation.

### Introduction to finite difference modeling of the heat equation

If you are unable to see the math equations below, open this Markdown document on [https://dillinger.io/](https://dillinger.io/)

See slides and introduction to problem recorded from class call on Sep. 14, especially, these slides outline the problem discretization and finite difference stencil operation.

If you're curious and want more info on how to derive finite difference approximations from Taylor series, check out [these notes](https://www.math.ubc.ca/~peirce/M257_316_2012_Lecture_8.pdf).


The following pseudocode describes a heat equation simulation:

```
Initialize the initial temperature state as the prior state 
Record a snapshot of the initial wavefield at time index i = 0
For each time step i = 1, 2, ..., nSteps: 
    For each row k:
        For each column j:
        	If a boundary point:
        		update the current state at k,j to be B
            If an interior point: 
            	update the current state at k,j as a stencil operation on the prior state
    Copy the current state values into the prior state array
    If i mod stepsPerCheckPt is 0, record a snapshot of the prior state array
```

### Deliverables

Go to [https://classroom.github.com/a/vHE5LF2S](https://classroom.github.com/a/vHE5LF2S) to get the starting GitHub repository.

0. **One-page planning document**, a pdf submitted to Canvas by Sep. 18.

For the coding portion of this assignment, you will create the following code and produce a markdown writeup with references, all due Oct. 2 through GitHub. An initial push of at least 30 lines of code should be in your GitHub repository due Sep. 25. No files other than the files initially provided and the files listed below should be submitted, and you must use the exact spelling and organization described here. 
1. **Makefile** (provided, don't edit)
2. **README.md** (provided, don't edit)
...I had code/material.h/c in here twice at 3. and 4. and am not renumbering everything below...
5. **code/checkPt.h** containing struct definition and function prototypes (not provided, you must create)
6. **code/checkPt.c** containing function implementations
7. **code/material.h** containing struct definition and function prototypes (provided, don't edit)
8. **code/material.c** containing function implementations (not provided, you must create)
9. **code/simulation.h** containing struct definition and function prototypes (provided, don't edit)
10. **code/simulation.c** containing function implementations (provided, but you modify)
11. **code/README.md** (provided, don't edit)
12. **obj/README.md** (provided, don't edit), and make sure you don't put any executables on GitHub
13. **results/README.md** (provided, don't edit)
14. **results/pointTest.txt** (not provided, text file result of your test), and make sure you don't put all the figures on GitHub
15. **results/pointTestSkip.txt** (not provided, text file result of your test), and make sure you don't put all the figures on GitHub
16. **writeup/README.md** (provided, don't edit)
17. **writeup/writeup.md** (not provided, you must create) markdown file where you will write up your results
18. **writeup/references.md** (not provided, you must create) markdown file where you will write a list of your collaborators and any references you used
19. **writeup/figname.png** (not provided, and you only put a couple  of figures specifically requested for the writeup) the images referenced in your writeup
20. **test/unitTestMat.c** (provided, don't edit)
21. **test/unitTestSim.c** (provided, don't edit)
22. **test/unitTestCheckPt.c** (provided, but you modify)
23. **test/pointSim.c** (provided, don't  edit)
24. **test/pointSimSkip.c** (not provided, you must create)
25. **test/readPlotSnaps.py** (provided, don't edit) a Python script provided to visualize results of the provided tests (provided)
26.  Your GitHub repository will be reviewed to ensure you met the first commit deadline and committed code at least 5 times throughout. Don't forget to push so the code is available remotely.

Note: only the files listed above should be pushed to your GitHub repository. No executable files or output data should be committed.

### Instructions
0. One page planning document: Carefully read the instructions, read through all of the provided source code, and try to diagram for yourself how the code is organized (and what pieces need to be filled in). If you see things you don't understand, start asking in office hours or on Piazza. Write up a one page document describing your plan to complete the assignment (1" margins, single-spaced, no smaller than 10 pt font, and can include diagrams/tables as needed). Save it as a pdf and upload to Canvas. This must describe: 
	- i. List the lecture and lab examples you will use to guide your code development
	- ii. Describe an outline of the provided code, where your new code will fit in, and how you will be able to tell if your code is correct. If you can think of any additional tests you plan to add, describe those.
	- iii. Dependencies between code pieces- For example, if function A calls function B, then function B must be tested and debugged before function A can be tested and debugged. 
	- iv. An estimated timeline for your code development, testing, and writeup (this is particular to your own working style, and the order you plan to approach each piece of code)

1. Implement these functions and structs. You must include some error checking in each function.
	* In material.c (following the prototype and struct def. in material.h) write a function called **initMaterial** that assigns all the important basic data to a material struct. 
		* 1st input: aMaterial, a pointer to a material struct that you want to assign data values to 
		* 2nd input: Nx, an unsigned integer representing the number of points in the x direction of the grid (i.e. number of columns in the matrix)
		* 3rd input: Ny, an unsigned integer representing the number of points in the y direction of the grid (i.e. number of rows in the matrix)
		* 4th input: dx, a float representing the spacing between successive x points (like Delta x in the slides)
		* 5th input: dy, a float representing the spacing between successive y points (like Delta y in the slides)
		* 6th input: alpha, a float representing the thermal diffusivity of the material
		* Output: flag, an integer that should be 0 if no issues were encountered and nonzero if issues were encountered
	* In checkPt.c (following the prototype in material.h) write a function called **calcNSnaps** that calculates the number of snapshots that should be recorded if a simulation will have nSteps time steps (including time 0) and every stepsPerCheckPt time steps have a snapshot recorded (including time 0). For instance, if a simulation will have 4 time steps, $$t_0, t_1, t_2, t_3$$ and a wavefield will be recorded every 3 time steps (at $$t_0$$ and $$t_3$$), this should return 2.
		* 1st input: nSteps, and integer representing the number of time steps in the simulation (including the 0th time step)
		* 2nd input: stepsPerCheckPt, an integer representing the number of time steps from one checkpoint to the next.
		* Output: an integer representing the number of snapshots that will be taken during the simulation.
	* In checkPt.c (following the prototype in checkPt.h) write a function called **cleanupCheckPtTime** that cleans up the memory allocated on the heap for a checkPtTime (which should be called before that checkPtTime goes out of scope).
		* Input: thisCheckPt, a pointer to a checkPtTime struct that you want to clean up 
		* Output: flag, an integer that is 0 if there are no problems, and 1 if any issues are encountered (for example, what if there's no memory to clean up?)
	* Explicit finite difference methods cannot jump too far ahead in time for a single time step (i.e. there's an upper bound on dt) or else the simulation is unstable and unrealistic. In simulation.c, write a function called **calcMaxTimeStep** that takes a sim struct through a pointer, and calculates an upper bound for dt. In this case, we'll use a bound of $$dt < min(dx^2, dy^2)/(4\alpha)$$. 
		* Input: thisSim, a pointer to a sim struct that's had its alpha, dx, dy and dt values initialized already
		* Output: dtMax, a float that should equal $$min(dx^2, dy^2)/(4\alpha)$$ for that sim
	* Fill out part of **initSim** in simulation.c that is denoted by comments, and has each part of the initialization process outlined with a comment before the space for you to fill in code.
		* 1st input: thisSim, a pointer to a sim struct that has been declared but not initialized yet
		* 2nd input: timeStep, a float denoting the value of dt the user wants to to assign to thisSim
		* 3rd input: valsForInitState, a pointer to a float array on the heap with as many points as the simulation's state (and this Material) has, which should hold the initial temperature distribution values
		* 4th input: bdryVal, a float indicating the temperature held constant on the boundary of the material throughout the simulation
		* 5th input: thisMaterial, a pointer to a material struct on which the simulation will take place. This assumes that thisMaterial has already been initialized with all its data.
		* Output: flag, an integer that's 0 if no issues were encountered and 1 if there were issues
	* Fill out part of **oneStep** in simulation.c that is denoted by the comment section, where you will fill in the code to update the current temperature state (both boundary and interior points using the stencil operation). You can assume that oneStep will only ever be called by runSim.
		* Input: thisSim, a pointer to a sim struct that has been previously initialized.
		* Output: flag, an integer that should be 0 if no problems were encountered and a nonzero integer (you can choose) if issues were encountered.
	* Fill out part of **cleanupSim** in simulation.c that cleans up the memory allocated on the heap for a sim struct (which should be called before that sim goes out of scope).
		* Input: thisSim, a pointer to a sim struct that you want to clean up
		* Output: flag, an integer that's 0 if no issues were encountered and 1 if issues were encountered.

2. As you are implementing each function be sure to run some test cases along the way in the  order that you planned. To help guide some of your debugging, you can use tests from test/unitTestMat.c, test/unitTestSim.c and test/unitTestCheckPt.c which all have corresponding rules in the Makefile.  Note that these are not exhaustive tests, so passing all provided tests does not guarantee your code will pass all the grader's tests. Learning to write your own tests is an extremely valuable skill, so you are encouraged to add some of your own unit tests or to add modified versions of these tests in addition to the original tests (but this is not mandatory).

3. Design and implement at least two of your own unit tests in test/unitTestCheckPt.c. You must be able to describe the logic behind these tests in your writeup. Remember that it is just as easy to have a bug in your test as it is a bug in the code you are testing. How do you know that your test is implemented correctly? 


4. Run the simulation defined in test/pointSim.c which has a $$N_x \times N_y = 20 \times 30$$ grid with $$dx=1.5$$, $$dy=1.0$$, $$dt = 0.1$$, $$B = 0.1$$, $$\alpha = 2.0$$, and starts out with a temperature of 0.1 everywhere except one point, $$u(x_8,y_5,0) = 100$$, then runs the simulation for 50 time steps. It will record output snapshots from every time step in results/pointTest.txt. You'll visualize these snapshots of the temperature field with the provided test/readPlotSnaps.py script. 

5. Create your own simulaton in test/pointSimSkip.c which has a $$Nx \times Ny = 50 \times 90$$ grid with $$dx=1.5$$, $$dy=1.0$$, $$dt = 0.04$$, $$B=2.0$$ and $$\alpha = 5.0$$. It will start out with a temperature of 2.0 everywhere except four points: $$u(x_{20},y_{30},0)=2000$$, $$u(x_{20},y_{60},0)=1000$$, $$u(x_{30},y_{30},0)=1000$$, $$u(x_{30},y_{50},0)=2000$$. It should run for 200 steps and record the output snapshot from every fourth time step in results/pointTestSkip.txt. You must build and run this test using the corresponding rule already provided in the Makefile. You'll visualize these snapshots of the temperature field with the provided test/readPlotSnaps.py script.

6. Answer the writeup questions below in **writeup/writeup.md** and include all references and collaborators you discussed the project with in **writeup/references.md**.

7. **Submit:** You should have been committing code changes regularly. You will lose points on your grade if your repo contains any executable files or any more figures than the ones requested in the writeup. Be sure those changes are pushed to GitHub via the status and log git commands(be sure to double check GitHub website at final submission).

### Writeup Questions
1. Describe the relationship between a checkPtTime, sim and material. How do these work together to perform a simulation? 
2. Describe each test case that you made up. For one of your new test cases, answer the following questions:
    1a. What were the possible issues you were trying to check for?
    1b. How is this different from the provided test cases? 
    1c. What is the setup of the test, and expected output for a correct implementation? 
3. Include the visuals pointPlots000.png, pointPlots001.png, pointPlots002.png pointPlots010.png, and pointPlots049.png as figures in your writeup here.
Just for fun, not credit: You can make a gif from your numbered figures using ImageMagick or similar programs to see a movie of how the temperature changes.
4. Based on both of your simulation results, does the temperature at a point always decrease? If not, what is a location in the test/pointSim.c simulation where that is not true, and what behavior do you observe over time for that location? If you have difficulty telling from the visuals, you may want to try copying the python script and modifying the copy so that it will print out the value of the temperature at one location across many times.
5. Include the visuals skipPlots000.png, skipPlots001.png, skipPlots002.png skipPlots010.png, and skipPlots049.png in your writeup here.
6. Why might you decide to only record a subset of the time steps' states in checkpoints as opposed to every time step? 
7. (no points designated, but this helps you maximize partial credit) If you believe your code still has any bugs, you must describe them here so that we can attempt to evaluate your work for partial credit. Which of your test cases shows these bugs? How would you expect the outputs of these test cases change were your code to be fixed? Which lines of code do you believe contain the bug? How have you attempted to fix this portion of code?



### Rubric
* (1.5 pts) Planning document
* (1.5 pts) First commit/push to GitHub
* (0.5 pts) Appropriate use of GitHub including at least 8 commits over course of project, with clear and concise comments describing commits
* (7 pts) Code correctness - will be judged based on provided tests as well as a variety of additional test cases. Note: the grader must be able to use scripts to automatically compile and run your code on Cascades for you to receive credit.
* (2 pts) Coding clarity, organization and comments
* (2.5 pts) Writeup with requested figures, collaborators and references

