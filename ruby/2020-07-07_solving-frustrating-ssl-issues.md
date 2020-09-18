# Solving frustrating ssl issues

Frustratingly there doesn't seem to be an entirely consistent way of handling this.

Back when I used RVM, the following was my go-to for ages:
```
rvm reinstall ruby-2.3.7 --with-openssl-dir=`brew --prefix openssl`
``` 

Then that failed to work, and I switched to the option found here:
https://github.com/rbenv/ruby-build/issues/1139
I commented previously that the --with-openssl-dir=$(brew --prefix openssl) would fix the problem, but tod

Then that failed to work, and I switched to the option found here:
> https://github.com/rbenv/ruby-build/issues/1139
> I commented previously that the --with-openssl-dir=$(brew --prefix openssl) would fix the problem, but today I tried to install Ruby 2.7.0 with rvm and it failed. What fixed it was, I did a brew info openssl to get some clues, and this worked:
>
> ```
> export PKG_CONFIG_PATH="/usr/local/opt/openssl@1.1/lib/pkgconfig"
> ```

In practice I added the following to my .profile:

```
export PKG_CONFIG_PATH="`brew --prefix openssl`/lib/pkgconfig"
```

But that doesn't seem to be used when installing MySQL2 gem, so once again:

```
gem install mysql2 -v '0.5.2' --source 'http://rubygems.org/' -- --with-opt-dir="$(brew --prefix openssl)"
```

Obviously all the version numbers above need to be updated to fix your needs.
