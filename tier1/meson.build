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
  include_directories: include_directories([
    '../public',
    '../public/tier0',
    '../public/tier1',
  ]),
  cpp_args: [
    sdklib_cflags,
    sdklib_defines,
    sdklib_undefines,
    '-D_LIB',
    '-DTIER1_STATIC_LIB',
  ],
)

tier1_dep = declare_dependency(
  link_with: [
    tier1,
  ],
  include_directories: include_directories([
    '../public/tier1',
  ])
)