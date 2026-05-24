A simple role to install GitLab runner.

# Examples
## Playbook
Here's an example of a playbook to install GitLab Runner on the local machine.
It does not require you have SSH running.

```yaml
- hosts: localhost
  connection: local
  become: true
  roles:
    - role: hax0rbana_adam.gitlab_runner
      gitlab_runner_reg_token: "YOUR_REG_TOKEN_GOES_HERE"
```

Here's an example of using docker to run CI jobs and having the docker
containers use a proxy when it checks out the source code.

```yaml
- hosts: all
  become: true
  remote_user: root
  roles:
    - role: hax0rbana_adam.gitlab_runner
      gitlab_runner_reg_token: "YOUR_REG_TOKEN_GOES_HERE"
      gitlab_runner_name: "{{ ansible_domain }}-{{ ansible_distribution | lower }}-{{ ansible_distribution_major_version }}"
      gitlab_runner_docker_image: "{{ ansible_distribution | lower }}:{{ ansible_distribution_release }}"
      gitlab_runner_extra_args: >
        --docker-image {{ gitlab_runner_docker_image }}
        --pre-get-sources-script "git config --global http.proxy $HTTP_PROXY; git config --global https.proxy $HTTPS_PROXY"
        --env "https_proxy={{ gitlab_runner_env_vars['https_proxy'] }}"
        --env "http_proxy={{ gitlab_runner_env_vars['http_proxy'] }}"
        --env "HTTPS_PROXY={{ gitlab_runner_env_vars['HTTPS_PROXY'] }}"
        --env "HTTP_PROXY={{ gitlab_runner_env_vars['HTTP_PROXY'] }}"
      gitlab_runner_env_vars:
        HTTP_PROXY: proxy.{{ ansible_domain }}:3128
        HTTPS_PROXY: proxy.{{ ansible_domain }}:3128
        https_proxy: http://proxy.{{ ansible_domain }}:3128
        http_proxy: http://proxy.{{ ansible_domain }}:3128
        RUNNER_TAG_LIST: "runner2,docker"
        RUNNER_EXECUTOR: docker
        RUNNER_NAME: "{{ gitlab_runner_name }}-docker"
        REGISTRATION_TOKEN: "{{ gitlab_runner_reg_token_docker }}"
        CI_SERVER_URL: "https://gitlab.{{ ansible_domain }}/"
        REGISTER_NON_INTERACTIVE: true
        CI_SERVER_TLS_CA_FILE:
```

# Official repo location
All activity takes place on the official GitLab instance:
[https://gitlab.hax0rbana.org/public-repos/ansible/ansible-role-gitlab_runner](https://gitlab.hax0rbana.org/public-repos/ansible/ansible-role-gitlab_runner)

Any other hosting providers, such as GitHub.com and GitLab.com, are just mirrors
and we do not monitor the issue trackers over there.

# Support
## Matrix channel
You can also join our Matrix channel: #ansible:hax0rbana.org

This is a good place to ask questions or make requests without having to sign
up for another account.

# Contributing
See [contributor guidelines](CONTRIBUTING.md).

# License
This project is licensed under MIT License. See [LICENSE](LICENSE) for more details.
