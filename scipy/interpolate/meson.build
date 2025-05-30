fitpack_src = [
  'fitpack/bispeu.f',
  'fitpack/bispev.f',
  'fitpack/clocur.f',
  'fitpack/cocosp.f',
  'fitpack/concon.f',
  'fitpack/concur.f',
  'fitpack/cualde.f',
  'fitpack/curev.f',
  'fitpack/curfit.f',
  'fitpack/dblint.f',
  'fitpack/evapol.f',
  'fitpack/fourco.f',
  'fitpack/fpader.f',
  'fitpack/fpadno.f',
  'fitpack/fpadpo.f',
  'fitpack/fpback.f',
  'fitpack/fpbacp.f',
  'fitpack/fpbfout.f',
  'fitpack/fpbisp.f',
  'fitpack/fpbspl.f',
  'fitpack/fpchec.f',
  'fitpack/fpched.f',
  'fitpack/fpchep.f',
  'fitpack/fpclos.f',
  'fitpack/fpcoco.f',
  'fitpack/fpcons.f',
  'fitpack/fpcosp.f',
  'fitpack/fpcsin.f',
  'fitpack/fpcurf.f',
  'fitpack/fpcuro.f',
  'fitpack/fpcyt1.f',
  'fitpack/fpcyt2.f',
  'fitpack/fpdeno.f',
  'fitpack/fpdisc.f',
  'fitpack/fpfrno.f',
  'fitpack/fpgivs.f',
  'fitpack/fpgrdi.f',
  'fitpack/fpgrpa.f',
  'fitpack/fpgrre.f',
  'fitpack/fpgrsp.f',
  'fitpack/fpinst.f',
  'fitpack/fpintb.f',
  'fitpack/fpknot.f',
  'fitpack/fpopdi.f',
  'fitpack/fpopsp.f',
  'fitpack/fporde.f',
  'fitpack/fppara.f',
  'fitpack/fppasu.f',
  'fitpack/fpperi.f',
  'fitpack/fppocu.f',
  'fitpack/fppogr.f',
  'fitpack/fppola.f',
  'fitpack/fprank.f',
  'fitpack/fprati.f',
  'fitpack/fpregr.f',
  'fitpack/fprota.f',
  'fitpack/fprppo.f',
  'fitpack/fprpsp.f',
  'fitpack/fpseno.f',
  'fitpack/fpspgr.f',
  'fitpack/fpsphe.f',
  'fitpack/fpsuev.f',
  'fitpack/fpsurf.f',
  'fitpack/fpsysy.f',
  'fitpack/fptrnp.f',
  'fitpack/fptrpe.f',
  'fitpack/insert.f',
  'fitpack/parcur.f',
  'fitpack/parder.f',
  'fitpack/pardeu.f',
  'fitpack/parsur.f',
  'fitpack/percur.f',
  'fitpack/pogrid.f',
  'fitpack/polar.f',
  'fitpack/profil.f',
  'fitpack/regrid.f',
  'fitpack/spalde.f',
  'fitpack/spgrid.f',
  'fitpack/sphere.f',
  'fitpack/splder.f',
  'fitpack/splev.f',
  'fitpack/splint.f',
  'fitpack/sproot.f',
  'fitpack/surev.f',
  'fitpack/surfit.f',
  'fitpack/pardtc.f'
]

# TODO: Add flags for 64 bit ints
fitpack_lib = static_library('fitpack_lib',
  fitpack_src,
  fortran_args: '-Wno-maybe-uninitialized'
)

interpnd = py3.extension_module('interpnd',
  spt_cython_gen.process('interpnd.pyx'),
  c_args: ['-Wno-discarded-qualifiers', cython_c_args],
  include_directories: [incdir_numpy],
  dependencies: [py3_dep],
  install: true,
  subdir: 'scipy/interpolate'
)

py3.extension_module('_ppoly',
  linalg_cython_gen.process('_ppoly.pyx'),
  c_args: cython_c_args,
  include_directories: [incdir_numpy],
  dependencies: [py3_dep],
  install: true,
  subdir: 'scipy/interpolate'
)

_bspl = py3.extension_module('_bspl',
  cython_gen.process('_bspl.pyx'),
  c_args: cython_c_args,
  include_directories: ['src/', incdir_numpy],
  dependencies: [py3_dep],
  install: true,
  subdir: 'scipy/interpolate'
)

# TODO: Add flags for 64 bit ints
_fitpack = py3.extension_module('_fitpack',
  ['src/_fitpackmodule.c'],
  c_args: numpy_nodepr_api,
  link_with: [fitpack_lib],
  include_directories: ['src/', incdir_numpy],
  dependencies: [py3_dep],
  link_language: 'fortran',
  install: true,
  subdir: 'scipy/interpolate'
)

dfitpack_module = custom_target('dfitpack_module',
  output: ['dfitpack-f2pywrappers.f', 'dfitpackmodule.c'],
  input: 'src/fitpack.pyf',
  command: [py3, generate_f2pymod, '@INPUT@', '-o', '@OUTDIR@']
)

# TODO: Add flags for 64 bit ints
dfitpack = py3.extension_module('dfitpack',
  dfitpack_module,
  c_args: [numpy_nodepr_api, '-Wno-unused-variable'],
  include_directories: [incdir_numpy, incdir_f2py],
  dependencies: [py3_dep, lapack, fortranobject_dep],
  link_with: [fitpack_lib],
  install: true,
  link_language: 'fortran',
  subdir: 'scipy/interpolate'
)

if use_pythran
  if cc.has_argument('-Wno-unused-but-set-variable')
    Wno_unused_but_set = ['-Wno-unused-but-set-variable']
  else
    Wno_unused_but_set = []
  endif
  _rbfinterp_pythran = custom_target('_rbfinterp_pythran',
    output: ['_rbfinterp_pythran.cpp'],
    input: '_rbfinterp_pythran.py',
    command: [pythran, '-E', '@INPUT@', '-o', '@OUTDIR@/_rbfinterp_pythran.cpp']
  )

  _rbfinterp_pythran = py3.extension_module('_rbfinterp_pythran',
    [_rbfinterp_pythran],
    cpp_args: [
      '-Wno-unused-function', '-Wno-unused-variable',
      '-Wno-deprecated-declarations',
      '-Wno-cpp', '-Wno-int-in-bool-context',
      Wno_unused_but_set
    ] + cpp_args_pythran,
    include_directories: [incdir_pythran, incdir_numpy],
    dependencies: [py3_dep],
    install: true,
    subdir: 'scipy/interpolate'
  )
endif

py3.install_sources([
    '__init__.py',
    '_bsplines.py',
    '_cubic.py',
    '_fitpack_impl.py',
    '_fitpack_py.py',
    '_fitpack2.py',
    '_interpnd_info.py',
    '_interpolate.py',
    '_ndgriddata.py',
    '_pade.py',
    '_polyint.py',
    '_rbf.py',
    '_rbfinterp.py',
    '_rbfinterp_pythran.py',
    '_rgi.py',
    'fitpack.py',
    'fitpack2.py',
    'interpolate.py',
    'ndgriddata.py',
    'polyint.py',
    'rbf.py'
  ],
  pure: false,
  subdir: 'scipy/interpolate'
)

subdir('tests')
