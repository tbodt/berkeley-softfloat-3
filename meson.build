project('softfloat', 'c', license: 'BSD 3-clause', version: '3d',
    default_options: ['default_library=static'])

specialize_type = get_option('specialize_type')

compiler = meson.get_compiler('c')

# check endian
result = compiler.run('''
int main() {
    int x = 1;
    return *(char *) &x == 1;
}
''', name: 'endian')
if result.returncode() == 1
    add_project_arguments('-DLITTLEENDIAN', language: 'c')
endif

# thread local syntax
if compiler.links('__thread int foo;main(){}', name: 'thread locals')
    add_project_arguments('-DTHREAD_LOCAL=__thread', language: 'c')
endif

# a reasonable assumption for the moment
add_project_arguments('-DINLINE=inline', '-DINLINE_LEVEL=' + get_option('inline_level'), language: 'c')

# should add some way to override this
if compiler.sizeof('long') == 8
    add_project_arguments('-DSOFTFLOAT_FAST_INT64', language: 'c')
endif
add_project_arguments('-DSOFTFLOAT_FAST_DIV64TO32', '-DSOFTFLOAT_FAST_DIV32TO16', language: 'c')
if get_option('round_odd')
    add_project_arguments('-DSOFTFLOAT_ROUND_ODD', language: 'c')
endif

includes = include_directories('source/' + specialize_type, 'source/include')
subdir('source')
libsoftfloat = library('softfloat', src, include_directories: includes)

export_includes = include_directories('source/include')
softfloat_dep = declare_dependency(link_with: libsoftfloat, include_directories: export_includes)
