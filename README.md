Covert
=======

Yet another management tool for Docker secret with time-based secret versioning.

## Installation

Download the release source code and make the script available on your path
by modifying your `$PATH` variable or put it somewhere like `/usr/bin` or
`/usr/local/bin` so you can invoke the `covert` tool from any directories.

## Usage

To add new secret to the Docker secret, use `covert commit`.

```
$ covert commit my-file.conf
18029348-my-file.conf
```

Covert will output the version number of your file and will update older
my-file.conf in Docker secret if exists. Then you can add the secret to service
using `covert add -s <secret_name> -x <self_secret_name> <service_name>`.

If the secret is a configuration file and does not contain any sensitive
information, you can add it with `-s, --secret` option.

```
$ covert add -s my-file.conf my-service
```

Otherwise, if you adding some kind of private key that should only readable
by root, use `-x, --self-secret` option.

```
$ covert add -x privkey.pem my-service
```

If you modify the file and want to update the current secret, first commit it
with `covert commit` and run `covert update` command. You can also cleanup the
older secret from Docker secret by running `covert clean`.

```
$ covert commit my-file.conf
18029432-my-file.conf
$ covert update -s my-file.conf my-service
$ covert clean my-file.conf
18029348-my-file.conf
```

Please note that if you add the file using the self secret (`-x, --self-secret`
option), you should update the file using the same option or the updated file
will be exposed to global read unless you explicitly define the `--mode` option.

For complete usage information, please consult `covert --help` or `covert
COMMAND --help`.

## License

Copyright (c) 2017 Fadhli Dzil Ikram. Please see accompanying LICENSE file.
