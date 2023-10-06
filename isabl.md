# Isabl

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


### Isabl cli Quick Start

export this in your environment to use isabl utils
```
export ISABL_API_URL=https://isabl.shahlab.mskcc.org/api/v1/
```

To get all results for mondrian alignment pipeline from spectrum project:
```
import isabl_cli as ii
analyses = ic.get_analyses(
    application__name__in=['MONDRIAN-ALIGNMENT'],
    targets__projects__pk=46,
    status='SUCCEEDED',
    limit=300,
)
```

To get mondrian alignment bam file for a specific sample
```
import isabl_cli as ii
analyses = ic.get_analyses(
    application__name__in=['MONDRIAN-ALIGNMENT'],
    targets__sample__identifier=sample,
    status='SUCCEEDED',
    limit=300,
)
assert len(analyses) == 1
bamfile = analyses[0].results.bam
```
