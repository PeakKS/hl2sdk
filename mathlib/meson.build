mathlib_src = [
  'anorms.cpp',
  'bumpvects.cpp',
  'color_conversion.cpp',
  'halton.cpp',
  'IceKey.cpp',
  'imagequant.cpp',
  'lightdesc.cpp',
  'mathlib_base.cpp',
  'polyhedron.cpp',
  'powsse.cpp',
  'quantize.cpp',
  'randsse.cpp',
  'simdvectormatrix.cpp',
  'sparse_convolution_noise.cpp',
  'sse.cpp',
  'sseconst.cpp',
  'ssenoise.cpp',
  'vector.cpp',
  'vmatrix.cpp',
]

mathlib = static_library(
  'mathlib',
  mathlib_src,
  name_prefix: '',
  pic: true,
  include_directories: include_directories([
    '../public',
    '../public/tier0',
    '../public/tier1',
    '../public/mathlib',
  ]),
  cpp_args: [
    sdklib_cflags,
    sdklib_defines,
    sdklib_undefines,
    '-D_LIB',
  ],
)

mathlib_dep = declare_dependency(
  link_with: [
    mathlib,
  ],
  include_directories: include_directories([
    '../public/mathlib',
  ])
)