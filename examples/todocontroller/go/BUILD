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
package(default_visibility = ["//visibility:public"])

licenses(["notice"])  # Apache 2.0

load("@io_bazel_rules_docker//go:image.bzl", "go_image")

go_image(
    name = "controller",
    srcs = ["controller.go"],
    importpath = "github.com/bazelbuild/rules_k8s/examples/todocontroller/go",
    deps = [
        "//examples/todocontroller/go/apis/cr/v1:go_default_library",
        "@io_k8s_apimachinery//pkg/fields:go_default_library",
        "@io_k8s_apimachinery//pkg/runtime:go_default_library",
        "@io_k8s_apimachinery//pkg/runtime/serializer:go_default_library",
        "@io_k8s_apimachinery//pkg/util/wait:go_default_library",
        "@io_k8s_api//core/v1:go_default_library",
        "@io_k8s_client_go//rest:go_default_library",
        "@io_k8s_client_go//tools/cache:go_default_library",
        "@io_k8s_client_go//util/workqueue:go_default_library",
        "@com_github_golang_glog//:go_default_library",
    ],
)

load("@k8s_deploy//:defaults.bzl", "k8s_deploy")

k8s_deploy(
    name = "controller-deployment",
    images = {
        "us.gcr.io/not-my-project/todo-controller:staging": ":controller",
    },
    template = "//examples/todocontroller:deployment.yaml",
)

load("@io_bazel_rules_k8s//k8s:objects.bzl", "k8s_objects")

k8s_objects(
    name = "staging",
    objects = [
        "//examples/todocontroller:todo-crd",
        ":controller-deployment",
    ],
)
