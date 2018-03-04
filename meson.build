project('ddbc', 'd',
    meson_version: '>=0.44.1',
    license: 'BSL-1.0',
    version: '0.3.7',
    subproject_dir: 'deps/'
)

project_soversion = '0'

#
# Dependencies
#
pkgc = import('pkgconfig')
derelict_pq_dep = dependency('derelict-pq', version: '>= 2.2.0', fallback: ['derelict-pq', 'derelict_pq_dep'])

#
# Sources
#
ddbc_src = [
    'source/ddbc/all.d',
    'source/ddbc/common.d',
    'source/ddbc/core.d',
    'source/ddbc/drivers/mysqlddbc.d',
    'source/ddbc/drivers/odbcddbc.d',
    'source/ddbc/drivers/pgsqlddbc.d',
    'source/ddbc/drivers/sqliteddbc.d',
    'source/ddbc/drivers/utils.d',
    'source/ddbc/package.d',
    'source/ddbc/pods.d'
]

src_dir = include_directories('source/')

# determine the right version flag for the compiler we use
dc = meson.get_compiler('d')
dc_version_prefix = '-fversion='
if dc.get_id() == 'llvm'
    dc_version_prefix = '-d-version='
elif dc.get_id() == 'dmd'
    dc_version_prefix = '-version='
endif

#
# Targets
#
pg_module_versions = ['USE_PGSQL', 'Derelict_Static']

ddbc_pg_lib = library('ddbc-pgsql',
        [ddbc_src],
        include_directories: [src_dir],
        dependencies: [derelict_pq_dep],
        install: true,
        version: meson.project_version(),
        soversion: project_soversion,
        d_module_versions: pg_module_versions
)
pkgc.generate(name: 'ddbc-pgsql',
              libraries: ddbc_pg_lib,
              subdirs: 'd/ddbc',
              version: meson.project_version(),
              requires: ['derelict-pq'],
              description: 'DDBC D database connector built for PostgreSQL.',
              d_module_versions: pg_module_versions
)

# prepare flags for declare_dependency
pg_dep_compile_flags = []
foreach m : pg_module_versions
    pg_dep_compile_flags += dc_version_prefix + m
endforeach

# to allow others to easily use this as a subproject
ddbc_pg_dep = declare_dependency(
    link_with: [ddbc_pg_lib],
    include_directories: [src_dir],
    dependencies: [derelict_pq_dep],
    compile_args: pg_dep_compile_flags
)

#
# Install
#
install_subdir('source/ddbc/', install_dir: 'include/d/ddbc/')
