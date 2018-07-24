# Credit-card-management-system-Hadoop-Hive
This project create tables using the data from HDFS.


-------------
1. Four tables will be created at deafult database:

                              CDW_SAPP_D_BRANCH;
                              CDW_SAPP_D_CREDITCARD;
                              CDW_SAPP_D_CUSTOMER;
                              CDW_SAPP_D_TIME;
                              
2. A static table is built first to prepare for partition.

3. A dynamic table is then built, partioned by the static table.
