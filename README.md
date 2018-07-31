# Credit-card-management-system-Hadoop-Hive
This project use HIVE to create tables using the data in HDFS.


-------------
Author : Peter Li

In this project, I used Hive:

1.Create a staging table, load the data from HDFS directory( where sqoop imports to),

  The staging table is an external table which locates at '/user/maria_dev/CREDIT_CARD_HIVE_TABLES/CDW_SAPP_BRANCH_STATIC'
  (Taking Branch as an example)
  
  when you running query in Hive, run the first part, take Branch table as example:
                                    
                                   
                                  create external table CDW_SAPP_D_BRANCH_static
                                  (BRANCH_CODE string, 
                                   BRANCH_NAME string,
                                   BRANCH_STREET string,
                                   BRANCH_CITY string,
                                   BRANCH_STATE string,
                                   BRANCH_ZIP string,
                                   BRANCH_PHONE string,
                                   LAST_UPDATED timestamp)
                                   ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
                                   ESCAPED BY '"' LINES TERMINATED BY '\n'
                                   location '/user/maria_dev/CREDIT_CARD_HIVE_TABLES/CDW_SAPP_BRANCH_STATIC';

                                   load data inpath '/user/maria_dev/CREDIT_CARD_SYSTEM/CDW_SAPP_BRANCH/'
                                   overwrite into table CDW_SAPP_D_BRANCH_static;  
  
2. Then create a final table partitioned by specific column, load the data from the staging table.
    
   The final table is located at '/user/maria_dev/CREDIT_CARD_HIVE_TABLES/CDW_SAPP_BRANCH'(Taking Branch as an exmaple);
                                   
                                   
                                   SET hive.exec.dynamic.partition=true;
                                   SET hive.exec.dynamic.partition.mode=nonstrict;

                                   create table CDW_SAPP_D_BRANCH
                                   (BRANCH_CODE int, 
                                   BRANCH_NAME string,
                                   BRANCH_STREET string,
                                   BRANCH_CITY string,
                                   BRANCH_ZIP int,
                                   BRANCH_PHONE string,
                                   LAST_UPDATED timestamp)
                                   partitioned by (BRANCH_STATE string)
                                   ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
                                   ESCAPED BY '"' LINES TERMINATED BY '\n'
                                   stored as textfile
                                   location '/user/maria_dev/CREDIT_CARD_HIVE_TABLES/CDW_SAPP_BRANCH';

                                   insert overwrite table CDW_SAPP_D_BRANCH
                                   partition(BRANCH_STATE)
                                   SELECT BRANCH_CODE,BRANCH_NAME, BRANCH_STREET, BRANCH_CITY, BRANCH_ZIP, BRANCH_PHONE, LAST_UPDATED,                           BRANCH_STATE
                                   FROM CDW_SAPP_D_BRANCH_static;
                                   
3. After executing the query in hive, you are able to see two table in Hive view, CDW_SAPP_D_BRANCH_static and CDW_SAPP_D_BRANCH in default database.

CDW_SAPP_D_BRANCH_static is the staging table and CDW_SAPP_D_BRANCH is the final table.

These steps work the same for other three tables.

