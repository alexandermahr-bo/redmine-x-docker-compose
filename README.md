# docker-compose setup for redmine vanilla

Run the setup via:

    docker-compose up --build

logs of run found at [docker-compose-redmine-x-empty-database-dump.html](docker-compose-redmine-x-empty-database-dump.html) (best to be downloaded and viewed in browser)

used database dump was [./database-setup/db.sql.gz](./database-setup/db.sql.gz)

output also here inline (yet not colorized)
    
    
    [?2004h[alex@bobox redmine-x.docker-compose]$ find | grep -v .git
    [?2004l
    .
    ./volume
    ./docker-compose.yml
    ./database-setup
    ./database-setup/entrypoint.sh
    ./database-setup/db.sql.gz
    ./database-setup/Dockerfile
    ./database-setup/db.sql.only.schema.ourdb.gz
    [?2004h[alex@bobox redmine-x.docker-compose]$ grep -I . * database-setup/*
    [?2004l
    grep: database-setup: Is a directory
    docker-compose.yml:version: "2.4"
    docker-compose.yml:services:
    docker-compose.yml:  redmine-x:
    docker-compose.yml:    image: registry.berlinonline.net/redmine_x/dockerimage:latest
    docker-compose.yml:    depends_on:
    docker-compose.yml:      database-setup:
    docker-compose.yml:        condition: service_completed_successfully
    docker-compose.yml:    environment:
    docker-compose.yml:      MYSQL_USER: &MYSQL_USER projectino
    docker-compose.yml:      MYSQL_HOST: &MYSQL_HOST percona
    docker-compose.yml:      MYSQL_PASSWORD: &MYSQL_PASSWORD 'something else'
    docker-compose.yml:      MYSQL_DATABASE: &MYSQL_DATABASE projectino
    docker-compose.yml:    ports:
    docker-compose.yml:      - "8080:9000"
    docker-compose.yml:  database-setup:
    docker-compose.yml:    pull_policy: build
    docker-compose.yml:    build: database-setup
    docker-compose.yml:    environment:
    docker-compose.yml:      MYSQL_USER: *MYSQL_USER
    docker-compose.yml:      MYSQL_HOST: *MYSQL_HOST 
    docker-compose.yml:      MYSQL_PASSWORD: *MYSQL_PASSWORD
    docker-compose.yml:      MYSQL_DATABASE: *MYSQL_DATABASE
    docker-compose.yml:      MYSQL_RANDOM_ROOT_PASSWORD: "1"
    docker-compose.yml:    depends_on: 
    docker-compose.yml:      - percona 
    docker-compose.yml:    
    docker-compose.yml:  percona:
    docker-compose.yml:    environment:
    docker-compose.yml:      MYSQL_USER: *MYSQL_USER
    docker-compose.yml:      MYSQL_HOST: *MYSQL_HOST 
    docker-compose.yml:      MYSQL_PASSWORD: *MYSQL_PASSWORD
    docker-compose.yml:      MYSQL_DATABASE: *MYSQL_DATABASE
    docker-compose.yml:      MYSQL_RANDOM_ROOT_PASSWORD: "1"
    docker-compose.yml:    image: percona:5.7
    docker-compose.yml:    ports: 
    docker-compose.yml:      - "3306:3306"
    grep: volume: Is a directory
    database-setup/Dockerfile:From alpine
    database-setup/Dockerfile:RUN apk update
    database-setup/Dockerfile:RUN apk add mariadb-client
    database-setup/Dockerfile:RUN echo hallo
    database-setup/Dockerfile:COPY db.sql.gz /
    database-setup/Dockerfile:RUN echo "build 2023.02.01"
    database-setup/Dockerfile:COPY entrypoint.sh /
    database-setup/Dockerfile:RUN apk add vim
    database-setup/Dockerfile:CMD cat /entrypoint.sh; sh /entrypoint.sh 
    database-setup/entrypoint.sh:#!/bin/sh
    database-setup/entrypoint.sh:set -xe
    database-setup/entrypoint.sh:while ! ( echo "SELECT true;" |  mysql -h "$MYSQL_HOST" -p"$MYSQL_PASSWORD" -u"$MYSQL_USER" "$MYSQL_DATABASE")
    database-setup/entrypoint.sh:do
    database-setup/entrypoint.sh:  sleep 1;
    database-setup/entrypoint.sh:  echo "wating for service '$MYSQL_HOST' to become ready"
    database-setup/entrypoint.sh:done
    database-setup/entrypoint.sh:echo "now adding the database"
    database-setup/entrypoint.sh:zcat /db.sql.gz | mysql -h "$MYSQL_HOST" -p"$MYSQL_PASSWORD" -u"$MYSQL_USER" "$MYSQL_DATABASE" 
    database-setup/entrypoint.sh:echo "exit code of mysql import $? done"
    database-setup/entrypoint.sh:echo "output of current tables in database '$MYSQL_DATABASE'"
    
    Starting redmine-xdocker-compose_percona_1 ... done
    
    Starting redmine-xdocker-compose_database-setup_1 ... done
    
    Recreating redmine-xdocker-compose_redmine-x_1    ... done
    Attaching to redmine-xdocker-compose_percona_1, redmine-xdocker-compose_database-setup_1, redmine-xdocker-compose_redmine-x_1
    database-setup_1  | #!/bin/sh
    database-setup_1  | 
    database-setup_1  | set -xe
    database-setup_1  | 
    database-setup_1  | while ! ( echo "SELECT true;" |  mysql -h "$MYSQL_HOST" -p"$MYSQL_PASSWORD" -u"$MYSQL_USER" "$MYSQL_DATABASE")
    database-setup_1  | do
    database-setup_1  |   sleep 1;
    database-setup_1  |   echo "wating for service '$MYSQL_HOST' to become ready"
    database-setup_1  | done
    database-setup_1  | 
    database-setup_1  | echo "now adding the database"
    database-setup_1  | zcat /db.sql.gz | mysql -h "$MYSQL_HOST" -p"$MYSQL_PASSWORD" -u"$MYSQL_USER" "$MYSQL_DATABASE" 
    database-setup_1  | echo "exit code of mysql import $? done"
    database-setup_1  | echo "output of current tables in database '$MYSQL_DATABASE'"
    database-setup_1  | mysqldump -h "$MYSQL_HOST" -p"$MYSQL_PASSWORD" -u"$MYSQL_USER" "$MYSQL_DATABASE"  | grep CREATE 
    percona_1         | 2023-02-01T23:30:47.449986Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
    percona_1         | 2023-02-01T23:30:47.450866Z 0 [Note] mysqld (mysqld 5.7.35-38) starting as process 1 ...
    database-setup_1  | + echo 'SELECT true;'
    percona_1         | 2023-02-01T23:30:47.452762Z 0 [Note] InnoDB: PUNCH HOLE support available
    percona_1         | 2023-02-01T23:30:47.452774Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
    percona_1         | 2023-02-01T23:30:47.452777Z 0 [Note] InnoDB: Uses event mutexes
    percona_1         | 2023-02-01T23:30:47.452779Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
    percona_1         | 2023-02-01T23:30:47.452781Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
    percona_1         | 2023-02-01T23:30:47.452783Z 0 [Note] InnoDB: Using Linux native AIO
    database-setup_1  | + mysql -h percona '-psomething else' -uprojectino projectino
    database-setup_1  | TRUE
    percona_1         | 2023-02-01T23:30:47.452937Z 0 [Note] InnoDB: Number of pools: 1
    database-setup_1  | 1
    percona_1         | 2023-02-01T23:30:47.453014Z 0 [Note] InnoDB: Using CPU crc32 instructions
    database-setup_1  | + echo 'now adding the database'
    database-setup_1  | now adding the database
    percona_1         | 2023-02-01T23:30:47.453848Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
    database-setup_1  | + zcat /db.sql.gz
    database-setup_1  | + mysql -h percona '-psomething else' -uprojectino projectino
    database-setup_1  | exit code of mysql import 0 done
    database-setup_1  | output of current tables in database 'projectino'
    percona_1         | 2023-02-01T23:30:47.456107Z 0 [Note] InnoDB: Completed initialization of buffer pool
    database-setup_1  | + echo 'exit code of mysql import 0 done'
    database-setup_1  | + echo 'output of current tables in database '"'"'projectino'"'"
    database-setup_1  | + mysqldump -h percona '-psomething else' -uprojectino projectino
    database-setup_1  | + grep CREATE
    database-setup_1  | mysqldump: Error: 'Access denied; you need (at least one of) the PROCESS privilege(s) for this operation' when trying to dump tablespaces
    percona_1         | 2023-02-01T23:30:47.457217Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
    database-setup_1  | CREATE TABLE `ar_internal_metadata` (
    database-setup_1  | CREATE TABLE `attachments` (
    database-setup_1  | CREATE TABLE `auth_sources` (
    database-setup_1  | CREATE TABLE `boards` (
    database-setup_1  | CREATE TABLE `changes` (
    database-setup_1  | CREATE TABLE `changeset_parents` (
    database-setup_1  | CREATE TABLE `changesets` (
    database-setup_1  | CREATE TABLE `changesets_issues` (
    database-setup_1  | CREATE TABLE `comments` (
    database-setup_1  | CREATE TABLE `custom_field_enumerations` (
    database-setup_1  | CREATE TABLE `custom_fields` (
    database-setup_1  | CREATE TABLE `custom_fields_projects` (
    database-setup_1  | CREATE TABLE `custom_fields_roles` (
    database-setup_1  | CREATE TABLE `custom_fields_trackers` (
    database-setup_1  | CREATE TABLE `custom_values` (
    database-setup_1  | CREATE TABLE `documents` (
    database-setup_1  | CREATE TABLE `easy_entity_assignments` (
    database-setup_1  | CREATE TABLE `easy_settings` (
    database-setup_1  | CREATE TABLE `email_addresses` (
    database-setup_1  | CREATE TABLE `enabled_modules` (
    database-setup_1  | CREATE TABLE `enumerations` (
    database-setup_1  | CREATE TABLE `groups_users` (
    database-setup_1  | CREATE TABLE `import_items` (
    database-setup_1  | CREATE TABLE `imports` (
    database-setup_1  | CREATE TABLE `issue_categories` (
    database-setup_1  | CREATE TABLE `issue_relations` (
    database-setup_1  | CREATE TABLE `issue_statuses` (
    database-setup_1  | CREATE TABLE `issues` (
    database-setup_1  | CREATE TABLE `journal_details` (
    database-setup_1  | CREATE TABLE `journals` (
    database-setup_1  | CREATE TABLE `member_roles` (
    database-setup_1  | CREATE TABLE `members` (
    database-setup_1  | CREATE TABLE `messages` (
    database-setup_1  | CREATE TABLE `news` (
    percona_1         | 2023-02-01T23:30:47.468839Z 0 [Note] InnoDB: Crash recovery did not find the parallel doublewrite buffer at /var/lib/mysql/xb_doublewrite
    database-setup_1  | CREATE TABLE `projects` (
    database-setup_1  | CREATE TABLE `projects_trackers` (
    database-setup_1  | CREATE TABLE `queries` (
    database-setup_1  | CREATE TABLE `queries_roles` (
    database-setup_1  | CREATE TABLE `repositories` (
    database-setup_1  | CREATE TABLE `roles` (
    database-setup_1  | CREATE TABLE `roles_managed_roles` (
    database-setup_1  | CREATE TABLE `schema_migrations` (
    database-setup_1  | CREATE TABLE `settings` (
    database-setup_1  | CREATE TABLE `time_entries` (
    database-setup_1  | CREATE TABLE `tokens` (
    database-setup_1  | CREATE TABLE `trackers` (
    database-setup_1  | CREATE TABLE `user_preferences` (
    database-setup_1  | CREATE TABLE `users` (
    database-setup_1  | CREATE TABLE `versions` (
    database-setup_1  | CREATE TABLE `watchers` (
    database-setup_1  | CREATE TABLE `wiki_content_versions` (
    database-setup_1  | CREATE TABLE `wiki_contents` (
    database-setup_1  | CREATE TABLE `wiki_pages` (
    database-setup_1  | CREATE TABLE `wiki_redirects` (
    database-setup_1  | CREATE TABLE `wikis` (
    database-setup_1  | CREATE TABLE `workflows` (
    percona_1         | 2023-02-01T23:30:47.469221Z 0 [Note] InnoDB: Highest supported file format is Barracuda.
    percona_1         | 2023-02-01T23:30:47.475980Z 0 [Note] InnoDB: Created parallel doublewrite buffer at /var/lib/mysql/xb_doublewrite, size 3932160 bytes
    percona_1         | 2023-02-01T23:30:47.480038Z 0 [Note] InnoDB: Creating shared tablespace for temporary tables
    percona_1         | 2023-02-01T23:30:47.480071Z 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
    percona_1         | 2023-02-01T23:30:47.493335Z 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
    redmine-xdocker-compose_database-setup_1 exited with code 0
    percona_1         | 2023-02-01T23:30:47.493789Z 0 [Note] InnoDB: 96 redo rollback segment(s) found. 96 redo rollback segment(s) are active.
    percona_1         | 2023-02-01T23:30:47.493795Z 0 [Note] InnoDB: 32 non-redo rollback segment(s) are active.
    percona_1         | 2023-02-01T23:30:47.494087Z 0 [Note] InnoDB: Percona XtraDB (http://www.percona.com) 5.7.35-38 started; log sequence number 13031173
    percona_1         | 2023-02-01T23:30:47.494232Z 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
    percona_1         | 2023-02-01T23:30:47.494431Z 0 [Note] Plugin 'FEDERATED' is disabled.
    percona_1         | 2023-02-01T23:30:47.496548Z 0 [Note] InnoDB: Buffer pool(s) load completed at 230201 23:30:47
    percona_1         | 2023-02-01T23:30:47.497777Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
    percona_1         | 2023-02-01T23:30:47.497787Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
    percona_1         | 2023-02-01T23:30:47.497791Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
    percona_1         | 2023-02-01T23:30:47.497792Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
    percona_1         | 2023-02-01T23:30:47.498194Z 0 [Warning] CA certificate ca.pem is self signed.
    percona_1         | 2023-02-01T23:30:47.498214Z 0 [Note] Skipping generation of RSA key pair as key files are present in data directory.
    percona_1         | 2023-02-01T23:30:47.498458Z 0 [Note] Server hostname (bind-address): '*'; port: 3306
    percona_1         | 2023-02-01T23:30:47.498478Z 0 [Note] IPv6 is available.
    percona_1         | 2023-02-01T23:30:47.498483Z 0 [Note]   - '::' resolves to '::';
    percona_1         | 2023-02-01T23:30:47.498494Z 0 [Note] Server socket created on IP: '::'.
    percona_1         | 2023-02-01T23:30:47.504254Z 0 [Note] Event Scheduler: Loaded 0 events
    percona_1         | 2023-02-01T23:30:47.504398Z 0 [Note] mysqld: ready for connections.
    percona_1         | Version: '5.7.35-38'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  Percona Server (GPL), Release 38, Revision 3692a61
    redmine-x_1       | Your Gemfile lists the gem puma (~> 3.7) more than once.
    redmine-x_1       | You should probably keep only one of them.
    redmine-x_1       | Remove any duplicate entries and specify the gem only once.
    redmine-x_1       | While it's not a problem now, it could cause errors if you change the version of one of them later.
    redmine-x_1       | Your Gemfile lists the gem redmine_crm (>= 0) more than once.
    redmine-x_1       | You should probably keep only one of them.
    redmine-x_1       | Remove any duplicate entries and specify the gem only once.
    redmine-x_1       | While it's not a problem now, it could cause errors if you change the version of one of them later.
    redmine-x_1       | The Gemfile's dependencies are satisfied
    redmine-x_1       | /data/projectino/public_html/plugins/redmine_ckeditor/init.rb:27: warning: constant Loofah::HTML5::WhiteList is deprecated
    redmine-x_1       | == 20150705172511 CreateEasySettings: migrating ===============================
    redmine-x_1       | -- table_exists?(:easy_settings)
    redmine-x_1       |    -> 0.0003s
    redmine-x_1       | -- column_exists?(:easy_settings, :type)
    redmine-x_1       |    -> 0.0006s
    redmine-x_1       | -- index_exists?(:easy_settings, [:name, :project_id], {:unique=>true, :name=>"index_easy_settings_on_name_and_project_id"})
    redmine-x_1       |    -> 0.0003s
    redmine-x_1       | == 20150705172511 CreateEasySettings: migrated (0.0012s) ======================
    redmine-x_1       | 
    redmine-x_1       | == 20160519161300 CreateEntityAssignments: migrating ==========================
    redmine-x_1       | -- table_exists?(:easy_entity_assignments)
    redmine-x_1       |    -> 0.0004s
    redmine-x_1       | == 20160519161300 CreateEntityAssignments: migrated (0.0005s) =================
    redmine-x_1       | 
    redmine-x_1       | == 20190206121100 RemoveForeignKeyFromEasySettings: migrating =================
    redmine-x_1       | -- remove_foreign_key(:easy_settings, :projects)
    redmine-x_1       | == 20190206121100 RemoveForeignKeyFromEasySettings: migrated (0.0004s) ========
    redmine-x_1       | 
    redmine-x_1       | [1] Puma starting in cluster mode...
    redmine-x_1       | [1] * Version 3.12.6 (ruby 2.6.5-p114), codename: Llamas in Pajamas
    redmine-x_1       | [1] * Min threads: 1, max threads: 2
    redmine-x_1       | [1] * Environment: production
    redmine-x_1       | [1] * Process workers: 2
    redmine-x_1       | [1] * Preloading application
    redmine-x_1       | /data/projectino/public_html/plugins/redmine_ckeditor/init.rb:27: warning: constant Loofah::HTML5::WhiteList is deprecated
    redmine-x_1       | [1] ! Unable to load application: ActiveRecord::StatementInvalid: Mysql2::Error: Table 'projectino.notes' doesn't exist: SHOW FULL FIELDS FROM `notes`
    redmine-x_1       | bundler: failed to load command: puma (/usr/local/bundle/bin/puma)
    redmine-x_1       | /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:148:in `_query': Mysql2::Error: Table 'projectino.notes' doesn't exist: SHOW FULL FIELDS FROM `notes` (ActiveRecord::StatementInvalid)
    redmine-x_1       | 	from /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:148:in `block in query'
    redmine-x_1       | 	from /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:147:in `handle_interrupt'
    redmine-x_1       | 	from /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:147:in `query'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:187:in `block (2 levels) in execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:48:in `block in permit_concurrent_loads'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/concurrency/share_lock.rb:187:in `yield_shares'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:47:in `permit_concurrent_loads'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:186:in `block in execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_adapter.rb:581:in `block (2 levels) in log'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/monitor.rb:235:in `mon_synchronize'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_adapter.rb:580:in `block in log'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/notifications/instrumenter.rb:23:in `instrument'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_adapter.rb:571:in `log'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:185:in `execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/mysql/database_statements.rb:28:in `execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:196:in `execute_and_free'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:793:in `column_definitions'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract/schema_statements.rb:113:in `columns'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/schema_cache.rb:69:in `columns'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/schema_cache.rb:75:in `columns_hash'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:466:in `load_schema!'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/attributes.rb:234:in `load_schema!'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/attribute_decorators.rb:51:in `load_schema!'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:459:in `block in load_schema'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/monitor.rb:235:in `mon_synchronize'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:456:in `load_schema'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:336:in `columns_hash'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/inheritance.rb:78:in `descends_from_active_record?'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/inheritance.rb:84:in `finder_needs_type_condition?'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/core.rb:287:in `relation'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/scoping/named.rb:50:in `default_scoped'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/scoping/named.rb:36:in `all'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/querying.rb:12:in `joins'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal_note.rb:30:in `<class:DealNote>'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal_note.rb:20:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `block in require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:257:in `load_dependency'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:378:in `block in require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `block in load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:14:in `block in loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/concurrency/share_lock.rb:151:in `exclusive'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:13:in `loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:356:in `require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:510:in `load_missing_constant'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:195:in `const_missing'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:542:in `load_missing_constant'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:195:in `const_missing'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal.rb:101:in `<class:Deal>'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal.rb:20:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `block in require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:257:in `load_dependency'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:378:in `block in require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `block in load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:14:in `block in loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/concurrency/share_lock.rb:151:in `exclusive'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:13:in `loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:356:in `require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:334:in `depend_on'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:246:in `require_dependency'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:478:in `block (2 levels) in eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:477:in `each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:477:in `block in eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:475:in `each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:475:in `eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:356:in `eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/application/finisher.rb:69:in `each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/application/finisher.rb:69:in `block in <module:Finisher>'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:32:in `instance_exec'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:32:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:61:in `block in run_initializers'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:228:in `block in tsort_each'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:350:in `block (2 levels) in each_strongly_connected_component'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:431:in `each_strongly_connected_component_from'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:349:in `block in each_strongly_connected_component'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:347:in `each'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:347:in `call'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:347:in `each_strongly_connected_component'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:226:in `tsort_each'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:205:in `tsort_each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:60:in `run_initializers'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/application.rb:361:in `initialize!'
    redmine-x_1       | 	from /data/projectino/public_html/config/environment.rb:16:in `<top (required)>'
    redmine-x_1       | 	from config.ru:3:in `require'
    redmine-x_1       | 	from config.ru:3:in `block in <main>'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:116:in `eval'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:116:in `new_from_string'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:105:in `load_file'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:66:in `parse_file'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/configuration.rb:320:in `load_rackup'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/configuration.rb:245:in `app'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/runner.rb:147:in `load_and_bind'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/cluster.rb:412:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/launcher.rb:186:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/cli.rb:80:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/bin/puma:10:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/bin/puma:23:in `load'
    redmine-x_1       | 	from /usr/local/bundle/bin/puma:23:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli/exec.rb:58:in `load'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli/exec.rb:58:in `kernel_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli/exec.rb:23:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli.rb:486:in `exec'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor/invocation.rb:127:in `invoke_command'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor.rb:392:in `dispatch'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli.rb:31:in `dispatch'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor/base.rb:485:in `start'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli.rb:25:in `start'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/exe/bundle:48:in `block in <top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/friendly_errors.rb:120:in `with_friendly_errors'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/exe/bundle:36:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/bin/bundle:23:in `load'
    redmine-x_1       | 	from /usr/local/bundle/bin/bundle:23:in `<main>'
    redmine-x_1       | /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:148:in `_query': Table 'projectino.notes' doesn't exist (Mysql2::Error)
    redmine-x_1       | 	from /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:148:in `block in query'
    redmine-x_1       | 	from /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:147:in `handle_interrupt'
    redmine-x_1       | 	from /usr/local/bundle/gems/mysql2-0.5.4/lib/mysql2/client.rb:147:in `query'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:187:in `block (2 levels) in execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:48:in `block in permit_concurrent_loads'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/concurrency/share_lock.rb:187:in `yield_shares'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:47:in `permit_concurrent_loads'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:186:in `block in execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_adapter.rb:581:in `block (2 levels) in log'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/monitor.rb:235:in `mon_synchronize'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_adapter.rb:580:in `block in log'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/notifications/instrumenter.rb:23:in `instrument'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_adapter.rb:571:in `log'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:185:in `execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/mysql/database_statements.rb:28:in `execute'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:196:in `execute_and_free'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract_mysql_adapter.rb:793:in `column_definitions'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/abstract/schema_statements.rb:113:in `columns'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/schema_cache.rb:69:in `columns'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/connection_adapters/schema_cache.rb:75:in `columns_hash'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:466:in `load_schema!'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/attributes.rb:234:in `load_schema!'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/attribute_decorators.rb:51:in `load_schema!'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:459:in `block in load_schema'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/monitor.rb:235:in `mon_synchronize'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:456:in `load_schema'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/model_schema.rb:336:in `columns_hash'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/inheritance.rb:78:in `descends_from_active_record?'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/inheritance.rb:84:in `finder_needs_type_condition?'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/core.rb:287:in `relation'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/scoping/named.rb:50:in `default_scoped'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/scoping/named.rb:36:in `all'
    redmine-x_1       | 	from /usr/local/bundle/gems/activerecord-5.2.4.2/lib/active_record/querying.rb:12:in `joins'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal_note.rb:30:in `<class:DealNote>'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal_note.rb:20:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `block in require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:257:in `load_dependency'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:378:in `block in require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `block in load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:14:in `block in loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/concurrency/share_lock.rb:151:in `exclusive'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:13:in `loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:356:in `require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:510:in `load_missing_constant'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:195:in `const_missing'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:542:in `load_missing_constant'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:195:in `const_missing'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal.rb:101:in `<class:Deal>'
    redmine-x_1       | 	from /data/projectino/public_html/plugins/redmine_contacts/app/models/deal.rb:20:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/polyglot-0.3.5/lib/polyglot.rb:65:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `block in require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:257:in `load_dependency'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:291:in `require'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:378:in `block in require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `block in load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:14:in `block in loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/concurrency/share_lock.rb:151:in `exclusive'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies/interlock.rb:13:in `loading'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:37:in `load_interlock'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:356:in `require_or_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:334:in `depend_on'
    redmine-x_1       | 	from /usr/local/bundle/gems/activesupport-5.2.4.2/lib/active_support/dependencies.rb:246:in `require_dependency'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:478:in `block (2 levels) in eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:477:in `each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:477:in `block in eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:475:in `each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:475:in `eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/engine.rb:356:in `eager_load!'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/application/finisher.rb:69:in `each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/application/finisher.rb:69:in `block in <module:Finisher>'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:32:in `instance_exec'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:32:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:61:in `block in run_initializers'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:228:in `block in tsort_each'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:350:in `block (2 levels) in each_strongly_connected_component'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:431:in `each_strongly_connected_component_from'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:349:in `block in each_strongly_connected_component'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:347:in `each'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:347:in `call'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:347:in `each_strongly_connected_component'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:226:in `tsort_each'
    redmine-x_1       | 	from /usr/local/lib/ruby/2.6.0/tsort.rb:205:in `tsort_each'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/initializable.rb:60:in `run_initializers'
    redmine-x_1       | 	from /usr/local/bundle/gems/railties-5.2.4.2/lib/rails/application.rb:361:in `initialize!'
    redmine-x_1       | 	from /data/projectino/public_html/config/environment.rb:16:in `<top (required)>'
    redmine-x_1       | 	from config.ru:3:in `require'
    redmine-x_1       | 	from config.ru:3:in `block in <main>'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:116:in `eval'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:116:in `new_from_string'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:105:in `load_file'
    redmine-x_1       | 	from /usr/local/bundle/gems/rack-2.2.4/lib/rack/builder.rb:66:in `parse_file'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/configuration.rb:320:in `load_rackup'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/configuration.rb:245:in `app'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/runner.rb:147:in `load_and_bind'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/cluster.rb:412:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/launcher.rb:186:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/lib/puma/cli.rb:80:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/puma-3.12.6/bin/puma:10:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/bin/puma:23:in `load'
    redmine-x_1       | 	from /usr/local/bundle/bin/puma:23:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli/exec.rb:58:in `load'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli/exec.rb:58:in `kernel_load'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli/exec.rb:23:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli.rb:486:in `exec'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor/invocation.rb:127:in `invoke_command'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor.rb:392:in `dispatch'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli.rb:31:in `dispatch'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/vendor/thor/lib/thor/base.rb:485:in `start'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/cli.rb:25:in `start'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/exe/bundle:48:in `block in <top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/lib/bundler/friendly_errors.rb:120:in `with_friendly_errors'
    redmine-x_1       | 	from /usr/local/bundle/gems/bundler-2.3.25/exe/bundle:36:in `<top (required)>'
    redmine-x_1       | 	from /usr/local/bundle/bin/bundle:23:in `load'
    redmine-x_1       | 	from /usr/local/bundle/bin/bundle:23:in `<main>'
    redmine-xdocker-compose_redmine-x_1 exited with code 1
    
    Killing redmine-xdocker-compose_percona_1         ... error
    
    ERROR: for redmine-xdocker-compose_percona_1  Cannot kill container: 2c1c5443267bd024262e66c72648e4f72a24304a93a911f4bb2aab5eb8a2f009: Container 2c1c5443267bd024262e66c72648e4f72a24304a93a911f4bb2aab5eb8a2f009 is not running
    ERROR: 2
    [?2004h[alex@bobox redmine-x.docker-compose]$ 
    (reverse-i-search)`': d': docker-compose up --build
    
    Removing redmine-xdocker-compose_database-setup_1 ... 
    Removing redmine-xdocker-compose_percona_1        ... 
    Removing redmine-xdocker-compose_redmine-x_1      ... done
    Removing network redmine-xdocker-compose_default
    [?2004h[alex@bobox redmine-x.docker-compose]$ docker-compose down
    [?2004l
    Removing network redmine-xdocker-compose_default
    WARNING: Network redmine-xdocker-compose_default not found.
    [?2004h[alex@bobox redmine-x.docker-compose]$ exit
    [?2004l
    exit
    
