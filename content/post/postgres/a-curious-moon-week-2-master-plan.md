---
title: "A Curious Moon | Week 2 | Master Plan"
date: 2019-11-06T00:01:23-07:00
draft: false
categories:
- development
- postgres
tags:
- a curious moon
- postgres
---

# A Curious Moon | Week 2 | Master Plan

## Create Master Plan table
{{< highlight postgres >}}
create table master_plan(
    the_date date,
    title varchar(100), 
    description text
);
{{< /highlight >}}

We have a table! Just as we were getting exciting we find out that we were supposed to add a primary key and also what if the table already exists? So in order to fix those things our new create table looks like this:

{{< highlight postgres >}}
drop table if exists master_plan;
create table master_plan(
    id serial primary key,
    the_date date,
    title varchar(100), 
    description text
);
{{< /highlight >}}

The `serial primary key` is some postgres shortcode that will add a column of integer type that can't be NULL and will auto increment.

## Drop Master Plan table
{{< highlight postgres >}}
drop table master_plan;
{{< /highlight >}}

## Importing data into Master Plan table
Now that we have a table we need to get the data from the CSV into it. Turns out that postgres has a convenient command `COPY FROM` that will allow us to do just that. Our command looks like this:

{{< highlight postgres >}}
COPY master_plan
FROM '[PATH TO DIRECTORY]/master_plan.csv' WITH DELIMITER ',' HEADER CSV;
{{< /highlight >}}

This bit of code caused us more problems than we care to admit. The `FROM` requires the full path the directory. This became very tricky to get it all correct. Normally I'm able to find the path very easily, however, as April, Kaleb and Austin were working on this I didn't have the ability to see their screens and so I was doing a lot of guessing to get them to where they needed to be to get the full path. I knew we just needed to get them to the folder where the `master_plan.csv` was located and then use the `pwd` command to get the full path and copy+paste that. Unfortunately everyone had their folders set up differently. Once we had it figured out and we added the correct path, the imported the records nicely.

## Schema
Now we had a table and it had the data in it. We were definitely excited to see that. Next thing we had to do was to put the table into a schema so that it wasn't inside the default `public` schema. The suggestion was to use `import` so we just went with that. It was simple enough to do.

{{< highlight postgres >}}
create schema if not exists import;
{{< /highlight >}}

## Build.sql File - Idempotence
We had code taht worked and was doing what we needed. We even had the table inside a schema. The next big thing we learned was `idempotence`. 

>__...denoting an element of a set that is unchanged in value when multiplied or otherwise operated on by itself.__

Basically we needed to create a script file that would create the table and import the data and it needed to be able to be run over and over again without issue. To do this we built a `build.sql` file.

{{< highlight postgres >}}
create schema if not exists import;
drop table if exists master_plan;
create table master_plan(
  start_time_utc text,
  duration text,
  date text,
  team text,
  spass_type text,
  target text,
  request_name text,
  library_definition text,
  title text,
  description text
);
COPY import.master_plan FROM '/Users/ammon/curious_moon/data/master_plan.csv' WITH DELIMITER ',' HEADER CSV;
{{< /highlight >}}

We only create the schema if it exists. We will drop the table if it exists and re-create it adn then we will always import the data fresh. This checks all the boxes for idempotence.

## On the next Episode
The next episode will be creating a make file to generate the `build.sql` and normailzing the database. We googled that and basically it means that we don't want to have duplicated inforamtion in the same table. We'll be craeting lookup tables. Sounds fun!

[< Previous Episode - Setup]({{< ref "a-curious-moon-week-1-setup.md" >}})

[Next Episode - Setup >]({{< ref "a-curious-moon-week-3-make-normalize.md" >}})
