# Lab 05: Scalability, Triangular Matrix-Vector Multiply
### CMDA 3634 Fall 2020
### Due 11:59 PM Eastern, Tue. Oct. 13, 2020

### Collaboration and References
You may complete this lab with a partner. No groups of 3 will be approved. You may work alone if you choose. The work you submit must be your own work (or the work of you + partner if doing a partner submission. You are allowed to discuss big-picture concepts with other classmates and seek help from teaching staff. The only classmate you are allowed to screenshare code with is your partner. You must list every classmate you collaborate with (even just to share references or discuss big picture concepts with) in your references.md file. 

Recommended references: 
* Useful SLURM (job submission) commands including checking job status, cancelling jobs, etc...: [https://docs.rc.fas.harvard.edu/kb/convenient-slurm-commands/](https://docs.rc.fas.harvard.edu/kb/convenient-slurm-commands/)
* Doing arithmetic with variables in bash scripts: [https://ryanstutorials.net/bash-scripting-tutorial/bash-arithmetic.php](https://ryanstutorials.net/bash-scripting-tutorial/bash-arithmetic.php)
* Working with arrays in bash scripts: [https://www.geeksforgeeks.org/array-basics-shell-scripting-set-1/](https://www.geeksforgeeks.org/array-basics-shell-scripting-set-1/)

*Partner Submission Expectations:*
* The expectations for submissions by partners and for individuals are the same. That is, if you complete the assignment alone, the expectations are not lowered.
* When working with a collaborator, it is your responsibility to regularly push and pull changes. If your versions are out of synch, this could lead to merge conflicts which you will need to resolve. A reference on these issues can be found at [https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/resolving-a-merge-conflict-on-github](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/resolving-a-merge-conflict-on-github)
* You will choose your partner. You have a chance to choose a new partner on each lab, or to complete any lab alone. 
* Communicate early and often with your partner about who will contribute what, and set intermediate deadlines. 
* Each partner is expected to contribute equally. Graders will review the number of lines of code committed by each partner. Graders recognize that code line numbers alone are not enough to reflect full effort in algorithm design, debugging, testing, and analysis of writeup questions, thus partners must also include a list of each person's specific contributions to the coding, testing, debugging and writeup in writeup/references.md. 
* If the contribution appears to be a 60/40 split or more uneven, the partner with lower contribution will have their grade multiplied by a contribution factor between 0 and 1 (with 0 being no contribution by that partner, and 1 being around 40 percent of the assignment contributed by that partner). Apart from these very uneven contributions, both partners will receive the same grade, regardless of which partner was responsible for which parts.
* When the list of specific contributions is missing from writeup/references.md in a partner submission, both partners will have an automatic contribution factor of 0.9 (effectively losing a letter grade), and that may be further reduced for either partner based on proportion of code commits.



### Deliverables
For this assignment, you will create/modify/use the following files. You will submit them by pushing to your (or you and your partner's shared) Github repository that is automatically created at [https://classroom.github.com/g/4EILBC1X](https://classroom.github.com/g/4EILBC1X).

0. **readme.md** You will create this readme in which you will indicate both partners names (or if it was comleted individually).
1. **code/Vector.c** with function implementations related to setup, filling entries, and teardown of vectors. This is provided and will not be modified.
2. **code/Vector.h** with prototypes related to Vector structs. This is provided and will not be modified.
3. **code/Matrix.c** with function implementations related to triangular matrices and matrix-vector multiplication. This is provided and will not be modified.
4. **code/Matrix.h** with prototypes related to TriMatrix structs. This is provided and will not be modified.
5. **code/matvecTiming.c** is the main function that runs some different OpenMP matrix-vector multiplications.  This is provided and will not be modified.
6. **code/Makefile** includes rules to build the serial and parallel tests. This is provided and will not be modified.
7. **code/weak_scaling.sh** You'll create and write this weak scaling test script.
8. **code/strong_scaling.sh**  You'll create and write this strong scaling test script.
9. **code/visualize_strong.py** script to visualize strong scalability after you copy in your results
10. **code/visualize_weak.py** script to visualize weak scalability after you copy in your results
11. **writeup/writeup.md** a markdown file where you will write up your results, **must include both partner's names at the top (since GitHub username is different)**
12. **writeup/references.md** a markdown file where you will write a list of your collaborators and any references you used. If you are submitting with a partner, you must include an itemized list of the contributions of every contribution by each partner.
13. **writeup/fig/strongScaling.png** showing your scalability results
14. **writeup/fig/weakScaling.png** showing your scalability results
15. **code/slurm-######.out** You should leave two slurm-######.out files in your directory (one for strong scalability, one for weak scalability).
16. **code/bin/readme.md** just a generic readme so the bin folder shows up in your repo. This is provided and will not be modified.


Note: no binaries or other figures should be uploaded to Github. Points will be deducted if they are present. 

### Instructions
0. Accept the assignment and create your team (could just be you, or you and a partner) following the instructions at (https://www.youtube.com/watch?v=-52quDR2QSc&feature=emb_logo)[https://www.youtube.com/watch?v=-52quDR2QSc&feature=emb_logo]. If you're the first partner in your partnership, you'll need to create a team and choose a name, then let your partner know what your team name is so they can join. The assignment has an automatic team size restriction so that no groups bigger than 2 are possible.

1. Log into your account on Cascades. Load the Anaconda module. Clone the starter code with two subdirectories inside: **code/** and **writeup/** . Create **readme.md** in the parent directory to say the full names and GitHub usernames of both partners, or a statement that this lab was completed alone. The files that you will have in each of these subdirectories is listed in the Deliverables section above. You are expected to regularly use git to commit code changes with comments and push those changes. 

2. **Review the provided code:** You are provided with defintions of lower triangular matrices (i.e. matrices where $$a_{i,j} \neq 0$$ if and only if $$j \leq i$$) and vectors, along with setup/teardown methods, methods to initialize entries with zeros or random numbers, and functions to perform a lower triangular matrix-vector multiply in parallel with three  different scheduling schemes: static, guided and dynamic. 
(Writeup question #1: If you have a 100x100 matrix and the code is run with 2 threads with a static scheduling scheme, how many operations are performed by thread 0 and how many operations are performed by thread 1? Show your work.)


3. **Write a script for a strong scalability test:** Create a strong scalability test in strong\_scaling.sh that: (1) prints "Running strong scaling test", (2) Says which node you ran on, (3) loads gcc/7.3.0, (4) builds your binary executables, (5) prints the start time of the job, (6) loops through using 1, 2, 4, and 8 threads to run timing tests for $$n = 10,000$$, including printing out the number of threads used for each test, (7) prints the end time of the job. Select an appropriate number of threads and time to request. This will be fast enough to run in the dev\_q. (Writeup question #2: This question is about how the total number of floating point operations scale with $$n$$. (1a) If $$n$$ is doubled, how many more operations are required for a matrix-vector multiplication? (1b) If $$n$$ is quadrupled, how many more operations are required for a matrix-vector multiplication?)

4. **Run your strong scalability test and report:** Run your test. Copy the times from your slurm-######.out file into the corresponding Python visualization script. Run the visualization script and look at your scalability plots. (Writeup question #3: Copy your timings into a table with 4 columns: # threads, timing regular, timing guided, timing dynamic. Include your figure in your writeup markdown file. In complete sentences, compare the scalability and speed of the three scheduling schemes. For example it might say scheduling scheme A faster than scheme B faster than scheme C. Why was A faster than B? Why was C slower than B? You must comment on how much work there is for each row, and how that connects to the performance of each of the three schemes.)

5. **Write a script for a weak scalability test:** Create a weak scalability test in weak\_scaling.sh that: (1) prints "Running weak scaling test", (2) Says which node you ran on, (3) loads gcc/7.3.0, (4) builds your binary executables, (5) prints the start time of the job, (6) loops through using 1, 2, 4, and 8 threads to run timing tests for $$n$$ equal to 1000 (1 thread), 1414 (2 threads), 2000 (4 threads), 2828 (8 threads), including printing out the number of threads used for each test, (7) prints the end time of the job. Select an appropriate number of threads and time to request. This will be fast enough to run in the dev\_q.

6. **Run your weak scalability test:**  Run your test. Copy the times from your slurm-######.out file into the corresponding Python visualization script. Run the visualization script and look at your scalability plots. (Writeup question #4: Copy your timings into a table with 4 columns: # threads, timing regular, timing guided, timing dynamic. Include your figure in your writeup markdown file. In complete sentences, compare the scalability and speed of the three scheduling schemes (scheme A faster than scheme B faster than scheme C). Why was A faster than B? Why was C slower than B? You must comment on how much work there is for each row, and how that connects to the performance of each of the three schemes.)

7. **References:**  In writeup/references.md make a list of any references you use and any collaborators you worked with. Note that you must also include a comment on any line of code that is based on code from class or a reference. 

8. **Writeup:** In writeup/writeup.md answer the 4 writeup questions using complete sentences. If you are submitting with a partner, you must include an itemized list of the contributions of every contribution by each partner. Since almost everything is provided for you, the grading for this lab is *much more* focused you explaining your understanding of scalability than about the code itself. Be sure your actual name(s) is/are at the top of your writeup along with your GitHub username.

9. **Submit:** Remove any binary executable files in your bin subdirectory. You should have been committing code changes regularly. Be sure those changes are pushed to Github.

