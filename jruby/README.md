# JRuby

## Installation

To install this package run the following command within your Drexler project:

```sh
$ drexler pkg get github.com/aramiza/packages/jruby
```

Once installed edit the file `.drexler/config.toml` to register the JRuby package.
For example:

```toml
apps = [
  "github.com/aramiza/packages/jruby@v0.1.0/9/9.2.toml"
]
```

Running `drexler ls` within your project directory will list the following list
of applications:

```sh
drexler
├── apps
│   ├── bundle
│   ├── bundler
│   ├── gem
│   ├── irb
│   ├── rake
│   ├── rdoc
│   ├── ri
│   └── ruby
└── servers
```

To run any of the above applications use the following command:

```sh
$ drexler <app> <args>
```

For example:

```sh
$ drexler ruby --version
jruby 9.2.0.0 (2.5.0) 2018-05-24 81156a8 OpenJDK 64-Bit Server VM 25.181-b13
on 1.8.0_181-8u181-b13-1~deb9u1-b13 +jit [linux-x86_64]
```

## vendor/bundle

If you use bundler to vendor your gems, your project will have a directory
structure similar to this:

```sh
└── .bundle
│   └── config
├── Gemfile
├── Gemfile.lock
└── vendor
    ├── bundle
    └── cache
```

If you want to use bundler to install your project's dependencies and execute
your application, you will need to add an override file to your project in order
to mount `.bundle/config` and `vendor/bundle` inside the container.

The reason is that the official Docker image has the following defaults which
cannot be overriden at run time.

- `BUNDLE_PATH=/usr/local/bundle`
- `BUNDLE_APP_CONFIG=/usr/local/bundle`

To add the appropriate mounts, create the file `.drexler/overrides/github.com/aramiza/packages/jruby/9/9.2.toml`
with the following contents:

```toml
[options]
_mount = [
  "type=bind,src=$DREXLER_WS/.bundle/config,dst=/usr/local/bundle/config",
  "type=bind,src=$DREXLER_WS/vendor/bundle,dst=/usr/local/bundle"
]
```

Running `drexler bundle list` should show the gems installed in your projects
`vendor/bundle` directory, and `drexler bundle config` should reflect the config
options as specified in `.bundle/config`.
