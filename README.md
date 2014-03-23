# openssh-stdinkey

This project is a fork of [OpenSSH][1] which modifies the
`AuthorizedKeysCommand` directive in `sshd_config` (present in OpenSSH 6.2 and
above) such that the public key and the fingerprint of the public key of the
incoming connection is passed to the command via environment variables.  This
provides a way to more efficiently generate a list of authorized keys by only
printing the key(s) that match the incoming key.

The public key, including the type (e.g. `ssh-rsa`, `ssh-dsa`, etc.), is in
the environment variable `SSH_KEY`.

The fingerprint of the public key is the MD5 fingerprint, which is what is
generated by `ssh-keygen -l`, is in the environment variable
`SSH_KEY_FINGERPRINT`.

The inspiration for this project was to be able to provide a service similar
to some popular version control repository hosting sites, where a user uploads
their SSH public key(s) via a web interface and accesses their repositories
over SSH using a common SSH user account like "git" or "hg".  However, there
are likely many other use cases for this project.

## Branches

This git repository is organized into this branch (master), pristine OpenSSH
branches (those that are just version numbers), and patched OpenSSH branches
(those that end with `-stdinkey`).  The master branch contains this README.md
file and patches suitable for input to the `patch` command against a specific
version of the OpenSSH source code.

## Caveats

* It is probably a good idea to limit the use of the `AuthorizedKeysCommand`
  directive to the specific user which you would like to have this behavior
  using a `Match` block:

      Match user git
          AuthorizedKeysCommand /usr/libexec/lookup-ssh-key

[1]: http://www.openssh.com/
