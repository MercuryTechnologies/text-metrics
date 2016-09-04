# Text Metrics

[![License BSD3](https://img.shields.io/badge/license-BSD3-brightgreen.svg)](http://opensource.org/licenses/BSD-3-Clause)
[![Hackage](https://img.shields.io/hackage/v/text-metrics.svg?style=flat)](https://hackage.haskell.org/package/text-metrics)
[![Stackage Nightly](http://stackage.org/package/text-metrics/badge/nightly)](http://stackage.org/nightly/package/text-metrics)
[![Stackage LTS](http://stackage.org/package/text-metrics/badge/lts)](http://stackage.org/lts/package/text-metrics)
[![Build Status](https://travis-ci.org/mrkkrp/text-metrics.svg?branch=master)](https://travis-ci.org/mrkkrp/text-metrics)
[![Coverage Status](https://coveralls.io/repos/mrkkrp/text-metrics/badge.svg?branch=master&service=github)](https://coveralls.io/github/mrkkrp/text-metrics?branch=master)

The library provides efficient implementations of various strings metrics.
It works with strict `Text` values and returns either `Natural` numbers
(because the metrics cannot be negative), or `Ratio Natural` values because
returned values are rational non-negative numbers by definition.

The current version of the package implements:

* [Levenshtein distance](http://en.wikipedia.org/wiki/Levenshtein_distance)
* [Normalized Levenshtein distance](http://en.wikipedia.org/wiki/Levenshtein_distance)
* [Damerau-Levenshtein distance](http://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)
* [Normalized Damerau-Levenshtein distance](http://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)
* [Hamming distance](http://en.wikipedia.org/wiki/Hamming_distance)

TODO list:

* [Overlap coefficient](http://en.wikipedia.org/wiki/Overlap_coefficient)
* [Jaccard similarity coefficient](http://en.wikipedia.org/wiki/Jaccard_index)
* [Jaro distance](http://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance)
* [Jaro-Winkler distance](http://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance)

## Comparison with the `edit-distance` package

There
is [`edit-distance`](https://hackage.haskell.org/package/edit-distance)
package whose scope overlaps with the scope of this package. The differences
are:

* `edit-distance` allows to specify costs for every operation when
  calculating Levenshtein distance (insertion, deletion, substitution, and
  transposition). This is rarely needed though in real-world applications,
  IMO.

* `edit-distance` only provides single Levenshtein distance, `text-metrics`
  aims to provide implementations of most string metrics algorithms.

* `edit-distance` works on `Strings`, while `text-metrics` works on strict
  `Text` values.

* As `README.md` of `edit-distance` states, “[the algorithms] have been
  fairly heavily optimized”, which is apparently true, yet the
  `text-metrics` is faster for short strings (under 64 characters) and even
  faster for longer strings (scales better). How much faster? For short
  strings more than ×2.5, and about ×4 for longer strings.

## Implementation

All “meat” of the algorithms is written is C in a rather straightforward
way. Levenshtein variants are based on the “iterative algorithm with two
matrix rows” from Wikipedia with additional improvement that we do not copy
current row of distances into previous row, but just swap the pointers
(which is OK, since the arrays have equal length and current row will be
overwritten in the next iteration anyway).

Normalized versions are defined as thin (inlined) Haskell wrappers.

## License

Copyright © 2016 Mark Karpov

Distributed under BSD 3 clause license.
