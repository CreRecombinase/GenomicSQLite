# Genomics Extension for SQLite

### ("GenomicSQLite")

This [SQLite3 loadable extension](https://www.sqlite.org/loadext.html) adds features to the [ubiquitous](https://www.sqlite.org/mostdeployed.html) embedded RDBMS supporting applications in genome bioinformatics:

* genomic range indexing for overlap queries & joins
* in-SQL utility functions, e.g. reverse-complement DNA, parse "chr1:2,345-6,789"
* automatic streaming storage compression (also available [standalone](https://github.com/mlin/sqlite_zstd_vfs))
* reading directly from HTTP(S) URLs (also available [standalone](https://github.com/mlin/sqlite_web_vfs))
* pre-tuned settings for "big data"

This October 2020 poster discusses the context and long-run ambitions:

[<img src="https://mlin.github.io/GenomicSQLite/GA4GH_8thPlenary_poster_mlin_v2.thumb.jpeg" width="400" alt="GenomicSQLite Poster"/>](https://mlin.github.io/GenomicSQLite/GA4GH_8thPlenary_poster_mlin_v2.pdf)

Our **[Colab notebook](https://colab.research.google.com/drive/1OlHPOcRQBhDmEnS1wtOdtUGDkcD7LtKx?usp=sharing)** demonstrates key features with Python, one of several language bindings.

**USE AT YOUR OWN RISK:** This project is not associated with the SQLite developers. The database storage extensions are designed to preserve ACID transaction safety, but they're young and unlikely to be totally bug-free.

## [Installation & Programming Guide](https://mlin.github.io/GenomicSQLite/)

**Start Here 👉 [full documentation site](https://mlin.github.io/GenomicSQLite/)**

We supply the extension [prepackaged](https://github.com/mlin/GenomicSQLite/releases) for Linux x86-64 and macOS Catalina. An up-to-date version of SQLite itself is also required, as specified in the docs.

Programming language support:

* C/C++
* Python &ge;3.6
* Java & JVM languages
* Rust

More to come. (Help wanted; see [Language Bindings Guide](https://mlin.github.io/GenomicSQLite/bindings/))

## Building from source

[![build](https://github.com/mlin/GenomicSQLite/workflows/build/badge.svg?branch=main)](https://github.com/mlin/GenomicSQLite/actions?query=workflow%3Abuild)

Most will prefer to install a pre-built shared library (see above). To build from source, see our [Actions yml (Ubuntu 20.04)](https://github.com/mlin/GenomicSQLite/blob/main/.github/workflows/build.yml) or [Dockerfile (CentOS 7)](https://github.com/mlin/GenomicSQLite/blob/main/Dockerfile) used to build the more-portable releases. Briefly, you'll need:

* C++11 build system
* CMake &ge; 3.14
* Dev packages: SQLite &ge; 3.31.0, Zstandard &ge; 1.3.4, libcurl

And incantations:

```
cmake -DCMAKE_BUILD_TYPE=Release -B build .
cmake --build build -j 4 --target genomicsqlite
```

...generating `build/libgenomicsqlite.so`. To run the test suite, you'll furthermore need:

* htslib &ge; 1.9, samtools, and tabix
* pigz
* Python &ge; 3.6 and packages: pytest pytest-xdist pre-commit black pylint flake8 
* JDK, mvn, rust
* clang-format & cppcheck

to:

```
pre-commit run --all-files  # formatters+linters
cmake -DCMAKE_BUILD_TYPE=Debug -B build .
cmake --build build -j 4
env -C build ctest -V
```
