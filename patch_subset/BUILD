load("@rules_cc//cc:defs.bzl", "cc_binary")
load("@rules_cc//cc:defs.bzl", "cc_library")
load("@rules_cc//cc:defs.bzl", "cc_proto_library")
load("@rules_proto//proto:defs.bzl", "proto_library")

proto_library(
    name = "patch_subset_proto",
    srcs = ["patch_subset.proto"],
)

cc_proto_library(
    name = "patch_subset_cc_proto",
    deps = [":patch_subset_proto"],
)

cc_library(
    name = "server",
    srcs = [
        "brotli_binary_diff.cc",
        "file_font_provider.cc",
        "harfbuzz_subsetter.cc",
        "patch_subset_server_impl.cc",
    ],
    hdrs = [
        "binary_diff.h",
        "brotli_binary_diff.h",
        "file_font_provider.h",
        "font_provider.h",
        "harfbuzz_subsetter.h",
        "patch_subset_server_impl.h",
        "subsetter.h",
    ],
    copts = [
        "-std=c++17",
    ],
    deps = [
        ":common",
        ":patch_subset_cc_proto",
        "//common",
        "@brotli//:brotlienc",
        "@com_google_absl//absl/strings",
        "@harfbuzz",
    ],
)

cc_library(
    name = "client",
    srcs = [
        "patch_subset_client.cc",
    ],
    hdrs = [
        "binary_patch.h",
        "null_request_logger.h",
        "patch_subset_client.h",
        "request_logger.h",
    ],
    copts = [
        "-std=c++17",
    ],
    deps = [
        ":common",
        ":patch_subset_cc_proto",
        "//common",
        "@com_google_absl//absl/strings",
        "@harfbuzz",
    ],
)

cc_library(
    name = "common",
    srcs = [
        "compressed_set.cc",
        "farm_hasher.cc",
        "hb_set_unique_ptr.cc",
        "sparse_bit_set.cc",
    ],
    hdrs = [
        "compressed_set.h",
        "farm_hasher.h",
        "font_data.h",
        "hasher.h",
        "hb_set_unique_ptr.h",
        "patch_subset_server.h",
        "sparse_bit_set.h",
    ],
    copts = [
        "-std=c++17",
    ],
    deps = [
        ":patch_subset_cc_proto",
        "//common",
        "@com_google_absl//absl/strings",
        "@farmhash",
        "@harfbuzz",
    ],
)

cc_test(
    name = "tests",
    size = "small",
    srcs = [
        "compressed_set_test.cc",
        "fake_subsetter.h",
        "file_font_provider_test.cc",
        "harfbuzz_subsetter_test.cc",
        "mock_binary_diff.h",
        "mock_binary_patch.h",
        "mock_font_provider.h",
        "mock_hasher.h",
        "mock_patch_subset_server.h",
        "patch_subset_client_test.cc",
        "patch_subset_server_impl_test.cc",
        "sparse_bit_set_test.cc",
    ],
    copts = [
        "-std=c++17",
    ],
    data = [
        ":testdata",
    ],
    deps = [
        ":client",
        ":common",
        ":patch_subset_cc_proto",
        ":server",
        "//common",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
        "@farmhash",
        "@gtest//:gtest_main",
        "@harfbuzz",
    ],
)

filegroup(
    name = "testdata",
    srcs = glob(["testdata/*"]),
)