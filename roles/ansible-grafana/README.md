Role Name
=========

An [Ansible] role to install/configure [Grafana]

Requirements
------------

Install all Ansible role requirements.
````
sudo ansible-galaxy install -r requirements.yml -f
````

Information
-----------

Example dashboards are included in `dashboards/`. I would love to see some others
being added via PR's!

Vagrant
-------
Spin up Environment under Vagrant to test.
````
vagrant up
````

Usage
-----

````
username: admin
password: admin
````  

#### Vagrant
http://127.0.0.1:3000

#### Non-Vagrant
http://iporhostname:3000

Role Variables
--------------

```
---
# defaults file for ansible-grafana
grafana_config: true
grafana_enable_plugins: true
grafana_enable_vsphere_support: false
grafana_analytics:
  check_for_updates: 'true'
  reporting_enabled: 'true'
  google_analytics_ua_id: ''
grafana_anonymous_auth_info:
  enabled: 'false' # enable anonymous access
  org_name: 'Main Org.' # specify organization name that should be used for unauthenticated users
  org_role: 'Viewer' # specify role for unauthenticated users
grafana_apt_key: 'https://packagecloud.io/gpg.key'
grafana_apt_repo: 'deb https://packagecloud.io/grafana/stable/debian/ wheezy main'
grafana_auth_basic: true
grafana_auth_ldap: false
grafana_auth_ldap_config_file: '/etc/grafana/ldap.toml'
grafana_db_info: # Only required if not using sqlite3 as grafana_db_type
  host: '127.0.0.1:3306'
  name: 'grafana'
  user: 'root'
  password: 'root'
grafana_db_type: 'sqlite3' # Either "mysql", "postgres" or "sqlite3"
grafana_debian_pre_reqs:
  - 'git'
  - 'maven'
  - 'python-pip'
  - 'unzip'
grafana_github_auth_info: # http://docs.grafana.org/installation/configuration/#auth-github
  allow_sign_up: 'false'
  allowed_organizations: ''
  api_url: 'https://api.github.com/user'
  auth_url: 'https://github.com/login/oauth/authorize'
  client_id: 'some_id'
  client_secret: 'some_secret'
  enabled: 'false'
  scopes: 'user:email,read:org'
  team_ids: ''
  token_url: 'https://github.com/login/oauth/access_token'
grafana_google_auth_info: # http://docs.grafana.org/installation/configuration/#auth-google
  allow_sign_up: 'false'
  allowed_domains: ''
  api_url: 'https://www.googleapis.com/oauth2/v1/userinfo'
  auth_url: 'https://accounts.google.com/o/oauth2/auth'
  client_id: 'some_client_id'
  client_secret: 'some_client_secret'
  enabled: 'false'
  scopes: 'https://www.googleapis.com/auth/userinfo.profile https://www.googleapis.com/auth/userinfo.email'
  token_url: 'https://accounts.google.com/o/oauth2/token'
grafana_logging_info:
  level: 'info' # Either "trace", "debug", "info", "warn", "error", "critical", default is "info"
  mode:
    - 'console'
    - 'file'
    # - 'syslog'
  syslog_info:
    address: 'syslog.{{ pri_domain_name }}'
    enabled: false
    facility: '' # Syslog facility. user, daemon and local0 through local7 are valid.
    format: 'text' # log line format, valid options are text, console and json
    network: 'udp' # udp, tcp or unix
    tag: ''
grafana_paths: # Define paths for each grafana source
  data: '/var/lib/grafana'
  logs: '/var/log/grafana'
  plugins: '/var/lib/grafana/plugins'
grafana_plugins:
  - name: 'grafana-clock-panel'
    state: "present" # absent|present
grafana_security_info:
  admin_password: 'admin'
  admin_user: 'admin'
  cookie_remember_name: 'grafana_remember'
  cookie_username: 'grafana_user'
  data_source_proxy_whitelist: '' # data source proxy whitelist (ip_or_domain:port separated by spaces)
  disable_gravatar: 'false' # disable gravatar profile images
  login_remember_days: '7' # Auto-login remember days
  secret_key: 'SW2YcwTIb9zpOOhoPsMm' # used for signing
grafana_server_info:
  domain: 'localhost'
  enable_gzip: 'false'
  enforce_domain: 'false'
  http_addr: ''
  http_port: '3000'
  protocol: 'http'
  root_url: '%(protocol)s://%(domain)s:%(http_port)s/'
  router_logging: 'false'
  static_root_path: 'public'

# Provider config options
# memory: not have any config yet
# file: session dir path, is relative to grafana data_path
# redis: config like redis server e.g. `addr=127.0.0.1:6379,pool_size=100,db=grafana`
# mysql: go-sql-driver/mysql dsn config string, e.g. `user:password@tcp(127.0.0.1:3306)/database_name`
# postgres: user=a password=b host=localhost port=5432 dbname=c sslmode=disable
grafana_session_info: # Either "memory", "file", "redis", "mysql", "postgres", default is "file"
  cookie_name: 'grafana_sess' # Session cookie name
  cookie_secure: 'false' # If you use session in https only, default is false
  provider: 'file'
  provider_config: 'sessions' # File based
  # provider_config: 'addr=127.0.0.1:6379,pool_size=100,db=grafana' # Redis based
  session_life_time: '86400' # Session life time, default is 86400
grafana_smtp_info:
  cert_file: ''
  enabled: 'false'
  from_address: 'admin@grafana.localhost'
  host: 'localhost:25'
  key_file: ''
  password: ''
  skip_verify: 'false'
  user: ''
grafana_snapshots_info:
  external_enabled: 'true'
  external_snapshot_name: 'Publish to snapshot.raintank.io'
  external_snapshot_url: 'https://snapshots-origin.raintank.io'
grafana_users_info:
  allow_org_create: 'true' # Allow non admin users to create organizations
  allow_sign_up: 'true' # disable user signup / registration
  auto_assign_org_role: 'Viewer' # Default role new users will be automatically assigned (if disabled above is set to true)
  auto_assign_org: 'true' # Set to true to automatically assign new users to the default organization (id 1)
  default_theme: 'dark' # Default UI theme ("dark" or "light")
  login_hint: 'email or username' # Background text for the user field on the login page
grafana_welcome_emails: false
pri_domain_name: 'example.org'
```

Dependencies
------------

#### GitHub
````
ansible-collectd
ansible-snmpd
ansible-timezone
ansible-grafana
ansible-graphite
````
#### Galaxy
````
mrlesmithjr.collectd
mrlesmithjr.snmpd
mrlesmithjr.timezone
mrlesmithjr.grafana
mrlesmithjr.graphite
````

Example Playbook
----------------

#### GitHub
````
---
- name: provisions grafana
  hosts: all
  become: true
  vars:
  roles:
    - role: ansible-collectd
    - role: ansible-snmpd
    - role: ansible-timezone
    - role: ansible-grafana
    - role: ansible-graphite
  tasks:
````
#### Galaxy
````
---
- name: provisions grafana
  hosts: all
  become: true
  vars:
  roles:
    - role: mrlesmithjr.collectd
    - role: mrlesmithjr.snmpd
    - role: mrlesmithjr.timezone
    - role: mrlesmithjr.grafana
    - role: mrlesmithjr.graphite
  tasks:
````

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com

[Ansible]: <https://www.ansible.com/>
[Grafana]: <http://grafana.org/>
