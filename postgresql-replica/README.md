# PostgreSQL replica

This is an example for configuring two PosgreSQL server instances for data replication

## How to run

For implementing this configuration:

1. Execute the command `docker compose up -d`

2. Stop the **main** container

3. Copy the files from **conf/main** folder into **main** container path **/var/lib/posgresql/data**

   ```
   docker cp conf/main/pg_hba.conf main:/var/lib/postgresql/data
   docker cp conf/main/postgresql.conf main:/var/lib/postgresql/data
   ```

4. Start **main** container

5. Execute next commands in **replica** container

   ```
   rm -rf /var/lib/postgresql/data/* && pg_basebackup -U postgres -R -D /var/lib/postgresql/data/ --host=172.30.0.2 --port=5432

   ```

6. Stop the **replica** container

7. Copy the files from **conf/replica** folder into **replica** container path **/var/lib/posgresql/data**

   ```
   docker cp conf/replica/postgresql.conf replica:/var/lib/postgresql/data
   ```

8. Start the **replica** container
