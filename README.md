# GitHub Action: Setup NGINX

[![GitHub](https://img.shields.io/badge/github-nyurik/action--setup--nginx-8da0cb?logo=github)](https://github.com/nyurik/action-setup-nginx)
[![CI build](https://github.com/nyurik/action-setup-nginx/actions/workflows/ci.yml/badge.svg)](https://github.com/nyurik/action-setup-nginx/actions)
[![Marketplace](https://img.shields.io/badge/market-action--setup--nginx-6F42C1?logo=github)](https://github.com/marketplace/actions/setup-nginx-service-for-linux-macos-windows)

This action sets up a NGINX web server.

* Runs on Linux, macOS and Windows GitHub runners
    * On Linux, assumes it is pre-installed
    * On macOS, installs using [Homebrew](https://formulae.brew.sh/formula/nginx)
    * On Windows, assumes it is pre-installed
* Overrides default configuration with a simpler and more cross-platform consistent one (can be user-supplied)
* As output, provides the location of the root html dir, process ID, and access and error log files.
* [Easy to check](action.yml) that IT DOES NOT contain malicious code.

See also [action-setup-postgis](https://github.com/nyurik/action-setup-postgis) to configure PostGIS service.

## Usage

```yaml
steps:
  - uses: nyurik/action-setup-nginx@v1
    id: nginx

  - run: |
      echo "Hello, world!" > "${{ steps.nginx.outputs.html-dir }}/index.html"
      
      curl http://localhost:${{ steps.nginx.outputs.port }}/
      # Expected output: Hello, world!

      cat "${{ steps.nginx.outputs.access-log }}"
      # Expected to contain a line with   GET /   HTTP/1.1   200
```

#### Input parameters

| Param             | Description                                                                                                                           | Default |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------|
| port              | The port number to use for the NGINX service, unless conf-file-text is set.                                                           | 8080    |
| conf-file-text    | Optional content of the nginx.conf file, overrides the default one                                                                    |         |
| output-unix-paths | If set to a non-empty value, will use Unix paths in the output. This is only relevant on Windows runners, ignored on other platforms. |         |

#### Outputs

| Value      | Description                                                                                 |
|------------|---------------------------------------------------------------------------------------------|
| bin        | The path to the NGINX binary.                                                               |
| conf-path  | The path to the NGINX configuration file.                                                   |
| html-dir   | Default directory NGINX service uses as the root. This can be overridden by conf-file-text. |
| pid        | The process ID of the NGINX service.                                                        |
| port       | The port number used by the NGINX service.                                                  |
| access-log | The path to the NGINX access log file, unless conf-file-text is provided.                   |
| error-log  | The path to the NGINX error log file, unless conf-file-text is provided.                    |

## License

Licensed under either of

* Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0>)
* MIT license ([LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT>)
  at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally
submitted for inclusion in the work by you, as defined in the
Apache-2.0 license, shall be dual licensed as above, without any
additional terms or conditions.
