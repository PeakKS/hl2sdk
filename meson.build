project('hl2sdk-cs2', 'c', 'cpp', default_options: [
  'warning_level=1',
  'werror=false',
  'cpp_std=c++14',
])

supported_platforms = {
  'linux': ['x86_64'],
  'windows': ['x86_64'],
}

sdklib_cflags = [
  '-O3',
  '-march=nocona',
  '-mmmx',
  '-msse',
  '-msse2',
  '-m64',
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
]

sdklib_defines = [
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
]

if target_machine.system() == 'linux'
  sdklib_defines += [
    '-D_LINUX',
    '-DLINUX',
    '-DPOSIX',
    '-DGNUC',
    '-DCOMPILER_GCC',
    '-DNO_MALLOC_OVERRIDE',
  ]
endif

sdklib_undefines = [
  '-Usprintf',
  '-Ustrncpy',
  '-UPROTECTED_THINGS_ENABLE',
]

compiler = meson.get_compiler('cpp')

if target_machine.system() == 'linux'
  tier0 = compiler.find_library('tier0', dirs: [meson.project_source_root() + '/lib/linux64'])
  protobuf = compiler.find_library('protobuf', dirs: [meson.project_source_root() + '/lib/linux64/release'])
  protoc = find_program('devtools/bin/linux/protoc')
elif target_machine.system() == 'windows'
  tier0 = compiler.find_library('tier0', dirs: [meson.project_source_root() + '/lib/public/win64'])
  protobuf = compiler.find_library('protobuf', dirs: [meson.project_source_root() + '/lib/public/win64/2015'])
  protoc = find_program('devtools/bin/protoc.exe')
endif


tier0_dep = declare_dependency(
  dependencies: [
    tier0,
  ],
  include_directories: include_directories([
    'public/tier0',
  ])
)

protobuf_dep = declare_dependency(
  dependencies: [
    protobuf,
  ],
  include_directories: include_directories([
    'thirdparty/protobuf-3.21.8/src/'
  ]),
)

subdir('tier1')
subdir('mathlib')
subdir('interfaces')

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

proto_gen_src=[]
subdir('common')
subdir('game')
subdir('gcsdk')

proto_dep = declare_dependency(
  compile_args: [
    '-D_GLIBCXX_USE_CXX11_ABI=0',
  ],
  sources: [
    proto_gen_src,
  ],
  include_directories: [
    include_directories([
      'thirdparty/protobuf-3.21.8/src',
    ])
  ],
)

if target_machine.system() in supported_platforms
  if target_machine.cpu_family() in supported_platforms[target_machine.system()]
    cs2_dep = declare_dependency(
      variables: {
        'suffix': '2.cs2',
        'engine': 'source2',
      },
      compile_args: [
        '-DSOURCE_ENGINE=25',
        '-DSE_CS2=25',
        '-DCS2',
      ],
      sources: [
        'tier1/convar.cpp',
      ],
      dependencies: [
        tier0_dep,
        tier1_dep,
        mathlib_dep,
        interfaces_dep,
        protobuf_dep,
        proto_dep,
      ],
      include_directories: include_directories([
        'public',
        'public/engine',
        'public/vstdlib',
        'public/entity2',
        'public/game/server',
        'game/shared',
        'game/server',
        'common',
      ]),
    )
  endif
endif