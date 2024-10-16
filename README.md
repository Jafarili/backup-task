# Backup Taskfile
This is a Taskfile to backup files, directories, MySQL and MongoDB databases to a remote storage like Google Drive, GCS and S3.
It uses `7z` to compress the files and `rclone` to upload them to a remote storage.
It supports multiple backup profiles and can be easily extended.

## Installation
Install `Taskfile`, `7z` and `rclone` on your machine.
Create a profile for `rclone` and configure it to use a remote storage like Google Drive, GCS and S3.
Create a `.env.<profile>` file in the same directory as the `Taskfile` and add overrides for the environment variables defined in the `.env.dist`.
Run `PROFILE=<profile> task backup` to backup according to the profile.

## Configuration
The configuration is done using environment variables.
The following environment variables are supported:
- `BACKUP_WORK_DIRECTORY`: The directory to operate in. Default: `/tmp/backup`
- `COMPRESSION_7Z_PATH`: The path to the `7z` executable. Default: `/usr/bin/7z`
- `COMPRESSION_LEVEL`: The compression level to use. Default: `5`
- `COMPRESSION_PASSWORD`: The password to use for the compression. Default: ""
- `BACKUP_INCLUDE_DIRS`: The directories to include in the backup. Enter the directories separated by a comma. Example: `/var/www/dir1,/var/www/dir2`
- `BACKUP_EXCLUDE_DIRS`: The directories to exclude from the backup. Enter the directories separated by a comma. Example: `dir1/vendor,/var/www/dir2/sensitive/data`
- `MYSQL_USERNAME`: The username to use to connect to the MySQL database.
- `MYSQL_PASSWORD`: The password to use to connect to the MySQL database.
- `MYSQL_DATABASES`: The databases to backup. Enter the databases separated by a comma. Example: `db1,db2`
- `MONGODB_USERNAME`: The username to use to connect to the MongoDB database.
- `MONGODB_PASSWORD`: The password to use to connect to the MongoDB database.
- `MONGODB_AUTH_DB`: The authentication database to use.
- `MONGODB_DATABASES`: The databases to backup. Enter the databases separated by a comma. Example: `db1,db2`
- `RCLONE_PATH`: The path to the `rclone` executable. Default: `/usr/bin/rclone`
- `RCLONE_CONFIG_PATH`: The path to the `rclone` configuration file. Default: `~/.config/rclone/rclone.conf`
- `RCLONE_REMOTE`: The profile to use for `rclone`. Example: `gdrive`
- `RCLONE_DESTINATION`: The destination to use for `rclone`. Example: `/`
- `BACKUP_RETENTION`: The retention policy to use. Example: `15d`

## MySQL Backup
The MySQL backup is done using the `mysqldump` command.
Make sure to create a user with the necessary permissions to backup the databases.
```shell
CREATE USER 'backup'@'localhost' IDENTIFIED BY 'password';
GRANT SELECT ON 'db1'.* TO 'backup'@'localhost'; # You might need to grant more permissions depending on your use case
FLUSH PRIVILEGES;
```

## MongoDB Backup
The MongoDB backup is done using the `mongodump` command.
Make sure to create a user with the necessary permissions to backup the databases.
```shell
use db1;
db.createUser({user: "backup", pwd: "password", roles: ["read"]});
```

## Usage
Run `PROFILE=<profile> task backup` to backup your files.
Set cron jobs to run the backup at regular intervals.
```shell
0 0 * * * cd /root/backup && PROFILE=<profile> task backup
```

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments
- [Taskfile](https://taskfile.dev/)
- [7z](https://www.7-zip.org/)
- [rclone](https://rclone.org/)
- [MySQL](https://www.mysql.com/)
- [MongoDB](https://www.mongodb.com/)
