workspace(name = "io_k8s_cloud_provider_gcp")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "b2038e2de2cace18f032249cb4bb0048abf583a36369fa98f687af1b3f880b26",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.48.1/rules_go-v0.48.1.zip",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.48.1/rules_go-v0.48.1.zip",
    ],
)

http_archive(
    name = "bazel_gazelle",
    integrity = "sha256-MpOL2hbmcABjA1R5Bj2dJMYO2o15/Uc5Vj9Q0zHLMgk=",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-gazelle/releases/download/v0.35.0/bazel-gazelle-v0.35.0.tar.gz",
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/v0.35.0/bazel-gazelle-v0.35.0.tar.gz",
    ],
)


http_archive(
    name = "bazel_skylib",
    sha256 = "f7be3474d42aae265405a592bb7da8e171919d74c16f082a5457840f06054728",
    type = "tar.gz",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
        "https://github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
    ],
)

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "b1e80761a8a8243d03ebca8845e9cc1ba6c82ce7c5179ce2b295cd36f7e394bf",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.25.0/rules_docker-v0.25.0.tar.gz"],
)

load("@bazel_skylib//lib:versions.bzl", "versions")

versions.check(minimum_bazel_version = "5.3.0")

load("@io_bazel_rules_go//go:deps.bzl", "go_download_sdk", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_download_sdk(
    name = "go_sdk",
    version = "1.23.3",
)

go_register_toolchains()

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)
load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")

container_deps()

container_pull(
    name = "distroless",
    digest = "sha256:c6d5981545ce1406d33e61434c61e9452dad93ecd8397c41e89036ef977a88f4",
    registry = "gcr.io",
    repository = "distroless/static",
    tag = "b54513ef989c81d68cb27d9c7958697e2fedd2c4",
)

container_pull(
    name = "go-runner",
    # this digest is actually go-runner-amd64
    digest = "sha256:4decba1ba68d6db721b8ce9bcd1e8567829f0b5bec9a6ceea0b0c094d027c1ac",
    registry = "registry.k8s.io",
    repository = "build-image/go-runner",
    # 'tag' is also supported, but digest is encouraged for reproducibility.
    tag = "v2.4.0-go1.23.3-bookworm.0",
)

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()

git_repository(
    name = "io_k8s_repo_infra",
    commit = "df02ded38f9506e5bbcbf21702034b4fef815f2f",
    remote = "https://github.com/kubernetes/repo-infra.git",
)

load("//defs:repo_rules.bzl", "fetch_kube_release")

fetch_kube_release(
    name = "io_k8s_release",
    archives = {
        "kubernetes-node-linux-amd64.tar.gz": "a94cd519872b170273d6016ff113101bf83a84eb2a28288dadb283b5f3179b19",
        "kubernetes-manifests.tar.gz": "1a6907eb411421e00bcecd34405a44e1c43de06135a0cbaa0302617524e4296b",
        "kubernetes-server-linux-amd64.tar.gz": "067642d462b75742ef32f14e59b4bc7b5c2808fcf64befb25f2496db3eff87e6",
        "kubernetes-node-windows-amd64.tar.gz": "ebf4d2476f0307de676f6060ffd470f20ac59b21c1c2ce6b7888cb8e5d5a47ee",
    },
    version = "v1.31.5",
)
