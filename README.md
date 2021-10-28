# DBT-Redshift-project
Create your first DBT project for aws redshift 🎉🎉

#  Creating DBT Project for AWS redshift
### _Run SQL query on AWS Redshift using DBT(data build tool) CLI_ 

[dbt (data build tool)][dbt] enables analytics engineers to transform data in their warehouses by simply writing select statements. dbt handles turning these select statements into tables and views.
dbt does the T in ELT (Extract, Load, Transform) processes – it doesn’t extract or load data, but it’s extremely good at transforming data that’s already loaded into your warehouse.
[learn more about dbt][dbt]
# Let's start
First we need to install the DBT in our system.
### Installation (DBT)
We recommend you use install dbt using one of three tried and tested methods:
 - `homebrew (recommended for MacOS)`
- `pip`
- `from source`

Open your favorite Terminal and run these commands.
##### Homebrew:
if you not have Homebrew, install from using this [link](https://brew.sh/). After that run this commands for installing DBT
```sh
brew update
brew install git
brew tap dbt-labs/dbt
brew install dbt
```
Test your installation with `dbt --version` 
##### pip:
dbt is a Python module distributed on pypi, and can be installed via pip. First, we recommend python virtual environments to namespace pip modules. Here's an example setup:

```sh
python3 -m venv dbt-env             # create the environment
source dbt-env/bin/activate         # activate the environment
```
If you install dbt in a virtual environment, that same virtual environment must be re-activated each time a shell window or session is created.
```sh
pip install dbt
```
Test your installation with `dbt --version` 

### Install from source
Create a dbt virtual environment as detailed in the pip section above.
Then, install dbt from GitHub source
```sh
git clone https://github.com/dbt-labs/dbt.git
cd dbt
pip install -r requirements.txt
```
i installed dbt via pip .After successful installation of dbt , we need to  create dbt project.
## create a dbt project
`dbt init <project_name>`
```sh
dbt init DBT_Redshift_Demo       # DBT_Redshift_Demo is project name
```
It will create DBT_Redshift_Demo folder and in this folder you find all dbt related files like _modals, macros, logs etc_.
you can find some sample sql file in modals folder.
### How to connect to your warehouse when using the CLI
In our case the warehouse is redshift. For setup redshift connection we need to configure profile.yml
When you invoke dbt from the CLI, dbt parses your dbt_project.yml for the name of the profile to use to connect to your data warehouse.
you can find the profile.yml file on default path `C:\Users\username\.dbt`.
Configure your profile.yml for redshift:
```sh
default:
  target: dev
  outputs:
    dev:
      type: redshift
      host: redshift-hostname
      user: username
      password: Password
      port: 5439
      dbname: dev
      schema: schemaname
      threads: 4
      keepalives_idle: 0 # default 0, indicating the system default
      connect_timeout: 10 # default 10 seconds
      ra3: true # enables cross-database sources
```
A typical profile for an analyst using dbt locally will have a target named dev, and have this set as the default. You may also have a prod target within your profile, which creates the objects in your production schema.
Now all is set,lets create .sql files.

## Write SQL files
For example lets create new table `employee`  in redshift. you can perform any sql query on redshift using .sql file.
```javascript
{{ config(materialized='table') }}

/*
Inserting data into employee
*/
with employee as (
    (select 1 as emp_id, 'Virat' as emp_name, 36 as emp_age, 1 as department_id)
    union
    (select 2 as emp_id, 'Sachin' as emp_name, 46 as emp_age, 2 as department_id)
    union
    (select 3 as emp_id, 'Vikram' as emp_name, 23 as emp_age, 1 as department_id)
    union
    (select 4 as emp_id, 'Shivam' as emp_name, 25 as emp_age, 1 as department_id)
)
/*
Creating table employee, loading data into Redshift
*/
select * from employee
```

##### Now we need to edit the schema.yml
```javascript
version: 2
models:
  - name: employee
    columns:
      - name: emp_id
      - name: emp_name
      - name: emp_age
      - name: department_id
```
## All is set now, lets run it
For run the dbt project using cli run this commands
First open the cmd on dbt_project path
```sh
dbt run
```
output:
```sh
Running with dbt=0.21.0
Found 1 model, 0 tests, 0 snapshots, 0 analyses, 188 macros, 0 operations, 0 seed files, 0 sources, 0 exposures

19:33:27 | Concurrency: 4 threads (target='dev')
19:33:27 |
19:33:27 | 1 of 1 START table model demo.employee_department.................... [RUN]
19:33:32 | 1 of 1 OK created table model demo.employee_department............... [SELECT in 5.75s]
19:33:36 |
19:33:36 | Finished running 1 table model in 19.85s.

Completed successfully

Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1
```
**Bravo 🎉🎉,All done. You run your first dbt project**
