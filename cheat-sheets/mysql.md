---
title: MySQL
description: 
published: true
date: 2020-10-29T09:17:31.348Z
tags: 
editor: undefined
dateCreated: 2020-08-10T02:34:19.898Z
---

## Creating a new user

```mysql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```

## Grant privileges

Replace `ALL PRIVILEGES` to one of these if needed

`ALL PRIVILEGES` - allow a MySQL user full access to a designated database
`CREATE` - allows them to create new tables or databases
`DROP` - allows them to them to delete tables or databases
`DELETE` - allows them to delete rows from tables
`INSERT` - allows them to insert rows into tables
`SELECT` - allows them to use the SELECT command to read through databases
`UPDATE` - allow them to update table rows
`GRANT OPTION` - allows them to grant or remove other users’ privileges

```mysql
GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
```

## Refreshing privileges

```mysql
FLUSH PRIVILEGES;
```

## Disable foreign key checks

Other times, it is understandable to need to disable foreign key checks for tables under maintenance. Do this with caution.

```mysql
SET FOREIGN_KEY_CHECKS=0;
```

To re-enable, just set the value to 1.