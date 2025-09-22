# heroku-valkey-cli-app

This sample Heroku app uses [the Apt buildpack](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-apt) to install the `valkey` package - and dependencies. This allows for directly `valkey-cli` usage through one-off dynos, as the package isn't installed by default in Heroku's stack images. The resulting application itself isn't operational and doesn't run any other code.

This approach can be used, for example, to spin a one-off dyno and use `valkey-cli` to connect to a Heroku Key-Value store or other Valkey/Redis-compatible add-on.

## Usage

1. Deploy the app 

2. Start a one-off dyno:
    ```
    heroku run bash -a APP_NAME
    ```
    Alternatively, you can also start a `bash` console through the Heroku Dashboard (`App > More > Run console`, run command `bash`). This Dashboard approach allows connecting to the add-on without the need of any CLI tools installed.

3. Connect to your Heroku Key-Value Store add-on using `valkey-cli`.
   
    I recommend attaching the Heroku Key-Value Store add-on that you want to connect to to this app, in order to have its connection string directly available in your env. Handle your add-on credentials and secrets carefully.

    Use the `--insecure` flag is required to skip certificate validation due to the self-signed nature of the certs. The traffic from your dyno to your Valkey instance doesn't leave your Private/Shield space.
   
    Use the `--user default` option to set up authentication without a username, for compatibility with Heroku Key-Value Store credentials.

    ```
   valkey-cli -u $REDIS_URL --insecure --user default
    ```
  

### References
  * [Valkey CLI documentation](https://valkey.io/topics/cli/)
