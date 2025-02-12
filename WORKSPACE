load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

#=============================================================================
# Go rules

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "513c12397db1bc9aa46dd62f02dd94b49a9b5d17444d49b5a04c5a89f3053c1c",
    urls = [
        "https://storage.googleapis.com/bazel-mirror/github.com/bazelbuild/rules_go/releases/download/v0.19.5/rules_go-v0.19.5.tar.gz",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.19.5/rules_go-v0.19.5.tar.gz",
    ],
)

http_archive(
    name = "bazel_gazelle",
    sha256 = "7fc87f4170011201b1690326e8c16c5d802836e3a0d617d8f75c3af2b23180c4",
    urls = [
        "https://storage.googleapis.com/bazel-mirror/github.com/bazelbuild/bazel-gazelle/releases/download/0.18.2/bazel-gazelle-0.18.2.tar.gz",
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/0.18.2/bazel-gazelle-0.18.2.tar.gz",
    ],
)

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains(
    go_version = "1.12.9",
)

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies", "go_repository")

gazelle_dependencies()

#=============================================================================
# Docker rules

git_repository(
    name = "io_bazel_rules_docker",
    commit = "267cc613f61921caa5540a6a9437d939f953a90b",
    remote = "https://github.com/bazelbuild/rules_docker.git",
    shallow_since = "1568404961 -0400",
    # tag = "v0.10.1",
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")

container_deps()

load(
    "@io_bazel_rules_docker//repositories:go_repositories.bzl",
    docker_go_deps = "go_deps",
)

docker_go_deps()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

container_pull(
    name = "debian_hyperkube_base_amd64",
    # 'tag' is also supported, but digest is encouraged for reproducibility.
    digest = "sha256:cc782ed16599000ca4c85d47ec6264753747ae1e77520894dca84b104a7621e2",
    registry = "k8s.gcr.io",
    repository = "debian-hyperkube-base-amd64",
    tag = "0.10",
)

git_repository(
    name = "distroless",
    commit = "f905a6636c5106c36cc979bdcc19f0fe4fc01ede",
    remote = "https://github.com/googlecloudplatform/distroless.git",
    shallow_since = "1570036739 -0700",
)

load(
    "@distroless//package_manager:package_manager.bzl",
    "package_manager_repositories",
)

package_manager_repositories()

load(
    "@distroless//package_manager:dpkg.bzl",
    "dpkg_list",
    "dpkg_src",
)

dpkg_src(
    name = "debian_stretch",
    arch = "amd64",
    distro = "stretch",
    sha256 = "da378b113f0b1edcf5b1f2c3074fd5476c7fd6e6df3752f824aad22e7547e699",
    snapshot = "20190520T104418Z",
    url = "http://snapshot.debian.org/archive",
)

dpkg_list(
    name = "package_bundle",
    packages = [
        "cgmanager",
        "dbus",
        "libapparmor1",
        "libcgmanager0",
        "libcryptsetup4",
        "libdbus-1-3",
        "libnih-dbus1",
        "libnih1",
        "libpam-systemd",
        "libprocps6",
        "libseccomp2",
        "procps",
        "systemd",
        "systemd-shim",
    ],
    sources = [
        "@debian_stretch//file:Packages.json",
    ],
)

# We use the prebuilt utils.tar.gz containing socat & conntrack, building it in bazel is really painful
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_file")

http_file(
    name = "utils_tar_gz",
    sha256 = "5c956247241dd94300ba13c6dd9cb5843382d4255125a7a6639d2aad68b9050c",
    urls = ["https://kubeupv2.s3.amazonaws.com/kops/1.12.1/linux/amd64/utils.tar.gz"],
)

# TODO(fejta): use load.bzl, repos.bzl from repo-infra
git_repository(
    name = "io_k8s_repo_infra",
    commit = "db6ceb5f992254db76af7c25db2edc5469b5ea82",
    remote = "https://github.com/kubernetes/repo-infra.git",
    shallow_since = "1570128715 -0700",
)

http_archive(
    name = "bazel_toolchains",
    sha256 = "a019fbd579ce5aed0239de865b2d8281dbb809efd537bf42e0d366783e8dec65",
    strip_prefix = "bazel-toolchains-0.29.2",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-toolchains/archive/0.29.2.tar.gz",
        "https://github.com/bazelbuild/bazel-toolchains/archive/0.29.2.tar.gz",
    ],
)

load("@bazel_toolchains//rules:rbe_repo.bzl", "rbe_autoconfig")

rbe_autoconfig(name = "rbe_default")

go_repository(
    name = "com_github_google_go_containerregistry",
    importpath = "github.com/google/go-containerregistry",
    sum = "h1:PTrxTL8TNRbZts4KqdJMsqRlrdjoiKFDq6MVitj8mPk=",
    version = "v0.0.0-20190829181151-21b2e01cec04",
)

# Start using distroless base
container_pull(
    name = "distroless_base",
    digest = "sha256:a4624843fb1d7d43d9e3d62f6d76b51b6e02b3d03221e29fef4e223d81ef3378",
    registry = "gcr.io",
    repository = "distroless/base",
)
