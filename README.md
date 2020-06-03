# MonstaFtp

Ansible role to quickly setup MonstaFTP

# Requirements

A webserver (apache/nginx) with php(7.1+) support

# Role Variables

```yaml
monstaftp_version: 2.10.1
monstaftp_filename: monsta_ftp_{{ monstaftp_version }}_install.zip
monstaftp_file_src: https://www.monstaftp.com/downloads/{{ monstaftp_filename }}
monstaftp_configPathSettings: "settings.json"
monstaftp_configTimeZone: "UTC"
monstaftp_configTempDir: ""
monstaftp_configMaxFileSize: "1024M"
monstaftp_configChunkUploadSize: "default"
monstaftp_configMaxExecutionTimeSeconds: 1800
monstaftp_configSSHAgentAuthEnabled: false
monstaftp_configSSHKeyAuthEnabled: false
monstaftp_configPageTitle: "Monsta FTP"

monstaftp_configMaxLoginFailures: 3
monstaftp_configLoginFailuresResetTimeMinutes: 5

monstaftp_configMftpActionLogPath: 'null'
monstaftp_configMftpActionLogFunction: 'null'

monstaftp_configLogToSyslog: false
monstaftp_configMftpSyslogFacility: "LOG_USER"

monstaftp_configLogToFile: false
monstaftp_configMftpLogFilePath: 'null'
monstaftp_configMftpLogLevelThreshold: "LOG_WARNING"
monstaftp_configDisableLatestVersionCheck: false
```
* If you have a pro license

```yaml
monstaftp_configPathProfiles:
monstaftp_configPathLicense:
```

# Example Playbook

```yaml
---
- name: Setup Monsta Webftp Server
  hosts: monstaftp
  strategy: free
  become: true
  vars:
    php_default_version_debian: 7.2
    php_packages_state: latest
    php_packages_extra:
      - libapache2-mod-php7.2

    apache_remove_default_vhost: true
    apache_global_vhost_settings: |
      DirectoryIndex index.php
    apache_vhosts:
      - servername: "monstaftp.domain.com"
        serveralias: "monstaftp.domain.com"
        documentroot: "/usr/share/monstaftp/mftp"
        options: "FollowSymLinks"
        allow_override: "Limit FileInfo"
        extra_parameters: "Redirect / https://monstaftp.domain.com/"

    apache_vhosts_ssl:
      - servername: "monstaftp.domain.com"
        serveralias: "monstaftp.domain.com"
        documentroot: "/usr/share/monstaftp/mftp"
        certificate_file: "/etc/ssl/private/monstaftp.domain.com.crt"
        certificate_key_file: "/etc/ssl/private/monstaftp.domain.com.key"
        certificate_chain_file: "/etc/ssl/private/monstaftp.domain.com.int"
        allow_override: None

    apache_mods_enabled:
      - rewrite.load
      - ssl.load
      - php7.2.load
  roles:
    - geerlingguy.apache
    - geerlingguy.php
    - zfuller.monstaftp
```

# License
GNU GPLv3

# Author Information
zfuller
