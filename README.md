# Credit-card-management-system-Hadoop-Hive
This project use HIVE to create tables using the data in HDFS.


-------------
1. Four tables will be created at deafult database:

                              CDW_SAPP_D_BRANCH;
                              CDW_SAPP_D_CREDITCARD;
                              CDW_SAPP_D_CUSTOMER;
                              CDW_SAPP_D_TIME;
                              
2. A static table is built first to prepare for partition.

3. A dynamic table is then built, partioned by the static table.

4. Hive will remove the text files in HDFS when building the hive tables.
