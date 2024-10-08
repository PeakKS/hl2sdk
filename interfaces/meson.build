interfaces_src = [
	'interfaces.cpp',
]

interfaces = static_library(
  'interfaces',
  interfaces_src,
  name_prefix: '',
  pic: true,
  include_directories: include_directories([
    '../public',
    '../public/tier0',
  ]),
  cpp_args: [
    sdklib_cflags,
    sdklib_defines,
    sdklib_undefines,
    '-D_LIB',
  ],
)

interfaces_dep = declare_dependency(
  link_with: [
    interfaces,
  ],
  include_directories: include_directories([
    '../public/interfaces',
  ])
)