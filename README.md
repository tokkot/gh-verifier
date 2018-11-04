This is the verifier code used by the backend for [https://golf.horse](https://golf.horse).
```
Usage:
./verifier <wordlist> <program.js> [v8 options]
```

This verifier code builds against the v8 monolithic library, which you can create more or less like this (assuming you have the v8 source in $HOME/v8/v8 and depot_tools in $HOME/depot_tools):

```
export PATH=$PATH:$HOME/depot_tools
cd v8/v8

#Use https://omahaproxy.appspot.com/ to figure out latest stable chrome version, grab related branch:
git checkout -b remotes/branch-heads/7.0

#Use this to start a build
tools/dev/gm.py x64.release

#But then ^C it and edit out/x64.release/args.gn to add:
use_custom_libcxx = false
v8_use_external_startup_data = false
v8_monolithic = true

#then do:
ninja -C out/x64.release v8_monolith
```

Now you should have an ```out/x64.release/obj/libv8_monolith.a```; update the Makefile to point ```V8DIR``` to the right spot and you should be good to build.
