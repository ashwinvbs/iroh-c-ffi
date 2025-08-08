project('iroh_c_ffi', 'c', meson_version: '>=1.0.0', version: '0.90.0')

#
# Basic dependency setup
#
cc = meson.get_compiler('c')

mod_cargo = subproject('meson-cargo-1')

#
# Configure cargo integration
#

libiroh_c_ffi = custom_target(
        'libiroh_c_ffi_static',
        kwargs: mod_cargo.get_variable('build_dict') + {
                'input': meson.project_source_root() / 'Cargo.toml',
                'output': ['libiroh_c_ffi.a'],
        },
)

m_dep = cc.find_library('m', required : false)

libiroh_c_ffi_dep = declare_dependency(
        include_directories: include_directories('.'),
        link_with: libiroh_c_ffi,
        version: meson.project_version(),
        dependencies: m_dep,
)

meson.add_dist_script(
        mod_cargo.get_variable('dist_cmd') + [
                meson.project_source_root() / 'Cargo.toml',
        ],
)

run_target(
        'vendor',
        kwargs: mod_cargo.get_variable('vendor_dict') + {
                'command': mod_cargo.get_variable('vendor_cmd') + [
                        meson.project_source_root() / 'Cargo.toml',
                ],
        },
)

#
# Tests
#

executable('multi_thread_client', ['multi-thread-client.c'], dependencies: [libiroh_c_ffi_dep])
executable('multi_thread_server', ['multi-thread-server.c'], dependencies: [libiroh_c_ffi_dep])
executable('single_thread_server', ['multi-thread-client.c'], dependencies: [libiroh_c_ffi_dep])