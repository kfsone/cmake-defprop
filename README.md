CMake 'define-property' validation
==================================

CMake documentation suggests that using `define_property` will cause
`get_target_property` to return no value if the property is defined
as a `TARGET` property and `INHERITED`.

Use case
--------

I wanted to use target properties on INTERFACE libraries to allow me
to forward runtime file requirements on INTERFACE libraries so that
post-build functions can be called naively and simply consume the
list of runtime requirements if they are inherited up to a target.


```
define_property (TARGET PROPERTY runtime_stuff INHERITED)
if (complex-conditional)
  set_target_properties (libsoandso PROPERTIES runtime_stuff strings.bundle config.ini)
endif ()


function (post_build_soandso TARGET)
    get_property (runtimes ${TARGET} runtime_stuff)
    if (runtimes)
        add_custom_command ...
    endif ()
endfunction ()
```

This doesn't work, because `runtimes` gets set to `runtimes-NOTFOUND`.

While not the worst thing in the world, it's a weird case to try and teach.

```
    get_property (runtimes ${TARGET} runtime_stuff)
    if (runtimes AND NOT ${runtimes} STREQUAL "runtimes-NOTFOUND")
```

this is not something I expect developers who aren't living in CMake to get right,
so `define_property` promises to give me a better behavior.

It doesn't appear to work, though.
