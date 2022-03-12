# Modules directory

No package manager yet, need to manage modules with git tags.

Configurations inside `calcit.cirru` and `compact.cirru`:

```cirru
:configs $ {}
  :modules $ [] |memof/compact.cirru |lilac/
```

Paths defined in `:modules` field are just loaded as files from `~/.config/calcit/modules/`, i.e. `~/.config/calcit/modules/memof/compact.cirru`.

Modules that ends with `/`s are automatically suffixed compact.cirru since it's the default filename.

To load modules in CI environments, make use of `git clone`.
