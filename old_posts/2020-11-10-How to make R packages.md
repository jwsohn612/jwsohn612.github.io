---
layout: post
title: How to make R packages with Rcpp Armadilo?
subtitle: To be a R developer
date: 2021-05-28 00:00:00 
---

## R 패키지 만들기 (Rcpp, RcppArmadillo)

이번 포스팅에서는 Rstudio를 이용하여 R패키지 만들기를 해보려 한다. 특별히, `C++ `가 패키지 빌딩에 사용되는 경우를 다루어 보려고 한다. R패키지와 관련한 자세한 사항은 다음 문서에서 참조하도록 한다. [R packages](https://r-pkgs.org/) 

**개발환경**

- Window 10
- R (>=3.6.3)



#### 1. 패키지 설치하기

먼저 패키지 작성에 유용한 두 패키지를 설치한다. Rstudio를 실행한 뒤 패키지 만들기에 필요한 다음의 두 패키지를 설치한다.  

- devtools: 패키지 생성과 배포에 필요한 전반적인 기능을 제공해준다.
- roxygen2: 패키지 도큐멘테이션을 편리하게 만들어 준다. 

```R
> install.packages(c("devtools","roxygen2"))
```



#### 2. 패키지 뼈대 만들기

C++코드를 R에서 동작시킬 수 있는 패키지 뼈대를 설치한다. 만드려는 패키지는 다음과 같다.

- 두 정수의 합을 구하는 함수는 C++코드로
- 여러 개의 정수 집합에 대해 위 함수를 적용하는 함수는 R코드로 설계할 것이다. 

```R
> install.packages(c("Rcpp"))
> # setwd("your_working_directory")
> Rcpp::Rcpp.package.skeleton("myvectorsum")
```

위 코드를 실행시키면 `myvectorsum`이란 디렉토리가 새로 생성되고 몇 개의 폴더와 파일이 생성된다. 

- `R` : 사용하는 R함수들은 이 폴더에 집합시킨다.
- `src` : 사용하는 C++함수들은 이 폴더에 집합시킨다. 
- `man` : 사용하는 함수들의 문서(*.Rd)들을 집합시킨다. 
- `NAMESPACE` : 사용할 함수들의 이름공간이다. 
- `DESCRIPTION` : 패키지를 설명하는 문서이다. 

`RcppArmadillo`를 이용하고 싶으면, 아래를 실행시키면 된다.

```R
> RcppArmadillo::RcppArmadillo.package.skeleton("myvectorsum")
```





#### 3. 새로운 project 시작 

만드려는 패키지 myvectorsum에 새로운 R project를 시작한다. 프로젝트를 정의하면 패키지 만드는 과정을 관리하는데 편리하다. `File > New Project > Existing Directory > "~/R/workspace/myvectorsum"` .



#### 4. 최초 문서화 

아래 함수를 호출하여 각 폴더에 존재하는 함수들을 정리해준다. 예를 들어, `NAMESPACE`를 정리하고, 각 함수들의 `Rd`파일을 만들어 `man`폴더에 보관한다. 위의 문서화를 하는데 보통 `roxygen2`을 이용한다. 

```R
> devtools::document()
```



#### 5. 함수 작성

**우선 C++코드를 작성하자.**  C++코드를 어떻게 작성해야하는지는 다루지 않겠다. 여기서 중요한 건, `// [[Rcpp::export]]`를 입력해주는게 중요하다. 저 문장을 써주어야 패키지 내에서 사용이 가능하다. 작성한 코드는 `src`폴더에 저장한다. 

```c++
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
int sum_two(int x, int y) {
    int z = x + y;
    return z ;
}

```

**다음으로 R코드를 작성하자.** `#'`를 문두에 놓아 `roxygen2`문법을 사용할 수 있다. 이 패키지의 문법에 대한 설명은 다른 포스팅들이 많으니 그것들을 참고하길 바랍니다. `@useDynLib package-name`와 `@importFrom Rcpp evalCpp`를 명시해야 C++코드를 사용할 수 있다. 또한, `@export`를 해주어야 아래 함수를 유저가 사용할 수 있습니다. 마찬가지로 C++함수에 대해서도 roxygen2 문법을 적용할 수 있다. `.onUnload`함수는 `*.dll`을 실행시키지 않는다.

```R
#' @useDynLib myvectorsum
#' @importFrom Rcpp evalCpp
NULL

.onUnload <- function (libpath) {
  library.dynam.unload("myvectorsum", libpath)
}

#' My function
#' 
#' @param x  A list that each element in this has two integers.
#' @return Integer
#' 
#' @example 
#' x <- list(c(1L,2L),c(-4L,3L),c(3L,2L))
#' do_sum(x)
#' 
#' @export
do_sum <- function(x) {
  purrr::map(x, ~ sum_two(.x[1], .x[2]))
}

```

그리고 아래 함수를 실행한다. 만약, `NAMESPACE`가 업데이트 되지 않는다면, 제거하고 다시 아래 코드를 실행시키면, 업데이트 된 버전이 자동생성된다. 

```R
> devtools::document()
```



#### 6. 패키지 빌드

`ctrl+shift+B`를 입력하여 패키지를 로컬환경에서 빌드한다. 이후 패키지가 정상 동작하는지 체크한다.

```R
> library(myvectorsum)
> x <- list(c(1L,2L),c(-4L,3L),c(3L,2L))
> do_sum(x)
[[1]]
[1] 3

[[2]]
[1] -1

[[3]]
[1] 5
```

패키지가 정상 동작하면, 아래의 코드로 roxygen2문법이 무엇을 했는지 살펴보자. 

```R
>??do_sum
```



#### 7. Github과 연결

깃헙에 `repo`를 만들고 로컬에 존재하는 R패키지 디렉토리 `~/myvectorsum`과 연결하는 과정은 설명하지 않겠다. 연결 뒤 해줄 일은 ignore파일을 설정하는 것이다. 

- `.gitignore` 파일 편집

  ```
  myvectorsum.Rproj
  .Rhistory
  .Rproj.user
  ```

- `.Rbuildignore` 파일 편집

  ```
  ^.*\.Rproj$
  ^\.Rproj\.user$
  
  myvectorsum.Rproj
  .gitignore
  README.md
  .Rhistory
  ```



#### 8. 패키지 설치 확인

위의 과정을 모두 완료했다면, R에서 패키지를 다음과 같이 설치할 수 있다.

```R
> devtools::install_github("jwsohn612/myvectorsum")
```



