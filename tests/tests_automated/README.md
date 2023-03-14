# README
#  Contents
  This directory contains the tests and the robot automation framework for
  automated testing of DRAW tool. 
  Directories are structured as below:

  - tests : This directory contains the Excel/JSON files used by robot  framework during testing
  - customLibraries : This directory conatins the custom libraries (python scripts) used by robot framework for testing.
  - resources: This directory contains the common resources (.robot files) frequently used by robot test suit for testing. 
  - robotTestSuit: This directory contains the robot files for testing. More files can be added to the testsuit as per the need and feature. 
  - output: This directory contains the report and log files generated by robot tool. By default this directory is empty.
  - downloads: This directory will be created during automated testing and used during testing as a download directory. By default this directory is empty.
  - testlogging: This directory will be created during automated testing and used during testing for log files generation. By default this directory is empty. After running of the tests the logs of generated files can be referred from this directory.

  Before running/rerunning the robot tests the following directories should be clean:
  - downloads:
  - testlogging:

# Creating a docker image for regressing/running the tests from a user branch:

   - Step 0: Ensure all changes are checked into the user branch and are pushed into git repository.   
   - Step 1: Copy the Dockerfile, requirements_robot.txt and run_tests.sh from current directory to a new directory.
   - Step 2: Change the name of branch in Dockerfile to the user branch created for development in line no 21.
      RUN git switch draw_v2.0.5
   - Step 3: From the given directory run the following command.

      `$ docker build -t <run_draw_tests> `.

    This step will create an image using Dockerfile with the following installed:
     ubuntu 20.04
     python3.9
     unzip
     wget
     robot
     rcc 
     This step will also clone the contents of user branch to the draw directory within the docker container.

   - Step 4: You may want to verify the container contains expected dependencies and paths. Follow this step for the same or continue to Step 5.

      `$ docker run -it <image_id> /bin/bash`

      `$ which python3.9`

      `$ which wget`

      `$ which robot `

      `$ which rcc`

      All these steps should return path of appropriate executables 

      `$ cd draw/test_automation`      
         Should be a valid path

      `$ exit`
        Exit the container

  - Step 5: Run the tests:

   Before next step run http-server from a shell from user branch root directory. ie draw directory where index.html is present.
   In the following command replace URL by the URL returned by the http-server command and image id
   by the image id of the image created.

   `$ docker run -it -e  URL=http://172.27.231.130:8080 -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix 248541d6b95b sh -c "./run_tests.sh"`

   NOTE: If DISPLAY is not specified some tests may fail which are dependent on Trick Service.

   ## Remove the docker container(s) and image after testing:

    List the docker images using the following command:
    `$ docker images `

    `REPOSITORY                TAG       IMAGE ID       CREATED             SIZE`
    `run_draw_tests            latest    78401b28ca97   About an hour ago   1.35GB`
    `ubuntu                    20.04     d5447fc01ae6   3 weeks ago         72.8MB`
`
    Remove both the images 78401b28ca97 and d5447fc01ae6

      `$ docker rmi -f 78401b28ca97`
      `$ docker rmi -f d5447fc01ae6`

   ## Failing tests:
    If any tests fail look at the output/ directory report.html or log.html to root cause the problem.
     
# Standard Robot Framework

The files in directory robotTestSuit are derived from
- [Robot Framework](https://robocorp.com/docs/languages-and-frameworks/robot-framework/basics) syntax.
- Robot can be configured by `robot.yaml`.
- Dependencies can be configued in `conda.yaml`.

## Learning material for robot framework

- [Robocorp Developer Training Courses](https://robocorp.com/docs/courses)
- [Documentation links on Robot Framework](https://robocorp.com/docs/languages-and-frameworks/robot-framework)
- [Example bots in Robocorp Portal](https://robocorp.com/portal)

## Commands for running robot from CLI:

### Parallel testing and CLI options

####  pabot tool
  Use pabot tool for parallel testing of robot:
  Plugin for parallel testing:

  `$ pip3 install robotframework-pabot`

  Parallel running tests:

  `$ pabot [path to tests]`

  Run testcases within a file in parallel mode:

  `$pabot --testlevelsplit [path to split]`

#### CLI options

  Run only a single test:

  `$ robot -t "Testcase name" testFile.robot`

  Runs all robot files :
  `$ robot . `

  Regular expressions also work with paths: 

  `$robot -t "Validate*" `

  Tag testcases as below:

  Example in RObot file:
  
  Add tag in RObot file:

  Validate Test1
	   [Tags]	SMOKE

  Now Run tags as below:

  `$ robot --include <tagname> .`

  More examples:

  `$ robot --include <tagname>  AND <tagname> . `

  `$robot --exclude <tagname> .`

  `$robot --suit <foldermame>`

  `$robot --rerunfailed output.xml .`
