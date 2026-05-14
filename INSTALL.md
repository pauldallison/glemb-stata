# Installing glemb for Stata

## GitHub installation

From Stata, install directly from GitHub:

```stata
net install glemb, from("https://raw.githubusercontent.com/pauldallison/glemb-stata/main/")
help mi impute glemb
```

## Local installation

From Stata, install from the local package directory:

```stata
net from "path\to\glemb-stata"
net install glemb
help mi impute glemb
```

## Development use without installing

During development, you can add the source directory directly to the adopath:

```stata
adopath ++ "path\to\glemb-stata"
help mi impute glemb
```

## Files in the package

- `lglemb.mata`
- `mi_impute_cmd_glemb.ado`
- `mi_impute_cmd_glemb_parse.ado`
- `mi_impute_cmd_glemb_cleanup.ado`
- `mi_impute_glemb.sthlp`
- `glemb.ado`
- `glemb.sthlp`
- `README.md`

The recommended user-facing command is `mi impute glemb`.
