secret-file-buildpack
====

Add secret files to your build from env variables.

The buildpack will save the all secrets (variables starting with `SECRET_FILE_`) to your build at `~/.secrets`,
 to override set `SECRETS_DIR`.

## Usage

1. Add the buildpack to your Heroku app:

    ```
    $ heroku buildpacks:add https://github.com/heroku/heroku-buildpack-ssh-key.git -i 1
    ```

    Keep in mind that the buildpack order is important. If you'll specify this buildpack after your default 
    one (e.g. `heroku/nodejs`) it'll not work. 
    See [https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app) for details.

2. Set `SECRET_FILE_PATH_TO_SECRET_KEY` to the secret you want created:

    ```
    $ heroku config:set SECRET_FILE_MY_SECRET_KEY="$(cat my_secret.key | base64)"
    # saved to `~/.secrets/my_secret.key`

    $ heroku config:set SECRET_FILE_OTHER_SECRET_PEM="$(cat my_secret.pem | base64)"
    # saved to `~/.secrets/other_secret.pem`

    $ heroku config:set SECRET_FILE_PATH__TO__MY_SECRET_PEM="$(cat my_secret.pem | base64)"
    # saved to `~/.secrets/path/to/my_secret.pem`
    ```


For basic obfuscation and simplicity in saving an env var as a string `base64` encoding is used. It has the handy
benefit of removing/restoring whitespace formatting, often an issue for picky secrets text formats.

It is possible to override this behaviour, by setting `SECRETS_DECODE=cat`. You could then set your secrets 
without the encode step.

    ```
    $ heroku config:set SECRET_FILE_MY_SECRET_KEY="$(cat my_secret.key)"
    ```