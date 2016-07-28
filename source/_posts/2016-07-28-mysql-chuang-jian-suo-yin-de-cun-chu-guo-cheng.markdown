---
layout: post
title: "Mysql 创建索引存储过程"
date: 2016-07-28 21:54:27 +0800
comments: true
categories: Mysql
---

在开发过程中，经常会使用到索引。为方便创建索引，写了一个存储过程，在这里分享一下。

```mysql
USE yourdatabase;

DELIMITER $$

DROP PROCEDURE IF EXISTS `create_index_if_not_exists`$$

CREATE DEFINER=`datacenter`@`%` PROCEDURE `create_index_if_not_exists`(IN in_table_name VARCHAR(255), IN in_index_name VARCHAR(50), IN in_index_type VARCHAR(10),IN in_filed_list VARCHAR(255))
BEGIN
	SET@index_cnt = (
			SELECT COUNT(1)cnt FROM INFORMATION_SCHEMA.STATISTICS
			WHERE table_name=in_table_name AND index_name=in_index_name
	);

	IF IFNULL(@index_cnt, 0)=0 THEN SET@execute_sql=CONCAT('Alter table ', in_table_name, ' ADD ', in_index_type, ' ',in_index_name, '(',in_filed_list,');');
	PREPARE stmt FROM @execute_sql;
	EXECUTE stmt;
	DEALLOCATE PREPARE stmt;
	END IF;
END$$

DELIMITER ;
```


调用存储过程，call create_index_if_not_exists(table_name,index_name,index_type,filed_list);

参数含义如下： 

1. table_name:表名。  
2. index_name:索引名。  
3. index_type:INDEX,UNIQUE等索引类型关键字。  
4. filed_list:需要添加索引的字段。  

例：

call create_index_if_not_exists('role','roleId','UNIQUE','roleId');

call create_index_if_not_exists('user','userId','INDEX','userId');

call create_index_if_not_exists('roleuserrelation','role_user','INDEX','roleId,userId');

