#Postgres practice repository

This repository is just a dataset in CSV format.  It's meant to illustrate how you could download data from somewhere else and import it into a SQL (or relational)
 database.


#Installation

1. Create a GitHub account.
1. [Fork this repository](https://help.github.com/articles/fork-a-repo/) to your own account (create your own copy).
1. Find the `clone or download` button (make sure it says `Clone with SSH`) and copy the repository's name.
1. Go to Cloud 9, where we have a workspace installed with ruby, rails, and postgres.
1. Create a new folder in the root of your workspace and call it `postgres`.  
1. In the terminal window of cloud 9 and cd into the new folder.
1. Once you are in the new folder, clone the repository with this command: `git clone ` + what you copied from GitHub.  It should look like:
```
  git clone git@github.com:Gmfholley/postgres-practice.git
```

Once you hit enter, it should do some things, and you should have this saved locally!


#Postgres Set Up

You are going to practice SQL using the dataset in this repo.  The flavor of SQL we will be using is postgres.  It should already be installed in your workspace.  But we still need to set up a few things.

You need to start a postgres server, so that when you run postgres, it will handle your queries.  In your terminal, run:

```
  sudo service postgresql start
```

It should run some stuff and then show `done`.


Next, try running `psql` in terminal.  Because of the way we set up your workspace, it should already be installed.

Running `psql` should change your command lines:
```
  ubuntu=#
```

You are running postgres, and you can type in SQL and expect it to execute.  Let's first create the database we will use.

```
  CREATE DATABASE "practice";
```

Type `\list` to see the databases and confirm that the new one is amde.

Then type `\ q` to quit postgres.

Now we're going to enter a postgres session where we can only work with our database.  Type: `psql practice`, and you should have a new postgres session.

#Converting CSV data into postgres

We will be definine a new table.  We need to define each column name and what data type it will store. You can find [data types here](https://www.techonthenet.com/postgresql/datatypes.php).  An example create table command might be:
```
  CREATE TABLE happiness (
    id int, 
    country text, 
    ranking int, 
    score double precision
  );
```

1. There are two csv files - one with the column names and types and one with the data.  Build a SQL command to build the table you will need.
1. Next, you will convert the csv files into data in the table.  Normally, you would use a `INSERT` command to create new records, but postgres has a `COPY` command we will use for this purpose.

```
  COPY happiness FROM './happiness_data.csv' DELIMITER ',' CSV;
```

If it says it cannot find the file, you may have to use `pwd` (print working directory) to find the path of the file as you've stored it locally.  Then try changing the filename to `#{path_pwd_returned}/happiness_data.csv`.

#Playing with the data

You should have your data loaded!  Yay!  Now you can run SQL queries over it.

Want to know if the world is trending happier or unhappier?  Try this?
```
  SELECT year, AVG(score) AS average_happiness FROM happiness_index
  GROUP BY year
  ORDER BY year;
```

Want to see if the 10 unhappiest countries in 2017?
```
SELECT year, country, score as average_happiness FROM happiness_index
WHERE year=2017
ORDER BY score
LIMIT 10;

```

What about the average happiness by region instead of country?
```
  SELECT year, region, AVG(score) AS average_happiness FROM happiness_index
  GROUP BY year, region
  ORDER BY year, average_happiness;
```

Have fun!  Find your own data sets on [kaggle.com](kaggle.com/datasets).