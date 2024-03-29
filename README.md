## 🧪 Flaky Tests Validty Threats (Assignment 3)

# A) Baseline
The following parts are copied-over from assignment 2:


"
This is a reproduction efforts to re-create the original paper: "What is the Vocabulary of Flaky Tests?" as part of the 
study program "Mining Software Repositories (MSR 2020-21)" provided by SoftLang Team at University of Koblenz-Landau. 
" - Amir Dashti

## MetaData:
This repository describes the recreation of 2020 Paper "What is the Vocabulary of Flaky Tests?" written by: Gustavo Pinto and
               Breno Miranda and
               Supun Dissanayake and
               Marcelo d'Amorim and
               Christoph Treude and
               Antonia Bertolino.


Here, we try to re-create what the original work has achieved to re-confirm and evaluate the challenges involved in
making or verification of an MSR paper. The flaky tests are tests that pass and fail periodically without any code changes. 
There might be different underlying reasons causing such tests to occur. This paper continues investigations of 2 existing papers.
They used the base dataset of manually tracked flaky tests and automated the suggested framework to find vocabulary (simply tokens and keywords) that relate to flakiness using NLP. 


Links to Original Work:

DBLP: https://dblp.org/rec/conf/msr/0001MDdTB20.html?view=bibtex

Conference Link: https://2020.msrconf.org/details/msr-2020-papers/43/What-is-the-Vocabulary-of-Flaky-Tests-

GitHub Link: https://github.com/damorimRG/msr4flakiness

-----------------
Research Questions (*** adopted from the paper):

RQ1. How frequently do flaky tests happen and how prevalent they are?

RQ2. What is the precision of predicting test flakiness using tokens collected from the source code?

RQ3. What is the importance of different features within the model?

RQ4. Find the tokens that are highly connected to flakiness of a tests.

## Requirements:

Minimum Hardware:
- MacOs Catalina or Linux Distributions
  
- Intel Core-i7 x ,2.2 GHZ 6 Cores  

- RAM 8 GB (I recommend at least 16 GB)
  
Software:
  
- Vanilla ZSH bash (or equivalent)

- Python 3.8 or above (use Conda)

- Docker (already running) * ⛴ Docker is only needed if you use the heavy mode

## Process:
The general re-creation work done by us goes through the following steps:

![Alt text](https://github.com/gocontractdev/flaky-tests-reproduction/blob/main/doc/diagrams/general-procedure.png?raw=true "General Procedure")

0- Re-organize: the original repository is very poorly organized the data and codes are mixed we will automatically clone and reorganize it in right places.

1- Detection: Depending on your decision can prepare flaky / non-flaky samples:

> e - EASY mode will use already processed initial data.
> 
> h - HEAVY mode will fully re-calculate flaky and non-flaky samples.

2- Identification (tokenization): Uses NLP approaches to find tokens and candidate features.

3- Discovery: Finalizes the classifier model and produces the outputs.

4- Comparison: Open-ended process to check and verify the output.


If you have a compatible extended shell simply running the 'initialization' file will execute 
everything necessary for this project. From there follow the steps console asks (it automatically sets the right environment and installs necessary packages):


```shell
# to initialize and run everything
zsh ./process/initialization.sh

# it will prompt and asks question if needed

# Please choose the mode? [Easy Mode (e or ez) | Heavy Mode (h or hv)]

> h # if you want full process
> e # if you want demo process

# the process might take few minutes
```

👉 Please refer to
[Process iPython](https://github.com/gocontractdev/flaky-tests-reproduction/blob/main/process/process.ipynb) file to see the full explanations about the process.


- Important 1: The password is your machine's password (needed for Git Clone).

- Important 2: Refer to the extra section if something did not work.

- Important 3: I have made a code to make env on the fly but it fails sometimes; easisest way is just to use automatic tool
if you have PyCharm or InteliJ products:
  
![Alt text](https://github.com/gocontractdev/flaky-tests-reproduction/blob/main/doc/diagrams/env.png?raw=true "  ")

 ✅  How to Validate the works?
--------------------
If everything works properly, the output produced by the code 'features_frequency.csv' will include the
top most related vocabulary to flakiness of the tests. You can quickly verify this by running coe in the easy (e) mode.

Refering to the paper: "we noted that
job, action, and services are commonly associated with flaky tests.".
So, the output here should be also similar.



## Data:
Following is the general description of data types along with the schema overview:

---
- Basics:

> Positive === Flaky Test
> 
> Negative === Estimated Flaky Test (a test that is not proven to be flaky so far)

- Initial Dataset:

#### The main dataset that has been used for this work is called "DeFlaker".


> The original paper faces the following issues using the dataset:
>
> 1- The dataset does not list the non-flaky tests but since it was needed for their model training; they ran the tests multiple times to find possible candidates.
>
> 2- One of the projects fails in build so they use 24 out of 25 Java projects provided by the dataset.
>
>
>For further on DeFlaker: http://www.deflaker.org/icsecomp/
> 
>  <sup>Note: This part is adopted (in simple words) from the paper. </sup>

The structure of our data: (To see sample parts of data refer to: [Delta iPython](https://github.com/gocontractdev/flaky-tests-reproduction/blob/main/process/delta.ipynb))

A) Input:

- historical_projects_copied.csv: A csv file used as an input in original paper. It includes list of github projects that have been monitored.

- histoircal_rerun_flaky_tests.csv: A csv file used as an initial input that includes detailed list of test files and test methods that are known as being flaky.

- list-flaky.csv: A csv file very similar to histoircal_rerun_flaky_tests.csv but it also includes SHA and version of flakiness since they might have change or fixed.

- ez/ : This directory includes a copy of the mentioned files for representation purposes. It is not used in real processes.

B) Temp:

- test_files/ : Includes multiple text files each file including the full text from a specific test file.
  File name represents the test file name and its category.

- test_cases/ : Running methods jar file will separate test files to test cases. This directory includes multiple text files; 
 each file includes text of a specific test known to be flaky.

- sample_flaky/ : Includes multiple text files that are separated line by line; each line has a token grabbed and processed from a respective
test case file.

C) Output:

- features_raw.csv: Includes raw list of all tokens with their frequencies. It is not yet decided how effective they are in flakiness.

- features_frequency.csv: It includes top filtered tokens that associate with flaky tests and make the vocabulary of flaky tests.

- total_process_original.txt: This file includes the whole code strings of original repository. -- Used to calculated Levenshtein distance.
  
- total_process_rework.txt: This file includes the whole code strings of re-production repository. -- Used to calculated Levenshtein distance.

- total_data_original.txt: This file includes the whole data string of rhe original repository. -- Used to calculated Levenshtein distance.
  
- total_data_rework.txt: This file includes the whole data string of rhe re-production repository. -- Used to calculated Levenshtein distance.

- ez/ : This directory includes a copy of the mentioned files for representation purposes. It is not used in real processes.


## 🔥 Delta:

Please refer to my Jupyter
[Delta iPython](https://nbviewer.jupyter.org/github/gocontractdev/flaky-tests-reproduction/blob/main/process/delta.ipynb) file to see the explanations about the project's delta. Here is the summary:
#### A) about Process:
The methodology used in original and recreation work are same however due to issues in the code or un-explained manaul parts; we had to employ remedees or alternatives in the implementation and exection of the proccess.
The alternatives we had to adopt include:

1- Re-organization: The data and code are poorly mixed in the original work. The first part of the 'actuall.py' focuses on moving file to their correct directory.

2- Fixing python codes that do not run: There are parts of the code that need minor changes to work. I have used the original work and deep-linked them using __importy__() method.
In cases that it was not possible, I have made a custom method that includes the fixed version of original code.

3- Unexplained gradle build and java runtimes: Parts that use the java jdk and gradle are un-explained in the original work. It seems like the authors have ran them manually.
Since running manually is difficult for me. I have made a DockerFile that starts and Alpine injected with Java and Gradle to handle the builds and making output results. This change means you need both Python environment and Docker to run this project.


### 📊 Mathematical Distances: 

Hamming Distance [noamlized]: 1.6694457422028464e-06
Jacard dis-similarity 4.42110833769081e-05
Sorensen-Dice:  8.841825768685356e-05

#### B) about Data:
Flow of data and what is being generated are almost identical to each other since same approaches have been adopted.
We can divide the data into 3 sections:

1- Input: Input data is completely identical; we use exact same inputs.

2- Temp: Temporary data created by us resembles the original work. We only missed the re-runs folder since developing a docker
that runs projects multiple times took too much effort and was out of scope of me doing it alone.

3- Output: Uses similar format, naming and outputs. We have extra output that have been used for calculation of distance between original work and recreation.


###  📊 Mathematical Distances:

Hamming Distance [noamlized]: 0.055623301256152224
Jacard dis-similarity 0.12280896383403594
Sorensen-Dice:  0.2187530876395613

## Extra:
#### A) What should I do if installing pyCurl failed?

```shell
# if pycurl failed for you on MAC -- couldn't find any other solution 
xcode-select --install

# if necessart export the path of anaconda
export PATH="/usr/local/anaconda3/bin:$PATH"

# quick login to alpein (it is only used for builds and running java) 
docker container exec -it my_little_alpine bin/bash

# You can activate the environment if you want but the initialization does that Automatically
source ez_env/bin/activate
# to exit env simply type
deactivate
```

#### B) Known Issues:

1- The environment which comes along with the project is broken.
It fails in installation of 'pyCurl' depending on you O.S. and your SSL settings it may happen or not happen.

2- Some python files do not actually work. I have modified them to achieve the goals; further on this is explained in 
[Process iPython](https://github.com/gocontractdev/flaky-tests-reproduction/blob/main/process/process.ipynb).

3- For me, without anaconda path it does not work; Make it somehow that zsh and anaconda work fine:

![Alt text](https://github.com/gocontractdev/flaky-tests-reproduction/blob/main/doc/diagrams/anaconda.png?raw=true "  ")

4- If you stop process after making folder and before cloning it just skips it becasue it only checks the folder; generally do not stop script it fails completely.

![Alt text](https://github.com/gocontractdev/flaky-tests-reproduction/blob/main/doc/diagrams/whenempty-cloning-ignored.png?raw=true "   ")


---------------------

# B) Experiment
In this assignment, we would investigate one of the threats to internal validity found in "What is the Vocabulary of Flaky Tests?“ paper.
The threat and the rationale behind it are as follows:

## Threat:


![threat](https://github.com/gocontractdev/flaky-test-threat/blob/main/doc/threatcall.png?raw=true "What is the threat")


This paper discusses a semi-automated procedure to generate a model for detecting flaky tests.
The model, relies on the tokens (or simply keywords) extracted and learned from the flaky and non-flaky test cases.
Although the flaky tests have been confirmed both by human readers and through running them multiple times, not enough confidence for tests labeled as non-flaky is established.
In other words, the paper only relies on estimations for the tests labeled as non-flaky without proving nor justifying the choice of 100 re-runs to assure a single test is non-flaky.
 
Hence, a massive threat to internal validity of this work exists in the initial labelling section.
If one or more tests that attribute to flaky behaviour are labeled incorrectly, the final model may struggle in detecting true patterns and tokens attributing to each class.

## Traces:
Finding the traces for the threats to validity of the present paper is rather easy, since they are all thoroughly mentioned in the section „6.1 Threats to Validity“. The authors also mentioned it back in the section  „4 Objective of Analysis“:
 
„Indeed, the diagnosis of non-flakiness is an estimate—there is no guarantee a test is non-flaky with a given number of runs“ *1 (4 Objective of Analysis)
 
This sentence openly states that we do not have a guarantee for non-flakiness and this is in-fact a probability.
 
„When performing our first experiment (running the test cases of 24 Java projects 100 times to find flaky tests), we noticed that 55% of the test cases passed 99 times, and failed just once. This result suggests that the strategy of rerunning tests several times to detect flakiness could miss cases of flakiness as tests could have been insufficiently executed“ *1 (6.1 Threats to Validity)
 
The statistics mentioned over here is rather alarming. Claiming that running tests 99 times could result in missing almost half of their flaky tests suggests that maybe the chosen threshold is far from optimal.

## Theory:
As mentioned above, it seems like the initial non-flaky test cases used in training the model might be labeled incorrectly. I have quoted parts of the paper that confirm this threat potentially exists. My hypothesis is : „Executing test cases more than 100 times can result in finding previously undetected flaky tests“. I will try to falsify this hypothesis. To do so, I will run a set of tests exhaustively to find a new more optimal threshold for reruns. Then show more executions do not result in a significant deviations from our confidence interval (or the hypothesis can not be falsified).


Our hypothesis based on information we have about the paper is as follows:

H0: Executing tests more than 100 times will not result in finding significantly more flaky tests.

Ha: Alternative Hypothesis claims 100 is not close to ideal and there is possible to find an optimal re-run threshold that results in finding signfincantly more flaky tests.

> For the complete formation of hypothesis you can refer to [comparison jupyter file](./process/comparison.ipynb).

## Feasibility:
It is obviously a heavy task to run unit tests for many times. As a result, I will first try to statistically calculate a new threshold as maximum execution efforts based on the present data we already have access to. Next, I run tests until noticing the first occurrence of a switch in output (true test resulting false or vice versa) or reaching the max threshold for small chosen set of test cases. Lastly, I will compare the original distribution and the new distribution of flaky/ non-flaky tests and find out whether the difference is significant enough.
 
In small scale, this task seems to be feasible, I will try to justify the statistical calculations and avoid introduction of new biases in selection of samples. It is worth mentioning that I might fail in proving the new threshold performs better but still show the threat exists. As for our course, it would be enoguh to only show the re-execution threshold chosen on this paper is not optimal and as a result the trained model of this MSR paper is incomplete and there is a room for study (futher investigations are more time consuming).

## ⚙️ Implementation:
Since in this project I am mainly using Shell and Python scrips the implementation of the exact hypothesis into code was not very challenging however I had to do some extra steps to have all the files necessary for the test:

0- Modify the assignment 2 to work properly here:

actuall.py file from previous assignment needed an extra state ['SIMULATE'] in which it could process and save files in a format needed for this assignment. Instead of multiple text log files for each run time we required a csv to simply track the frequency and the flakiness of each test. 
We have achived this by adding a run-time argument and a state check on actuall.py:

```shell
# run through OS shell
os.system('python ' + process_path + '/actuall.py ' + 'SIMULATE')

# in actuall.py 
if len(sys.argv) >= 2:
   simulate_mode()
```

simulate_mode is rather an I/O heavy process since it has to open multiple project. For each proeject it will find all logs of each 100 re-execuations and for each re-execution reads line-by-line each test execution and tracks comulate all results in all_reruns.csv.
So, it has time complexity of 03.


1- Generation and Placement:

generator.py is the new file that takes care of placing files or codes in the right directory and generating raw samples to be tested; it collects 1% of the total population randomly. 

2- Actual Testing:

For actual testing and comparison, I have used the well-known python libraries. This file, reads the csv file; calculates the mean and sigma and handles the underlying science needed to compare samples.

> For the complete explanation you can refer to [comparison jupyter file](./process/comparison.ipynb).

## 📈 Results:
The main idea of us was to first prove the chosen number of 100 for number of re-executions is sub-optimal. We had done it through
statitical calculations. We have calculated the new threshold of (mu + 3 x Sigma) as it is very unlikely to see any value after this distance from mean of a population.
The new threshold is calculated to as: 163.

Next, I tried to nullify our hypothesis by showing that our samples are significantly far from original distribution.
We have used 561 (1 % of populations) samples for our hypothesis. The sample mean and standard deviation are as follows:

sample mean: 110.44

st: 42.3

![Alt text](https://github.com/gocontractdev/flaky-test-threat/blob/main/doc/dist.png?raw=true "  ")
- The diagram compares the population distribution [blue] with the sample distribution [orange]

With our assumption of having the normal gaussian distribution these numbers result in z-score of 0.287 which is too small to reject the null hypothesis.
This means we have only 61% confidence that our the difference is significant. This shows a room for further sampling or investigations.
Random reshuffling, multiple sampling or using a better distribution (than normal) would improve the invstigations however it would take too much effort.
With the existing calculations we can not nullify the hypothesis without more advanced investigations.



> For the accessing all charts and summary you can refer to [comparison jupyter file](./process/comparison.ipynb).

## Requirements:
The requirements for this task do not surpass the assignment 2. In fact most heavy processes take place in assginment 2.

Minimum Hardware:
- MacOs Catalina or LTS Linux Distributions
  
- Intel Core-i7 x ,2.2 GHZ 6 Cores  

- RAM 16 GB
  
Software:
  
- Vanilla ZSH bash (or equivalent)

- Python 3.8 or above (use Conda, it should have virtualization)


⚠️ For running samples only I would recommend running tests on a server with twice capacity
(For running base shell here you DO NOT need this).

## Process:
The general steps for the process of verifying and addressing the threat is as follows:

0- Boostrapping: The shell file will automatically install and clone required repositories and places them on the right spots. 
Afterward it runs the generator.py to grab the files necessary from the previous assignment and to run actuall.py.

1- Simulate: actuall.py is actually the main python script from the previous task. I have modified it here to accept a runtime argument: "SIMULATE". 
When running actuall.py on simualte mode it will only process the reruns folder to make all_reruns.csv. This file basically includes 
all the test cases used in the work (± 568000). For each test case the information about its location and the whether falky behavior has been observed is stored.
If flaky behavior is observed the column 'frequency' shows number of re-execution when the test first showed flipping behavior and was confirmed to be flaky.

2- Sample: In this step we collect a random sample with 1% size of original population from the all_reruns. Test files selected will be stored temporary in sample_runs.csv. These tests have to be executed multiple times.

3- Comapre: This last step is done in [comparison jupyter](./process/comparison.ipynb). We compare the population and our sampling distribution to see if the difference is really significant enough to nullify the base hypothesis.


To execute the process run the following shell command. It automatically initiates and runs all necessary codes.
You need to have the ez_env python virtual env just like the previus assigment. If you use modern IDEs (like InteliJ's PyCharm) it automatically prompts you to make this environment with the correct name if not just make it by hand.

```shell
# to bootstrap and run everything
zsh ./process/bootstrap.sh

> it will prompt and asks question when needed

# if some requirements are failing on MAC it is really just because of Xcode
# I think after any BigSur update you have to install it
xcode-select --install

``` 

- Important: Please refer to assignment 2 for debuging or help.

## Data:
Our data structure is rather easy this time. We have the same folder structure as before:

- /input:
    - /original_repo: This is the original repository from the authors of the paper.
    - /all_reruns: There are multiple log files in the original repo that show the result of 100 times re-executing every test case.
      This file gathers all tests and calculates the frequencies till observing frequency behavior. The structure of it is as follows:
      
    > Example: all_reruns.csv ± 568000 tests 
    >
    > dir,test,first_result,frequency,flaky,
     ch.qos.logback.core.ContextBaseTest,contextThreadpoolIsDaemonized,pass,100,False,
     org.springframework.boot.web.embedded.jetty.JettyServletWebServerFactoryTests,noCompressionForSmallResponse,pass,100,False,
     alluxio.client.cli.fs.command.CopyFromLocalCommandIntegrationTest,copyFromLocalDir,pass,100,False,
     org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfigurationTests,emptyTemplateLocation{CapturedOutput},pass,100,False,
     org.apache.hadoop.log.TestLogThrottlingHelper,testNamedLoggersWithoutSpecifiedPrimary,pass,100,False,
    ...
  
    1- dir: Location of the test file.
    
    2- test: name of the test file.
    
    3- first_result: result of the first run; it is either PASS or ERROR. 
    This is used to compare with any following trails to see if the flaky behavior is observed.
    
    4- frequency: It is a boolean. When a test shows a flipping behavior and change from PASS to ERROR or vice-versa this boolean is set to TRUE.
    
    - /list-flaky.csv: Not used in this assignment.
    
    - /historical_projects.csv: Not used in this assignment.
    
    - /historical_rerun_flaky_tests.csv: Not used in this assignment.

    - /ez_mode:
    - ℹ️ sample_runs.csv: This file will be generated in temp however it has to be ran manually. These are the samples I tested from the temp output.  
    - ℹ️ all_reruns.csv: Same as calculated file exaplined above. This is a backup file also is used for fallback in juptyer.

- /temp:
  - /assignment2: This repository is cloned during the run time and it includes the whole assignment 2 project.

  - /reruns: This is a folder from original work that is cloned and moved here for further processes. 
    It includes multiple directories each representing one of the tested projects. Each of them have multiple log files with similar format.
    The 100 log files show the outcome of 100 re-execution for each test case in a line-by-line standard:
    
    > Example: ./some-project/runX.log 
    > 
    > alluxio.client.cli.fs.GetConfTest, getConfWithCorrectUnit, pass
     alluxio.client.cli.fs.GetConfTest, getConfFromMasterWithSource, pass
     alluxio.client.cli.fs.GetConfTest, getConfByAlias, pass
     alluxio.client.cli.fs.GetConfTest, getConf, pass
     alluxio.client.cli.fs.GetConfTest, getConfWithWrongUnit, pass
     alluxio.client.cli.fs.GetConfTest, getConfWithInvalidConf, pass
     alluxio.client.cli.fs.GetConfTest, getConfFromMaster, pass
     alluxio.client.cli.fs.JobServiceFaultToleranceShellTest, distributedMv, error
     ...
    
    Each line represents one test case location and name and the result of the execution.
    
  - sample_runs.csv: This is a comma separated csv that is generated on the run time by generator.py.
    This file includes RANDOMLY chosen test case candidates from all tests (all_reruns) to ran with the new max-run threshold.
    The format of it is as follows:
    
    > Example: sample_runs.csv 
    >
    > dir,test,first_result,frequency,flaky,sample_run 
     ch.qos.logback.core.ContextBaseTest,contextThreadpoolIsDaemonized,pass,100,False,
     org.springframework.boot.web.embedded.jetty.JettyServletWebServerFactoryTests,noCompressionForSmallResponse,pass,100,False,
     alluxio.client.cli.fs.command.CopyFromLocalCommandIntegrationTest,copyFromLocalDir,pass,100,False,
     org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfigurationTests,emptyTemplateLocation{CapturedOutput},pass,100,False,
     org.apache.hadoop.log.TestLogThrottlingHelper,testNamedLoggersWithoutSpecifiedPrimary,pass,100,False,
    ...
    
    1- dir: Location of the test file.
    
    2- test: name of the test file.
    
    3- first_result: result of the first run; it is either PASS or ERROR. 
    This is used to compare with any following trails to see if the flaky behavior is observed.
    
    4- frequency: It is a boolean. When a test shows a flipping behavior and change from PASS to ERROR or vice-versa this boolean is set to TRUE.
    
    5- smaple_runs: This is the number of runs till observing flaky behavior. If value is equal to the threshold; it is not flaky.
    

- /output:

There is no special output in for this work. All charts and calculations are stored in comparison jupyter file.


extra files:
- Files named 'keep' are just extra files used so version control keep the folder in track.

- doc folder: doc folder includes some images and graphics used in the comparison jupyter or readme file.

----

🚑 On emergency case if the comparison jupyter did not load on github use the mirror.

https://nbviewer.jupyter.org/github/gocontractdev/flaky-test-threat/blob/main/process/comparison.ipynb

If both did not work contact me. Best,


“Simplicity is the ultimate sophistication”. Leonardo da Vinci