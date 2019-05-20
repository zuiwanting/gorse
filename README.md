Language: English | [中文](https://github.com/zhenghaoz/gorse/blob/master/README.zh-cn.md)

# gorse: Go Recommender System Engine

<img width=160 src="https://img.sine-x.com/gorse.png"/>

| Build | Build (AVX2) | Coverage | Report | GoDoc | RTD |
|---|---|---|---|---|---|
| [![Build Status](https://travis-matrix-badges.herokuapp.com/repos/zhenghaoz/gorse/branches/master/1)](https://travis-ci.org/zhenghaoz/gorse) | [![Build Status](https://travis-matrix-badges.herokuapp.com/repos/zhenghaoz/gorse/branches/master/2)](https://travis-ci.org/zhenghaoz/gorse) | [![codecov](https://codecov.io/gh/zhenghaoz/gorse/branch/master/graph/badge.svg)](https://codecov.io/gh/zhenghaoz/gorse) | [![Go Report Card](https://goreportcard.com/badge/github.com/zhenghaoz/gorse)](https://goreportcard.com/report/github.com/zhenghaoz/gorse) | [![GoDoc](https://godoc.org/github.com/zhenghaoz/gorse?status.svg)](https://godoc.org/github.com/zhenghaoz/gorse) | [![Documentation Status](https://readthedocs.org/projects/gorse/badge/?version=latest)](https://gorse.readthedocs.io/en/latest/?badge=latest) |

`gorse` is an offline recommender system backend based on collaborative filtering written in Go. 

This project is aim to provide a high performance, easy-to-use, programming language irrelevant recommender micro-service based on collaborative filtering. We could build a simple recommender system on it, or set up a more sophisticated recommender system using candidates generated by it. It features:

- **Pipeline**: supports data loading, data splitting, model training, model evaluation and model selection.
- **Tools**: provides the data import/export tool, model evaluation tool and RESTful recomender server.
- **Optimization**: accelerates computations by SIMD instructions and multi-threading.

## Install

To start using gorse, install Go and run `go get`:

```bash
go get github.com/zhenghaoz/gorse/...
```

It will download all packages and build the `gorse` command line into your `$GOBIN` path.

If your CPU supports AVX2 and FMA3 instructions, use the `avx2` build tag to enable AVX2 and FMA3 instructions.

```bash
go get -tags='avx2' github.com/zhenghaoz/gorse/...
```

## Usage

```
gorse is an offline recommender system backend based on collaborative filtering written in Go.

Usage:
  gorse [flags]
  gorse [command]

Available Commands:
  export-feedback Export feedback to CSV
  export-items    Export items to CSV
  help            Help about any command
  import-feedback Import feedback from CSV
  import-items    Import items from CSV
  serve           Start a recommender sever
  test            Test a model by cross validation
  version         Check the version

Flags:
  -h, --help   help for gorse

Use "gorse [command] --help" for more information about a command.
```

It's easy to setup a recomendation service with `gorse`. 

- **Step 1**: Import feedback and items.

```bash
gorse import-feedback ~/.gorse/gorse.db u.data --sep $'\t'
gorse import-items ~/.gorse/gorse.db u.item --sep '|'
```

It imports feedback and items from CSV files into the database file `~/.gorse/gorse.db`. The low level storage engine is implemented by BoltDB. `u.data` is the CSV file of ratings in [MovieLens 100K](https://grouplens.org/datasets/movielens/100k/) dataset and `u.item` is the CSV file of items in [MovieLens 100K](https://grouplens.org/datasets/movielens/100k/) dataset. All CLI tools are listed in the [CLI-Tools](https://github.com/zhenghaoz/gorse/wiki/CLI-Tools) section of Wiki.

- **Step 2**: Start a server.

```bash
./gorse server -c config.toml
```

It loads configurations from [config.toml](https://github.com/zhenghaoz/gorse/blob/master/example/file_config/config.toml) and start a recommendation server. It may take a while to generate all recommendations. Detailed information about configuration is in the [Configuration](https://github.com/zhenghaoz/gorse/wiki/Configuration) section of Wiki. Before set hyper-parameters for the model, it is useful to test the performance of chosen hyper-parameters by the [model evaluation tool](https://github.com/zhenghaoz/gorse/wiki/CLI-Tools#cross-validation-tool).

- **Step 3**: Get recommendations.

```bash
curl 127.0.0.1:8080/recommends/1?number=5
```

It requests 5 recommended items for the 1-th user. The response might be:

```
[
    {
        "ItemId": 202,
        "Score": 2.901297852545712
    },
    {
        "ItemId": 151,
        "Score": 2.8871064286482864
    },
    ...
]
```

`"ItemId"` is the ID of the item and `"Score"` is the score generated by the recommendation model used to rank. See [RESTful APIs](https://github.com/zhenghaoz/gorse/wiki/RESTful-APIs) in Wiki for more information about RESTful APIs.

## Document

- Visit [GoDoc](https://godoc.org/github.com/zhenghaoz/gorse) for detailed documentation of codes.
- Visit [ReadTheDocs](https://gorse.readthedocs.io/) for tutorials, examples and usages.

## Performance

`gorse` is much faster than [Surprise](http://surpriselib.com/), and comparable to [librec](https://www.librec.net/) while using less memory space than both of them. The memory efficiency is achieved by sophisticated data structures. 

- Cross-validation of SVD on MovieLens 100K \[[Source](https://github.com/zhenghaoz/gorse/tree/master/example/benchmark_perf)\]:

<img width=320 src="https://img.sine-x.com/perf_time_svd_ml_100k.png"><img width=320 src="https://img.sine-x.com/perf_mem_svd_ml_100k.png">

- Cross-validation of SVD on MovieLens 1M \[[Source](https://github.com/zhenghaoz/gorse/tree/master/example/benchmark_perf)\]:

<img width=320 src="https://img.sine-x.com/perf_time_svd_ml_1m.png"><img width=320 src="https://img.sine-x.com/perf_mem_svd_ml_1m.png">

## Contributors

[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/0)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/0)[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/1)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/1)[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/2)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/2)[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/3)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/3)[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/4)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/4)[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/5)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/5)[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/6)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/6)[![](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/images/7)](https://sourcerer.io/fame/zhenghaoz/zhenghaoz/gorse/links/7)

Any kind of contribution is expected: report a bug, give a advice or even create a pull request.

## Acknowledgments

`gorse` is inspired by following projects:

- [Guibing Guo's librec](https://github.com/guoguibing/librec)
- [Nicolas Hug's Surprise](https://github.com/NicolasHug/Surprise)
- [Golang Samples's gopher-vector](https://github.com/golang-samples/gopher-vector)

## Limitations

`gorse` has limitations and might not be applicable to some scenarios:

- **No Scalability**: `gorse` is a recommendation service on a single host, so it's unable to handle large data.
- **No Features**: `gorse` exploits interactions between items and users while features of items and users are ignored.
