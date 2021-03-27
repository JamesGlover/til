# Installing Mimemagic on MacOS

## Background

Mimemagic uses Mimetype information from freedesktop.org to drive its decisions.
In versions 0.3.6 and lower (As well as 0.4.0) this file was  included as part
of the gem itself. However, on 24th March 2021 freedesktop.org notified the
maintainer of mimemagic that it was in violation of the GPL.

https://github.com/mimemagicrb/mimemagic/issues/97

All existing gem versions (0.3.5 and below) were immediately
yanked, and new versions were released with GPL licences (0.3.6 and 0.4.0).
However as this change of license has serious repercussions for downstream
projects, work immediately begun to restore an MIT license.

Versions 0.3.7 and 0.4.1 were released on 25th March 2021, restoring the
MIT license. GPL licensed versions were yanked. In order to be compliant
with the GPL licensing terms mimemagic no longer includes `freedesktop.org.xml`
within the gem itself, and the file must be obtained manually.

The gem checks for the presence of this file on install, and thus will fail
to install if a copy is not available.

## The errors and fixes

Edit: Rails have updated marcel to remove the dependency on Mimemagic. For apps on 
Rails 5.2, 6.0 and 6.1 it is probably easiest to just update the rails components.
(Assuming this is the only gem with a mime-magic dependency)

### Using yanked gems

If you are attempting to install a project which has not yet updated mimemagic
you will see an error similar to the following:

```
  Your bundle is locked to mimemagic (0.3.6), but that version could not be found
  in any of the sources listed in your Gemfile. If you haven't changed sources,
  that means the author of mimemagic (0.3.6) has removed it. You'll need to update
  your bundle to a version other than mimemagic (0.3.6) that hasn't been removed
  in order to install.
```

This indicates that the project is still using the old version of mimemagic,
which is no longer available. You will need to install a newer version, and
update the Gemfile.lock.

```bash
bundle update mimemagic
```

However, before you do this, check the section below, as you may need to perform
another couple of steps.

### Installing the Freedesktop.org shared-mime-info

Upon attempting to install mimemagic (>0.3.7, >0.4.1) you may receive the
following error:

```
Installing mimemagic 0.3.9 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

current directory:
/Users/<username>/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/mimemagic-0.3.9/ext/mimemagic
/Users/<username>/.rbenv/versions/2.7.2/bin/ruby
-I/Users/<username>/.rbenv/versions/2.7.2/lib/ruby/2.7.0/rubygems -rrubygems
/Users/<username>/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/gems/rake-13.0.3/exe/rake
RUBYARCHDIR\=/Users/<username>/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/extensions/x86_64-darwin-19/2.7.0/mimemagic-0.3.9
RUBYLIBDIR\=/Users/<username>/.rbenv/versions/2.7.2/lib/ruby/gems/2.7.0/extensions/x86_64-darwin-19/2.7.0/mimemagic-0.3.9
rake aborted!
Could not find MIME type database in the following locations:
["/usr/local/share/mime/packages/freedesktop.org.xml",
"/opt/homebrew/share/mime/packages/freedesktop.org.xml",
"/usr/share/mime/packages/freedesktop.org.xml"]

Ensure you have either installed the shared-mime-info package for your
distribution, or
obtain a version of freedesktop.org.xml and set FREEDESKTOP_MIME_TYPES_PATH to
the location
of that file.
```

This indicates that mimemagic is unable to find a copy of freedesktop.org.xml.

#### MacOS

The suggested way of installing the dependencies is via brew:

```bash
brew install shared-mime-info
```

However, if your homebrew installation is not at the default location, this may
be insufficient to fully solve the problem.

```bash
$ brew --prefix shared-mime-info
  /Users/<username>/homebrew/opt/shared-mime-info
```

This indicates that the package is not installed at the default location of
`/opt/homebrew/share/mime/packages/freedesktop.org.xml` so you will need to tell
mimemagic where to find the file. If the above command returns
`/opt/homebrew/share/mime/packages/freedesktop.org.xml` you should be good to
go.

In order to fix the issue, you will need to set the environmental variable
`FREEDESKTOP_MIME_TYPES_PATH` to point to the freedesktop.org.xm file. You can
do this via:

```
export FREEDESKTOP_MIME_TYPES_PATH="`brew --prefix shared-mime-info`/share/shared-mime-info/packages/freedesktop.org.xml"
```

I recommend adding the above to your `.bashrc` or `.zshrc` to ensure its always
available. Note that you can also replace `brew --prefix shared-mime-info` with
the result of the command if you find its acting too slowly.

#### Other systems

See [the mimemagic repo](https://github.com/mimemagicrb/mimemagic#dependencies)
for more information.
