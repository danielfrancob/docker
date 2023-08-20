# Gitlab

This is an example for configuring gitlab community edition in a self managed environment.

## How to run

For implementing this configuration:

1. Execute the command `docker compose up -d`

2. Wait for some minutes until gitlab is started. You can test if gitlab is ready accessing through web browser to `http://localhost`. It wil load the login page.

3. Use this credentials to login

   - User: root
   - Password: Execute this command inside gitlab container to get the password `cat /etc/gitlab/initial_root_password`

   If you prefer, you can change the root password using this command: `gitlab-rake "gitlab:password:reset[root]"`

4. If you want to use pipeline feature, you need to configure a runner. When you are in runner creation page, choose docker as runner type. Then, you need to configure the runner in gitlab-runner container with the command given by gitlab.

   After doing it, you need to look for `/etc/gitlab-runner/config.toml` and add `network_mode = "gitlab_default"` configuration inside _[runners.docker]_. File should look like this:

   ```
   [runners.docker]
       tls_verify = false
       image = "python"
       privileged = false
       disable_entrypoint_overwrite = false
       oom_kill_disable = false
       disable_cache = false
       volumes = ["/cache"]
       shm_size = 0
       network_mode = "gitlab_default"
   ```

   Here, network_mode attribute is assuming the value as an external network.
