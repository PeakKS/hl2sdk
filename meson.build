project('hl2sdk-cs2', 'cpp', default_options: [
  'warning_level=1',
  'werror=true',
  'cpp_std=c++14',
])

sdk_platforms = {
  'linux': ['x86_64'],
  'windows': ['x86_64'],
}

sdk_suffix = '2.cs2'
sdk_code = 25
sdk_cpp_args = [
  '-DCS2'
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

if target_machine.system() == 'linux'
  if target_machine.cpu_family() == 'x86_64'
    sdk_static_libs = files([
      'lib/linux64/mathlib.a',
      'lib/linux64/tier1.a',
      'lib/linux64/interfaces.a',
      'lib/linux64/release/libprotobuf.a'
    ])
    sdk_dynamic_libs = files([
      'lib/linux64/libtier0.so'
    ])
  endif

  sdk_cpp_args = [
    '-D_GLIBCXX_USE_CX11_ABI=0'
  ]

  protoc = find_program('devtools/bin/linux/protoc')

elif target_machine.system() == 'windows'
  if target_machine.cpu_family() == 'x86_64'
    sdk_static_libs = files([
      'lib/public/win64/2015/libprotobuf.lib',
      'lib/public/win64/mathlib.lib',
      'lib/public/win64/tier0.lib',
      'lib/public/win64/tier1.lib',
      'lib/public/win64/interfaces.lib'
    ])
    sdk_dynamic_libs = files([])
  endif

  sdk_cpp_args = []

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