# Async Python Web Frameworks comparison
----------
#### Updated: 2021-04-28

[![benchmarks](https://github.com/klen/muffin-benchmark/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/muffin-benchmark/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/muffin-benchmark/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/muffin-benchmark/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp is an exception).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-04-28)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The sources of the applications for tests can be found at here.

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.

3. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

Applications' source code can be found
[here](https://github.com/klen/muffin-benchmark/tree/develop/frameworks).


## The Results (2021-04-28)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 17735 | 3.02 | 4.57 | 3.58
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 15435 | 3.93 | 5.28 | 4.10
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 14982 | 3.69 | 5.50 | 4.23
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 14742 | 4.07 | 5.51 | 4.30
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 10166 | 5.47 | 8.17 | 6.25
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 8022 | 6.41 | 10.49 | 7.98
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7866 | 8.16 | 8.26 | 8.13
| [quart](https://pypi.org/project/quart/) `0.14.1` | 3399 | 18.82 | 20.55 | 18.81
| [django](https://pypi.org/project/django/) `3.2` | 1649 | 38.71 | 42.87 | 38.77


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 5533 | 9.12 | 15.43 | 11.57
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 4139 | 12.47 | 20.74 | 15.43
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 3864 | 13.23 | 22.09 | 16.53
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 3424 | 16.62 | 24.35 | 18.75
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2320 | 29.37 | 35.30 | 27.54
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2183 | 29.22 | 29.53 | 29.31
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 2065 | 34.00 | 39.95 | 30.93
| [quart](https://pypi.org/project/quart/) `0.14.1` | 1318 | 48.33 | 50.33 | 48.49
| [django](https://pypi.org/project/django/) `3.2` | 878 | 71.81 | 81.75 | 72.73


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 9673 | 5.29 | 8.76 | 6.58
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 9169 | 5.59 | 9.23 | 6.94
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 8830 | 5.85 | 9.54 | 7.21
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 7904 | 6.49 | 10.72 | 8.06
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 6802 | 7.36 | 12.45 | 9.40
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 6110 | 8.40 | 14.03 | 10.44
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4531 | 14.06 | 14.24 | 14.13
| [quart](https://pypi.org/project/quart/) `0.14.1` | 2148 | 29.32 | 30.54 | 29.79
| [django](https://pypi.org/project/django/) `3.2` | 1346 | 47.81 | 52.17 | 47.49

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 494115 | 5.81 | 9.59 | 7.24
| [muffin](https://pypi.org/project/muffin/) `0.69.5` | 426060 | 7.42 | 11.85 | 8.91
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 410025 | 8.76 | 13.03 | 10.0
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 378090 | 13.18 | 17.17 | 13.28
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 280320 | 9.0 | 15.01 | 11.3
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 275115 | 15.96 | 20.72 | 15.87
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 218700 | 17.15 | 17.34 | 17.19
| [quart](https://pypi.org/project/quart/) `0.14.1` | 102975 | 32.16 | 33.81 | 32.36
| [django](https://pypi.org/project/django/) `3.2` | 58095 | 52.78 | 58.93 | 53.0

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)