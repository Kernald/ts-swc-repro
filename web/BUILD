load("@aspect_rules_esbuild//esbuild:defs.bzl", "esbuild")
load("@aspect_rules_swc//swc:defs.bzl", "swc_compile")
load("@aspect_rules_ts//ts:defs.bzl", "ts_config", "ts_project")
load("@bazel_skylib//lib:partial.bzl", "partial")

exports_files(
    [
        "index.html",
    ],
)

ts_config(
    name = "tsconfig",
    src = "tsconfig.json",
)

ts_project(
    name = "web",
    srcs = [
        "app.tsx",
        "index.tsx",
    ],
    declaration = True,
    transpiler = partial.make(
        swc_compile,
        swcrc = ".swcrc",
    ),
    tsconfig = ":tsconfig",
)

esbuild(
    name = "bundle",
    srcs = [":web"],
    config = {
        # https://github.com/aspect-build/rules_esbuild/issues/58
        "resolveExtensions": [".js"],
    },
    entry_point = "index.js",
)

esbuild(
    name = "bundle_prod",
    srcs = [":web"],
    config = {
        "define": {
            "process.env.NODE_ENV": "\"production\"",
        },
        "minify": True,
        # https://github.com/aspect-build/rules_esbuild/issues/58
        "resolveExtensions": [".js"],
        "treeShaking": True,
    },
    entry_point = "index.js",
)
