# Matterbridge-PaaS

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)
[![Deploy on Scalingo](https://cdn.scalingo.com/deploy/button.svg)](https://my.scalingo.com/deploy?source=https://github.com/betagouv/matterbridge-scalingo#master)


An inline buildpack for hosting Matterbridge on Heroku.


## Configuration

Configuration happens via environment variables and a configuration
template file.

### Environment: Buildpack

- `MATTERBRIDGE_VERSION` Required. Use a [matterbridge git tag][git-tags].
- `MATTERBRIDGE_URL` Optional. Use this to download the binary from a
  custom url instead of the tagged release from the official GitHub
repo.  (Setting this makes `MATTERBRIDGE_VERSION` ignored.)
    * With caution, you may want to use the [latest nightly matterbridge
      build](https://bintray.com/42wim/nightly/Matterbridge/_latestVersion)
      while waiting on the next official release.

### Environment: Matterbridge

Matterbridge has some configuration of its own, which is mostly
documented upstream. For starters, we'll review the most important
aspects:

* `DEBUG`. Set to "1" to log all message events across bridges.

Matterbridge uses Viper, and so each value in the TOML configuration can
be set by envvar.

Basically, here are the rules:

- Each config envvar is prefixed with `MATTERBRIDGE_`.
- Each nested level of config object is separated by an underscore `_`.
- Any dash in a config key is converted to an underscore `_`.

So for example, with this in your TOML config:

```toml
[slack.my-team]
Token="xoxp-xxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

You could instead set an environment variable for
`MATTERBRIDGE_SLACK_MY_TEAM_TOKEN` and leave that key out of in the
configuration file template.

### Template: Matterbridge

* Edit channel bridge config via [`config/config-template.toml`][config].

<!-- Links -->
   [git-tags]: https://github.com/42wim/matterbridge/tags
   [config]: config/config-template.toml

## Scalingo

You have to launch : `scalingo --app name scale worker:1:S` before the first deploy.