# Custom libraries and SIP

Once again, the custom homebrew install directory is causing pain,
and prevents libraries from being dynamically loaded by ffi.

```
/Users/username/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/ffi-1.15.0/lib/ffi/library.rb:145:in `block in ffi_lib': Could not open library 'libseabolt17': dlopen(libseabolt17, 5): image not found. (LoadError)
Could not open library 'libseabolt17.dylib': dlopen(libseabolt17.dylib, 5): image not found
```

We should be able to use the ENV `DYLD_FALLBACK_LIBRARY_PATH` to specify this, but
rather helpfully, MacOS system integrity protection un-sets these automatically for us.

Fortunately we can use tools like [rbenv-vars](https://github.com/rbenv/rbenv-vars) to set them properly:

```
bat ~/.rbenv/vars
───────┬────────────────────────────────────────────────────────
       │ File: /Users/username/.rbenv/vars
───────┼────────────────────────────────────────────────────────
   1   │ DYLD_FALLBACK_LIBRARY_PATH=/Users/jg16/homebrew/lib
───────┴────────────────────────────────────────────────────────
```
