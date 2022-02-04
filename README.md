# onboarding


## Atlassian
Please email

Diljot Grewal <grewald@mskcc.org> or

Andrew Mcpherson <mcphera1@mskcc.org>

to request access. Please include email address that you'd like to use for the Atlassian account. you should receive an invitation to join in the email. you can access confluence at shahcompbio.atlassian.net/wiki. Access to Jira (shahcompbio.atlassian.net) is granted on a case by case basis.

## One MSK Account:
Detailed instructions for One MSK account  can be found here: one MSK account


### HPC Account
Go to http://actg.mskcc.org/contact-new-user-form/ to request a new account.  (This step requires VPN/on-site access)

Choose the Juno Cluster

provide your Name and MSKCC email (personal emails are not allowed).

Select Dr. Sohrab Shah as your PI

Lab Admin: Cynthia Berry <berryc@mskcc.org> Office #: ZRC-578

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

## Github:
*External:* please send your github username to grewald@mskcc.org or dalai@bccrc.ca
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
