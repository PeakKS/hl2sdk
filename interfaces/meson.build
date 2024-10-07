interfaces_src = [
	'interfaces.cpp',
]

interfaces = static_library(
  'interfaces',
  interfaces_src,
  name_prefix: '',
  pic: true,
  include_directories: [
    public_inc,
    tier0_inc,
    tier1_inc,
  ],
  cpp_args: [
    sdklib_cflags,
    sdklib_defines,
    sdklib_undefines,
    '-D_LIB',
  ],
)