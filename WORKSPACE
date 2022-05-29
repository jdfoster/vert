load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")


######################################################################
# Requirements for Gazelle
######################################################################

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "ab21448cef298740765f33a7f5acee0607203e4ea321219f2a4c85a6e0fb0a27",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/archive/refs/tags/v0.32.0.zip",
        "https://github.com/bazelbuild/rules_go/archive/refs/tags/v0.32.0.zip",
    ],
)

http_archive(
    name = "bazel_gazelle",
    sha256 = "5982e5463f171da99e3bdaeff8c0f48283a7a5f396ec5282910b9e8a49c0dd7e",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-gazelle/releases/download/v0.25.0/bazel-gazelle-v0.25.0.tar.gz",
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/v0.25.0/bazel-gazelle-v0.25.0.tar.gz",
    ],
)


load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")
load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")


go_rules_dependencies()

go_register_toolchains(version = "1.18")

gazelle_dependencies()


######################################################################
# Python
######################################################################

http_archive(
    name = "rules_python",
    sha256 = "cdf6b84084aad8f10bf20b46b77cb48d83c319ebe6458a18e9d2cebf57807cdd",
    strip_prefix = "rules_python-0.8.1",
    url = "https://github.com/bazelbuild/rules_python/archive/refs/tags/0.8.1.tar.gz",
)


load("@rules_python//gazelle:deps.bzl", _py_gazelle_deps = "gazelle_deps")
load("@rules_python//python:pip.bzl", "pip_parse")
load("@rules_python//python:repositories.bzl", "python_register_toolchains")
load("@rules_python//gazelle/modules_mapping:def.bzl", "modules_mapping")

# Fetch third party dependencies requires for Python + Gazelle.
_py_gazelle_deps()

# To sync the build environment Python interpreter version between the build
# system and development environment, we have opted to use a hermetic python
# interpreter. This will lock the interpreter to fetch dependencies, as well
# as, the runtime use for test and binary.
python_register_toolchains(
    name = "python3_9",
    python_version = "3.9",
)

load("@python3_9//:defs.bzl", "interpreter")

# Define central repository for dependencies; we consume the dependencies
# using the generate requirement macro.
pip_parse(
    name = "pip",
    python_interpreter_target = interpreter,
    requirements_lock="//:requirements.txt" # this file will be a pip-compile file
)

load("@pip//:requirements.bzl", "all_whl_requirements", "install_deps")

install_deps()

modules_mapping(
    name = "modules_map",
    wheels = all_whl_requirements,
)
