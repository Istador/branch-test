# this is a simple branch test

## overview

remote servers:
- `origin` for `git.domain.tld`
- `github` for `github.com`

branches:
- `master` for the remote `origin`
- `github` for the remote `github`
- `public` used as an alias for the branch `github` on `github.com`

## setup

### git.domain.tld

1. Create the repository on server
2. Configure webserver for access to the repository with HTTP authentification and TLS

### github.com

1. Create a new repository at <https://github.com/new> with name `repo-name`
2. Generate a new token at <https://github.com/settings/tokens> with access to `public_repo` and write it down (will be needed later)

### localhost

1. local repository at `/local/path/` with branch `master` for `git.domain.tld`
```bash
git clone -o origin https://user:password@git.domain.tld/repo-name /local/path/
cd /local/path/
git checkout -b master   # create & use 'master' branch

# make local changes

git add .
git commit -m 'first commit'
git push                 # master -> git.domain.tld
```

2. branch `github` for remote `github` that links to `github.com` showing a branch name of `public`
```bash
git remote add github https://username:token@github.com/Username/repo-name.git
git checkout -b github   # create & use 'github' branch
git push --set-upstream github github:public  # github -> github.com
git config push.default upstream              # use upstream for the 'git push' command
git checkout master      # use master
```

## usage

1. working with the default branch `master`
```bash
git pull                 # git.domain.tld -> master

# make local changes

git add .
git commit -m 'commit message'
git push                 # master -> git.domain.tld
```

2. bringing changes from `git.domain.tld` to `github.com`
```bash
git pull                 # git.domain.tld -> master
git checkout github      # use github
git merge master         # master -> github
git push                 # github -> github.com
git checkout master      # use master
```

3. bringing changes from `github.com` to `git.comain.tld` (e.g. accepted pull requests from other developers)
```bash
git checkout github      # use github
git pull                 # github.com -> github
git checkout master      # use master
git merge github         # github -> master
git push                 # master -> git.domain.tld
```

