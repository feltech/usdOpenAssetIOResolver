
set(PLUGIN_NAME usdOpenAssetIOResolver)

set(
  SRC
    resolver.cpp
)

add_library(${PLUGIN_NAME}
    SHARED
    ${SRC}
)

target_link_libraries(${PLUGIN_NAME}
    PRIVATE
    ar
    OpenAssetIO::openassetio-core
    OpenAssetIO::openassetio-python-bridge
)

#-----------------------------------------------------------------------
# Activate warnings as errors, pedantic, etc.
set_default_compiler_warnings(${PLUGIN_NAME})


#-----------------------------------------------------------------------
# Add static analysis dependencies
if (OPENASSETIO_USDRESOLVER_ENABLE_CLANG_FORMAT)
    add_dependencies(${PLUGIN_NAME} openassetio-clangformat)
endif ()
if (OPENASSETIO_USDRESOLVER_ENABLE_CPPLINT)
    add_dependencies(${PLUGIN_NAME} openassetio-cpplint)
endif ()

# Whether to use the old or new C++ ABI with gcc.
#if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU" AND
#    CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL 5.0)
#    if (OPENASSETIO_GLIBCXX_USE_CXX11_ABI)
#        target_compile_definitions(${PLUGIN_NAME} PRIVATE _GLIBCXX_USE_CXX11_ABI=1)
#    else ()
#        target_compile_definitions(${PLUGIN_NAME} PRIVATE _GLIBCXX_USE_CXX11_ABI=0)
#    endif ()
#endif ()

#-----------------------------------------------------------------------
# Main install
install(
    TARGETS
        ${PLUGIN_NAME}
    DESTINATION
        .
)

#-----------------------------------------------------------------------
# Install plugInfo.json
install(
    FILES
		resources/plugInfo.json
    DESTINATION
        ./resources
)
