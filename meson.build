project('hl2sdk-cs2', 'c', 'cpp', default_options: [
  'warning_level=1',
  'werror=false',
  'cpp_std=c++14',
])

sdk_platforms = {
  'linux': ['x86_64'],
  'windows': ['x86_64'],
}

sdk_suffix = '2.cs2'
sdk_cpp_args = [
  '-DSOURCE_ENGINE=25',
  '-DSE_CS2=25',
  '-DCS2',
]

sdk_source2 = true
sdk_inc = include_directories([
  'thirdparty/protobuf-3.21.8/src',
  'public',
  'public/engine',
  'public/mathlib',
  'public/vstdlib',
  'public/tier0',
  'public/tier1',
  'public/entity2',
  'public/game/server',
  'game/shared',
  'game/server',
  'common'
])

public_inc = include_directories('public')
tier0_inc = include_directories('public/tier0')
tier1_inc = include_directories('public/tier1')
mathlib_inc = include_directories('public/mathlib')

sdklib_cflags = [
  '-fno-strict-aliasing',
  '-Wall',
  '-Wsign-compare',
  '-Wno-conversion',
  '-Wno-overloaded-virtual',
  '-Wno-non-virtual-dtor',
  '-Wno-invalid-offsetof',
  '-Wno-unknown-pragmas',
  '-Wno-unused',
  '-Wno-c++11-narrowing',
  '-Wno-register',
  '-O3',
  '-march=nocona',
  '-mmmx',
  '-msse',
  '-msse2',
  '-m64',
]

sdklib_defines = []
if target_machine.system() == 'linux'
  sdklib_defines += [
    '-D_LINUX',
    '-DLINUX',
  ]
endif

sdklib_defines += [
  '-DX64BITS',
  '-DPLATFORM_64BITS',
  '-DVPROF_LEVEL=1',
  '-DSWDS',
  '-D_finite=finite',
  '-Dstricmp=strcasecmp',
  '-D_stricmp=strcasecmp',
  '-D_strnicmp=strncasecmp',
  '-Dstrnicmp=strncasecmp',
  '-D_vsnprintf=vsnprintf',
  '-D_alloca=alloca',
  '-Dstrcmpi=strcasecmp',
  '-DPOSIX',
  '-DGNUC',
  '-DCOMPILER_GCC',
  '-DNO_MALLOC_OVERRIDE',
]

sdklib_undefines = [
  '-Usprintf',
  '-Ustrncpy',
  '-UPROTECTED_THINGS_ENABLE',
]

subdir('mathlib')
subdir('tier1')
subdir('interfaces')

sdk_libs = [
  mathlib,
  tier1,
  interfaces,
]

compiler = meson.get_compiler('cpp')

if target_machine.system() == 'linux'
  if target_machine.cpu_family() == 'x86_64'
    sdk_deps = [
      compiler.find_library('protobuf', dirs: [meson.project_source_root() + '/lib/linux64/release']),
      compiler.find_library('tier0', dirs: [meson.project_source_root() + '/lib/linux64']),
    ]
  endif

  sdk_cpp_args = [
    '-D_GLIBCXX_USE_CXX11_ABI=0'
  ]

  protoc = find_program('devtools/bin/linux/protoc')

elif target_machine.system() == 'windows'
  sdk_deps = [
    compiler.find_library('tier0', dirs: [meson.project_source_root() + '/lib/public/win64']),
    compiler.find_library('protobuf', dirs: [meson.project_source_root() + '/lib/public/win64/2015']),
  ]
  protoc = find_program('devtools/bin/protoc.exe')
endif

# @SOURCE_DIR@ is broken for subprojects see: https://github.com/mesonbuild/meson/issues/11994
proto_gen = generator(
  protoc,
  output: ['@BASENAME@.pb.cc', '@BASENAME@.pb.h'],
  arguments: [
    '--proto_path=@CURRENT_SOURCE_DIR@',
    '--proto_path=' + meson.project_source_root() + '/thirdparty/protobuf-3.21.8/src',
    '--proto_path=' + meson.project_source_root() + '/common',
    '--proto_path=' + meson.project_source_root() + '/game',
    '--proto_path=' + meson.project_source_root() + '/game/shared',
    '--proto_path=' + meson.project_source_root() + '/game/shared/cs',
    '--proto_path=' + meson.project_source_root() + '/game/shared/cstrike15',
    '--proto_path=' + meson.project_source_root() + '/game/shared/econ',
    '--proto_path=' + meson.project_source_root() + '/gcsdk',
    '--cpp_out=@BUILD_DIR@',
    '@INPUT@'
  ]
)

sdk_proto=[]
subdir('common')
subdir('game')
subdir('gcsdk')

sdk_proto_lib = static_library(
  'cs2proto',
  sources: sdk_proto,
  cpp_args: [],
  include_directories: [
    sdk_inc,
  ],
)