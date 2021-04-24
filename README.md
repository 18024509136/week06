# week06
1.用户表  
create table user_info  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   varchar(11) not null default '' comment '用户ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;name                 varchar(50) not null default '' comment '用户名',  
   &nbsp;&nbsp;&nbsp;&nbsp;password             varchar(32) not null default '' comment '密码，md5加密存放',  
   &nbsp;&nbsp;&nbsp;&nbsp;email                varchar(50) not null default '' comment '邮箱',  
   &nbsp;&nbsp;&nbsp;&nbsp;auth_str             varchar(200) not null default '' comment '权限标识，如user:query,goods:query',  
   &nbsp;&nbsp;&nbsp;&nbsp;active_flag          tinyint not null default 1 comment '激活标识，0：未激活 1：激活',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id),  
   &nbsp;&nbsp;&nbsp;&nbsp;key AK_unique_name (name)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  

2.用户地址表  
create table user_address_info  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   varchar(15) not null default '' comment '主键',  
   &nbsp;&nbsp;&nbsp;&nbsp;user_id              varchar(11) not null default '' comment '归属用户ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;recv_user            varchar(50) not null default '' comment '收件人',  
   &nbsp;&nbsp;&nbsp;&nbsp;recv_addr            varchar(200) not null default '' comment '收件地址',  
   &nbsp;&nbsp;&nbsp;&nbsp;zipcode              varchar(30) not null default '' comment '邮编',  
   &nbsp;&nbsp;&nbsp;&nbsp;tel                  varchar(15) not null default '' comment '联系方式',  
   &nbsp;&nbsp;&nbsp;&nbsp;default_flag         tinyint not null default 1 comment '是否默认，0：否 1：是',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '更新时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  

3.商品一级分类表  
create table product_first_category  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   int not null auto_increment comment '自增主键',  
   &nbsp;&nbsp;&nbsp;&nbsp;name                 varchar(50) not null default '' comment '名称',  
   &nbsp;&nbsp;&nbsp;&nbsp;status               tinyint not null default 1 comment '状态，0：无效 1：有效',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id),  
   &nbsp;&nbsp;&nbsp;&nbsp;key AK_unique_name (name)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  

4.商品二级分类表  
create table product_second_category  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   int not null auto_increment comment '自增主键',  
   &nbsp;&nbsp;&nbsp;&nbsp;parent_id            int not null default 0 comment '归属一级分类ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;name                 varchar(50) not null default '' comment '名称',  
   &nbsp;&nbsp;&nbsp;&nbsp;status               tinyint not null default 1 comment '状态，0：无效 1：有效',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id),  
   &nbsp;&nbsp;&nbsp;&nbsp;key AK_unique_name (name)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  

5.商品三级分类表  
create table product_third_category  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   int not null auto_increment comment '自增主键',  
   &nbsp;&nbsp;&nbsp;&nbsp;parent_id            int not null default 0 comment '归属二级分类ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;name                 varchar(50) not null default '' comment '名称',  
   &nbsp;&nbsp;&nbsp;&nbsp;status               tinyint not null default 1 comment '状态，0：无效 1：有效',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id),  
   &nbsp;&nbsp;&nbsp;&nbsp;key AK_unique_name (name)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  

6.规格表  
规格模板字段描述了共享规格约束和私有规格约束，例如[{'group':'主芯片',params:[{'key':'核数', 'global':true, 'options':[4,8,16,32]}]]  
create table product_spec  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;category_id          int not null default 0 comment '归属三级分类ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;spec_template        varchar(2000) not null default '' comment '规格模板，json格式',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (category_id)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  

7.商品SPU表  
create table product_spu  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   varchar(11) not null default '' comment '主键',  
   &nbsp;&nbsp;&nbsp;&nbsp;name                 varchar(200) not null default '' comment '名称',  
   &nbsp;&nbsp;&nbsp;&nbsp;description          varchar(500) not null default '' comment '描述',    
   &nbsp;&nbsp;&nbsp;&nbsp;third_cat_id         int not null default 0 comment '三级分类ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id),  
   &nbsp;&nbsp;&nbsp;&nbsp;key AK_unique_name (name)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  

8.商品SKU表  
name字段加唯一约束来保证商品名称的唯一。spu_id字段加索引保证通过spu_id快速查到下属的sku的记录集。spec字段保存了根据规格模板生成的具体的json数据    
create table product_sku  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   varchar(15) not null default '' comment '主键',  
   &nbsp;&nbsp;&nbsp;&nbsp;name                 varchar(100) not null default '' comment '名称',  
   &nbsp;&nbsp;&nbsp;&nbsp;brief                varchar(200) not null default '' comment '简介',  
   &nbsp;&nbsp;&nbsp;&nbsp;price                int not null default 0 comment '单价，单位分',  
   &nbsp;&nbsp;&nbsp;&nbsp;stock                int not null default 0 comment '库存',  
   &nbsp;&nbsp;&nbsp;&nbsp;spec                 varchar(500) not null default '' comment '规格描述，json格式',  
   &nbsp;&nbsp;&nbsp;&nbsp;sales                int not null default 0 comment '销量',  
   &nbsp;&nbsp;&nbsp;&nbsp;logo                 varchar(250) not null default '' comment '图片地址',  
   &nbsp;&nbsp;&nbsp;&nbsp;status               tinyint not null default 1 comment '状态，0：不可用 1：可用',  
   &nbsp;&nbsp;&nbsp;&nbsp;spu_id               varchar(11) not null default '0' comment '商品spuID',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id),  
   &nbsp;&nbsp;&nbsp;&nbsp;key AK_unique_name (name)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  
create index idx_spu_id on product_sku  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;spu_id  
);  

9.订单表  
订单id前前11位冗余了用户id，是为了充分利用聚簇索引范围查询的特点来解决根据用户id查询历史订单的需求。  
create table order_info  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;order_no             varchar(28) not null default '' comment '主键，前11位是用户ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;user_id              varchar(11) not null default '' comment '用户ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;user_addr_id         varchar(15) not null default '' comment '地址ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;pay_channel          tinyint not null default 0 comment '支付方式',  
   &nbsp;&nbsp;&nbsp;&nbsp;total_num            smallint not null default 0 comment '总数目',  
   &nbsp;&nbsp;&nbsp;&nbsp;total_amount         int not null default 0 comment '总金额，单位分',  
   &nbsp;&nbsp;&nbsp;&nbsp;express_price        int not null default 0 comment '运费，单位分',  
   &nbsp;&nbsp;&nbsp;&nbsp;status               tinyint not null default 1 comment '支付状态，1：未支付 2：支付中 3：支付完成 4：关闭',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (order_no)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  
create index idx_addr_id on order_info  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;user_addr_id  
);  

10.订单详情表  
create table order_detail  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;id                   varchar(32) not null default '' comment '主键',  
   &nbsp;&nbsp;&nbsp;&nbsp;order_id             varchar(28) not null default '' comment '归属订单ID',  
   &nbsp;&nbsp;&nbsp;&nbsp;product_sku_id       varchar(15) not null default '' comment '商品skuID',  
   &nbsp;&nbsp;&nbsp;&nbsp;number               smallint not null default 0 comment '商品数量',  
   &nbsp;&nbsp;&nbsp;&nbsp;price                int not null default 0 comment '商品当前单价，单位分',  
   &nbsp;&nbsp;&nbsp;&nbsp;create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   &nbsp;&nbsp;&nbsp;&nbsp;primary key (id)  
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;  
create index idx_order_no on order_detail  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;order_id  
);  
create index idx_sku_id on order_detail  
(  
   &nbsp;&nbsp;&nbsp;&nbsp;product_sku_id  
);  

