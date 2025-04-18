project('bore', 'c', 'cpp',
  version : '1',
  default_options : ['c_std=c17', 'cpp_std=c++20', 'warning_level=3', 'buildtype=release', 'b_ndebug=if-release'])

static_link_gsl = get_option('static_gsl')

includedirs = ['src']

sources = [
     'src/common.c',
     'src/common.h'
]

if host_machine.system() == 'windows'
    winmod = import('windows')
    sources += [winmod.compile_resources('src/bore.rc')]
endif

deps = []

libtype = get_option('libtype')

install = false

install_dir = ''

if libtype in ['vapoursynth', 'both']
    sources += ['src/bore.c']
    vs_installation = get_option('vs_install')
    if vs_installation
        deps += dependency('vapoursynth', version: '>=55').partial_dependency(compile_args : true, includes : true)
        install = true
        install_dir += join_paths(deps[0].get_pkgconfig_variable('libdir'), 'vapoursynth')
    endif
endif

if libtype in ['avisynth', 'both']
    sources += [
        'avs_c_api_loader/avs_c_api_loader.cpp',
        'avs_c_api_loader/avs_c_api_loader.hpp',
        'src/avs_bore.cpp'
    ]
endif

gsl_dep = dependency('gsl', static : static_link_gsl)

deps += gsl_dep

shared_module('bore', sources,
  dependencies : deps,
  install : install,
  install_dir : install_dir,
  link_args : '-s',
  gnu_symbol_visibility : 'hidden'
)
