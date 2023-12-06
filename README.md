
    # See https://github.com/angular/dev-i
    yarn = YARN_LABEL,
    yarn_lock = "//aio:yarn.lock",
)

yarn_install(
    name = "aio_example_deps",
    # Rename the default js_library target from "node_modules" as this obscures the
    # the source directory stamped as a filegroup in the manual BUILD contents below.
    all_node_modules_target_name = "node_modules_all",
    data = [
        YARN_LABEL,
        "//:.yarnrc",
    ],
    # Disabled because, when False, yarn_install preserves the node_modules folder
    # with bin symlinks in the external repository. This is needed to link the shared
    # set of deps for example e2es.
    exports_directories_only = False,
    manual_build_file_contents = """\
filegroup(
    name = "node_modules_files",


