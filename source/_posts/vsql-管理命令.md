---
title: vsql 管理命令
date: 2019-03-19 13:36:12
tags: vertica
categories: 数据库
---
vsql提供了很多有用的管理命令：
``` shell
General
  \c[onnect] [DBNAME|- [USER]]
                 connect to new database (currently "dbadmin")
  \cd [DIR]      change the current working directory
  \q             quit vsql
  \set [NAME [VALUE]]
                 set internal variable, or list all if no parameters
  \timing [on|off]
                 toggle timing of commands, or explicitly turn it on or off (currently off)
  \unset NAME    unset (delete) internal variable
  \! [COMMAND]   execute command in shell or start interactive shell
  \password [USER]
                 change user's password

Query Buffer
  \e [FILE]      edit the query buffer (or file) with external editor
  \g             send query buffer to server
  \g FILE        send query buffer to server and results to file
  \g | COMMAND   send query buffer to server and pipe results to command
  \p             show the contents of the query buffer
  \r             reset (clear) the query buffer
  \s [FILE]      display history or save it to file
  \w FILE        write query buffer to file

  Input/Output
  \echo [STRING] write string to standard output
  \i FILE        execute commands from file
  \o FILE        send all query results to file
  \o | COMMAND   pipe all query results to command
  \o             close query-results file or pipe
  \qecho [STRING]
                 write string to query output stream (see \o)

Informational
  \d [PATTERN]   describe tables (list tables if no argument is supplied)
                 PATTERN may include system schema name, e.g. v_catalog.* 
  \df [PATTERN]  list functions
  \dj [PATTERN]  list projections
  \dn [PATTERN]  list schemas
  \dp [PATTERN]  list table access privileges
  \ds [PATTERN]  list sequences
  \dS [PATTERN]  list system tables. PATTERN may include system schema name
                 such as v_catalog, v_monitor, or v_internal.
                 Example: v_catalog.a*
  \dt [PATTERN]  list tables
  \dtv [PATTERN] list tables and views
  \dT [PATTERN]  list data types
  \du [PATTERN]  list users
  \dv [PATTERN]  list views
  \l             list all databases
  \z [PATTERN]   list table access privileges (same as \dp)

  Formatting
  \a             toggle between unaligned and aligned output mode
  \b             toggle beep on command completion
  \C [STRING]    set table title, or unset if none
  \f [STRING]    show or set field separator for unaligned query output
  \H             toggle HTML output mode (currently off)
  \pset NAME [VALUE]
                 set table output option
                 (NAME := {format|border|expanded|fieldsep|footer|null|
                 recordsep|trailingrecordsep|tuples_only|title|tableattr|pager})
  \t             show only rows (currently off)
  \T [STRING]    set HTML <table> tag attributes, or unset if none
  \x             toggle expanded output (currently off)
```