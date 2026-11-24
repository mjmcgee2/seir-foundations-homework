

------

## Run 1 - 11.7.26
Instance ID: i-0136094c3d1d96dd1

### What went differently this time?
This was my first full repetition of the Balerica build, so I deliberately slowed down to understand what each step was actually doing instead of simply copying commands from the runbook. I gained a much better understanding of how the pieces fit together: EC2 provides the Linux server, Git retrieves the course code, the installer builds the Balerica Bank environment and COBOL core, and the Python virtual environment provides the dependencies needed to run the FastAPI layer.

### Error and how I fixed it
I made a couple of small mistakes during this run. I initially typed the time zone as america/new_york instead of America/New_York, which caused the date output to be incorrect, and I also typed account.dat instead of accounts.dat while working with the fixed-width ledger. Both were simple typing errors, but they reinforced how exact Linux commands, paths, filenames, and case can be.

### What I understand better now
The biggest thing I understood better was the relationship between the COBOL core and FastAPI. I tested the COBOL program directly first and received return code 0 for a successful account lookup. I then activated Balerica's Python virtual environment and started Uvicorn, which exposed the modern FastAPI layer that sits in front of the legacy banking functionality. I also understand that activating the venv is not entering another machine or container; it tells the current shell to use the Python environment and dependencies prepared specifically for Balerica.

### Exact output from this run
INFO:     Started server process [37720]

-------

## Run 2 - 11.7.26
Instance ID: i-0ea6817155485a8ad

### What went differently this time?
Run 2 went more quickly because I was becoming familiar with the overall build sequence and understood why I was performing each step. I also noticed that I was more comfortable with cloning the repository, running the installer, checking the COBOL core, activating the Python virtual environment, and starting Uvicorn than I had been during Run 1.

### Error and how I fixed it
The main issue I encountered was Linux case sensitivity. I cloned the repository as seir-foundations but initially tried to run the installer using an incorrect combination of api and SEIR-Foundations, which resulted in a "No such file or directory" error. After checking the actual directory names, I corrected the path to match the directory that existed and used the uppercase API directory.

### What I understand better now
This run reinforced that Linux paths and filenames are case-sensitive and that I should verify the actual directory structure rather than assume the capitalization. I also better understood that the overall build is a repeatable sequence rather than a collection of unrelated commands.

### Exact output from this run
INFO:     Started server process [36314]


-------
## Run 3 - 11.7.26
Instance ID: i-0c191255930efc929

### What went differently this time?
Run 3 felt more familiar because I was beginning to remember the overall sequence without having to think about every individual step. Instead of simply following commands, I was starting to recognize where I was in the build process and what should happen next.

### Error and how I fixed it
I typed the path to install.sh incorrectly, which produced a "No such file or directory" error. This time I compared the command I had typed with the correct command and identified the difference myself rather than simply copying the corrected command and moving on. I also accidentally typed the number 1 instead of a lowercase i in the ec2-metadata -i portion of the ID command. The resulting invalid-option error made the problem clear, and I corrected it.

### What I understand better now
I became more comfortable reading the error output and identifying what I had done wrong. The overall build process was also becoming clearer: provision the EC2 instance, install Git, clone the repository, install the bank, verify the COBOL core, activate the Python virtual environment, and start the FastAPI application with Uvicorn.

### Exact output from this run
INFO:     Started server process [36196]

-------
## Run 4 - 11.8.26
Instance ID: i-0dd31c733a9723bcb

### What went differently this time?
By Run 4, the application-building process itself felt much more familiar. I was spending less time trying to understand what each major stage was for and more time paying attention to command syntax, paths, and the evidence requirements.

### Error and how I fixed it
I again encountered a case-sensitivity problem with the cloned repository name. The clone created the local directory as SEIR-foundations, while I initially attempted to use SEIR-Foundations when running install.sh. I used the clone output and ls to identify the actual directory name and corrected the installer path. I also initially ran uptime without the -s option, which displayed how long the server had been running rather than the required boot timestamp. I stopped Uvicorn, reran the correct ID line with uptime -s, and restarted Uvicorn so the final screenshot contained the correct evidence.

### What I understand better now
I understand the evidence requirements much better now, especially the difference between ordinary uptime output and the boot timestamp returned by uptime -s. I am also more comfortable stopping, correcting a command, and rerunning a step rather than treating a mistake as a reason to rebuild the instance.

### Exact output from this run
INFO:     Started server process [37486]

-------

## Run 5
Instance ID: i-0f46162cb6e33fecc

### What went differently this time?
By Run 5, the overall build sequence was becoming much more familiar and I was moving through the EC2 build with less guidance. I was able to recognize where I was in the process and what needed to happen next without having to stop and rethink the overall architecture.

### Error and how I fixed it
I made two command-entry mistakes near the end of the run. I typed `ec2-metadata -1` instead of `ec2-metadata -i`, and I typed the Uvicorn host parameter as `--host --0.0.0.0` instead of `--host 0.0.0.0`. The incorrect Uvicorn command produced `ERROR: [Errno -2] Name or service not known`. I recognized both syntax mistakes, corrected the commands, and successfully started the application.

### What I understand better now
I understand the overall troubleshooting process better than I did during the earlier runs. A command failing does not necessarily mean the application or EC2 instance is broken. Reading the error, comparing the command with the expected syntax, and correcting the specific mistake was enough to continue without rebuilding anything.

### Exact output from this run
INFO:     Started server process [36315]

--------

## Run 6 - 11.8.26
Instance ID: i-006b7718e28e3e9b6

### What went differently this time?
For Run 6, I deliberately tried to complete the build primarily from my own notes and memory rather than following the runbook step by step. The overall sequence felt much more natural, and I was able to move from provisioning the instance through installing the bank, testing COBOL, activating the virtual environment, and starting FastAPI with much less guidance.

### Error and how I fixed it
I made another Linux case-sensitivity mistake when I entered `cd /opt/Balerica-bank` instead of `cd /opt/balerica-bank`. Linux returned `No such file or directory`. I recognized the capitalization error immediately and corrected the command without needing additional troubleshooting.  
Mike vs. Linux case sensitivity:  0-6 but I'm closing the gap...

### What I understand better now
The biggest difference in this run was that I could diagnose the error immediately because I now understand that Linux paths are case-sensitive. More broadly, the whole Balerica build is starting to feel like one repeatable process rather than a series of unrelated commands.

### Exact output from this run
INFO:     Started server process [35952]

-------

## Run 7 - 11.9.26
Instance ID: i-0cf32176edf85f0ae

### What went differently this time?
For Run 7, I completed almost the entire build from my notes and memory. Instead of waiting for path errors, I proactively used `ls -la` after cloning the repository to verify the exact directory name and capitalization before running the installer. I also used `pwd` before starting the bank API to confirm that I was in `/opt/balerica-bank`.

### Error and how I fixed it
I made one mistake while running the required ID line. I accidentally left out the `&&` between `ec2-metadata -i` and `uptime -s`, so the command did not return the required boot timestamp. I recognized that the evidence was incomplete, stopped Uvicorn, corrected the ID command, and restarted Uvicorn before taking the final screenshot.

### What I understand better now
By Run 7, I understand the Balerica build as one complete workflow rather than a list of commands to copy. I can provision the server, retrieve the application, build it, verify the COBOL core, prepare the Python environment, start FastAPI, and verify the evidence while understanding what each stage is accomplishing. I also learned to verify my state with commands such as `ls -la` and `pwd` before moving forward instead of assuming that my path or working directory is correct.

### Exact output from this run
INFO:     Started server process [36001]

-------

## COBOL Practice

I completed the return-code and fixed-width ledger exercises on Run 1, Instance ID `i-0136094c3d1d96dd1`. I completed the payment journal and post-payment balance verification on Run 7, Instance ID `i-0cf32176edf85f0ae`, because Run 1 had already been terminated before I realized I had not captured the required `payments.dat` journal output.

### Return Code 0 - Success

Command:

./cobol/bin/account_lookup 100001

Exact output:

ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD

I then ran:

echo $?

Return code:

0

Return code 0 means the COBOL program completed successfully. Account 100001 existed and the account information was returned correctly.


### Return Code 4 - Account Not Found

Command:

./cobol/bin/account_lookup 999999

Exact output:

ERROR=ACCOUNT NOT FOUND
ACCOUNT=999999

I then ran:

echo $?

Return code:

4

Return code 4 means the account was not found. The important thing I observed was that the COBOL program did not crash. It returned a defined error condition explaining what happened.


### Return Code 8 - Business Rejection

Command:

./cobol/bin/payment_post TXN-MM001 100001 0.00 USD TEST-ZERO

Exact output:

TXN=TXN-MM001
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO

I then ran:

echo $?

Return code:

8

I also tested an amount larger than the account balance:

./cobol/bin/payment_post TXN-MM-0003 100001 6000.00 USD TEST-FUNDS

Exact output:

TXN=TXN-MM-0003
ACCOUNT=100001
AMOUNT=6000.00
CURRENCY=USD
STATUS=REJECTED
REASON=INSUFFICIENT FUNDS

Return code:

8

This showed a second type of business-rule rejection. The transaction amount exceeded the available balance, so the COBOL program rejected the payment with REASON=INSUFFICIENT FUNDS while again returning code 8.


Return code 8 represents a business-rule rejection. The account and program were valid, but the transaction violated the application's business rules because the payment amount was zero. The COBOL program explained the rejection with `REASON=AMOUNT MUST BE GREATER THAN ZERO`.


### Return Code 12 - Environment Failure

For this test I first changed out of the Balerica Bank working directory with:

cd ~

I then ran the COBOL executable using its absolute path:

/opt/balerica-bank/cobol/bin/account_lookup 100001

Exact output:

ERROR=CANNOT OPEN data/accounts.dat
FILE-STATUS=35

I then ran:

echo $?

Return code:

12

Return code 12 represents an environment or file-access failure. The COBOL executable itself could run because I provided its absolute path, but the program could not open its expected relative file `data/accounts.dat` because I was no longer running from `/opt/balerica-bank`.


### Fixed-Width Ledger Decode

I decoded `data/accounts.dat` using the column positions provided in the runbook.

Account IDs:

100001
100002
100003

Customer names:

Alice Johnson
Robert Smith
Maria Garcia

Status:

ACTIVE
ACTIVE
ACTIVE

Raw balances:

000000500000
000000275000
000000810000

Currency:

USD
USD
USD

For Alice Johnson, account 100001, the raw 12-character balance is:

000000500000

The COBOL layout uses an implied decimal with the final two digits representing cents. Therefore:

000000500000 = 5000.00 = $5,000.00

There is no physical decimal point stored in the fixed-width file because the record definition tells the program where the decimal position belongs.


### Payment Journal and Balance Verification

I completed this verification on Run 7, Instance ID `i-0cf32176edf85f0ae`.

Before posting the payment, I looked up account 100001.

Exact output:

ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD

Return code:

0

I then posted a successful transaction using my own transaction ID:

./cobol/bin/payment_post TXN-MM-0002 100001 125.50 USD COBOL-PRACTICE

Exact output:

TXN=TXN-MM-0002
ACCOUNT=100001
AMOUNT=125.50
CURRENCY=USD
STATUS=POSTED
REASON=APPROVED

Return code:

0

I then ran:

cat data/payments.dat

The journal contained my transaction:

TXN-MM-0002         100001000000012550USDPOSTED  COBOL-PRACTICE

This shows that the $125.50 payment was recorded in the fixed-width payment journal as `000000012550`, again using an implied decimal.

Finally, I looked up account 100001 again.

Exact output:

ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD

The balance remained $5,000.00 even though the payment returned `STATUS=POSTED`, `REASON=APPROVED`, and appeared in `data/payments.dat`.

My best guess based on what I observed is that `payment_post` records the transaction in the payment journal but does not modify the balance in the account ledger that `account_lookup` reads. I verified this by comparing the account balance before and after the successful payment rather than assuming that `POSTED` meant the account balance had changed.


-------
## Mistakes I made

1. I repeatedly ran into Linux case-sensitivity issues with directory and file names. Examples included using SEIR-Foundations instead of SEIR-foundations, /opt/Balerica-bank instead of /opt/balerica-bank, and account.dat instead of accounts.dat. By the later runs, I started using ls, ls -la, and pwd to verify the actual path before running the next command.

2. I made several small command-line typing mistakes while collecting evidence. I typed ec2-metadata -1 instead of ec2-metadata -i, left out the && before uptime -s on Run 7, and initially ran uptime without -s on Run 4. These mistakes taught me to read the output carefully and confirm that the evidence actually contains the information I expect before taking the screenshot.

3. I mistyped the Uvicorn command during Run 5 as --host --0.0.0.0 instead of --host 0.0.0.0. Uvicorn returned an error and shut down. I compared my command against the correct syntax, fixed the extra --, and restarted the application successfully.

4. Early in the assignment I relied heavily on copying commands and checking each step before proceeding. As the runs progressed, I became more comfortable troubleshooting my own mistakes instead of assuming that a failed command meant something was wrong with EC2 or the Balerica application.

5. I learned that successful troubleshooting often starts by checking my current state before making another change. Commands such as pwd and ls -la became useful ways to verify where I was and what actually existed before proceeding.


## Commands I now know by heart

- sudo dnf install -y git
- git clone https://github.com/BalericaAI/SEIR-Foundations.git
- ls -la
- pwd
- cd /opt/balerica-bank
- ./cobol/bin/account_lookup 100001
- echo $?
- source venv/bin/activate
- whoami && TZ='America/New_York' date && ec2-metadata -i && uptime -s
- uvicorn app.main:app --host 0.0.0.0 --port 8000