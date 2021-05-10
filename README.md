# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-05-10

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-05-10)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B547320%2C477660%2C446820%2C374085%2C341130%2C319260%2C277860%2C224640%2C129690%2C113715%2C64935%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.

3. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.


## The Results (2021-05-10)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.4` | 19306 | 2.78 | 4.24 | 3.28
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 17047 | 3.11 | 4.84 | 3.72
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 15739 | 3.29 | 5.27 | 4.03
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 14155 | 3.78 | 5.87 | 4.48
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 13995 | 3.65 | 5.96 | 4.54
| [fastapi](https://pypi.org/project/fastapi/) `0.65.0` | 9943 | 5.02 | 8.52 | 6.40
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 9188 | 5.51 | 9.03 | 6.99
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7945 | 8.04 | 8.11 | 8.07
| [quart](https://pypi.org/project/quart/) `0.14.1` | 3742 | 17.45 | 18.37 | 17.12
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3460 | 18.46 | 18.64 | 18.49
| [django](https://pypi.org/project/django/) `3.2.2` | 1825 | 33.30 | 38.98 | 35.04


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.4` | 6164 | 8.07 | 14.17 | 10.35
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 4658 | 10.57 | 18.87 | 13.76
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 4368 | 11.24 | 19.77 | 14.68
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 3732 | 13.60 | 22.97 | 17.25
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 2546 | 19.36 | 34.32 | 25.10
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2377 | 26.87 | 27.29 | 26.91
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2273 | 28.07 | 28.47 | 28.16
| [fastapi](https://pypi.org/project/fastapi/) `0.65.0` | 2262 | 21.81 | 38.68 | 28.23
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 1545 | 38.23 | 46.56 | 41.41
| [quart](https://pypi.org/project/quart/) `0.14.1` | 1417 | 45.18 | 46.87 | 45.14
| [django](https://pypi.org/project/django/) `3.2.2` | 988 | 62.76 | 71.94 | 64.68


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.4` | 11018 | 4.55 | 7.71 | 5.77
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 10317 | 4.83 | 8.27 | 6.17
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 10139 | 4.88 | 8.39 | 6.28
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 8238 | 6.04 | 10.40 | 7.73
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 7728 | 6.41 | 10.87 | 8.29
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 7202 | 6.76 | 11.83 | 9.04
| [fastapi](https://pypi.org/project/fastapi/) `0.65.0` | 6319 | 7.79 | 13.65 | 10.09
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4654 | 13.76 | 14.04 | 13.75
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2913 | 21.90 | 22.23 | 21.97
| [quart](https://pypi.org/project/quart/) `0.14.1` | 2422 | 25.98 | 26.79 | 26.41
| [django](https://pypi.org/project/django/) `3.2.2` | 1516 | 41.35 | 46.52 | 42.15

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.4` | 547320 | 5.13 | 8.71 | 6.47
| [muffin](https://pypi.org/project/muffin/) `0.70.1` | 477660 | 6.19 | 10.7 | 7.92
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 446820 | 7.24 | 12.17 | 9.15
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 374085 | 9.73 | 16.86 | 12.44
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 341130 | 16.21 | 21.45 | 18.33
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 319260 | 7.72 | 13.22 | 9.99
| [fastapi](https://pypi.org/project/fastapi/) `0.65.0` | 277860 | 11.54 | 20.28 | 14.91
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 224640 | 16.22 | 16.48 | 16.24
| [tornado](https://pypi.org/project/tornado/) `6.1` | 129690 | 22.81 | 23.11 | 22.87
| [quart](https://pypi.org/project/quart/) `0.14.1` | 113715 | 29.54 | 30.68 | 29.56
| [django](https://pypi.org/project/django/) `3.2.2` | 64935 | 45.8 | 52.48 | 47.29

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)