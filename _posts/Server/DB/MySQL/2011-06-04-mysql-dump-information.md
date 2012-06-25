---
layout: post
category : MySQL
tags : [mysql,mysqldump]
title: mysqldump 详解
---
{% include JB/setup %}


mysqldump 命令位于 `mysql/bin/` 目录中,现有环境有两台 mysql 服务器,IP 分别为:

- A: ` 192.168.102.2 `
- B: ` 192.168.102.3 `

###完整备份 MySQL 的某个数据库：

	mysqldump –h hostname –u username –p password databasename > backupfile.sql

例如：将  ` 192.168.102.2 ` 服务器上的 book 数据库备份到 ` 192.168.102.3`

	mysqldump -h 192.168.102.2 -u backup -p book >book.sql

###备份 MySQL 数据库为带删除表的格式

备份 MySQL 数据库为带删除表的格式，能够让该备份覆盖已有数据库而不需要手动删除原有数据库。 这样可以保证导回 MySQL 数据库的时候不会出错，因为每次导回的时候，都会首先检查表是否存在，存在就删除

例如：将 ` 192.168.102.2 ` 服务器上的 book 数据库备份到 ` 192.168.102.3 `

	mysqldump -–add-drop-table -h hostname –u username –p password databasename > backupfile.sql

例如：将 ` 192.168.102.2 ` 服务器上的 book 数据库备份到 ` 192.168.102.3 `

	mysqldump --add-drop-table -h 192.168.102.2 -u backup -p book >book.sql

###直接将 MySQL 数据库压缩备份

	mysqldump –h hostname –u username –p password databasename | gzip > backupfile.sql.gz

例如：将 ` 192.168.102.2 ` 服务器上的 book 数据库压缩备份到 ` 192.168.102.3 ` 服务器

	mysqldump -h 192.168.102.2 -u backup -p book | gzip >book.sql

###备份 MySQL 数据库某个 ( 些 ) 表

	mysqldump –h hostname –u username –p password databasename specific_table1 specific_table2 > backupfile.sql

例如：将 ` 192.168.102.2 ` 服务器上的 backup 数据库中的 books 和 orders 表备份到 ` 192.168.102.3 ` 服务器

	mysqldump -h 192.168.102.2 -u backup -p backup books orders>book.sql
	
###仅仅备份数据库结构

	mysqldump --no-data –h hostname –u username –p password –d databasename1 databasename2 databasename3 >structurebackupfile.sql

例如：仅将 ` 192.168.102.2 `  服务器上的 backup 数据库的表结构备份到 ` 192.168.102.3 ` 服务器

	mysqldump --no-data -d backup -h 192.168.102.2 -u backup -p >book.sql

###备份指定条件的数据

例如只想把服务器 ` 192.168.102.2 ` 上的数据库 test 中的表 test 中的 id>1 的内容备份，可以使用下面的命令：

	mysqldump -h 192.168.102.2 -u backup -p test test --where "id>1">test.sql

###还原 MySQL 数据库的命令

	mysql –h hostname –u username –p password databasename < backupfile.sql

###还原压缩的 MySQL 数据库

	gunzip < backupfile.sql.gz | mysql –u username –p password databasename


<hr>

##mysqldump 选项，部分选项如下：


<table class="table table-bordered">
<thead>
<tr>
<th width="20%">Option
</th>
<th>Notes
</th>
</tr>
</thead>
<tbody>

<tr>
<td>
<code>–add-drop-table
</code>
</td>
<td>这个选项将会在每一个表的前面加上 <code>DROP TABLE IF EXISTS</code> 语句，这样可以保证导回 MySQL 数据库的时候不会出错，因为每次导回的时候，都会首先检查表是否存在，存在就删除
</td>
</tr>

<tr>
<td>
<code>–add-locks
</code>
</td>
<td>这个选项会在 <code>INSERT</code> 语句中捆上一个 <code>LOCK TABLE</code> 和 <code>UNLOCK TABLE</code> 语句。这就防止在这些记录被再次导入数据库时其他用户对表进行的操作
</td>
</tr>

<tr>
<td>
<code>-c or – complete_insert
</code>
</td>
<td>这个选项使得 mysqldump 命令给每一个产生 <code>INSERT</code> 语句加上列（ <code>field</code> ）的名字。当把数据导出导另外一个数据库时这个选项很有用。
</td>
</tr>

<tr>
<td>
<code>–delayed-insert 
</code>
</td>
<td>在 <code>INSERT</code> 命令中加入 DELAY 选项
</td>
</tr>


<tr>
<td>
<code>-F or -flush-logs
</code>
</td>
<td>使用这个选项，在执行导出之前将会刷新 MySQL 服务器的 log.
</td>
</tr>

<tr>
<td>
<code>-f or -force
</code>
</td>
<td>使用这个选项，即使有错误发生，仍然继续导出
</td>
</tr>

<tr>
<td>
<code>–full
</code>
</td>
<td>这个选项把附加信息也加到 <code>CREATE TABLE</code> 的语句中
</td>
</tr>

<tr>
<td>
<code>-l or -lock-tables
</code>
</td>
<td>使用这个选项，导出表的时候服务器将会给表加锁。
</td>
</tr>

<tr>
<td>
<code>-t or -no-create- info
</code>
</td>
<td>这个选项使的 <code>mysqldump</code> 命令不创建 <code>CREATE TABLE</code> 语句，这个选项在您只需要数据而不需要 DDL （数据库定义语句）时很方便。
</td>
</tr>

<tr>
<td>
<code>-d or -no-data
</code>
</td>
<td>这个选项使的 mysqldump 命令不创建 INSERT 语句。 在您只需要 DDL 语句时，可以使用这个选项
</td>
</tr>

<tr>
<td>
<code>–opt
</code>
</td>
<td>此选项将打开所有会提高文件导出速度和创造一个可以更快导入的文件的选项。
</td>
</tr>

<tr>
<td>
<code>-q or -quick 
</code>
</td>
<td>这个选项使得 MySQL 不会把整个导出的内容读入内存再执行导出，而是在读到的时候就写入导文件中。
</td>
</tr>

<tr>
<td>
<code>-T path or -tab = path 
</code>
</td>
<td>这个选项将会创建两个文件，一个文件包含 DDL 语句或者表创建语句，另一个文件包含数据。 DDL 文件被命名为 table_name.sql, 数据文件被命名为 table_name.txt. 路径名是存放这两个文件的目录。目录必须已经存在，并且命令的使用者有对文件的特权。
</td>
</tr>

<tr>
<td>
<code>-w “WHERE Clause” or -where = “Where clause ”
</code>
</td>
<td>如前面所讲的，您可以使用这一选项来过筛选将要放到 导出文件的数据。假定您需要为一个表单中要用到的帐号建立一个文件，经理要看今年（ 2004 年）所有的订单（ Orders ），它们并不对 DDL 感兴趣，并且需要文件有逗号分隔，因为这样就很容易导入到 Excel 中。 为了完成这个人物，您可以使用下面的句子：
<br><code>bin/mysqldump – p – where "Order_Date >='2000-01-01'" – tab = /home/mark – no-create-info – fields-terminated-by=, Meet_A_Geek Orders
</code>
</td>
</tr>

</tbody>
</table>

*示例*

	mysqldump –complete-insert –flush-logs –add-drop-database –add-drop-table –create-options –quick –lock-tables –quote-names –user=root –password=passwd–result-file=db.sql dbname

 