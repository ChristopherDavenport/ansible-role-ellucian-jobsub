# Ellucian Jobsub

This is not yet a completed role. It is based upon bringing these other roles
together and will be consolidated. Below is a sample playbook without variables.

I have inventory groups for gnucobol-jobsub and netcobol-jobsub which are both children
of jobsub.



```yml

- hosts: jobsub
  become: yes
  vars:
    java_type: Oracle

    icu_version: 52
    icu_set_home: True
    icu_ansi_compatibility: True

    oracle_client_install_type: Custom
    oracle_client_zip: /jobsub/files/linuxamd64_12102_client.zip

    oracle_client_databases:
      - sid: TEST
        host: banner.test.school.edu
        port: 2322
        service_name: TEST

    oracle_client_listener_host: banner.test.school.edu
    oracle_client_listener_port: 2322

    ellucian_environment_name: TEST
    ellucian_wgetrc_http_password: fakeWgetRcPassword
    ellucian_wgetrc_users:
        - name: banner

    jenkins_slave_username: banner
    jenkins_slave_authorize_key: /jobsub/files/jenkinsmaster_rsa.pub
    jenkins_slave_base: bmui_jenkins_{{ ellucian_environment_name }}

    banner_jobsub_repo: git@yourrepohere
    banner_jobsub_repo_branch: develop
    banner_key_file_folder: /jobsub/files/
    banner_key_file: gitreaderbot_rsa

  pre_tasks:
    - name: Install Required Packages
      package:
        name: "{{ item }}"
        state: present
      with_items:
        - unzip
        - git
  roles:
    - ChristopherDavenport.universal-java
    - ChristopherDavenport.universal-oracle-client
    - ChristopherDavenport.icu
    - ChristopherDavenport.jenkins-slave
    - ChristopherDavenport.ellucian-wgetrc
    - ChristopherDavenport.cups
    - ellucian-jobsub

- hosts: gnucobol-jobsub
  become: yes
  roles:
    - ChristopherDavenport.gnu-cobol

- hosts: netcobol-jobsub
  become: yes
  roles:
    - ChristopherDavenport.netcobol
```
