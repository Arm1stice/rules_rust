"""
cargo-raze crate build file.

DO NOT EDIT! Replaced on runs of cargo-raze
"""
package(default_visibility = [
  # Public for visibility by "@raze__crate__version//" targets.
  #
  # Prefer access through "//proto/raze", which limits external
  # visibility to explicit Cargo.toml dependencies.
  "//visibility:public",
])

licenses([
  "notice", # "MIT"
])

load(
    "@io_bazel_rules_rust//rust:rust.bzl",
    "rust_library",
    "rust_binary",
    "rust_test",
)

rust_binary(
    name = "protobuf_build_script",
    srcs = glob(["**/*.rs"]),
    crate_root = "build.rs",
    edition = "2015",
    deps = [
    ],
    rustc_flags = [
        "--cap-lints=allow",
    ],
    crate_features = [
      "bytes",
      "with-bytes",
    ],
    data = glob(["*"]),
    version = "2.8.2",
    visibility = ["//visibility:private"],
)

genrule(
    name = "protobuf_build_script_executor",
    srcs = glob(["*", "**/*.rs"]),
    outs = ["protobuf_out_dir_outputs.tar.gz"],
    tools = [
      ":protobuf_build_script",
    ],
    tags = ["no-sandbox"],
    cmd = "mkdir -p $$(dirname $@)/protobuf_out_dir_outputs/;"
        + " (export CARGO_MANIFEST_DIR=\"$$PWD/$$(dirname $(location :Cargo.toml))\";"
        # TODO(acmcarther): This needs to be revisited as part of the cross compilation story.
        #                   See also: https://github.com/google/cargo-raze/pull/54
        + " export TARGET='x86_64-unknown-linux-gnu';"
        + " export RUST_BACKTRACE=1;"
        + " export RUSTC=echo;"
        + " export CARGO_PKG_VERSION=0;"
        + " export CARGO_FEATURE_BYTES=1;"
        + " export CARGO_FEATURE_WITH_BYTES=1;"
        + " export OUT_DIR=$$PWD/$$(dirname $@)/protobuf_out_dir_outputs;"
        + " export BINARY_PATH=\"$$PWD/$(location :protobuf_build_script)\";"
        + " export OUT_TAR=$$PWD/$@;"
        + " cd $$(dirname $(location :Cargo.toml)) && $$BINARY_PATH && tar -czf $$OUT_TAR -C $$OUT_DIR .)"
)

# Unsupported target "coded_input_stream" with type "bench" omitted
# Unsupported target "coded_output_stream" with type "bench" omitted

rust_library(
    name = "protobuf",
    crate_root = "src/lib.rs",
    crate_type = "lib",
    edition = "2015",
    srcs = glob(["**/*.rs"]),
    deps = [
        "@raze__bytes__0_4_12//:bytes",
    ],
    rustc_flags = [
        "--cap-lints=allow",
    ],
    out_dir_tar = ":protobuf_build_script_executor",
    version = "2.8.2",
    crate_features = [
        "bytes",
        "with-bytes",
    ],
)

