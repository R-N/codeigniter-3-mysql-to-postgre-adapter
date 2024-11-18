# CodeIgniter 3 MySQL to PostgreSQL adapter

Adapts your codeigniter 3 project that was built on mysql to postgresql

## Usage

1. Migrate your database to postgresql (I recommend to use pgloader)
2. Copy `applications` folder onto yours
3. Add `"postgre"` to your helpers autoload (in `applications/config/autoload.php`)
4. Modify your database configuration to use postgres (in `applications/config/database.php`). Example:
```
$db['default'] = array(
    'dsn'   => '',
    'hostname' => 'localhost',
    'port'     => 5432,
    'username' => 'postgres',
    'password' => '',
    'database' => 'postgres',
    'schema'   => 'public',
    'dbdriver' => 'postgre',
    'sslmode' => 'allow',
    ...
);
```
5. PostgreSQL converts names to lowercase unless you use double quotes, so modify your code to access database values in lowercase. If you want compatibility with mysql, convert the mysql database to lowercase too. The names include database name, schema name, table and view name, column name. 

## Migration

You can use the migrate.lisp with pgloader. 
1. Modify it to use your datbase URL
2. Run `pgloader migrate.lisp`
3. Views, triggers, procedures, and triggers don't get migrated. Recreate them through SQL

## pgloader for windows

They don't ship pgloader binaries for Windows. Either you build them yourself, or use the linux one through WSL. Binaries built for windows are tied to your machine, idk if it's the architecture or whatever, but the one I found on the internet didn't work for me. 

1. Install cygwin64, choose the following packages:
![image](https://github.com/user-attachments/assets/61becd96-3573-47b5-8b03-8aaf067b8015)
2. Download [CCL](https://ccl.clozure.com/). Extract it somewhere. Preferably close to where you would clone the pgloader project.
3. Download [dblib_0.95.zip](https://github.com/dimitri/pgloader/files/3437567/dblib_0.95.zip). Copy all the files to cygwin64/bin.
4. Duplicate dblib.dll and rename it to sybdb.dll 
5. Download [sqlite3.dll](https://www.sqlite.org/download.html). Copy it to cygwin64/bin.
6. Clone the pgloader repo `git clone https://github.com/dimitri/pgloader.git`
7. Open cygwin64 terminal.
8. Change directory to the pgloader project directory.
9. Compile pgloader. `[your ccl.exe location] --no-init --load ./src/save.lisp`.
10. In build/bin, there will be a pgloader file . Rename it to pgloader.exe
11. run pgloader.exe, when it popped out the error, type `:GO`

[Reference thread for building pgloader](https://github.com/dimitri/pgloader/issues/652#issuecomment-2475414471)
