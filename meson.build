project('softfloat', 'c', license: 'BSD 3-clause', version: '3d',
    default_options: ['default_library=static'])

specialize_type = get_option('specialize_type')

compiler = meson.get_compiler('c')

# thread local syntax
thread_local = ''
if compiler.links('__thread int foo;main(){}', name: 'thread locals')
    thread_local = '__thread'
endif

# a reasonable assumption for the moment
add_project_arguments('-DINLINE_LEVEL=' + get_option('inline_level'), language: 'c')

# should add some way to override this
fast_int64 = compiler.sizeof('long') == 8

add_project_arguments('-DSOFTFLOAT_FAST_DIV64TO32', '-DSOFTFLOAT_FAST_DIV32TO16', language: 'c')
if get_option('round_odd')
    add_project_arguments('-DSOFTFLOAT_ROUND_ODD', language: 'c')
endif

includes = include_directories('source/' + specialize_type, 'source/include')
subdir('source')
libsoftfloat = library('softfloat', src, include_directories: includes)

export_includes = include_directories('source/include')
softfloat_dep = declare_dependency(link_with: libsoftfloat, include_directories: export_includes)
