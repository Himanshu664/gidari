# Gidari

[![PkgGoDev](https://img.shields.io/badge/go.dev-docs-007d9c?logo=go&logoColor=white)](https://pkg.go.dev/github.com/alpine-hodler/gidari)
![Build Status](https://github.com/alpine-hodler/gidari/actions/workflows/ci.yml/badge.svg)
[![Go Report Card](https://goreportcard.com/badge/github.com/alpine-hodler/gidari)](https://goreportcard.com/report/github.com/alpine-hodler/gidari)
[![Discord](https://img.shields.io/discord/987810353767403550)](https://discord.gg/3jGYQz74s7)

<p align="center"><img src="https://raw.githubusercontent.com/alpine-hodler/gidari/main/etc/assets/gidari-gopher.png" width="300"></p>

Gidari is a "web-to-storage" tool for querying web APIs and persisting the resulting data onto local storage. A configuration file is used to define how this querying and storing should occur. Once you have a configuration file, you can intiate this transport using the command `gidari --config <configuration.yml>`. See [here](https://youtu.be/NgeOJ50IWhY) for a quick demonstration.

## Installation

TODO

## Usage

Document how to use the package with `gidari.Transport` 

### Command Mode

All the existing CLI logic stays here, remove the "Configuration" header.

The configuration is a YAML file used to define a set of rules for making RESTful HTTP requests and where to store the data. See [here](https://github.com/alpine-hodler/gidari/tree/main/internal/transport/testdata/upsert) for example configurations.

| Key                    | Required | Type    | Description                                                                                                                                                                                                                            |
|------------------------|----------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `url`                  | Y        | string  | The URL for the RESTful API for making requests                                                                                                                                                                                        |
| `authentication`       | N        | map     | Data required for authenticating the web API requests                                                                                                                                                                                  |
| `connectionStrings`    | Y        | list    | List of connection strings for communicating with local/remote storage                                                                                                                                                                 |
| `rateLimit`            | Y        | map     | Data required for limiting the number of requests per second, avoiding 429 errors                                                                                                                                                      |
| `rateLimit.burst`      | Y        | int     | Number of requests that can be made per second                                                                                                                                                                                         |
| `rateLimit.period`     | Y        | int     | Period for the `rateLimit.burst`                                                                                                                                                                                                       |
| `truncate`             | N        | boolean | Truncate all tables in the database before performing request upserts                                                                                                                                                                  |
| `requests`             | N        | list    | List of requests to receive data from the web API for upserting into local/remote storage                                                                                                                                              |
| `request.endpoint`     | Y        | string  | Endpoint for making the RESTful API request                                                                                                                                                                                            |
| `table`                | N        | string  | Name of the table in the remote/local storage for upserting data. This field defaults to the last string in the endpoint path                                                                                                          |
| `timeseries`           | N        | map     | Data required for upserting time series data, which can be very resource intensive                                                                                                                                                    |
| `timeseries.startName` | Y        | string  | Name of the query/path parameter for the "start" date of the time series                                                                                                                                                               |
| `timeseries.endName`   | Y        | string  | Name of the query/path parameter for the "end" date of the time series                                                                                                                                                                 |
| `timeseries.period`    | Y        | int     | How often (in seconds) to build a new datetime range to batch over. For example, if your datetime range spans 24 hours and your period is 3600 then the request will be broken up into 24 smaller requests spanning the datetime range |
| `timseries.layout`     | Y        | string  | The layout for how to build a datetime to query over. For example, if your time series uses RFC3339 then the layout should be "2006-01-02T15:04:05Z07:00"                                                                              |
| `query`                | N        | map     | This is a non-deterministic map that holds the query parameters for a request

### SQL

TODO

### NoSQL

The NoSQL use case should require no overhead from the user. Just include the connection string in the `connectionString` list of the configuration file.

## Repository

The `repository` and `proto` packages are the only packages within the application that are public-facing stable API with the purpose of communicating CRUD requests to the storage devices used in the web-to-storage transfers.

## Contributing

Follow [this guide](docs/CONTRIBUTING.md) for information on contributing.


## Resources

- Public REST APIs: https://documenter.getpostman.com/view/8854915/Szf7znEe
- Artwork by [Victoria Trum](https://www.fiverr.com/victoria_trum?source=order_page_user_message_link)
