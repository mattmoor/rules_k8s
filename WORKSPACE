# Copyright 2017 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
workspace(name = "io_bazel_rules_k8s")

git_repository(
    name = "io_bazel_rules_docker",
    commit = "839a297d4e874216b4fd93f09dd35be5592dc10e",
    remote = "https://github.com/bazelbuild/rules_docker.git",
)

load(
    "@io_bazel_rules_docker//docker:docker.bzl",
    "docker_repositories",
)

docker_repositories()

load("//k8s:k8s.bzl", "k8s_repositories", "k8s_defaults")

k8s_repositories()

_CLUSTER = "gke_rules-k8s_us-central1-f_testing"

_NAMESPACE = "{BUILD_USER}"

k8s_defaults(
    name = "k8s_deploy",
    cluster = _CLUSTER,
    image_chroot = "us.gcr.io/rules_k8s/{BUILD_USER}",
    kind = "deployment",
    namespace = _NAMESPACE,
)

[k8s_defaults(
    name = "k8s_" + kind,
    cluster = _CLUSTER,
    kind = kind,
    namespace = _NAMESPACE,
) for kind in [
    "service",
    "crd",
    "todo",
]]

new_http_archive(
    name = "mock",
    build_file_content = """
# Rename mock.py to __init__.py
genrule(
    name = "rename",
    srcs = ["mock.py"],
    outs = ["__init__.py"],
    cmd = "cat $< >$@",
)
py_library(
   name = "mock",
   srcs = [":__init__.py"],
   visibility = ["//visibility:public"],
)""",
    sha256 = "b839dd2d9c117c701430c149956918a423a9863b48b09c90e30a6013e7d2f44f",
    strip_prefix = "mock-1.0.1/",
    type = "tar.gz",
    url = "https://pypi.python.org/packages/source/m/mock/mock-1.0.1.tar.gz",
)

# ================================================================
# Imports for examples/
# ================================================================

git_repository(
    name = "org_pubref_rules_protobuf",
    commit = "6361b3fd5813bf688a05364bb836502a068a71ac",  # patched v0.8.1 (Sep 27 2017)
    remote = "https://github.com/pubref/rules_protobuf.git",
)

load("@org_pubref_rules_protobuf//protobuf:rules.bzl", "proto_repositories")

proto_repositories()

load("@org_pubref_rules_protobuf//cpp:rules.bzl", "cpp_proto_repositories")

cpp_proto_repositories()

load("@org_pubref_rules_protobuf//java:rules.bzl", "java_proto_repositories")

java_proto_repositories()

# We use cc_image to build a sample service
load(
    "@io_bazel_rules_docker//cc:image.bzl",
    _cc_image_repos = "repositories",
)

_cc_image_repos()

# We use java_image to build a sample service
load(
    "@io_bazel_rules_docker//java:image.bzl",
    _java_image_repos = "repositories",
)

_java_image_repos()

git_repository(
    name = "io_bazel_rules_go",
    commit = "6d900bc95ae678bec5c91031b8e987957d2a7f93",
    remote = "https://github.com/bazelbuild/rules_go.git",
)

load(
    "@io_bazel_rules_go//go:def.bzl",
    "go_register_toolchains",
    "go_rules_dependencies",
    "go_repository",
)

go_rules_dependencies()

go_register_toolchains()

load(
    "@io_bazel_rules_go//proto:def.bzl",
    "proto_register_toolchains",
)

proto_register_toolchains()

# We use go_image to build a sample service
load(
    "@io_bazel_rules_docker//go:image.bzl",
    _go_image_repos = "repositories",
)

_go_image_repos()

# load("@org_pubref_rules_protobuf//go:rules.bzl", "go_proto_repositories")

# go_proto_repositories()

git_repository(
    name = "io_bazel_rules_python",
    commit = "c208292d1286e9a0280555187caf66cd3b4f5bed",
    remote = "https://github.com/bazelbuild/rules_python.git",
)

load(
    "@io_bazel_rules_python//python:pip.bzl",
    "pip_import",
    "pip_repositories",
)

pip_repositories()

pip_import(
    name = "examples_helloworld_pip",
    requirements = "//examples/hellogrpc/py:requirements.txt",
)

load(
    "@examples_helloworld_pip//:requirements.bzl",
    grpcpip_install = "pip_install",
)

grpcpip_install()

pip_import(
    name = "examples_hellohttp_pip",
    requirements = "//examples/hellohttp/py:requirements.txt",
)

load(
    "@examples_hellohttp_pip//:requirements.bzl",
    httppip_install = "pip_install",
)

httppip_install()

# We use py_image to build a sample service
load(
    "@io_bazel_rules_docker//python:image.bzl",
    _py_image_repos = "repositories",
)

_py_image_repos()

load("@org_pubref_rules_protobuf//python:rules.bzl", "py_proto_repositories")

py_proto_repositories()

git_repository(
    name = "io_bazel_rules_jsonnet",
    commit = "09ec18db5b9ad3129810f5f0ccc86363a8bfb6be",
    remote = "https://github.com/bazelbuild/rules_jsonnet.git",
)

load("@io_bazel_rules_jsonnet//jsonnet:jsonnet.bzl", "jsonnet_repositories")

jsonnet_repositories()

pip_import(
    name = "examples_todocontroller_pip",
    requirements = "//examples/todocontroller/py:requirements.txt",
)

load(
    "@examples_todocontroller_pip//:requirements.bzl",
    _controller_pip_install = "pip_install",
)

_controller_pip_install()

# This corresponds to this Git repo:
# https://github.com/kubernetes/apimachinery
go_repository(
    name = "io_k8s_apimachinery",
    build_file_proto_mode = "disable",
    commit = "18a564baac720819100827c16fdebcadb05b2d0d",
    importpath = "k8s.io/apimachinery",
)

go_repository(
    name = "com_github_golang_glog",
    commit = "23def4e6c14b4da8ac2ed8007337bc5eb5007998",
    importpath = "github.com/golang/glog",
)

go_repository(
    name = "com_github_gogo_protobuf",
    commit = "117892bf1866fbaa2318c03e50e40564c8845457",
    importpath = "github.com/gogo/protobuf",
)

go_repository(
    name = "com_github_json_iterator_go",
    commit = "aed5a81f09330b8d254a117fb853e2f886f1cb3b",
    importpath = "github.com/json-iterator/go",
)

go_repository(
    name = "com_github_ghodss_yaml",
    commit = "0ca9ea5df5451ffdf184b4428c902747c2c11cd7",
    importpath = "github.com/ghodss/yaml",
)

go_repository(
    name = "in_gopkg_yaml_v2",
    commit = "eb3733d160e74a9c7e442f435eb3bea458e1d19f",
    importpath = "gopkg.in/yaml.v2",
)

go_repository(
    name = "io_k8s_client_go",
    commit = "72e1c2a1ef30b3f8da039e92d4a6a1f079f374e8",
    importpath = "k8s.io/client-go",
)

go_repository(
    name = "io_k8s_api",
    commit = "218912509d74a117d05a718bb926d0948e531c20",
    importpath = "k8s.io/api",
)

go_repository(
    name = "com_github_juju_ratelimit",
    commit = "59fac5042749a5afb9af70e813da1dd5474f0167",
    importpath = "github.com/juju/ratelimit",
)

http_archive(
    name = "io_kubernetes_build",
    sha256 = "8e49ac066fbaadd475bd63762caa90f81cd1880eba4cc25faa93355ef5fa2739",
    strip_prefix = "repo-infra-e26fc85d14a1d3dc25569831acc06919673c545a",
    urls = ["https://github.com/kubernetes/repo-infra/archive/e26fc85d14a1d3dc25569831acc06919673c545a.tar.gz"],
)

go_repository(
    name = "in_gopkg_inf_v0",
    commit = "3887ee99ecf07df5b447e9b00d9c0b2adaa9f3e4",
    importpath = "gopkg.in/inf.v0",
)

go_repository(
    name = "com_github_davecgh_go_spew",
    commit = "ecdeabc65495df2dec95d7c4a4c3e021903035e5",
    importpath = "github.com/davecgh/go-spew",
)

go_repository(
    name = "io_k8s_kube_openapi",
    commit = "39a7bf85c140f972372c2a0d1ee40adbf0c8bfe1",
    importpath = "k8s.io/kube-openapi",
)

go_repository(
    name = "com_github_google_gofuzz",
    commit = "24818f796faf91cd76ec7bddd72458fbced7a6c1",
    importpath = "github.com/google/gofuzz",
)

go_repository(
    name = "com_github_go_openapi_spec",
    commit = "84b5bee7bcb76f3d17bcbaf421bac44bd5709ca6",
    importpath = "github.com/go-openapi/spec",
)

go_repository(
    name = "com_github_peterbourgon_diskv",
    commit = "53ef9e43a0bc608e737e6bfed35207ad9cb1ad54",
    importpath = "github.com/peterbourgon/diskv",
)

go_repository(
    name = "com_github_gregjones_httpcache",
    commit = "22a0b1feae53974ed4cfe27bcce70dba061cc5fd",
    importpath = "github.com/gregjones/httpcache",
)

go_repository(
    name = "com_github_spf13_pflag",
    commit = "97afa5e7ca8a08a383cb259e06636b5e2cc7897f",
    importpath = "github.com/spf13/pflag",
)

go_repository(
    name = "com_github_go_openapi_swag",
    commit = "cf0bdb963811675a4d7e74901cefc7411a1df939",
    importpath = "github.com/go-openapi/swag",
)

go_repository(
    name = "com_github_emicklei_go_restful",
    commit = "2dd44038f0b95ae693b266c5f87593b5d2fdd78d",
    importpath = "github.com/emicklei/go-restful",
)

go_repository(
    name = "com_github_go_openapi_jsonreference",
    commit = "36d33bfe519efae5632669801b180bf1a245da3b",
    importpath = "github.com/go-openapi/jsonreference",
)

go_repository(
    name = "com_github_go_openapi_jsonpointer",
    commit = "779f45308c19820f1a69e9a4cd965f496e0da10f",
    importpath = "github.com/go-openapi/jsonpointer",
)
