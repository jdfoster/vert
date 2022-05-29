load("@bazel_gazelle//:def.bzl", "gazelle")
load("@rules_python//gazelle/manifest:defs.bzl", "gazelle_python_manifest")
load("@rules_python//gazelle:def.bzl", "GAZELLE_PYTHON_RUNTIME_DEPS")
load("@rules_python//python/pip_install:requirements.bzl", "compile_pip_requirements")


# To refresh the pip-compile requirements file, run `bazel run
# //:requirements.update`.
compile_pip_requirements(
    name = "requirements",
    extra_args = [
        "--allow-unsafe",
    ],
)

# To recalculate the gazelle manifest, run `bazel run
# //:gazelle_python_manifest.update`
gazelle_python_manifest(
    name = "gazelle_python_manifest",
    modules_mapping = ":modules_map",
    pip_repository_name = "pip",
    pip_repository_incremental = True
)

gazelle(
    name = "gazelle",
    data = GAZELLE_PYTHON_RUNTIME_DEPS,
    gazelle = "@rules_python//gazelle:gazelle_python_binary",
)
