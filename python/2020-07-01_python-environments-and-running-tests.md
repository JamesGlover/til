# Python environments and running tests 

With pipenv installed (eg. `brew instal pipenv`) open a python shell

```
  pipenv shell
```

Running tests:

```
  python -m pytest -s
```

Useful flags:
 
  -v  Verbose mode
  -s  Disable log capture

First came across this with the crawler, which suggested using both flags
when running the suite, however personal preference is not to use them. Verbose mode made little visible difference, but by disabling capture the output just 
goes to stdout, saturating the output with logs. If capuring is enable, this output is only shown for failing tests, which feels a lot more useful.
