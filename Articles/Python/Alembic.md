# Alembic

Alembic is a lightweight database migration tool for SQLAlchemy (python). Just like we have a version control for Git, Alembic acts as a version control for the Database. 

## Setup

This section contains information about the steps to setup alembic and also what each of its files mean. If you want to know more about the
implementation, you can g

- Since Alembic is a python tool, you can easily install by using `pip` package manner. Just run the following command to install alembic
  in your environment.

    ```shell
    pip install alembic
    ```

    Once this is done, we are ready to use alembic, you can try writing `alembic` in the terminal to see if it is installed.

- Now to start with alembic, we need to initialise our project. So just like `git` we run the `init` method to set up alembic for our project.
  ```shell
  alembic init alembic
  ```
  Once you have initialised alembic, a new folder named `alembic` will pop up along with a file called `alembic.ini`. The folder structure for that folder will be as follows:
    ```
    alembic
    ├── README
    ├── env.py
    ├── script.py.mako
    └── versions/
    alembic.ini
    ```
  Here's what these files do: 1. `alembic.ini` : This is the configuration file for alembic. It defines the database connection and other information which alembic uses to do the migrations. We can also mention things like which tables to include/exclude etc.
  Following is a sample alembic.ini file:

  ```ini
    [alembic]
    script_location = alembic_migrations
    sqlalchemy.url = postgresql://user:password@localhost/dbname

    [alembic:exclude]
    tables = excluded_table1, excluded_table2

    [alembic:include]
    tables = included_table1, included_table2
  ```

  There might be cases where we want to use multiple `.ini` files for different cases. Say for example you can have a `prod.ini`, `dev.ini` etc. We can then explicitly specify which file to use.
    ```shell
    alembic -c dev.ini upgrade head
    ```

  2. `env.py` : This is a Python script that sets up the migration environment and provides the necessary configurations for Alembic to manage database migrations.

  3. `versions` : Versions is a folder which contains all the migration scripts for upgrading or downgrading the datbase. The files inside version are mostly auto-generated and we just need to edit them as per our use-case.

## General Flow of things

In alembic, for most use-cases, the following flow is followed: 

```mermaid
graph TD;
    A[Init Alembic]-->B[Check Current State];
    B --> C[Create a new revision];
    C --> D[Write upgrade and downgrade scripts];
    D --> E{If any conflicts?};
    E --> |Yes| F[Resolve Conflicts];
    E --> |No| G[Upgrade the DB to the latest revision];
    F --> G;
    G --> B;
```

## Key Commands

Alembic has the following commands available for us in its CLI: 
```txt
branches            Show current branch points.
check               Check if revision command with autogenerate has pending upgrade ops.
current             Display the current revision for a database.
downgrade           Revert to a previous version.
edit                Edit revision script(s) using $EDITOR.
ensure_version      Create the alembic version table if it doesn't exist already .
heads               Show current available heads in the script directory.
history             List changeset scripts in chronological order.
init                Initialize a new scripts directory.
list_templates      List available templates.
merge               Merge two revisions together. Creates a new migration file.
revision            Create a new revision file.
show                Show the revision(s) denoted by the given symbol.
stamp               'stamp' the revision table with the given revision; don't run any migrations.
upgrade             Upgrade to a later version.
``` 

From the above, the ones that are the most important are `history`, `revision` and `upgrade`.

### History

History is like your `git log`. It shows all the changes made in their appropriate order. It shows all the changes/commits or revisions along with their description and the head.

To view the history, we need to run `alembic history`

### Revision

A revision in alembic is like a git commit. It marks a change in the DB and contains the details about how to get to that state or come back from the state.  

Every revision has a description and also a unique hash. This hash can then be used upgrade or downgrade to that particular revision.

To create a new revision we run
```shell
alembic revision -m "This is a test revision"
```

This will create a new revision file inside the version. 

```python
"""This is a test revision

Revision ID: a3d1cd8212da
Revises: 
Create Date: 2023-07-29 15:32:10.024636

"""
from alembic import op
import sqlalchemy as sa


# revision identifiers, used by Alembic.
revision = 'a3d1cd8212da'
down_revision = None
branch_labels = None
depends_on = None


def upgrade() -> None:
    pass


def downgrade() -> None:
    pass
```

Here, we can see, every revision has an ID. It also states the revision it upgrades or goes back to. 
If in case there is a conflict where 2 revisions roll back to the same version, we can change the down_revision values to put them in the right order. 

The revision also has other features like 
```
alembic revision --autogenerate --rev-id my_custom_revision
```

- `--rev-id` is used to give a custom ID to the revisions.
- `--autogenerate` goes through the DB and compares the difference between the DB and the scripts. And it autogenerates the commands to bring the scripts upto the DB. 
This can be useful when you are doing manual additions to the DB and want to have them added to the scripts too.

> Alembic keeps a track of the current version in the DB with the help of a table called `alembic`. So if we ever need to check the verison of something we can check it on the DB. The local version is dictated by the revision files. Alembic iterates through all the revision files to generate a history.

### Upgrade and Downgrade

Whenever we have changes that need to be migrated to the DB, we run the upgrade command. The upgrade command checks the DB and runs all the scripts between the version in the DB and the current local version. 
```shell
alembic upgrade head
```
This brings the DB to the latest revision. In case we need to upgrade to a specific revision, we can directly mention the id of that commit. `alembic upgrade <id>`

Similar to upgrade, downgrade brings down the DB to a previous revision if we mention that ID OR we can mention it relative to the head. 
```shell
alembic downgrade -1 OR alembic downgrade <id>
```

## Conclusion
This was a very brief overview of alembic and what its capabilities are. There are a lot more concepts like branching, merging branches etc. But on the broader scale, this is how alembic is used to migrate and control database schemas. It's a useful tool for simplifying and organising database migrations.