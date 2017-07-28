# CSCI356-Tools
## Hello There

Hello, if you are here because you are a member of the USC CSCI 356 teaching team (Professor, TA, CP), then you are at the right place! Here are the tools I have made to make generating homework and grading easy.

Of course, this means all the the homework solutions for each student will be available, so I've encrypted all the files in a single TAR file. You'll need a password to access the files. Please ask the professor or TA for the password.

If you have any questions, feel free to contact me at stephen.sher.94@gmail.com. I'll try to help as much as I can. Have fun and happy grading!

## Project Setup

I've provided two scripts to setup the projects from Carnegie Mellon: the bomb project and the attacklab project.

### Prerequisites

You'll need three things to use the setup scripts:

1. An SSH key that has access to the course github (access to all the student's github accounts, both push and pull). You can find instructions on how to do so [here](http://bits.usc.edu/cs104/labs/lab01.html).

2. A csv file of all the student's USC usernames (a substring of their repo names, eg. ttrojan for the repo hw-ttrojan) as well as their USC IDs. The file should be formatted the same as the sample file `username_id.csv` found in the directory `resources`.

3. Make sure you have the relevant python packages installed:

```
$ sudo apt-get update
$ sudo apt-get install python-pip
$ pip install requests
```

### Joker Bomb Project

Since Dr. Evil is a bit outdated with the current generation of memes and pop culture references, the new bomb project is the Joker Bomb Project. You can find the setup directory under `$ resources/proj_setup/proj2_joker/`.

Once in this directory, the files / directories you should be interested in are:

* `bombs/`: Directory containing every student's bombs and solutions.
* `generate_bombs.py`: Script for creating every student's bombs and automatically pushing to their repos.
* `username_id.csv`: csv file containing every student and their id.

#### Generating All Bombs

To generate the bombs for every student, follow the steps below. **However please be aware that once you run the script the bombs will be pushed to the student's repo. I highly recommend you give it a test run on a dummy repo before you run the script for all the students**. Note that you want to run all the command line commands in `resources/proj_setup/proj2_joker/`.

1. In `generate_bombs.py`, change the `ORG_NAME` on line 9 to the correct github organization name (for your semester).

2. In `generate_bombs.py`, change the link `SSH_KEY` link on line 10 to reference the SSH key you want to use for all the scraping / pushing.

3. Import the student usernames and ids to `username_id.csv`.

4. Run `$ make cleanallfiles`. **Warning: This will delete all previous bombs created in the `bombs/` directory**.

5. Run `$ make`.

6. Run `python generate_bombs.py username_id.csv`. This script will take a while since you'll be pulling and pushing to very student's repository.

At this point you'll want to check github to see if the script worked.

#### Generate One Bomb

If you want to generate a single student's bomb (say you ran clean by mistake), simply run the command `$ perl makebomb.pl -i <id> -s ./src -b ./bombs -l bomblab -u <student_email> -v <username>`.

#### Solutions

You can find the solution to a specific bomb in `bombs/jokerbomb<id>/solution.txt`.

### Attacklab Project

This project is a really good introduction to reading and understanding assembly code. You can find the setup directory under `$ resources/proj_setup/proj3_attacklab/`.


Once in this directory, the files / directories you should be interested in are:

* `targets/`: Directory containing every student's attacklab targets and solutions.
* `generate_attack.py`: Script for creating every student's attacklab and automatically pushing to their repos.
* `username_id.csv`: csv file containing every student and their id.

#### Generating All Attacklabs

To generate the target for every student, follow the steps below. **However please be aware that once you run the script the target will be pushed to the student's repo. I highly recommend you give it a test run on a dummy repo before you run the script for all the students**. Note that you want to run all the command line commands in `resources/proj_setup/proj3_attacklab/`.

1. In `generate_attack.py`, change the `ORG_NAME` on line 9 to the correct github organization name (for your semester).

2. In `generate_attack.py`, change the link `SSH_KEY` link on line 10 to reference the SSH key you want to use for all the scraping / pushing.

3. Import the student usernames and ids to `username_id.csv`.

4. Run `$ make cleanallfiles`. **Warning: This will delete all previous attacklabs created in the `targets/` directory**.

5. Run `python generate_attack.py username_id.csv`. This script will take a while since you'll be pulling and pushing to very student's repository.

At this point you'll want to check github to see if the script worked.

#### Generate One Attacklab

If you want to generate a single student's bomb (say you ran clean by mistake), simply run the command `$ ./buildtarget.pl -u <username> -t <id>` from the directory `proj3_attacklab/src/build/`.


#### Solutions

You can find the solution to a specific bomb in `targets/target<id>/`.

## Grading Scripts

You can find the grading sripts under `resources/grading_tools/`. You will find the following:

1. `grade_project1.py`: This is the script to grade the `datalab-handout` lab.
2. `grade_project2.py`: This is the script to grade the `jokerbomb` lab.
3. `grade_project3.py`: This is the script to grade the `attackalb` lab.
4. `submissions_proj<x>`: This is where the student submission repositories will go. Simply copy and paste from the scraped submissions repo. The script will reference this submissions directory to make your life easier and not have to go into the script and change a lot of nested directory paths.
5. `username_id.csv`: A csv file of every student's USC username and USC id. Make sure you have an extra line at the bottom of the file.

### Prerequisites

The script will rely on knowing what the directory path to each homework is. The assumed path for each homework is listed below:

```
hw-ttrojan/
	proj1/
		datalab-handout files
	proj2/
		jokerbomb files
		solution<USC id>.txt
	proj3/
		solution1.txt
		solution2.txt
		solution3.txt
		report.txt
		attacklab files
```

Sometimes a studnet doesn't read the instructions, or the instructions will differ from this assumed directory structure. In this case you'll need to go into each script to change the project path accordingly, or grade an outlying student manually.

### Running the Scripts

Running the scripts for ecah homework is the same. For the instructions below simply replace the `<x>` with the appropriate project number:

1. Import the student submissions into `grading_tools/submissions_proj<x>/`.
2. Run `python grade_project<x> username_id.csv`
3. Check that the script worked by checking the directory `grading_tools/project<x>_grades/`.
4. Manually grade any student whose repo did not follow the expected structure (the script will identify those students).

### Changing Scripting Path

In the case where the students setup the repo structure differently, (say, instead of `hw-ttrojan/proj1/<files*>` the students have `hw-ttrojan/proj1/datalab-handout/<files*>`), or any other changes, you'll want to open the script files and change the following in each grading script:

1. `PROJECT_PATH`: The name of the project directory in the root directory of the student's repo
2. `SUBMISSIONS_PATH`: Path to the root directory of all the homework submissions
3. In each scrip there is going to be a pipe shell command (`./foo > ../../../bar.txt). These pipe commands are run from the directory of where the homework executable is. You'll want to change the number of `../` and path back to `resources/grading_tools`.

If you're confused, I suggest running the grading script with only 1 or 2 students in the csv file so you can debug easier.