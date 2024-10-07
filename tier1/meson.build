tier1_src = [
	'bitbuf.cpp',
	'commandbuffer.cpp',
  'convar.cpp',
	'generichash.cpp',
	'rangecheckedvar.cpp',
	'tier1.cpp',
	'utlbufferutil.cpp',
]

tier1 = static_library(
  'tier1',
  tier1_src,
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
    '-DTIER1_STATIC_LIB',
  ],
)