# Gitlab

Start a gitlab instance with a gitlab runner (which you need to register manually)

## Access

username: root
password: @Password123

NOTE: GITLAB_ROOT_PASSWORD feature works only if the specified password meets
gitlabs requirements otherwise it will generate a random password you can look
at here: `docker compose exec gitlab cat /etc/gitlab/initial_root_password`
In this case change password after login ( /-/profile/password/edit ), otherwise
it will be reconfigured after 24 hours.

## Post start config step
You need to register gitlab runner in gitlab:
- get the runner reg token here: /admin/runners click
  3 dots after "New instance runner" and copy token
- run: docker compose exec gitlab-runner gitlab-runner register \
    --non-interactive \
    --executor "docker" \
    --url "http://gitlab/" \
    --token "CcuChY5p_x4MDLPgY1mD"
