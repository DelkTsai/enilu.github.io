#mysql��������

    �汾��mysql5.6
    os:centos6.5
    ������������192.168.1.1�����ӷ�������(192.168.1.2)

##������������

- �޸�/etc/my.cnf�ļ� 
- ��[mysqld]�������ӣ�

        server-id=1
        log-bin=mysqlmaster-bin.log
        sync_binlog=1
        innodb_buffer_pool_size=8192M #��������Ϊ�ڴ��70%
        innodb_flush_log_at_trx_commit=1
        sql_mode=STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,NO_AUTO_VALUE_ON_ZERO
        lower_case_table_names=1
        log_bin_trust_function_creators=1
        read-only=0
        binlog-do-db=dbname #Ҫͬ�������ݿ⣬�������ö��
        binlog-do-db=dbname2

- ����mysql����

        sudo /etc/init.d/mysqld restart

## ���ôӷ�����

- �޸�/etc/my.cnf�ļ� 
 ��[mysqld]�������ӣ�

        server-id=2
        log-bin=mysqlslave-bin.log
        sync_binlog=1
        innodb_buffer_pool_size=8096M  #��������Ϊ�ڴ��70%
        innodb_flush_log_at_trx_commit=1
        sql_mode=STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,NO_AUTO_VALUE_ON_ZERO
        lower_case_table_names=1
        log_bin_trust_function_creators=1
- ����mysql����

        sudo /etc/init.d/mysqld restart
- �����ݿ������Ӵ����ݿ��˻�

��¼���ݿ⣬ִ�У� 

    mysql> GRANT REPLICATION SLAVE ON . to ��username��@��192.168.1.2�� identified by ��password��; 
    ������ֹ�ٲ��������Ի�ȡ�����ݿ�ĵĶ�������־���꣩�� 
    FLUSH TABLES WITH READ LOCK;

- ��ȡ�����ݿ��������־״̬��

        mysql> SHOW MASTER STATUS \G;
        *************************** 1. row ***************************
        File: mysqlmaster-bin.000001
        Position: 336
        Binlog_Do_DB: td
        Binlog_Ignore_DB: 
        Executed_Gtid_Set: 
        1 row in set (0.00 sec)
- �ڴ����ݿ�������е��������ݿ�����ݿ����ݣ�

        mysqldump -uroot -peasecredit2015 -h192.168.1.1  --all-databases  --triggers --routines --events >all.sql
-Ȼ���뵽 �����ݿ��Լ������ݿ��У�

        mysql -uroot -peasecredit2015 <all.sql
- �������ݿ����ø��Ƶ������ݿ���Ϣ��ע���޸�MASTER_LOG_FILE��MASTER_LOG_POS��ֵ��

        mysql> CHANGE MASTER TO MASTER_HOST='192.168.1.1',MASTER_USER='username',MASTER_PASSWORD='password',MASTER_LOG_FILE='mysqlmaster-bin.000001',MASTER_LOG_POS=336;
- �ڴӷ����������������ݿ�ĸ����߳�

        mysql> START slave;
- ���Ų�ѯ���ݿ��slave״̬��

        mysql> SHOW slave STATUS \G
- �������������������Yes����˵���������óɹ���

        Slave_IO_Running: Yes        
        Slave_SQL_Running: Yes
## ����

�������ݿ��ϴ���һ��test�����������ݣ��鿴�ӷ�����Ҳ�и��¼��ɡ�