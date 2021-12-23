# GitHub to FTP

Automate deploying websites and more with this GitHub action.

Credits: https://github.com/SamKirkland/FTP-Deploy-Action

---

### Usage Example
Place the following in `/.github/workflows/main.yml`
```yml
on: push
name: 🚀 Deploy website on push
jobs:
  web-deploy:
    name: 🎉 Deploy
    runs-on: ubuntu-latest
    steps:
    - name: 🚚 Get latest code
      uses: actions/checkout@v2
      
    - name: 📂 Sync files
      uses: webitsbr/github-to-ftp@1.0.1
      with:
        server: ${{ secrets.server }}
        server-dir: ${{ secrets.dir }}
        username: ${{ secrets.username }}
        password: ${{ secrets.password }}
```

### Settings
Keys can be added directly to your .yml config file or referenced from your project `Secrets` storage.

To add a `secret` go to the `Settings` tab in your project then select `Secrets`.
I strongly recommend you store your `password` as a secret.

| Key Name                | Required | Example                    | Default Value                                 | Description                                                                                                                                                        |
|-------------------------|----------|----------------------------|-----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `server`                | Yes      | `ftp.leandro.xyz`      |                                               | Deployment destination server                                                                                                                                      |
| `username`              | Yes      | `username@leandro.xyz` |                                               | FTP user name                                                                                                                                                      |
| `password`              | Yes      | `CrazyUniquePassword&%123` |                                               | FTP password, be sure to escape quotes and spaces                                                                                                                  |
| `port`                  | No       | `990`                      | `21`                                          | Server port to connect to (read your web hosts docs)                                                                                                               |
| `protocol`              | No       | `ftps`                     | `ftp`                                         | `ftp`: provides no encryption, `ftps`: full encryption newest standard (aka "explicit" ftps), `ftps-legacy`: full encryption legacy standard (aka "implicit" ftps) |
| `local-dir`             | No       | `./myFolderToPublish/`     | `./`                                          | Folder to upload from, must end with trailing slash `/`                                                                                                            |
| `server-dir`            | No       | `public_html/www/`         | `./`                                          | Folder to upload to (on the server), must end with trailing slash `/`                                                                                              |
| `state-name`            | No       | `folder/.sync-state.json`  | `.ftp-deploy-sync-state.json`                 | Path and name of the state file - this file is used to track which files have been deployed                                                                        |
| `dry-run`               | No       | `true`                     | `false`                                       | Prints which modifications will be made with current config options, but doesn't actually make any changes                                                         |
| `dangerous-clean-slate` | No       | `true`                     | `false`                                       | Deletes ALL contents of server-dir, even items in excluded with 'exclude' argument                                                                                 |
| `exclude`               | No       |                            | `**/.git*` `**/.git*/**` `**/node_modules/**` | An array of glob patterns, these files will not be included in the publish/delete process. [List must be in yaml array format](#exclude-files). You can use [a glob tester](https://www.digitalocean.com/community/tools/glob?comments=true&glob=%2A%2A%2F.git%2A%2F%2A%2A&matches=false&tests=test%2Fsam&tests=.git%2F&tests=.github%2F&tests=.git%2Ftest&tests=.gitattributes&tests=.gitignore&tests=.git%2Fconfig&tests=.git%2Ftest%2Ftest&tests=.github%2Fworkflows%2Fmain.yml&tests=test%2F.git%2Fworkflows%2Fmain.yml&tests=node_modules%2Ffolder%2F&tests=node_modules%2Fotherfolder%2F&tests=subfolder%2Fnode_modules%2F) to test your pattern(s).                     |
| `log-level`             | No       | `minimal`                  | `standard`                                    | `minimal`: only important info, `standard`: important info and basic file changes, `verbose`: print everything the script is doing                                 |
| `security`              | No       | `strict`                   | `loose`                                       | `strict`: Reject any connection which is not authorized with the list of supplied CAs. `loose`: Allow connection even when the domain is not certificate           |

#### Exclude files

```yml
on: push
name: 🚀 Deploy website on push
jobs:
  web-deploy:
    name: 🎉 Deploy
    runs-on: ubuntu-latest
    steps:
    - name: 🚚 Get latest code
      uses: actions/checkout@v2

    - name: 📂 Sync files
      uses: leandrorepos/ftp-deploy@1.0.0
      with:
        server: ftp.samkirkland.com
        username: myFtpUserName
        password: ${{ secrets.password }}
        exclude:
          - **/.git*
          - **/.git*/**
          - **/node_modules/**
          - fileToExclude.txt
```
