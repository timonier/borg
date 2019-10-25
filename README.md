# README

Deduplicating archiver with compression and authenticated encryption

## Installation

```sh
# Define installation folder

export INSTALL_DIRECTORY=/usr/bin

# Use local installation

sudo bin/installer install

# Use remote installation

curl --location "https://gitlab.com/timonier/restic/raw/master/bin/installer" | sudo sh -s -- install
```

__Note 1__: If you do not define `INSTALL_DIRECTORY`, `installer` will use in `/usr/local/bin`.

__Note 2__: `docker-for-mac` users have to configure [native NFS server](https://medium.com/@sean.handley/how-to-set-up-docker-for-mac-with-native-nfs-145151458adc).

## Usage

### borg

Run the command `borg`:

```sh
# See all borg options

borg --help

# Initialize a repository

export BORG_PASSPHRASE=my-super-password
borg init --encryption repokey /path/of/repository
# By default repositories initialized with this version will produce security
#errors if written to with an older version (up to and including Borg 1.0.8).
#
# If you want to use these older versions, you can disable the check by running:
#borg upgrade --disable-tam /srv
#
# See https://borgbackup.readthedocs.io/en/stable/changes.html#pre-1-0-9-manifest-spoofing-vulnerability for details about the security implications.
#
# IMPORTANT: you will need both KEY AND PASSPHRASE to access this repo!
# Use "borg key export" to export the key, optionally in printable format.
# Write down the passphrase. Store both at safe place(s).

borg create -v /path/of/repository::{hostname}_{now:%d.%m.%Y} /path/to/backup
# Creating archive at "/srv::{hostname}_{now:%d.%m.%Y}"
```

### borgmatic

Run the command `borgmatic`:

```sh
# See all borg options

borgmatic --help

# Create configuration

mkdir -p "${HOME}/.config/borgmatic"
cat > "${HOME}/.config/borgmatic/config.yaml" <<"EOF"
location:
    source_directories:
        - /path/to/backup

    repositories:
        - /path/of/repository

retention:
    keep_daily: 7
    keep_weekly: 4
    keep_monthly: 6

consistency:
    checks:
        - repository
        - archives
EOF

# Run borgmatic

borgmatic create --verbosity 1
# /path/of/repository: Creating archive
# Creating archive at "/path/of/repository::{hostname}-{now:%Y-%m-%dT%H:%M:%S.%f}"
```

## Contributing

1. Fork it.
2. Create your branch: `git checkout -b my-new-feature`.
3. Commit your changes: `git commit -am 'Add some feature'`.
4. Push to the branch: `git push origin my-new-feature`.
5. Submit a [merge request](https://docs.gitlab.com/ee/user/project/merge_requests/).

__Note 1__: [GitHub repository](https://github.com/timonier/borg) is a mirror. [Merge request](https://docs.gitlab.com/ee/user/project/merge_requests/) has to be submitted to the [GitLab repository](https://gitlab.com/timonier/borg).

__Note 2__: Use the script `bin/build-image` to test your modifications locally.

If you like / use this project, please let me known by adding a [★](https://help.github.com/articles/about-stars/) on the [GitHub repository](https://github.com/timonier/borg) or on the [GitLab repository](https://gitlab.com/timonier/borg).

## Links

* [borgbackup/borg](https://github.com/borgbackup/borg)
* [image "timonier/borg"](https://hub.docker.com/r/timonier/borg/)
* [s3cmd](https://s3tools.org/s3cmd)
* [set up docker for mac with native nfs](https://medium.com/@sean.handley/how-to-set-up-docker-for-mac-with-native-nfs-145151458adc)
* [timonier/dumb-entrypoint](https://gitlab.com/timonier/dumb-entrypoint)
* [timonier/version-lister](https://gitlab.com/timonier/version-lister)
* [witten/borgmatic](https://github.com/witten/borgmatic)
