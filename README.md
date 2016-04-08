# reposetup

Reposetup is a minimalist tool to manage repositories on a remote server. It
requires only an ssh server and optionally a web server.

It is a simpler alternative to more feature complete tools like [Gitolite][] if
you want to host repositories on your own server.

## Installation

Assuming you have a server with SSH installed and Apache with the [mod_userdir][]
module enabled, the following will setup Reposetup to manage Git repositories in
the `public_html/git` dir inside your user home dirs.

- Connect to your server

- Copy the `reposetup` script to a directory which is part of $PATH when
  running a command over SSH, for example `/usr/local/bin`.
  (`$HOME/bin` is likely not a good candidate because it is only added to $PATH
  when starting a shell)

- Create `/etc/reposetuprc` with the following content, replacing
  `<yourserver>` with the hostname of your server:

    ```
    # Path where repositories will be created
    REPO_BASE_DIR=$HOME/public_html/git

    # Repository url for read-write access
    REPO_RW_URL=$USER@<yourserver>:public_html/git/$REPO_NAME

    # Repository url for read-only access
    REPO_RO_URL=http://<yourserver>/~$USER/git/$REPO_NAME
    ```

Note: You can also create a `reposetuprc` file in the `$HOME/.config` dir of
each user.

## Usage

Reposetup is designed to be used over SSH, from your workstation.

Let's say user sheldon wants to create a repository named example on the
bazinga server:

    $ ssh sheldon@bazinga reposetup create example
    The "example" repository has been created. You can now push your repository to it:

        git remote add origin sheldon@bazinga:public_html/git/example
        git push -u origin master

    Url for read-only access:

        http://bazinga/~sheldon/git/example

To list your repositories:

    $ ssh sheldon@bazinga reposetup ls
    example

To rename the repository:

    $ ssh sheldon@bazinga reposetup rename example bbt

To delete the repository:

    $ ssh sheldon@bazinga repository rm bbt

## Shouldn't you host this on your own server?

You might find it ironic that a tool to manage repositories is hosted on
GitHub rather than self-hosted on a server I own.

The reason for this is that I am no sysadmin. I am not qualified to setup a
secure, public-facing Git server. I use Reposetup on private servers, but
hosting it on GitHub is simpler for me and probably safer for you.

That should not stop from hosting it yourself on your own server, that's the
beauty of Git.

[Gitolite]: https://github.com/sitaramc/gitolite
[mod_userdir]: https://httpd.apache.org/docs/2.4/en/mod/mod_userdir.html
