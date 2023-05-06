# SSH Deploying

Sometimes (but not often) I need to use SSH to deploy something to an existing website or server using GitHub Actions. To do this, I use [ssh-key-action](https://github.com/shimataro/ssh-key-action).

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Ensure we have the latest code
      uses: actions/checkout@v3
      with:
        fetch-depth: 1
    - name: Install the deployment SSH key
      uses: shimataro/ssh-key-action@v2
      with:
        key: ${{ secrets.DEPLOY_SSH_KEY }}
        name: id_rsa
        known_hosts: ${{ secrets.KNOWN_HOSTS }}
        if_key_exists: replace
    - name: Deploy something
      run: rsync -a --exclude 'README.md' --exclude '.git' --exclude '.ssh' --exclude '.github' . ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }}:${{ secrets.FOLDER }}
```

All of the `secrets` variable are GitHub Actions keys that need to be configured on the [settings/secrets/actions](https://github.com/rvaughan/til/settings/secrets/actions) page in your GitHub repository.

  * **DEPLOY_SSH_KEY** should be set to the private half of the SSH key you need to use to connect to the server.
  * **KNOWN_HOSTS** should include the known hosts entry for the server you want to connect to.
  * **SSH_HOST** should be the ip/address of the server.
  * **SSH_USER** should be the (pre-existing) user to connect to the server with, that uses the key.
  * **FOLDER** is the name of the folder on the server to copy the data into.
