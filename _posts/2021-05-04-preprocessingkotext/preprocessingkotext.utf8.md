---
title: "Preprocessing Example : Korean"
description: |
  Preprocessing Example For Korean in Text Mining
author:
  - name: Yeongeun Jeon
date: 05-04-2021
categories: Text Mining
output: 
  distill::distill_article:
        toc: TRUE
---





> 다루는 언어가 `한국어`일 때 전처리 과정을 보여준다.

<br />

> # **Contents**

- [영어표현 확인][**영어표현 확인**]
- [숫자표현 제거][**숫자표현 제거**]
- [문장부호 및 특수문자 제거][**문장부호 및 특수문자 제거**]
- [공란처리][**공란처리**]
- [토큰화][**토큰화**]
- [불용어 제거][**불용어 제거**]
- [어근 동일화][**어근 동일화**]
- [Document-Term Matrix][**Document-Term Matrix**]

<br />



<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='fu'>pacman</span><span class='fu'>::</span><span class='fu'><a href='https://rdrr.io/pkg/pacman/man/p_load.html'>p_load</a></span><span class='op'>(</span><span class='st'>"KoNLP"</span>, 
               <span class='st'>"tm"</span>, 
               <span class='st'>"stringr"</span><span class='op'>)</span>

<span class='co'># useSystemDic() # 시스템 사전 설정</span>
<span class='co'># useSejongDic() # 세종 사전 설정</span>정
<span class='fu'>useNIADic</span><span class='op'>(</span><span class='op'>)</span> <span class='co'># NIADic 사전 설정 (추천)</span></code></pre></div>

```
Backup was just finished!
utf8        (1.1.4  -> 1.2.1  ) [CRAN]
crayon      (1.3.4  -> 1.4.1  ) [CRAN]
glue        (1.4.1  -> 1.4.2  ) [CRAN]
vctrs       (0.3.0  -> 0.3.8  ) [CRAN]
pillar      (1.4.3  -> 1.6.1  ) [CRAN]
magrittr    (1.5    -> 2.0.1  ) [CRAN]
fansi       (0.4.1  -> 0.4.2  ) [CRAN]
colorspace  (1.4-1  -> 2.0-1  ) [CRAN]
viridisLite (0.3.0  -> 0.4.0  ) [CRAN]
R6          (2.4.1  -> 2.5.0  ) [CRAN]
labeling    (0.3    -> 0.4.2  ) [CRAN]
farver      (2.0.3  -> 2.1.0  ) [CRAN]
xfun        (0.19   -> 0.23   ) [CRAN]
digest      (0.6.25 -> 0.6.27 ) [CRAN]
stringi     (1.4.4  -> 1.6.2  ) [CRAN]
mime        (0.9    -> 0.10   ) [CRAN]
highr       (0.8    -> 0.9    ) [CRAN]
fastmap     (1.0.1  -> 1.1.0  ) [CRAN]
cachem      (1.0.4  -> 1.0.5  ) [CRAN]
tibble      (3.0.1  -> 3.1.2  ) [CRAN]
scales      (1.1.0  -> 1.1.1  ) [CRAN]
isoband     (0.2.0  -> 0.2.4  ) [CRAN]
tinytex     (0.19   -> 0.31   ) [CRAN]
jsonlite    (1.6.1  -> 1.7.2  ) [CRAN]
htmltools   (0.4.0  -> 0.5.1.1) [CRAN]
knitr       (1.28   -> 1.33   ) [CRAN]
Rcpp        (1.0.3  -> 1.0.6  ) [CRAN]
DBI         (1.1.0  -> 1.1.1  ) [CRAN]
ggplot2     (3.3.0  -> 3.3.3  ) [CRAN]
data.table  (1.12.8 -> 1.14.0 ) [CRAN]
rmarkdown   (2.6    -> 2.8    ) [CRAN]

  There are binary versions available but the source versions
  are later:
        binary source needs_compilation
pillar   1.6.0  1.6.1             FALSE
xfun      0.22   0.23              TRUE
stringi  1.6.1  1.6.2              TRUE
cachem   1.0.4  1.0.5              TRUE
tibble   3.1.1  3.1.2              TRUE

package 'utf8' successfully unpacked and MD5 sums checked
package 'crayon' successfully unpacked and MD5 sums checked
package 'glue' successfully unpacked and MD5 sums checked
package 'vctrs' successfully unpacked and MD5 sums checked
package 'magrittr' successfully unpacked and MD5 sums checked
package 'fansi' successfully unpacked and MD5 sums checked
package 'colorspace' successfully unpacked and MD5 sums checked
package 'viridisLite' successfully unpacked and MD5 sums checked
package 'R6' successfully unpacked and MD5 sums checked
package 'labeling' successfully unpacked and MD5 sums checked
package 'farver' successfully unpacked and MD5 sums checked
package 'digest' successfully unpacked and MD5 sums checked
package 'mime' successfully unpacked and MD5 sums checked
package 'highr' successfully unpacked and MD5 sums checked
package 'fastmap' successfully unpacked and MD5 sums checked
package 'scales' successfully unpacked and MD5 sums checked
package 'isoband' successfully unpacked and MD5 sums checked
package 'tinytex' successfully unpacked and MD5 sums checked
package 'jsonlite' successfully unpacked and MD5 sums checked
package 'htmltools' successfully unpacked and MD5 sums checked
package 'knitr' successfully unpacked and MD5 sums checked
package 'Rcpp' successfully unpacked and MD5 sums checked
package 'DBI' successfully unpacked and MD5 sums checked
package 'ggplot2' successfully unpacked and MD5 sums checked
package 'data.table' successfully unpacked and MD5 sums checked
package 'rmarkdown' successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\User\AppData\Local\Temp\RtmpELwZKZ\downloaded_packages
  
  
  
   checking for file 'C:\Users\User\AppData\Local\Temp\RtmpELwZKZ\remotes3bf4675851b7\NIADic/DESCRIPTION' ...
  
v  checking for file 'C:\Users\User\AppData\Local\Temp\RtmpELwZKZ\remotes3bf4675851b7\NIADic/DESCRIPTION' (460ms)

  
  
  
-  preparing 'NIADic':
   checking DESCRIPTION meta-information ...
  
   checking DESCRIPTION meta-information ... 
  
v  checking DESCRIPTION meta-information

  
   Error in loadVignetteBuilder(pkgdir, TRUE) : 
     비니에트 빌더 'knitr'를 찾을 수 없습니다

  
   실행이 정지되었습니다

glue       (1.4.1  -> 1.4.2  ) [CRAN]
vctrs      (0.3.0  -> 0.3.8  ) [CRAN]
pillar     (1.4.3  -> 1.6.1  ) [CRAN]
colorspace (1.4-1  -> 2.0-1  ) [CRAN]
xfun       (0.19   -> 0.23   ) [CRAN]
digest     (0.6.25 -> 0.6.27 ) [CRAN]
stringi    (1.4.4  -> 1.6.2  ) [CRAN]
fastmap    (1.0.1  -> 1.1.0  ) [CRAN]
cachem     (1.0.4  -> 1.0.5  ) [CRAN]
tibble     (3.0.1  -> 3.1.2  ) [CRAN]
htmltools  (0.4.0  -> 0.5.1.1) [CRAN]
Rcpp       (1.0.3  -> 1.0.6  ) [CRAN]

  There are binary versions available but the source versions
  are later:
        binary source needs_compilation
pillar   1.6.0  1.6.1             FALSE
xfun      0.22   0.23              TRUE
stringi  1.6.1  1.6.2              TRUE
cachem   1.0.4  1.0.5              TRUE
tibble   3.1.1  3.1.2              TRUE

package 'glue' successfully unpacked and MD5 sums checked
package 'vctrs' successfully unpacked and MD5 sums checked
package 'colorspace' successfully unpacked and MD5 sums checked
package 'digest' successfully unpacked and MD5 sums checked
package 'fastmap' successfully unpacked and MD5 sums checked
package 'htmltools' successfully unpacked and MD5 sums checked
package 'Rcpp' successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\User\AppData\Local\Temp\RtmpELwZKZ\downloaded_packages
  
  
  
   checking for file 'C:\Users\User\AppData\Local\Temp\RtmpELwZKZ\remotes3bf45cc8549d\NIADic/DESCRIPTION' ...
  
v  checking for file 'C:\Users\User\AppData\Local\Temp\RtmpELwZKZ\remotes3bf45cc8549d\NIADic/DESCRIPTION' (506ms)

  
  
  
-  preparing 'NIADic':
   checking DESCRIPTION meta-information ...
  
   checking DESCRIPTION meta-information ... 
  
v  checking DESCRIPTION meta-information

  
  
  
   checking vignette meta-information ...
  
v  checking vignette meta-information

  
  
  
-  checking for LF line-endings in source and make files and shell scripts

  
  
  
-  checking for empty or unneeded directories

  
  
  
-  building 'NIADic_0.0.1.tar.gz'

  
   

983012 words dictionary was built.
```

<div class="sourceCode"><pre><code><span class='va'>mytext</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"이 문장은 K-pop 가사에   자주 등장합니다!!"</span>NA<span class='st'>"이 문장은 \t 2021년에 씌여졌습니다:)"</span>)",
            <span class='st'>"모든 문장들이 흥미롭다."</span>NA<span class='op'>)</span>NA<span class='va'>corpus</span>  <span class='op'>&lt;-</span> <span class='fu'>VCorpus</span><span class='op'>(</span><span class='fu'>VectorSource</span><span class='op'>(</span><span class='va'>mytext</span><span class='op'>)</span><span class='op'>)</span>     <span class='co'># VectorSource : vector를 document로 해석</span>석
<span class='va'>corpus</span>
</code></pre></div>

```
<<VCorpus>>
Metadata:  corpus specific: 0, document level (indexed): 0
Content:  documents: 3
```

<div class="sourceCode"><pre><code><span class='fu'><a href='https://rdrr.io/r/base/summary.html'>summary</a></span><span class='op'>(</span><span class='va'>corpus</span><span class='op'>)</span>
</code></pre></div>

```
  Length Class             Mode
1 2      PlainTextDocument list
2 2      PlainTextDocument list
3 2      PlainTextDocument list
```

</div>



# **영어표현 확인**

> 어떠한 영어표현들이 있는지 확인하며 필요없는 단어들은 제거한다.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='va'>myEnglish</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>lapply</a></span><span class='op'>(</span><span class='va'>corpus</span>, <span class='kw'>function</span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span><span class='op'>{</span> <span class='fu'>str_extract_all</span><span class='op'>(</span><span class='va'>x</span><span class='op'>$</span><span class='va'>content</span>, <span class='st'>"[[:graph:]]{0,}([[:upper:]]{1}|[[:lower:]]{1})[[:lower:]]{0,}[[:graph:]]{0,}"</span><span class='op'>)</span><span class='op'>}</span><span class='op'>)</span>

<span class='fu'><a href='https://rdrr.io/r/base/table.html'>table</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/base/unlist.html'>unlist</a></span><span class='op'>(</span><span class='va'>myEnglish</span><span class='op'>)</span><span class='op'>)</span>
</code></pre></div>

```

K-pop 
    1 
```

</div>



# **숫자표현 제거**

> 텍스트 마이닝에서 숫자표현은 여러가지 의미를 가진다. `숫자 자체가 고유한 의미를 갖는 경우`는 단순히 제거하면 안되고, `단지 숫자가 포함된 표현`이라면 모든 숫자는 하나로 통일한다. `숫자가 문장에 아무런 의미가 없는 경우`는 제거를 한다.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='co'># 숫자표현 확인</span>NA<span class='va'>mydigits</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>lapply</a></span><span class='op'>(</span><span class='va'>corpus</span>, <span class='kw'>function</span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span><span class='op'>{</span><span class='fu'>str_extract_all</span><span class='op'>(</span><span class='va'>x</span><span class='op'>$</span><span class='va'>content</span>,
                                         <span class='st'>"[[:graph:]]{0,}[[:digit:]]{1,}[[:graph:]]{0,}"</span><span class='op'>)</span><span class='op'>}</span><span class='op'>)</span>
<span class='fu'><a href='https://rdrr.io/r/base/table.html'>table</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/base/unlist.html'>unlist</a></span><span class='op'>(</span><span class='va'>mydigits</span><span class='op'>)</span><span class='op'>)</span>
</code></pre></div>

```

2021년에 
       1 
```

</div>


- 숫자에 큰 의미가 없기 때문에 제거한다.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='va'>corpus</span> <span class='op'>&lt;-</span> <span class='fu'>tm_map</span><span class='op'>(</span><span class='va'>corpus</span>, <span class='va'>removeNumbers</span><span class='op'>)</span>  
</code></pre></div>

</div>



# **문장부호 및 특수문자 제거**

> 텍스트 마이닝에서 문장부호(".", "," 등) 및 특수문자("?", "!", "@" 등)은 일반적으로 제거한다. 그러나 `똑같은 부호라도 특별한 의미가 있는 경우는 제거할 때 주의`를 해야한다.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='co'># 특수문자 확인</span>
<span class='va'>mypuncts</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>lapply</a></span><span class='op'>(</span><span class='va'>corpus</span>, <span class='kw'>function</span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span><span class='op'>{</span> <span class='fu'>str_extract_all</span><span class='op'>(</span><span class='va'>x</span><span class='op'>$</span><span class='va'>content</span>,
                                        <span class='st'>"[[:graph:]]{0,}[[:punct:]]{1,}[[:graph:]]{0,}"</span><span class='op'>)</span><span class='op'>}</span><span class='op'>)</span>  
<span class='fu'><a href='https://rdrr.io/r/base/table.html'>table</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/base/unlist.html'>unlist</a></span><span class='op'>(</span><span class='va'>mypuncts</span><span class='op'>)</span><span class='op'>)</span>
</code></pre></div>

```

         K-pop   등장합니다!! 씌여졌습니다:)      흥미롭다. 
             1              1              1              1 
```

</div>

- "k-pop"의 특수문자는 삭제하여 "kpop"으로 수정한다.


<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='va'>corpus</span> <span class='op'>&lt;-</span> <span class='fu'>tm_map</span><span class='op'>(</span><span class='va'>corpus</span>, <span class='va'>removePunctuation</span><span class='op'>)</span>
</code></pre></div>

</div>



# **공란처리**

> 2개 이상의  공란이  연달아 발견될 경우 해당 `공란을 1개로 변환`시키는 작업이다.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='va'>corpus</span> <span class='op'>&lt;-</span> <span class='fu'>tm_map</span><span class='op'>(</span><span class='va'>corpus</span>, <span class='va'>stripWhitespace</span><span class='op'>)</span>
</code></pre></div>

</div>



# **토큰화**

> `수집된 문서들의 집합인 말뭉치(Corpus)를 토큰(Token) 으로 나누는 작업`이며, 토큰이 단어일 때는 단어 토큰화, 문장일 때는 문장 토큰화라고 부른다. 


<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='fu'>pacman</span><span class='fu'>::</span><span class='fu'><a href='https://rdrr.io/pkg/pacman/man/p_load.html'>p_load</a></span><span class='op'>(</span><span class='st'>"KoNLP"</span><span class='op'>)</span>

<span class='va'>myNounCorpus</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>lapply</a></span><span class='op'>(</span><span class='va'>corpus</span>, <span class='kw'>function</span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span><span class='op'>{</span><span class='fu'><a href='https://rdrr.io/r/base/paste.html'>paste</a></span><span class='op'>(</span><span class='fu'>extractNoun</span><span class='op'>(</span><span class='va'>x</span><span class='op'>$</span><span class='va'>content</span><span class='op'>)</span>,collapse<span class='op'>=</span><span class='st'>' '</span><span class='op'>)</span><span class='op'>}</span><span class='op'>)</span>  <span class='co'># extractNoun : 의미의 핵심이라고 할 수 있는 명사 추출 / 사전에 등록되어있는 명사 기준 </span>NA<span class='va'>words_nouns</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/base/lapply.html'>lapply</a></span><span class='op'>(</span><span class='va'>myNounCorpus</span>,
                      <span class='kw'>function</span><span class='op'>(</span><span class='va'>x</span><span class='op'>)</span><span class='op'>{</span><span class='fu'>str_extract_all</span><span class='op'>(</span><span class='va'>x</span>,<span class='fu'>boundary</span><span class='op'>(</span><span class='st'>"word"</span><span class='op'>)</span><span class='op'>)</span><span class='op'>}</span>  <span class='co'>#전체 말뭉치 단어를 확인</span>인
<span class='op'>)</span>

<span class='fu'><a href='https://rdrr.io/r/base/table.html'>table</a></span><span class='op'>(</span><span class='fu'><a href='https://rdrr.io/r/base/unlist.html'>unlist</a></span><span class='op'>(</span><span class='va'>words_nouns</span><span class='op'>)</span><span class='op'>)</span>
</code></pre></div>

```

      Kpop       가사         년       들이       등장       문장 
         1          1          1          1          1          3 
씌여졌습니         합     흥미롭 
         1          1          1 
```

</div>


# **불용어 제거**

> 빈번하게 사용되거나 구체적인 의미를 찾기 어려운 단어를 불용단어 또는 정지단어라 부른다. `영어는 "tm" 패키지를 통해 널리 알려진 불용어 목록이 있으나 한국어는 없다.`


<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='va'>corpus1</span> <span class='op'>&lt;-</span> <span class='fu'>tm_map</span><span class='op'>(</span><span class='va'>corpus</span>, <span class='va'>removeWords</span>,  words<span class='op'>=</span><span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='st'>"이"</span> "<span class='st'>"다"</span><span class='op'>)</span><span class='op'>)</span></code></pre></div>

</div>


# **어근 동일화**

> 같은 의미를 가진 단어라도 문법적 기능에 따라 다양한 형태를 가진다. 예를 들어, "go"는 "goes", "gone" 등과 같이 변할 수 있다. 이 때, `문법적 또는 의미적으로 변화한 단어의 원형`을 찾아내는 작업을 말한다.



# **Document-Term Matrix**
> 말뭉치(Corpus)에 대한 전처리를 거친 후 문서×단어 행렬(Document-Term Matrix, DTM)을 구축할 수 있다.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre><code><span class='va'>dtm</span> <span class='op'>&lt;-</span> <span class='fu'>DocumentTermMatrix</span><span class='op'>(</span><span class='va'>corpus1</span><span class='op'>)</span>           <span class='co'># 문서x단어 행렬</span>렬
<span class='fu'>inspect</span><span class='op'>(</span><span class='va'>dtm</span><span class='op'>)</span>
</code></pre></div>

```
<<DocumentTermMatrix (documents: 3, terms: 7)>>
Non-/sparse entries: 8/13
Sparsity           : 62%
Maximal term length: 6
Weighting          : term frequency (tf)
Sample             :
    Terms
Docs kpop 가사에 등장합니다 문장들이 문장은 씌여졌습니다 흥미롭다
   1    1      1          1        0      1            0        0
   2    0      0          0        0      1            1        0
   3    0      0          0        1      0            0        1
```

</div>


```{.r .distill-force-highlighting-css}
```
