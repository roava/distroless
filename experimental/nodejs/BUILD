package(default_visibility = ["//visibility:public"])

load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")
load("@io_bazel_rules_docker//container:container.bzl", "container_image")
load("@package_bundle//file:packages.bzl", "packages")
load("@package_bundle_debian10//file:packages.bzl", packages_debian10 = "packages")

pkg_tar(
    name = "tar",
    # The new_http_archive WORKSPACE rule leaves the original .tar.gz and bazel artifacts in the directory,
    srcs = glob(
        ["**/*"],
        exclude = [
            "WORKSPACE",
            "*.tar.gz",
            "BUILD.bazel",
        ],
    ),
    package_dir = "/nodejs",
    # See: https://github.com/bazelbuild/bazel/issues/2176
    strip_prefix = ".",
)

[container_image(
    name = "nodejs" if (not mode) else mode[1:],
    base = "//cc" + mode,
    entrypoint = ["/nodejs/bin/node"],
    tars = ["@nodejs//:tar"],
    debs = [
    	packages_debian10["apache-pulsar-client"],
    	packages_debian10["apache-pulsar-client-dev"],
    ],
    env = {"LANG": "C.UTF-8"},
) for mode in [
    "",
    ":debug",
]]
