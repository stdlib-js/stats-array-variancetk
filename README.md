<!--

@license Apache-2.0

Copyright (c) 2025 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# variancetk

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Calculate the [variance][variance] of an array using a one-pass textbook algorithm.

<section class="intro">

The population [variance][variance] of a finite size population of size `N` is given by

<!-- <equation class="equation" label="eq:population_variance" align="center" raw="\sigma^2 = \frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2" alt="Equation for the population variance."> -->

```math
\sigma^2 = \frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2
```

<!-- </equation> -->

where the population mean is given by

<!-- <equation class="equation" label="eq:population_mean" align="center" raw="\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i" alt="Equation for the population mean."> -->

```math
\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i
```

<!-- </equation> -->

Often in the analysis of data, the true population [variance][variance] is not known _a priori_ and must be estimated from a sample drawn from the population distribution. If one attempts to use the formula for the population [variance][variance], the result is biased and yields a **biased sample variance**. To compute an **unbiased sample variance** for a sample of size `n`,

<!-- <equation class="equation" label="eq:unbiased_sample_variance" align="center" raw="s^2 = \frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2" alt="Equation for computing an unbiased sample variance."> -->

```math
s^2 = \frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2
```

<!-- </equation> -->

where the sample mean is given by

<!-- <equation class="equation" label="eq:sample_mean" align="center" raw="\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i" alt="Equation for the sample mean."> -->

```math
\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i
```

<!-- </equation> -->

The use of the term `n-1` is commonly referred to as Bessel's correction. Note, however, that applying Bessel's correction can increase the mean squared error between the sample variance and population variance. Depending on the characteristics of the population distribution, other correction factors (e.g., `n-1.5`, `n+1`, etc) can yield better estimators.

</section>

<!-- /.intro -->



<section class="usage">

## Usage

```javascript
import variancetk from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-array-variancetk@v0.1.1-deno/mod.js';
```

#### variancetk( x\[, correction] )

Computes the variance of an array.

```javascript
var x = [ 1.0, -2.0, 2.0 ];

var v = variancetk( x );
// returns ~4.3333
```

The function has the following parameters:

-   **x**: input array.
-   **correction**: degrees of freedom adjustment. Setting this parameter to a value other than `0` has the effect of adjusting the divisor during the calculation of the [variance][variance] according to `N-c` where  `N` corresponds to the number of array elements and `c` corresponds to the provided degrees of freedom adjustment. When computing the [variance][variance] of a population, setting this parameter to `0` is the standard choice (i.e., the provided array contains data constituting an entire population). When computing the unbiased sample [variance][variance], setting this parameter to `1` is the standard choice (i.e., the provided array contains data sampled from a larger population; this is commonly referred to as Bessel's correction). Default: `1.0`.

By default, the function computes the sample [variance][variance]. To adjust the degrees of freedom when computing the [variance][variance], provide a `correction` argument.

```javascript
var x = [ 1.0, -2.0, 2.0 ];

var v = variancetk( x, 0.0 );
// returns ~2.8889
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   If provided an empty array, the function returns `NaN`.
-   If provided a `correction` argument which is greater than or equal to the number of elements in a provided input array, the function returns `NaN`.
-   The function supports array-like objects having getter and setter accessors for array element access (e.g., [`@stdlib/array-base/accessor`][@stdlib/array/base/accessor]).
-   Some caution should be exercised when using the one-pass textbook algorithm. Literature overwhelmingly discourages the algorithm's use for two reasons: 1) the lack of safeguards against underflow and overflow and 2) the risk of catastrophic cancellation when subtracting the two sums if the sums are large and the variance small. These concerns have merit; however, the one-pass textbook algorithm should not be dismissed outright. For data distributions with a moderately large standard deviation to mean ratio (i.e., **coefficient of variation**), the one-pass textbook algorithm may be acceptable, especially when performance is paramount and some precision loss is acceptable (including a risk of returning a negative variance due to floating-point rounding errors!). In short, no single "best" algorithm for computing the variance exists. The "best" algorithm depends on the underlying data distribution, your performance requirements, and your minimum precision requirements. When evaluating which algorithm to use, consider the relative pros and cons, and choose the algorithm which best serves your needs.

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import discreteUniform from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-array-discrete-uniform@deno/mod.js';
import variancetk from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-array-variancetk@v0.1.1-deno/mod.js';

var x = discreteUniform( 10, -50, 50, {
    'dtype': 'float64'
});
console.log( x );

var v = variancetk( x );
console.log( v );
```

</section>

<!-- /.examples -->

* * *

<section class="references">

## References

-   Ling, Robert F. 1974. "Comparison of Several Algorithms for Computing Sample Means and Variances." _Journal of the American Statistical Association_ 69 (348). American Statistical Association, Taylor & Francis, Ltd.: 859–66. doi:[10.2307/2286154][@ling:1974a].

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-array-variancetk.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-array-variancetk

[test-image]: https://github.com/stdlib-js/stats-array-variancetk/actions/workflows/test.yml/badge.svg?branch=v0.1.1
[test-url]: https://github.com/stdlib-js/stats-array-variancetk/actions/workflows/test.yml?query=branch:v0.1.1

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-array-variancetk/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-array-variancetk?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-array-variancetk.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-array-variancetk/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-array-variancetk/tree/deno
[deno-readme]: https://github.com/stdlib-js/stats-array-variancetk/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/stats-array-variancetk/tree/umd
[umd-readme]: https://github.com/stdlib-js/stats-array-variancetk/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/stats-array-variancetk/tree/esm
[esm-readme]: https://github.com/stdlib-js/stats-array-variancetk/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/stats-array-variancetk/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-array-variancetk/main/LICENSE

[variance]: https://en.wikipedia.org/wiki/Variance

[@ling:1974a]: https://doi.org/10.2307/2286154

[@stdlib/array/base/accessor]: https://github.com/stdlib-js/array-base-accessor/tree/deno

</section>

<!-- /.links -->
