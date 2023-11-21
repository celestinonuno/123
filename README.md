
               ge4.waitForNetworkIdle(
)

yarn_install(
    name = "aio_npm",
    # Note that we add the postinstall scripts here so that the dependencies are re-installed
    # when the postinstall patches are modified.
    data = [
        YARN_LABEL,
        "//:.yarnrc",
        "//aio:tools/cli-patches/bazel-architect-output.patch",
        "//aio:tools/cli-patches/patch.js",
    ],
    # Currently disabled due to:
    #  1. Missing Windows support currently.
    #  2. Incompatibilites with the `ts_library` rule.
    exports_directories_only = False,
    manual_build_file_contents = npm_package_archives(),
    package_json = "//aio:package.json",
    # We prefer to symlink the `node_modules` to only maintain a single install.
    # See https://github.com/angular/dev-infra/pull/446#issuecomment-1059820287 for details.
    symlink_node_modules = True,
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


