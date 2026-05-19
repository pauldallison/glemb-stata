# glemb for Stata

`glemb` adds a user-defined method for Stata's `mi impute` command. It performs multiple imputation for mixed categorical and continuous data using the general location model with EMB.

## Installation

Install directly from GitHub:

```stata
net install glemb, from("https://raw.githubusercontent.com/pauldallison/glemb-stata/main/")
help mi impute glemb
```

During development, you can also add a local checkout to the adopath:

```stata
adopath ++ "path\to\glemb-stata"
help mi impute glemb
```

## Example

Use `mi impute glemb` inside Stata's MI system:

```stata
use "C:\data\nlsymiss.dta", clear

mi set wide
mi register imputed self pov race momwork
mi register regular id anti childage divorce gender momage

mi impute glemb self pov race momwork = anti childage divorce gender momage, ///
    noms(pov race momwork divorce) ///
    add(25) ///
    rseed(1234) ///
    replace

mi estimate: regress anti self pov i.race childage divorce gender momage momwork
```

Variables with missing values that should be imputed by `glemb` must be on the left side of `mi impute glemb`. Variables after `=` are complete predictors and must be complete in the imputation sample.

Categorical variables with missing values should be listed in `noms()`. Complete categorical predictors can optionally be included in `noms()`, but it is not essential. Model variables not listed in `noms()` are treated as continuous.

Extended missing values (`.a`-`.z`) in imputation variables are left unchanged and are not imputed. `glemb` prints a note when such values are present. Convert extended missing values to ordinary missing (`.`) before imputation if you want them imputed.

## Main options

- `noms(varlist)`: categorical variables in the imputation model.
- `catinteract(2|3)`: maximum order of categorical interactions in the log-linear model; default is `catinteract(2)`.
- `meanmodel(main|saturated)`: model for continuous means; default is `meanmodel(main)`.
- `catprior(#)`: categorical pseudocount; default is `1 / n_cells`.
- `profile`: print timing and E-step row-count diagnostics after imputation.

## Practical notes

- Avoid putting many high-cardinality categorical variables in `noms()`. `glemb` forms a multiway categorical table and will stop if the implied table is too large.
- `catprior(0)` requires every possible categorical cell to be observed in the imputation sample.
- Continuous variables must have at least one observed value and nonzero observed variance.
- Very small samples relative to the number of model variables may produce unstable imputations; `glemb` prints a note in this situation.
- Imputed continuous values are draws from the fitted multivariate normal model and can fall outside the observed range.
- The standalone `glemb` command is retained as a development and diagnostic interface. The recommended user-facing command is `mi impute glemb`.

## Distribution files

The Stata command needs these files on the adopath:

- `lglemb.mata`
- `mi_impute_cmd_glemb.ado`
- `mi_impute_cmd_glemb_parse.ado`
- `mi_impute_cmd_glemb_cleanup.ado`
- `mi_impute_glemb.sthlp`
- `glemb.ado`
- `glemb.sthlp`
- `stata.toc`
- `glemb.pkg`
