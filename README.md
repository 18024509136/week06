# week06
1.用户表  
create table user_info  
(  
   id                   varchar(11) not null default '' comment '用户ID',  
   name                 varchar(50) not null default '' comment '用户名',  
   password             varchar(32) not null default '' comment '密码，md5加密存放',  
   email                varchar(50) not null default '' comment '邮箱',  
   auth_str             varchar(200) not null default '' comment '权限标识，如user:query,goods:query',  
   active_flag          tinyint not null default 1 comment '激活标识，0：未激活 1：激活',  
   create_time          datetime not null default CURRENT_TIMESTAMP comment '创建时间',  
   update_time          datetime not null default CURRENT_TIMESTAMP comment '修改时间',  
   primary key (id),  
   key AK_unique_name (name)  
);  
