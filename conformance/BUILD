# Copyright (c) 2024, Google LLC
# All rights reserved.
#
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file or at
# https://developers.google.com/open-source/licenses/bsd

# Protobuf conformance tests.

load("@rules_cc//cc:cc_library.bzl", "cc_library")
load("@rules_cc//cc:cc_test.bzl", "cc_test")
load("@rules_cc//cc:defs.bzl", "cc_binary", "objc_library")
load("@rules_java//java:defs.bzl", "java_binary")
load("@rules_python//python:py_binary.bzl", "py_binary")
load("@rules_ruby//ruby:defs.bzl", "rb_binary")
load(
    "//:protobuf.bzl",
    "internal_csharp_proto_library",
    "internal_objc_proto_library",
    "internal_php_proto_library",
    "internal_py_proto_library",
    "internal_ruby_proto_library",
)
load("//bazel:cc_proto_library.bzl", "cc_proto_library")
load("//bazel:java_lite_proto_library.bzl", "java_lite_proto_library")
load("//bazel:java_proto_library.bzl", "java_proto_library")
load("//bazel:proto_library.bzl", "proto_library")
load("//build_defs:internal_shell.bzl", "inline_sh_binary")
load("//ruby:defs.bzl", "internal_ruby_proto_library")

exports_files([
    "bazel_conformance_test_runner.sh",
    "failure_list_cpp.txt",
    "failure_list_csharp.txt",
    "failure_list_java.txt",
    "failure_list_java_lite.txt",
    "failure_list_objc.txt",
    "failure_list_php.txt",
    "failure_list_php_c.txt",
    "failure_list_python.txt",
    "failure_list_python_cpp.txt",
    "failure_list_python_upb.txt",
    "failure_list_ruby.txt",
    "failure_list_jruby.txt",
    "failure_list_jruby_ffi.txt",
    "text_format_failure_list_cpp.txt",
    "text_format_failure_list_java.txt",
    "text_format_failure_list_java_lite.txt",
    "text_format_failure_list_php.txt",
    "text_format_failure_list_php_c.txt",
    "text_format_failure_list_python.txt",
])

cc_library(
    name = "conformance_test_lib",
    testonly = 1,
    srcs = [
        "conformance_test.cc",
        "conformance_test_runner.cc",
    ],
    hdrs = [
        "conformance_test.h",
    ],
    deps = [
        ":conformance_cc_proto",
        ":failure_list_trie_node",
        ":fork_pipe_runner",
        ":test_runner",
        "//src/google/protobuf",
        "//src/google/protobuf:descriptor_legacy",
        "//src/google/protobuf:endian",
        "//src/google/protobuf/util:differencer",
        "@abseil-cpp//absl/container:btree",
        "@abseil-cpp//absl/container:flat_hash_set",
        "@abseil-cpp//absl/log:absl_check",
        "@abseil-cpp//absl/log:absl_log",
        "@abseil-cpp//absl/status",
        "@abseil-cpp//absl/strings",
        "@abseil-cpp//absl/strings:str_format",
    ],
)

cc_library(
    name = "conformance_test_main",
    testonly = 1,
    srcs = ["conformance_test_main.cc"],
    deps = [
        ":binary_json_conformance_suite",
        ":conformance_test_lib",
        ":text_format_conformance_suite",
    ],
)

cc_library(
    name = "failure_list_trie_node",
    srcs = ["failure_list_trie_node.cc"],
    hdrs = ["failure_list_trie_node.h"],
    deps = [
        "@abseil-cpp//absl/status",
        "@abseil-cpp//absl/strings",
        "@abseil-cpp//absl/strings:str_format",
        "@abseil-cpp//absl/types:optional",
    ],
)

cc_test(
    name = "failure_list_trie_node_test",
    srcs = ["failure_list_trie_node_test.cc"],
    deps = [
        ":failure_list_trie_node",
        "@abseil-cpp//absl/status",
        "@abseil-cpp//absl/status:statusor",
        "@abseil-cpp//absl/strings",
        "@abseil-cpp//absl/types:optional",
        "@googletest//:gtest",
        "@googletest//:gtest_main",
    ],
)

# Add more meta-testing here. This is not to be confused with a conformance test itself.
test_suite(
    name = "conformance_framework_tests",
    tests = [
        "failure_list_trie_node_test",
    ],
)

cc_library(
    name = "binary_json_conformance_suite",
    testonly = 1,
    srcs = ["binary_json_conformance_suite.cc"],
    hdrs = ["binary_json_conformance_suite.h"],
    deps = [
        ":binary_wireformat",
        ":conformance_cc_proto",
        ":conformance_test_lib",
        "//:test_messages_proto2_cc_proto",
        "//:test_messages_proto3_cc_proto",
        "//conformance/test_protos:test_messages_edition2023_cc_proto",
        "//editions:test_messages_proto2_editions_cc_proto",
        "//editions:test_messages_proto3_editions_cc_proto",
        "//src/google/protobuf",
        "//src/google/protobuf:endian",
        "//src/google/protobuf:protobuf_lite",
        "//src/google/protobuf/json",
        "//src/google/protobuf/util:type_resolver",
        "@abseil-cpp//absl/log:absl_check",
        "@abseil-cpp//absl/log:absl_log",
        "@abseil-cpp//absl/log:die_if_null",
        "@abseil-cpp//absl/status",
        "@abseil-cpp//absl/strings",
        "@abseil-cpp//absl/strings:str_format",
        "@abseil-cpp//absl/strings:string_view",
        "@jsoncpp",
    ],
)

cc_library(
    name = "text_format_conformance_suite",
    testonly = 1,
    srcs = ["text_format_conformance_suite.cc"],
    hdrs = ["text_format_conformance_suite.h"],
    deps = [
        ":conformance_test_lib",
        "//:test_messages_proto2_cc_proto",
        "//:test_messages_proto3_cc_proto",
        "//conformance/test_protos:test_messages_edition2023_cc_proto",
        "//editions:test_messages_proto2_editions_cc_proto",
        "//editions:test_messages_proto3_editions_cc_proto",
        "//src/google/protobuf",
        "@abseil-cpp//absl/log:absl_log",
        "@abseil-cpp//absl/log:die_if_null",
        "@abseil-cpp//absl/strings",
        "@abseil-cpp//absl/strings:str_format",
    ],
)

cc_library(
    name = "test_runner",
    hdrs = ["test_runner.h"],
)

cc_library(
    name = "fork_pipe_runner",
    srcs = ["fork_pipe_runner.cc"],
    hdrs = ["fork_pipe_runner.h"],
    deps = [
        ":conformance_cc_proto",
        ":test_runner",
        "//src/google/protobuf:endian",
        "@abseil-cpp//absl/log:absl_log",
        "@abseil-cpp//absl/strings:str_format",
    ],
)

cc_library(
    name = "binary_wireformat",
    testonly = 1,
    srcs = ["binary_wireformat.cc"],
    hdrs = ["binary_wireformat.h"],
    deps = [
        "//src/google/protobuf:endian",
        "//src/google/protobuf:protobuf_lite",
        "@abseil-cpp//absl/strings",
    ],
)

cc_test(
    name = "binary_wireformat_test",
    srcs = ["binary_wireformat_test.cc"],
    deps = [
        ":binary_wireformat",
        "@abseil-cpp//absl/strings",
        "@abseil-cpp//absl/strings:string_view",
        "@googletest//:gtest",
        "@googletest//:gtest_main",
    ],
)

cc_binary(
    name = "conformance_test_runner",
    testonly = 1,
    visibility = ["//visibility:public"],
    deps = [":conformance_test_main"],
)

proto_library(
    name = "conformance_proto",
    srcs = ["conformance.proto"],
    visibility = ["//visibility:public"],
)

cc_proto_library(
    name = "conformance_cc_proto",
    deps = [":conformance_proto"],
)

java_proto_library(
    name = "conformance_java_proto",
    deps = [":conformance_proto"],
)

java_lite_proto_library(
    name = "conformance_java_proto_lite",
    deps = [":conformance_proto"],
)

exports_files([
    "bazel_conformance_test_runner.sh",
    "failure_list_cpp.txt",
    "failure_list_csharp.txt",
    "failure_list_java.txt",
    "failure_list_java_lite.txt",
    "failure_list_objc.txt",
    "failure_list_php.txt",
    "failure_list_php_c.txt",
    "failure_list_python.txt",
    "failure_list_python_cpp.txt",
    "failure_list_python_upb.txt",
    "failure_list_ruby.txt",
    "failure_list_jruby.txt",
    "failure_list_jruby_ffi.txt",
    "text_format_failure_list_cpp.txt",
    "text_format_failure_list_java.txt",
    "text_format_failure_list_java_lite.txt",
    "text_format_failure_list_php.txt",
    "text_format_failure_list_php_c.txt",
    "text_format_failure_list_python.txt",
])

internal_csharp_proto_library(
    name = "conformance_csharp_proto",
    srcs = ["conformance.proto"],
    visibility = [
        "//csharp:__subpackages__",
    ],
)

internal_objc_proto_library(
    name = "conformance_objc_proto",
    srcs = ["conformance.proto"],
    visibility = [
        "//conformance:__pkg__",
        "//objc:__pkg__",
    ],
)

internal_py_proto_library(
    name = "conformance_py_proto",
    srcs = ["conformance.proto"],
    srcs_version = "PY2AND3",
    visibility = [
        "//python:__subpackages__",
    ],
)

internal_php_proto_library(
    name = "conformance_php_proto",
    srcs = ["conformance.proto"],
    outs = [
        "Conformance/ConformanceRequest.php",
        "Conformance/ConformanceResponse.php",
        "Conformance/FailureSet.php",
        "Conformance/JspbEncodingConfig.php",
        "Conformance/TestCategory.php",
        "Conformance/WireFormat.php",
        "GPBMetadata/Conformance.php",
    ],
    visibility = [
        "//conformance:__pkg__",
        "//php:__pkg__",
    ],
)

internal_ruby_proto_library(
    name = "conformance_ruby_proto",
    srcs = ["conformance.proto"],
    visibility = [
        "//conformance:__pkg__",
        "//ruby:__pkg__",
    ],
)

cc_binary(
    name = "conformance_cpp",
    testonly = 1,
    srcs = ["conformance_cpp.cc"],
    visibility = [
        "//pkg:__pkg__",
        "//src:__subpackages__",
    ],
    deps = [
        ":conformance_cc_proto",
        "//:protobuf",
        "//:test_messages_proto2_cc_proto",
        "//:test_messages_proto3_cc_proto",
        "//conformance/test_protos:test_messages_edition2023_cc_proto",
        "//editions:test_messages_proto2_editions_cc_proto",
        "//editions:test_messages_proto3_editions_cc_proto",
        "//src/google/protobuf",
        "//src/google/protobuf:port",
        "//src/google/protobuf:protobuf_lite",
        "//src/google/protobuf/json",
        "//src/google/protobuf/stubs",
        "//src/google/protobuf/util:json_util",
        "//src/google/protobuf/util:type_resolver",
        "@abseil-cpp//absl/log:absl_check",
        "@abseil-cpp//absl/log:absl_log",
        "@abseil-cpp//absl/status",
        "@abseil-cpp//absl/status:statusor",
        "@abseil-cpp//absl/strings",
    ],
)

java_binary(
    name = "conformance_java",
    testonly = 1,
    srcs = ["ConformanceJava.java"],
    main_class = "ConformanceJava",
    visibility = [
        "//java:__subpackages__",
    ],
    deps = [
        ":conformance_java_proto",
        "//:protobuf_java",
        "//:protobuf_java_util",
        "//:test_messages_proto2_java_proto",
        "//:test_messages_proto3_java_proto",
        "//conformance/test_protos:test_messages_edition2023_java_proto",
        "//editions:test_messages_proto2_editions_java_proto",
        "//editions:test_messages_proto3_editions_java_proto",
    ],
)

java_binary(
    name = "conformance_java_lite",
    testonly = 1,
    srcs = ["ConformanceJavaLite.java"],
    main_class = "ConformanceJavaLite",
    visibility = [
        "//java:__subpackages__",
    ],
    deps = [
        ":conformance_java_proto_lite",
        "//:protobuf_java_util",
        "//:protobuf_javalite",
        "//:test_messages_proto2_java_proto_lite",
        "//:test_messages_proto3_java_proto_lite",
        "//conformance/test_protos:test_messages_edition2023_java_proto_lite",
        "//editions:test_messages_proto2_editions_java_proto_lite",
        "//editions:test_messages_proto3_editions_java_proto_lite",
    ],
)

py_binary(
    name = "conformance_python",
    testonly = 1,
    srcs = ["conformance_python.py"],
    imports = [
        "..",
        "../python",
    ],
    srcs_version = "PY2AND3",
    visibility = ["//python:__subpackages__"],
    deps = [
        ":conformance_py_proto",
        "//:protobuf_python",
        "//conformance/test_protos:test_messages_edition2023_py_pb2",
        "//editions:test_messages_proto2_editions_py_pb2",
        "//editions:test_messages_proto3_editions_py_pb2",
        "//python:_message",  # Make upb visible if we need it.
        "//python:conformance_test_py_proto",
    ],
)

py_binary(
    name = "update_failure_list",
    srcs = ["update_failure_list.py"],
)

inline_sh_binary(
    name = "conformance_php",
    testonly = 1,
    srcs = [
        "autoload.php",
        "conformance_php.php",
    ],
    cmd = """
        php -d include_path=php/generated:conformance:src/google/protobuf:editions/golden \\
            -d auto_prepend_file=$(rootpath autoload.php) \\
            $(rootpath conformance_php.php)
    """,
    visibility = ["//php:__subpackages__"],
    deps = [
        ":conformance_php_proto",
        "//:test_messages_proto3_php_proto",
        "//editions:test_messages_proto3_editions_php_proto",
        "//php:source_files",
    ],
)

inline_sh_binary(
    name = "conformance_php_c",
    testonly = 1,
    srcs = [
        "conformance_php.php",
        "//php:extension",
    ],
    cmd = """
        php -dextension=$(rootpath //php:extension) \\
            -d include_path=conformance:src/google/protobuf:editions/golden \\
            $(rootpath conformance_php.php)
    """,
    visibility = ["//php:__subpackages__"],
    deps = [
        ":conformance_php_proto",
        "//:test_messages_proto3_php_proto",
        "//editions:test_messages_proto3_editions_php_proto",
    ],
)

inline_sh_binary(
    name = "conformance_csharp",
    testonly = 1,
    srcs = ["//csharp/src/Google.Protobuf.Conformance:conformance_dll"],
    cmd = "dotnet $(rootpath //csharp/src/Google.Protobuf.Conformance:conformance_dll)",
    visibility = ["//csharp:__subpackages__"],
    deps = [
        "//conformance/test_protos:test_messages_edition2023_csharp_proto",
        "//csharp/src/Google.Protobuf.Conformance:conformance_runfiles",
    ],
)

objc_library(
    name = "conformance_objc_lib",
    testonly = 1,
    non_arc_srcs = ["conformance_objc.m"],
    # See https://github.com/bazelbuild/bazel/issues/12897.
    tags = ["manual"],
    deps = [
        ":conformance_objc_proto",
        "//:test_messages_proto2_objc_proto",
        "//:test_messages_proto3_objc_proto",
        "//conformance/test_protos:test_messages_edition2023_objc_proto",
        "//editions:test_messages_proto2_editions_objc_proto",
        "//editions:test_messages_proto3_editions_objc_proto",
    ],
)

cc_binary(
    name = "conformance_objc",
    testonly = 1,
    # See https://github.com/bazelbuild/bazel/issues/12897.
    tags = ["manual"],
    visibility = ["//objectivec:__subpackages__"],
    deps = [":conformance_objc_lib"],
)

rb_binary(
    name = "conformance_ruby",
    testonly = True,
    srcs = ["conformance_ruby.rb"],
    main = "conformance_ruby.rb",
    visibility = ["//ruby:__subpackages__"],
    deps = [
        ":conformance_ruby_proto",
        "//conformance/test_protos:test_messages_edition2023_ruby_proto",
        "//ruby:conformance_editions_test_ruby_proto",
        "//ruby:conformance_test_ruby_proto",
        "//ruby:protobuf",
    ],
)

################################################################################
# Distribution files
################################################################################

filegroup(
    name = "all_files",
    srcs = glob(["**/*"]),
    visibility = ["//src/google/protobuf/compiler/csharp:__pkg__"],
)
