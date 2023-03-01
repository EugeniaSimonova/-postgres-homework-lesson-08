# MVCC, vacuum и autovacuum

создать GCE инстанс типа e2-medium и диском 10GB
установить на него PostgreSQL 14 с дефолтными настройками
применить параметры настройки PostgreSQL из прикрепленного к материалам занятия файла
В файле /etc/postgresql/14/main/postgresql.conf указала настройки:

```
max_connections = 40
shared_buffers = 1GB
effective_cache_size = 3GB
maintenance_work_mem = 512MB
checkpoint_completion_target = 0.9
wal_buffers = 16MB
default_statistics_target = 500
random_page_cost = 4
effective_io_concurrency = 2
work_mem = 6553kB
min_wal_size = 4GB
max_wal_size = 16GB
```

выполнить pgbench -i postgres
запустить pgbench -c8 -P 60 -T 600 -U postgres postgres
дать отработать до конца
дальше настроить autovacuum максимально эффективно

Я использовала настройки из презентации

```
log_autovacuum_min_duration = 0
autovacuum_max_workers = 10
autovacuum_naptime = 15s
autovacuum_vacuum_threshold = 25
autovacuum_vacuum_scale_factor = 0.05
autovacuum_vacuum_cost_delay = 10
autovacuum_vacuum_cost_limit = 1000
```
Изменяла значения этих параметров, но наиболее ровное значение tps получилось с настройками из презентации. Также пробовала выставлять autovacuum_max_workers равным (tps2 autovacuum_max_workers=2) и меньше (tps1 autovacuum_max_workers=1) числу CPU
виртуалки, как рекомендовал преподаватель.

построить график по получившимся значениям
так чтобы получить максимально ровное значение tps

![diagram](/image1.png)

