project(
	'streamvbyte', 'c',
	version : 'v1.0.0',
	meson_version : '>= 0.58',
	default_options : [
		'c_std=c99',
		'warning_level=3',
	 'optimization=3'
	]
)
cc = meson.get_compiler('c')

if get_option('warnings_errors')
  if cc.get_id() != 'msvc'
			add_project_arguments(cc.get_supported_arguments(['-Werror']), language: 'c')
		else
			add_project_arguments(cc.get_supported_arguments(['-WX']), language: 'c')

		 if get_option('warnings_emit')
				add_project_arguments(cc.get_supported_arguments(['-W3', '-w34714', '-wd5045', '-sdl']), language: 'c')
			endif


		endif
endif

if get_option('warnings_emit') and not get_option('warnings_errors')
	add_project_arguments(cc.get_supported_arguments(['-Wall']), language: 'c')
endif

if get_option('sanitize_undefined')
 	add_project_arguments(cc.get_supported_arguments(['-fsanitize=undefined','-fno-sanitize-recover=all']), language: 'c')
		add_project_arguments(cc.get_supported_link_arguments(['-fsanitize=undefined','-fno-sanitize-recover=all']), language: 'c')
endif

compiler_args = [
		'-Wshadow'
]

if cc.get_id() == 'msvc'
  compiler_args += ['-D__restrict__=__restrict']
endif

if target_machine.cpu_family() == 'aarch64'
 	compiler_args += ['-D__ARM_NEON__']
endif

add_project_arguments(cc.get_supported_arguments(compiler_args), language: 'c')

dir_incs = './include/'
path_incs = include_directories(dir_incs)

src_streamvbyte = files([
		'./src/streamvbyte_encode.c',
		'./src/streamvbyte_decode.c',
		'./src/streamvbyte_zigzag.c',
		'./src/streamvbytedelta_encode.c',
		'./src/streamvbytedelta_decode.c',
		'./src/streamvbyte_0124_encode.c',
		'./src/streamvbyte_0124_decode.c',
])

lib_streamvbyte = library('streamvbyte', src_streamvbyte, install : false, include_directories : path_incs)

dep_streamvbyte = declare_dependency(
	link_with           : [ lib_streamvbyte ],
	include_directories : path_incs,
)

if get_option('enable_examples')
exe_streamvbyte_example = executable(
   'example',
   files(['./examples/example.c']),
   link_with : [ lib_streamvbyte ],
   dependencies : [ dep_streamvbyte ]
)

endif
