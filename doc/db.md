template ǰ̨��Ŀģ���
```sql
CREATE TABLE `template` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '����id',
  `tenant_id` bigint(20) NOT NULL COMMENT '�⻧id',
  `template_code` varchar(50) NOT NULL COMMENT 'ģ����',
  `template_name` varchar(64) NOT NULL COMMENT 'ģ������',
  `scene` tinyint(4) NOT NULL COMMENT 'Ӧ�ó���:1-С������Ʒ,2-��������������Ʒ',
  `category_level` varchar(50) NOT NULL COMMENT '��Ŀ���㼶',
  `status` tinyint(4) NOT NULL COMMENT '״̬:1-����,0-ͣ��',
  `limit_store` tinyint(4) NOT NULL COMMENT '�ŵ�:0-�����ŵ�,1-��ѡ�ŵ�',
  PRIMARY KEY (`id`),
  KEY `idx_tid_tcode` (`tenant_id`,`template_code`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8 COMMENT='ǰ̨��Ŀģ���';


```
template_store_rel ģ���ŵ�
```sql
CREATE TABLE `template_store_rel` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '����id',
  `tenant_id` bigint(20) NOT NULL COMMENT '�⻧id',
  `template_code` varchar(50) NOT NULL COMMENT 'ģ����',
  `scene` tinyint(4) NOT NULL COMMENT 'Ӧ�ó���:1-С������Ʒ,2-�ŵ�����Ʒ',
  `store_id` bigint(20) NOT NULL COMMENT '�ŵ�id',
  `store_name` varchar(64) NOT NULL COMMENT '�ŵ�����',
  PRIMARY KEY (`id`),
  KEY `idx_tcode_scene` (`tenant_id`,`template_code`,`scene`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8 COMMENT='ǰ̨��Ŀģ������ŵ��';

```
front_category ǰ̨��Ŀ��
```sql
CREATE TABLE `front_category` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '��������ID',
  `tenant_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '�⻧ID',
  `category_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '��ĿID',
  `name` varchar(100) NOT NULL COMMENT '��Ŀ����',
  `parent_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '����ĿID',
  `img_url` varchar(256) DEFAULT NULL COMMENT '��ĿͼƬ',
  `level` int(11) DEFAULT '1' COMMENT '��Ŀ�ȼ�',
  `is_hide` tinyint(4) DEFAULT '1' COMMENT '�Ƿ����� 0����ʾ 1������',
  `order_sort` tinyint(4) DEFAULT '1' COMMENT '����ʽ 1���ۺ����� 2����Ʒ�������� 3��15�������������� 4��7�������������� 5���۸�����',
  `source` tinyint(4) DEFAULT NULL COMMENT '��Դ: 1:ģ�壻2:�Զ���',
  `status` tinyint(4) DEFAULT '1' COMMENT '״̬��0��Ч��1��Ч��-1ɾ��',
  `template_code` varchar(50) NOT NULL DEFAULT '001' COMMENT '��Ŀģ����',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE KEY `uniq_tenantId_categoryId` (`tenant_id`,`category_id`) USING BTREE,
  KEY `idx_tenantId_hide` (`tenant_id`,`is_hide`) USING BTREE,
  KEY `idx_tenantId_parentId` (`tenant_id`,`parent_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=32 DEFAULT CHARSET=utf8 COMMENT='ǰ̨��Ŀ��';

```
product_pool_main ��Ʒ������
```sql
CREATE TABLE `product_zone` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '��������',
  `tenant_id` bigint(20) NOT NULL COMMENT '�⻧Id',
  `pool_id` bigint(20) NOT NULL COMMENT '��Ʒ�ر���',
  `zone_name` varchar(48) NOT NULL COMMENT '��Ʒ������',
  `status` tinyint(2) NOT NULL DEFAULT '0' COMMENT '״̬: 0ͣ�ã�1����',
  `source_mode` tinyint(2) NOT NULL COMMENT '��Դ: 1�Զ���ȡ,2�Զ�����Ʒ',
  `store_rule` text COMMENT 'ָ���ŵ꼯�� ALL-ȫ�ŵ� storeid����-�����ŵ�',
  `category_rule` text COMMENT 'ָ����Ŀ����',
  `promotion_rule` text COMMENT 'ָ����������',
  `default_sort_by` tinyint(2) DEFAULT '1' COMMENT 'Ĭ���������1.��Ʒ�ϼ� 2.�����۸� 3.����GMV',
  `specify_range` tinyint(2) DEFAULT '2' COMMENT '��Ʒ��Χ���Զ�����Ʒʱ����ָ��1-spu2-sku(Ĭ��)',
  `specify_product` text COMMENT 'ָ��spuId����',
  `specify_sku` text COMMENT 'ָ��skuId����',
  `batch_no` bigint(10) DEFAULT NULL COMMENT '��������',
  `create_from` int(3) DEFAULT NULL COMMENT '������Դ',
  PRIMARY KEY (`id`),
  KEY `idx_tenant_id` (`tenant_id`,`pool_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 COMMENT='��Ʒ����Ϣ��';
```

product_pool_store_sku ��Ʒ�ص�Ʒ��
```sql
CREATE TABLE `product_zone_store_sku` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '��������',
  `tenant_id` bigint(20) NOT NULL COMMENT '�⻧Id',
  `pool_id` bigint(20) NOT NULL COMMENT '��Ʒ�ر���',
  `store_id` bigint(20) NOT NULL COMMENT '�ŵ�id',
  `product_id` varchar(50) NOT NULL COMMENT '��ƷId',
  `sku_id` varchar(50) NOT NULL COMMENT 'skuId',
  `sku_name` varchar(256) DEFAULT NULL COMMENT '��Ʒsku����',
  `status` tinyint(2) NOT NULL DEFAULT '1' COMMENT '��Ԥ����״̬ 1-����',
  `stock_status` tinyint(2) NOT NULL COMMENT '���״̬: 0������1�л�',
  `gmv_seven_day` decimal(14,2) DEFAULT NULL COMMENT '����GMV',
  `sort` tinyint(4) DEFAULT NULL COMMENT '����Ȩ��',
  `batch_no` bigint(10) DEFAULT NULL COMMENT '��������',
  PRIMARY KEY (`id`),
  KEY `idx_tenant_id` (`tenant_id`,`pool_id`,`store_id`,`product_id`,`sku_id`,`sort`) USING BTREE,
  KEY `idx_tenant_sku_id` (`tenant_id`,`store_id`,`sku_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=191 DEFAULT CHARSET=utf8 COMMENT='��Ʒ����Ʒ���ݱ�';
```
