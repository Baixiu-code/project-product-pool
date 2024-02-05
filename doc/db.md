template 前台类目模版表
```sql
CREATE TABLE `template` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键id',
  `tenant_id` bigint(20) NOT NULL COMMENT '租户id',
  `template_code` varchar(50) NOT NULL COMMENT '模板编号',
  `template_name` varchar(64) NOT NULL COMMENT '模板名称',
  `scene` tinyint(4) NOT NULL COMMENT '应用场景:1-小程序商品,2-线下收银场景商品',
  `category_level` varchar(50) NOT NULL COMMENT '类目最大层级',
  `status` tinyint(4) NOT NULL COMMENT '状态:1-启用,0-停用',
  `limit_store` tinyint(4) NOT NULL COMMENT '门店:0-不限门店,1-自选门店',
  PRIMARY KEY (`id`),
  KEY `idx_tid_tcode` (`tenant_id`,`template_code`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8 COMMENT='前台类目模板表';


```
template_store_rel 模版门店
```sql
CREATE TABLE `template_store_rel` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键id',
  `tenant_id` bigint(20) NOT NULL COMMENT '租户id',
  `template_code` varchar(50) NOT NULL COMMENT '模板编号',
  `scene` tinyint(4) NOT NULL COMMENT '应用场景:1-小程序商品,2-门店快捷商品',
  `store_id` bigint(20) NOT NULL COMMENT '门店id',
  `store_name` varchar(64) NOT NULL COMMENT '门店名称',
  PRIMARY KEY (`id`),
  KEY `idx_tcode_scene` (`tenant_id`,`template_code`,`scene`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8 COMMENT='前台类目模板关联门店表';

```
front_category 前台类目表
```sql
CREATE TABLE `front_category` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增主键ID',
  `tenant_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '租户ID',
  `category_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '类目ID',
  `name` varchar(100) NOT NULL COMMENT '类目名称',
  `parent_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '父类目ID',
  `img_url` varchar(256) DEFAULT NULL COMMENT '类目图片',
  `level` int(11) DEFAULT '1' COMMENT '类目等级',
  `is_hide` tinyint(4) DEFAULT '1' COMMENT '是否隐藏 0、显示 1、隐藏',
  `order_sort` tinyint(4) DEFAULT '1' COMMENT '排序方式 1、综合排序 2、新品降序排序 3、15天销量降序排序 4、7天销量降序排序 5、价格升序',
  `source` tinyint(4) DEFAULT NULL COMMENT '来源: 1:模板；2:自定义',
  `status` tinyint(4) DEFAULT '1' COMMENT '状态：0无效、1有效、-1删除',
  `template_code` varchar(50) NOT NULL DEFAULT '001' COMMENT '类目模板编号',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE KEY `uniq_tenantId_categoryId` (`tenant_id`,`category_id`) USING BTREE,
  KEY `idx_tenantId_hide` (`tenant_id`,`is_hide`) USING BTREE,
  KEY `idx_tenantId_parentId` (`tenant_id`,`parent_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=32 DEFAULT CHARSET=utf8 COMMENT='前台类目表';

```
product_pool_main 商品池主表
```sql
CREATE TABLE `product_zone` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增主键',
  `tenant_id` bigint(20) NOT NULL COMMENT '租户Id',
  `pool_id` bigint(20) NOT NULL COMMENT '商品池编码',
  `zone_name` varchar(48) NOT NULL COMMENT '商品池名称',
  `status` tinyint(2) NOT NULL DEFAULT '0' COMMENT '状态: 0停用，1启用',
  `source_mode` tinyint(2) NOT NULL COMMENT '来源: 1自动拉取,2自定义商品',
  `store_rule` text COMMENT '指定门店集合 ALL-全门店 storeid集合-部分门店',
  `category_rule` text COMMENT '指定类目集合',
  `promotion_rule` text COMMENT '指定促销集合',
  `default_sort_by` tinyint(2) DEFAULT '1' COMMENT '默认排序规则：1.新品上架 2.基础价格 3.七天GMV',
  `specify_range` tinyint(2) DEFAULT '2' COMMENT '商品范围，自定义商品时，需指定1-spu2-sku(默认)',
  `specify_product` text COMMENT '指定spuId集合',
  `specify_sku` text COMMENT '指定skuId集合',
  `batch_no` bigint(10) DEFAULT NULL COMMENT '数据批号',
  `create_from` int(3) DEFAULT NULL COMMENT '创建来源',
  PRIMARY KEY (`id`),
  KEY `idx_tenant_id` (`tenant_id`,`pool_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 COMMENT='商品池信息表';
```

product_pool_store_sku 商品池店品表
```sql
CREATE TABLE `product_zone_store_sku` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增主键',
  `tenant_id` bigint(20) NOT NULL COMMENT '租户Id',
  `pool_id` bigint(20) NOT NULL COMMENT '商品池编码',
  `store_id` bigint(20) NOT NULL COMMENT '门店id',
  `product_id` varchar(50) NOT NULL COMMENT '商品Id',
  `sku_id` varchar(50) NOT NULL COMMENT 'skuId',
  `sku_name` varchar(256) DEFAULT NULL COMMENT '商品sku名称',
  `status` tinyint(2) NOT NULL DEFAULT '1' COMMENT '（预留）状态 1-正常',
  `stock_status` tinyint(2) NOT NULL COMMENT '库存状态: 0售罄，1有货',
  `gmv_seven_day` decimal(14,2) DEFAULT NULL COMMENT '七天GMV',
  `sort` tinyint(4) DEFAULT NULL COMMENT '排序权重',
  `batch_no` bigint(10) DEFAULT NULL COMMENT '数据批号',
  PRIMARY KEY (`id`),
  KEY `idx_tenant_id` (`tenant_id`,`pool_id`,`store_id`,`product_id`,`sku_id`,`sort`) USING BTREE,
  KEY `idx_tenant_sku_id` (`tenant_id`,`store_id`,`sku_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=191 DEFAULT CHARSET=utf8 COMMENT='商品池商品数据表';
```
