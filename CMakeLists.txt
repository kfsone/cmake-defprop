cmake_minimum_required (VERSION 3.24.2)
project (test LANGUAGES C)

# v1
define_property (TARGET PROPERTY IS_DEFINED INHERITED)

add_executable (main main.c)

# v2
set (propname OTHER_PROP)
define_property (TARGET PROPERTY ${propname} INHERITED)

# ---------------------------------------------

# v1
get_target_property (IS_DEFINED main IS_DEFINED)
if (IS_DEFINED STREQUAL "IS_DEFINED-NOTFOUND")
	message (SEND_ERROR "${IS_DEFINED}")
endif ()

# v2
get_target_property (${propname} main ${propname})
if (${propname} STREQUAL "${propname}-NOTFOUND")
	message (SEND_ERROR "${${propname}}")
endif ()
