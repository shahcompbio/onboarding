# onboarding



### HPC Account
Go to https://rts.mskcc.org/service to request a new account.  (This step requires VPN/on-site access)

Choose "HPC account change request"

In the following page:
- provide your Name and MSKCC email (personal emails are not allowed).
- set request type to "new account"
- set affiliation to "SKI"
- Choose "Sohrab Shah" under group name if you're in shah lab Or choose your current group. If you dont see your group, choose other and then choose the principal investigator the next drop down 
- Choose juno as the cluster
- Lab Admin: Cynthia Berry <berryc@mskcc.org> Office #: ZRC-578

See #onboarding channel description in slack for cost center and Fund numbers.

Provide your public key. see https://hpcdocs.mskcc.org/display/CLUS/Secure+Shell+SSH for details.

After you submit, you should get an email notifying you that the ticket is awaiting approval (you will receive multiple approval emails before being granted access). Please contact Dr. Andrew Mcpherson if the approval is not granted within 24 hours.

You can log into juno with

```
ssh juno.mskcc.org
```

To sign into the shahlab head node for the cluster:
```
ssh ceto.mskcc.org
```

Check out documentation at hpc.mskcc.org and HPC for more details. 

### On Demand

Send an email to HPC-request@cbio.mskcc.org, requesting access to ondemand and the ability to run development apps in ondemand.

## Github:


*External:* please send your github username to grewald@mskcc.org


*Internal:* MSK internal git repo https://github.mskcc.org/repositories, create a ticket for an account here https://solportal/EnterpriseDevTools


## Slack:

Go to myit.mskcc.org and fill out "Collaboration Account Request".  This link may work:
https://thespot.mskcc.org/esc?id=sc_cat_item&sys_id=6613a2cadbdf5c10636a0e85ca9619d8
Then wait until the request is approved and closed (check http://myit.mskcc.org for status updates). 

Go to https://mskcc.enterprise.slack.com/.  Search for sohrabshahlab and request to join.

wait for approval and sign in at http://sohrabshahlab.slack.com

Please contact grewald@mskcc.org if the request is not approved within one business day.

### Slack on phone
Please go to 
https://mskcc.sharepoint.com/sites/pub-Td/SitePages/ezMobileMainPage.aspx

and sign up for ezmobile. Once you're enrolled, you can install the slack app from the MSK App store.


## vpn access

You can request vpn access here
https://thespot.mskcc.org/esc/?id=sc_cat_item&sys_id=3a46665edb35a450636a0e85ca961927

If that link does not work,  Please go to https://thespot.mskcc.org and search `vpn`. Select `Request Global Protect VPN Remote Access`


## Isabl
Sign up over here https://isabl.shahlab.mskcc.org/signup.
Reach out to Eli Havasov (havasove@mskcc.org) because you will most likely need to be granted a number of permissions.

Helpful links:
* Isabl CLI - https://github.com/shahcompbio/isabl_cli
* Isabl ShahLab apps - https://github.com/shahcompbio/shahlab_apps
* Isabl documentation - https://docs.isabl.io/retrieve-data
* Isabl Utils, repo to make retrieving metadata & data easy - https://github.mskcc.org/shahcompbio/isabl_utils

Setting up Isabl CLI with apps:
```bash
git clone https://github.com/shahcompbio/isabl_cli.git
git clone https://github.com/shahcompbio/shahlab_apps

python3 -m venv ./venv
source venv/bin/activate

cd isabl_cli && pip install -r requirements.csv & python setup.py install && cd ..
cd shalab_apps && python setup.py install & cd ..
pip install pandas packaging  # temporary, needs to be addded to reqs

export ISABL_API_URL=https://isabl.shahlab.mskcc.org/api/v1/
export ISABL_CLIENT_ID=3  # production client, registers the apps

# login & see available commands
isabl login
isabl --help
```

## bsub
Please remember that all computationally intensive jobs should be submitted through `bsub`, which will submit your jobs into the compute cluster.<br>
You'll likely pet peeves for the HPC team if you submit large jobs in the login node (like juno or ceto), and if worse your job might eat up all the memory or CPUs of the cluster that the login node will be unaccessible for all other users.<br>
Some useful commands are: `bsub` for job submission, `bjobs` for reviewing your jobs, `bqueues` for checking queues, `bjobs -l $BSUB_JOB_ID` for detailed job info.<br>
You can see the `bsub` manual by using `man bsub`, but here are some example usages.

### 1. Wrapping your command with `bsub`
```bash
THREADS=12
MEMORYCAP=60  # max memory 60G
WALLTIME=24:00  # 24 hours
JOBNAME=test
STDOUT_FILE=/path/to/myjob.out
STDERR_FILE=/path/to/myjob.err
CMD="some command that DOES NOT have stdin or stdout brackets"
bsub -n $THREADS -M $MEMORYCAP -W $WALLTIME -J $JOBNAME -o $STDOUT_FILE -e $STDERR_FILE $CMD
```

### 2. Using a standalone script
You can create a bash script like the following and run by `bsub < script.sh`
```bash
#!/bin/bash
#BSUB -n 12
#BSUB -M 60
#BSUB -W 24:00
#BSUB -J test
#BSUB -o /path/to/myjob.out
#BSUB -e /path/to/myjob.err

module load singularity/3.6.2

singularity run -B /juno docker://some/image \
some complex commands \
that can have \
stdout and stderr in the command lines \
> like.this.out \
2> like.this.err
```
