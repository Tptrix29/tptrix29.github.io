---
title:  "R Package Development"
date:   2023-11-25 -0500
categories: programming
tags: R
header:
    teaser: /assets/img/training-guidence.png
---

Notes for R package building

## Tools
- `devtools`: 
- `roxygen2`: documentation
- `R6Class`: OOP (Object Oriented Programming) Implementation
- `pkgdown`: setup package website 

## Pipeline
1. Set-up
2. Documentation
3. Testing
4. Build


## C++ plugin usage
### Package
- `Rcpp`: interface for basic data type transmission
- `RcppEigen`: interface for C++ linear algebra library ([Eigen]())

### Config
- `Make.vars`: UNIX 
- `Make.vars.win`: windows
