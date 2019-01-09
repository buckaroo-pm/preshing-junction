load('//:subdir_glob.bzl', 'subdir_glob')
load('//:buckaroo_macros.bzl', 'buckaroo_deps', 'buckaroo_cell')

turf = buckaroo_cell('github.com/buckaroo-pm/preshing-turf')

genrule(
  name = 'cmake',
  out = 'out',
  srcs = glob([
    '*.txt',
    'cmake/**/*.cmake',
    'cmake/**/*.in',
  ]),
  cmd = ' && '.join([
    'cp -r $SRCDIR/. $TMP/cmake',
    'cp -r $(location ' + turf + '//:macros) $TMP/turf',
    'mkdir -p $OUT',
    'cd $OUT',
    'cmake -G "Unix Makefiles" $TMP/cmake || true',
  ]),
)

genrule(
  name = 'config-h',
  out = 'junction_config.h',
  cmd = 'cp $(location :cmake)/include/junction_config.h $OUT',
)

genrule(
  name = 'userconfig-h',
  out = 'junction_userconfig.h',
  cmd = 'cp $(location :cmake)/include/junction_userconfig.h $OUT',
)

cxx_library(
  name = 'junction',
  header_namespace = '',
  exported_headers = dict(
    subdir_glob([
      ('', 'junction/**/*.h'),
      ('junction/impl', '**/*.inc'),
    ]).items() + [
      ('junction_config.h', ':config-h'),
      ('junction_userconfig.h', ':userconfig-h'),
    ]
  ),
  srcs = glob([
    'junction/**/*.cpp',
    'junction/**/*.c',
  ]),
  licenses = [
    'LICENSE',
  ],
  deps = buckaroo_deps(),
  visibility = [
    'PUBLIC'
  ],
)
