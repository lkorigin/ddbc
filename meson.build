project('ddbc', 'd',
    meson_version: '>=0.40',
    license: 'BSL-1.0',
    version: '0.3.7'
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

#
# Targets
#
ddbc_pg_lib = library('ddbc-pgsql',
        [ddbc_src],
        include_directories: [src_dir],
        dependencies: [derelict_pq_dep],
        install: true,
        version: meson.project_version(),
        soversion: project_soversion,
        d_module_versions: ['USE_PGSQL', 'Derelict_Static']
)
pkgc.generate(name: 'ddbc-pgsql',
              libraries: ddbc_pg_lib,
              subdirs: 'd/ddbc',
              version: meson.project_version(),
              requires: ['derelict-pq'],
              description: 'DDBC D database connector built for PostgreSQL.',
              d_module_versions: ['USE_PGSQL', 'Derelict_Static']
)

# to allow others to easily use this as a subproject
ddbc_pg_dep = declare_dependency(
    link_with: [ddbc_pg_lib],
    include_directories: [src_dir],
    dependencies: [derelict_pq_dep]
)

#
# Install
#
install_subdir('source/ddbc/', install_dir: 'include/d/ddbc/')
