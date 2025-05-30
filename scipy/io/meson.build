fortran_ignore_warnings = [
  '-Wno-tabs', '-Wno-conversion',
  '-Wno-argument-mismatch', '-Wno-unused-dummy-argument',
  '-Wno-maybe-uninitialized', '-Wno-unused-label',
  '-Wno-unused-variable'
]

_test_fortran_module = custom_target('_test_fortran_module',
  output: ['_test_fortranmodule.c'],
  input: '_test_fortran.pyf',
  command: [py3, generate_f2pymod, '@INPUT@', '-o', '@OUTDIR@']
)

_test_fortran = py3.extension_module('_test_fortran',
  [
    _test_fortran_module,
    '_test_fortran.f'
  ],
  c_args: [numpy_nodepr_api, '-Wno-unused-variable'],
  fortran_args: fortran_ignore_warnings,
  include_directories: [incdir_numpy, incdir_f2py],
  dependencies: [py3_dep, lapack, fortranobject_dep],
  install: true,
  link_language: 'fortran',
  subdir: 'scipy/io'
)

py3.install_sources([
    '__init__.py',
    '_fortran.py',
    '_idl.py',
    '_mmio.py',
    '_netcdf.py',
    'harwell_boeing.py',
    'idl.py',
    'mmio.py',
    'netcdf.py',
    'wavfile.py'
  ],
  pure: false,
  subdir: 'scipy/io'
)

subdir('tests')
subdir('matlab')
subdir('arff')
subdir('_harwell_boeing')
