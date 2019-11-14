---
title: "A Curious Moon | Week 1 | Getting Setup"
date: 2019-10-30T23:12:30-06:00
draft: false
categories:
- development
- postgres
tags:
- a curious moon
- postgres
---

# A Curious Moon | Week 1 | Getting Setup

After completing our Python course, [Complete Python Bootcamp: Go from zero to hero in Python 3 at Udemy](https://www.udemy.com/share/100058A0cdcVZbR34=/), we decided the next thing we would like to learn more about would be databases. At work I've been participating in a course in PostgreSQL called [A curious Moon](https://bigmachine.io/products/a-curious-moon/) and I suggested that to the group. They have agreed and so we have begun.

## Setting up; It's never easy

We actually started a week or maybe 2 ago and ran into more issues that I care to cover trying to get April and Kaleb up and running on their Windows laptops. So many, in fact, that we decided instead that we would install Ubuntu on their laptops and dual-boot so that we could have them inside a Linux environment where I could better help them as issues come up. I, and my brother Austin, are both working on Mac OSX and so this will make a more consistent environment. One that I hope means any issues they come across I will have also and will therefore be able to assist them. (I'm still debating if I should install Ubuntu, I do have it on a VirtualBox VM if I absolutely need it...).

## Beginning

Now that we have April and Kaleb using Ubuntu we were able to get through the first section of the book. This section involved us creating a database in PostgreSQL and becoming more comfortable with the terminal as well as `psql`.

We started by creating a database using `createdb`. Initially April and Kaleb needed to use the sytem `postgres` user to create users and personal databases for them in order to run get everything running as it is in the book. Not a big deal, I think it was actually a nice forray into troubleshooting. Something we'll become more and more comfortable with as we continue with these projects.

## Creating Tables

The last thing that we did was learn how to use `\h <COMMAND>` to get more information about a command. We did this with `create table` so that we could see how we would have to do that. Today we just created a very simple table from the example in the book. Something to get our feet wet. We ran the following to create our first table:
{{< highlight postgres >}}
create table master_plan(
    the_date date,
    title varchar(100),
    description text
);
{{< /highlight >}}

## On the next Episode >
The next chapter, which we will be doing this week and we will talk about on November 6th, is creating **The Master Plan Table**. I'm sure this section will have us create this table in a much better and more complete way. I'm excited for lies ahead for us, especially excited to see how Austin, April and Kaleb do.

[Next Episode - Master Plan Table >]({{< ref "a-curious-moon-week-2-master-plan.md" >}})