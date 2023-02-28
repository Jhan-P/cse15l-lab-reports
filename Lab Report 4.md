# Lab Report 4 - Reproducing the In-Class Competition Task

__Disclaimer:__ Before beginning this task, I already created public SSH keys for GitHub and forked the repository onto my account.

## Step 1 - Logging into ieng6

I typed in ``ssh cs15lwi23ahf@ieng6.ucsd.edu`` into command prompt to access my ieng6 account remotely.

![Image](1.jpg)

We are now accessing the remote server.

## Step 2 - Cloning the repository fork into my account

On the GitHub page containing my fork, I pressed the green "Code" dropdown and clicked on "SSH".

![Image](2.jpg)

I copied the given URL and ran the following command (with the actual URL replacing the "<URL>" from the screenshot):

![Image](3.jpg)

![Image](4.jpg)

`lab7` is now located in our remote directory.

## Step 3 - Running and confirming the failure of the JUnit tests

After going into the `lab7` directory, I entered the following commands:

Compiling the java files: `javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java`
Executing the JUnit file: `java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests`

Here are the input and output of those commands:

![Image](5.jpg)

The output at the bottom shows that there is 1 failure as expected.
