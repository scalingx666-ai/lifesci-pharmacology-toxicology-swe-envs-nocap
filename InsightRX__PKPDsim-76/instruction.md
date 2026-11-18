# Issue

**Title:** RXR-1689 Fix warnings in linear models upon installation

## Description

This just fixes some warnings that were thrown during the compilation of the hardcoded linear compartmental models included with PKPDsim. The warnings were harmless, mostly just declaration of unused variables, so this is just cleanup.

The warnings only showed when installing PKPDsim from scratch, or when running `R CMD INSTALL . --preclean`.
On my local machine:

```
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_1cmt_iv_infusion.cpp -o pk_1cmt_iv_infusion.o
pk_1cmt_iv_infusion.cpp:20:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
1 warning generated.
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_1cmt_oral.cpp -o pk_1cmt_oral.o
pk_1cmt_oral.cpp:25:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
1 warning generated.
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_2cmt_iv_bolus.cpp -o pk_2cmt_iv_bolus.o
pk_2cmt_iv_bolus.cpp:24:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
1 warning generated.
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_2cmt_iv_infusion.cpp -o pk_2cmt_iv_infusion.o
pk_2cmt_iv_infusion.cpp:25:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
1 warning generated.
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_2cmt_oral.cpp -o pk_2cmt_oral.o
pk_2cmt_oral.cpp:8:14: warning: unused variable 'k10' [-Wunused-variable]
  double ka, k10, k20, k30, k23, k32, E1, E2, E3, lambda1, lambda2, t, A1last, A2last, A3last, A2term1, A2term2, A3term1, A3term2;
             ^
pk_2cmt_oral.cpp:29:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
pk_2cmt_oral.cpp:8:39: warning: unused variable 'E1' [-Wunused-variable]
  double ka, k10, k20, k30, k23, k32, E1, E2, E3, lambda1, lambda2, t, A1last, A2last, A3last, A2term1, A2term2, A3term1, A3term2;
                                      ^
3 warnings generated.
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_3cmt_iv_bolus.cpp -o pk_3cmt_iv_bolus.o
pk_3cmt_iv_bolus.cpp:30:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
1 warning generated.
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_3cmt_iv_infusion.cpp -o pk_3cmt_iv_infusion.o
pk_3cmt_iv_infusion.cpp:31:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
1 warning generated.
clang++ -mmacosx-version-min=10.13 -std=gnu++14 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG -D_HAS_AUTO_PTR_ETC=0 -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/BH/include' -I'/Library/Frameworks/R.framework/Versions/4.2/Resources/library/Rcpp/include' -I/usr/local/include   -fPIC  -Wall -g -O2  -c pk_3cmt_oral.cpp -o pk_3cmt_oral.o
pk_3cmt_oral.cpp:8:54: warning: unused variable 'E1' [-Wunused-variable]
  double ka, k10, k20, k23, k32, k24, k42, k30, k40, E1, E2, E3, E4;
                                                     ^
pk_3cmt_oral.cpp:10:51: warning: unused variable 'A1term2' [-Wunused-variable]
  double A1last, A2last, A3last, A4last, A1term1, A1term2, A2term1, A2term2, A2term3, A3term1, A3term2, A3term3, A4term1, A4term2, A4term3;
                                                  ^
pk_3cmt_oral.cpp:10:42: warning: unused variable 'A1term1' [-Wunused-variable]
  double A1last, A2last, A3last, A4last, A1term1, A1term2, A2term1, A2term2, A2term3, A3term1, A3term2, A3term3, A4term1, A4term2, A4term3;
                                         ^
pk_3cmt_oral.cpp:8:14: warning: unused variable 'k10' [-Wunused-variable]
  double ka, k10, k20, k23, k32, k24, k42, k30, k40, E1, E2, E3, E4;
             ^
pk_3cmt_oral.cpp:35:30: warning: unused variable 'it' [-Wunused-variable]
  std::vector<int>::iterator it;
                             ^
5 warnings generated.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bfa4d09ac7f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.