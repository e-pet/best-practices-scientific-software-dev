# Best Practices for Scientific Software Development

### 1. Make your research reproducible
An absolutely minimal requirement for good research is for it to always be reproducible. [And this will also save you a *lot* of time and nerves in the long run.]

*Disclaimer*: This section is strongly inspired by http://kbroman.org/steps2rr/.

* **Organize your data and code.**
    * Gather all data and code for your project in a single directory.
    * Separate raw data files from derived data.
    * Separate data from code.
    * Use relative paths.
    * Choose meaningful file names.
    * Write ReadMe files.


* **Make everything an easily runnable script.**
    * Always start your analysis from the raw data and make every data conversion or filtering a step in a script.
    * Never hand-edit data files.
    * Don't export figures or results manually.
    * Don't require manual intervention (changing variable values etc.) to reproduce results.
    * If there is a non-obvious chain of commands that needs to be run, make it a script! (Optionally: a [Makefile](http://kbroman.org/steps2rr/pages/automate.html).)
    * If you have found an interesting example, save it as a script. Save seeds for random number generators.


* **Store the results of your analyses.**
    * Store the results of your analyses as log files, data files (.csv/.mat), and image files.
    * Create automatically updated research reports.
        * Mix code samples, explanatory text or maths, and numerical or graphical results (i.e., figures) in a single, beautifully rendered research document.
        * Check out [Jupyter Notebooks](http://jupyter.org/) for python.
        * Check out the [Matlab Live Editor](https://de.mathworks.com/products/matlab/live-editor.html) for Matlab.
        * Check out [RMarkdown](https://rmarkdown.rstudio.com/index.html) for R.
    * Fixes the need to rerun (old, currently broken...) scripts to show results to collaborators: Always have something to show for discussions available.
    * Especially important when analyses take longer than some seconds.


* **Make your scripts understandable.**
    * Document rationale for parameter values, examples, choice of algorithms, etc.
    * Document assumptions made in the code: data types of input arguments, dimensionality of variables, structure of data, etc.
    * Document limitations and known bugs: Only works in the 1-D case, not correct in some improbable corner case, etc.
    * Provide literature references (also to your own thesis) for algorithms.

### 2. Make your research correct
When implementing complex scientific analyses, it can be very difficult to ensure that the results are actually *correct*: 
 * When analyses are complex, intuition about the validity of results is often lacking.
 * Often, there is no reference to compare with (since we're doing new research!).
 * When performing grouping results (i.e., calculating mean +/- standard deviation of results in a number of experiments or patients), it becomes impossible to judge whether individual results appear correct.
 * There can be very subtle and difficult to trace bugs that make results *appear* correct when they are not: Performing element-wise instead of matrix multiplication, accidentally reusing previous variable values, using sine instead of cosinde, etc.

*Really* making sure that everything works correctly is very hard, but there are some steps that can greatly increase both your own and your collaborators' confidence in the validity of your analyses:
 * **Understand what your code is doing.**
   * *Never* reuse code of somebody else (example code on the internet, software toolbox of other researchers, software libraries) without exactly understanding what it is doing internally.
   * Examples or legacy code may have been intended for different use cases. Chances are high you're using it wrongly.
   * For many standard algorithms (linear regression, Kalman filtering, etc.), there are different versions of implementing them, each with different pros and cons.
   * Software libraries are buggy! (Yes, even the big ones.)
   * If you can't explain how the underlying algorithm works, noone will believe in your results.


 * **Make your code easily understandable and maintainable.**
   * Greatly reduces the chance of introducing bugs!
   * Document assumptions, limitations, etc. (see above).
   * Separate examples / data analysis from algorithm implementation.
   * Use meaningful variable and function names.
   * Avoid very long scripts / functions.
   * Make your code modular: separate functionalities, e.g. data import, data analysis, and results reporting. It should be easy to understand what your code is doing in which order at a high level.
   * Don't optimize for performance, optimize for understandability, maintainability and correctness.
   * Break up complex and hard to understand pieces of code into several smaller, easy to understand chunks of code (functions).
   * Extract duplicate code into functions.


 * **Check intermediate variable values.**
   * At the very least, step through the code one line at a time for some example, inspect all variables and make sure that they have meaningful data types and values. (*Learn how to use a debugger!*)
   * Make intermediate variable values always available without manual debugging: report them in a log file, generate figures, etc.
   * Explicitly implement assertions about variable values and data types in your code: Expected value ranges, dimensions, etc. Python example:
```python
assert isinstance(variable, np.ndarray)
assert variable.shape == (N, 1) and np.all(0 <= variable <= 1)
```


* **Implement unit tests.**
  * Unit tests are test cases for which the correct answer is *known*, so that the validity of an algorithm can be tested at least for this test case.
  * Unit tests can (and should) be implemented at different levels: for very low-level, atomic functions (i.e., a simple filtering algorithm or some data restructuring function), and for higher-level, more complex algorithms.
  * Often, correct results can be calculated manually for very simple examples: low dimension, (very) few data points, etc.
  * Useful test cases often include corner cases: 0, inf or nan values.
  * Comparison with a reference algorithm is a good option to enable testing more complex cases. For example, for nonlinear filtering algorithms, it is expected that they yield results equivalent to the standard linear Kalman filter *for linear examples*.
  * If the exact results are too difficult to compute, one can still check assumptions that should be fulfilled: dimensionality, value range, monotonicity, etc.


### 3. Stick to the pathfinder rule ###
 * Always respecting all of the above recommendations is *hard*.
 * You want to work on good scientific results, not on perfect software quality, after all.
 * Try to improve things one at a time: When you notice something that isn't optimal, try to fix that one thing right now. Over time, your code quality will get better and better.
 * Make following good code practices a *habit*.

&nbsp; 
#### References ####
 * http://clean-code-developer.de/
 * [Axelrod (2014): Minimizing bugs in cognitive neuroscience programming](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4269119/)
 * [Software Carpentry: Defensive Programming in Matlab](http://swcarpentry.github.io/matlab-novice-inflammation/06-defensive/)
&nbsp; 
&nbsp; 
---
This document has been written in [Markdown][df1] using the [Dillinger web editor](https://dillinger.io). Markdown is a lightweight markup language based on the formatting conventions that people naturally use in email.

Eike Petersen, May 2018.

   [df1]: <http://daringfireball.net/projects/markdown/>
