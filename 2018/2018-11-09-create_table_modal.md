---
title: mysql 建库建表模板
date: 2018-11-09
tag: [mysql]
---

# 建库建表模板

### 建库

```mysql
CREATE DATABASE test DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```

### 建表

```mysql
CREATE TABLE `user` (
  `id` BIGINT(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `nickname` VARCHAR(200) NOT NULL DEFAULT '' COMMENT '昵称',
  `avatar` VARCHAR(200) NOT NULL DEFAULT '' COMMENT '头像',
  `description` VARCHAR(500) NOT NULL DEFAULT '' COMMENT '简评',
  `signature` VARCHAR(200) NOT NULL DEFAULT'' COMMENT '个性签名',
  `gender` INT(11) NOT NULL DEFAULT '0' COMMENT '1男 2女 0未知',
  `score` INT(11) NOT NULL DEFAULT '0' COMMENT '分数',
  `coin` INT(11) NOT NULL DEFAULT '0' COMMENT '金币',
  `city` VARCHAR(200) NOT NULL DEFAULT '' COMMENT '所在城市',
  `birthday` VARCHAR(200) NOT NULL DEFAULT '' COMMENT '生日',
  `constellation` VARCHAR(200) NOT NULL DEFAULT '未填写' COMMENT '星座',
  `mobile` VARCHAR(200) NOT NULL DEFAULT '' COMMENT '手机号',
  `email` VARCHAR(200) NOT NULL DEFAULT '' COMMENT '邮箱',
  
  `extra` VARCHAR(10000) NOT NULL DEFAULT '' COMMENT '备用字段',
  `created` INT(11) unsigned NOT NULL DEFAULT '0' COMMENT '创建时间',
  `updated` INT(11) unsigned NOT NULL DEFAULT '0' COMMENT '更新时间',
  `is_deleted` INT(11) NOT NULL DEFAULT '0' COMMENT '0未删除 1已删除',
  PRIMARY KEY (`id`),
  KEY `idx_created` (`created`),
  KEY `idx_updated` (`updated`)
) ENGINE=InnoDB AUTO_INCREMENT=10001 DEFAULT CHARSET=utf8mb4 COMMENT='用户表';
```

