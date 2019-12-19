---
title: "A Curious Moon | Week 3 | Makefile & Normalize Data"
date: 2019-12-04T00:47:57-07:00
draft: false
categories:
- development
- postgres
tags:
- a curious moon
- postgres
---

# A Curious Moon | Week 3 | Makefile & Normalize Data

## Apologies, it's been a while
We are sorry, life hit us hard in November. I always forget how quickly holidays are upon us. This year April and Kaleb took a much deserved vacation and so they were out of town for a few weeks and so we opted to wait for them instead of charging on ahead of them. I think it was a wise decision. We don't want anyone becoming so far behind that they feel they can't catch up. The idea is to do this together so waiting made the most sense.

## We're coding now!
Up until this chapter, most of the stuff we were doing we could do right in `psql`. Turns out that really threw a few of us for a loop. There were a lot of people that didn't realize we needed to be writing the code in an editor to create the Makefile and then we would run that in order to create a build file that would generate the normalized table.

Once we had that squared away, there was some time spent getting the group up to speed on how to use a code editor. If you followed our first coding adventure, where we learned Python, we were able to do most of that inside of Jupyter and so that's why for many of the group and IDE like Visual Studio Code is very new territory for them. I spent some time walking them through the basics and explaing why we have to run the Makefile from the folder it's in from the Terminal. All very good lessons for the group.

### Terminal Quest
As I was explaining this April asked, "Yeah, but how do I get to that folder in the terminal". It made me realize how new some of us are to the terminal. Which reminded me of the game that my son has been playing called **Terminal Quest** on his [Kano Computer Kit](https://kano.me/us/store/products/computer-kit) Computer. I think we should go through something like that. I did a quick search and was able to find the project on [Github for the source code](https://github.com/KanoComputing/terminal-quest), but it's pretty dependent on Kano OS so I'll look some more tomorrow for a similar game we can do together. Should be a lot of fun!

## Makefile
Now that we are comfortable with Visual Studio Code and the terminal, let's talk about the Makefile. As part of the course there is a folder that includes the code that was written for the book. So we started there. We read through the chapter and then we created our own folder and called it `my_code` in the same parent directory with the `code` folder that is park of the assets with the course. As we started to write our own file we compared what we were writing to what was in the `code` folder and decided we could just copy+paste and then adjust. So that's what we did.

We copied over the scripts folder as well that includes the `import.sql` file and the `normalize.sql` table. Let's take some time and cover each of these files.

Our Makefile:
{{< highlight Makefile >}}

# Set up variables to use in the script
DB=enceladus
BUILD=${CURDIR}/build.sql
SCRIPTS=${CURDIR}/scripts
CSV='${CURDIR}/../data/master_plan.csv'
MASTER=$(SCRIPTS)/import.sql
NORMALIZE = $(SCRIPTS)/normalize.sql

all: normalize
	psql $(DB) -f $(BUILD)

master:
	@cat $(MASTER) >> $(BUILD)

import: master
	@echo "COPY import.master_plan FROM $(CSV) WITH DELIMITER ',' HEADER CSV;" >> $(BUILD)

normalize: import
	@cat $(NORMALIZE) >> $(BUILD)

clean:
	@rm -rf $(BUILD)

{{< /highlight >}}

### Import.sql
This file cleans up existing tables, creates a schema called `import` and creates a `master_plan` table in that schema. We need to normalize the `master_plan` table. We will do that by creating a bunch of tables, which is exactly what the `normalize.sql` file does.

{{< highlight postgres >}}
drop table if exists events cascade;
drop table if exists teams cascade;
drop table if exists targets cascade;
drop table if exists spass_types cascade;
drop table if exists requests cascade;
drop table if exists event_types cascade;

create schema if not exists import;
drop table if exists import.master_plan;
create table import.master_plan(
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
{{< /highlight >}}

### Normalize.sql
In order to normalize the data we are going to create a bunch of lookup tables. We've decided that we need the following lookup tables:

- teams
- targets
- spass_types
- requests
- event_types

We drop the tables if they exist before creating them. This is a best practice thing, in order to prevent errors when creating a table that already exists. It makes it so that we can re-run the Makefile as many times as we need to and it will work each time.

To create the tables we are using the `distinct` command in order to only have unique records for each of the lookup tables. We then alter the table and add a primary key to it. Then the last thing that we do to normalize the tables is to create an events table that will have ids for each of the lookup tables and will reference them. Here is what that looks like:

{{< highlight postgres >}}
-- TEAM
drop table if exists teams;
select distinct(team)
as description
into teams
from import.master_plan;

alter table teams
add id serial primary key;

-- SPASS TYPES
drop table if exists spass_types;
select distinct(spass_type)
as description
into spass_types
from import.master_plan;

alter table spass_types
add id serial primary key;

-- TARGET
drop table if exists targets;
select distinct(target)
as description
into targets
from import.master_plan;


alter table targets
add id serial primary key;

-- EVENT TYPES
drop table if exists event_types;
select distinct(library_definition)
as description
into event_types
from import.master_plan;


alter table event_types
add id serial primary key;

--REQUESTS
drop table if exists requests;
select distinct(request_name)
as description
into requests
from import.master_plan;

alter table requests
add id serial primary key;

-- EVENTS TABLE
-- do this after the others are created so `references` works.
create table events(
  id serial primary key,
  time_stamp timestamp not null,
  title varchar(500),
  description text,
  event_type_id int references event_types(id),
  target_id int references targets(id),
  team_id int references teams(id),
  request_id int references requests(id),
  spass_type_id int references spass_types(id)
);

insert into events(
  time_stamp, 
  title, 
  description, 
  event_type_id, 
  target_id, 
  team_id, 
  request_id,
  spass_type_id
)	
select 
  import.master_plan.start_time_utc::timestamp, 
  import.master_plan.title, 
  import.master_plan.description,
  event_types.id as event_type_id,
  targets.id as target_id,
  teams.id as team_id,
  requests.id as request_id,
  spass_types.id as spass_type_id
from import.master_plan
left join event_types 
  on event_types.description 
  = import.master_plan.library_definition
left join targets 
  on targets.description 
  = import.master_plan.target
left join teams 
  on teams.description 
  = import.master_plan.team
left join requests 
  on requests.description 
  = import.master_plan.request_name
left join spass_types 
  on spass_types.description 
  = import.master_plan.spass_type;
{{< /highlight >}}

### Fixing errors
Remember how we said we would copy+paste the code from the course assets to ours. Well it turns out that the formatting of those files didn't have proper tabs and so we had to fix that on the `import.sql` file to properly create the table. After that it worked beautifully!

## A taste of what's to come
Now that we had tables created, we couldn't help but try a few commands to see what we could figure out. We wanted to see if we could find the records for Enceladus flybys since we know that's what is next based on the letter at the end of this chapter. Here is the command we ran as our first attempt to find the flybys:

{{< highlight postgres >}}
SELECT targets.description as target,
time_stamp,
title
FROM events
INNER JOIN targets on target_id=targets.id
WHERE title ilike '%flyby%'
AND target_id = 28;
{{< /highlight >}}

We learned that postgres has `ilike` which is the same as `like` but it's case insensitive. Which is exactly what we need for something like this.

## On the next Episode
The next episode will be running some queries to figure out how many times and on what dates Cassini did flybys of Enceladus. We are all very excited to mess around with the data more!

[< Previous Episode - Master Plan]({{< ref "a-curious-moon-week-2-master-plan.md" >}})
