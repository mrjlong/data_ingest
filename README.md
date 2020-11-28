# de-ingest
---------------------------


This directory contains [PipelineWise](https://github.com/PotomacInnovation/pipelinewise) YAML config files
for creating data pipelines from various sources to our environment specific data lakes (`DATA_LAKE_{DEV|STG|PROD}`).

## Install pipelinewise

To run the taps configured in this repo, you'll need to have the pipelinewise cli installed. The recommended way to run the cli is with docker. Follow the instructions below to setup the pipelinewise cli system wide. After installing the cli, go to the `How to use` step in the documentation.

1. Clone the pipelinewise repo and build your docker image

```bash
git clone https://github.com/PotomacInnovation/pipelinewise.git
cd pipelinewise
make image
```

2. Add an alias in your `~/.bashrc` or `~/.zshrc` file

```bash
alias pipelinewise="$(pwd)/bin/pipelinewise-docker"
```

3. Confirm everything works

```bash
pipelinewise status

Tap ID    Tap Type      Target ID     Target Type      Enabled    Status    Last Sync    Last Sync Result
--------  ------------  ------------  ---------------  ---------  --------  -----------  ------------------
0 pipeline(s)
```


## How to use?

To enable YAML files rename the ones that you need. You will need to enable at least one `tap_....yml` and
one `target_...yml` file:

1. Enable the source (tap) and target files that you need by renaming at least one `tap_....yml` and one `target_...yml` file and removing the `.sample` postfixes. For example if you want to replicate data from MySQL to Snowflake:

```
  $ mv tap_mysql_mariadb.yml.sample tap_my_mysql_db_one.yml
  $ mv target_snowflake.yml.sample  target_snowflake.yml
```

2. Edit the the new files with your faviourite text editor

3. Import into pipelinewise
You need a `.env` file. An example env file is provide: `.env.sample`. Rename it to `.env` and fill in all required variables. After you'll be able to run the following make command: 
   
```
  $ make import
```

4. Check if the configuration imported successfully:
```
  $ pipelinewise status
  Warehouse ID    Source ID     Enabled    Type       Status    Last Sync    Last Sync Result
  --------------  ------------  ---------  ---------  --------  -----------  ------------------
  snowflake       mysql_sample  True       tap-mysql  ready                  unknown
  1 pipeline(s)
```
