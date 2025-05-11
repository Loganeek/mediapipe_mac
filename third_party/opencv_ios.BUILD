# # Description:
# #   OpenCV libraries for video/image processing on iOS

# load(
#     "@build_bazel_rules_apple//apple:apple.bzl",
#     "apple_static_framework_import",
# )

# licenses(["notice"])  # BSD license

# exports_files(["LICENSE"])

# apple_static_framework_import(
#     name = "OpencvFramework",
#     framework_imports = glob(["opencv2.framework/**"]),
#     visibility = ["//visibility:public"],
# )

# objc_library(
#     name = "opencv_objc_lib",
#     deps = [":OpencvFramework"],
# )

# cc_library(
#     name = "opencv",
#     hdrs = glob([
#         "opencv2.framework/Versions/A/Headers/**/*.h*",
#     ]),
#     copts = [
#         "-std=c++11",
#         "-x objective-c++",
#     ],
#     include_prefix = "opencv2",
#     linkopts = [
#         "-framework AssetsLibrary",
#         "-framework CoreFoundation",
#         "-framework CoreGraphics",
#         "-framework CoreMedia",
#         "-framework Accelerate",
#         "-framework CoreImage",
#         "-framework AVFoundation",
#         "-framework CoreVideo",
#         "-framework QuartzCore",
#     ],
#     strip_include_prefix = "opencv2.framework/Versions/A/Headers",
#     visibility = ["//visibility:public"],
#     deps = [":opencv_objc_lib"],
# )

# third_party/opencv_ios.BUILD

# OpenCV iOS 框架的 Bazel 规则修正版
load("@build_bazel_rules_apple//apple:apple.bzl", "apple_static_framework_import")


apple_static_framework_import(
    name = "OpencvFramework",
    framework_imports = glob([
        "opencv2.framework/**",
        # "opencv2.framework/Versions/A/opencv2"
    ]),
    visibility = ["//visibility:public"],
)

# 创建 .a 符号链接以绕过 Bazel 的扩展名检查
genrule(
    name = "opencv_symlink",
    srcs = ["opencv2.framework/Versions/A/opencv2"],
    outs = ["libopencv.a"],
    cmd = "ln -sf $(location opencv2.framework/Versions/A/opencv2) $@",
)

# 2. 使用 cc_import 包装静态库 (核心修复)
cc_import(
    name = "opencv_arm64",
    static_library = ":libopencv.a",
    alwayslink = True,
)

cc_library(
    name = "opencv",
    hdrs = glob([
        "opencv2.framework/Versions/A/Headers/**/*.h",  # 正确头文件路径
    ]),
    includes = ["opencv2.framework/Versions/A/Headers"],  # 包含路径修正
    linkopts = [
        "-framework Accelerate",
        "-framework CoreImage",
        "-framework AVFoundation",
        "-framework CoreVideo",
        "-force_load $(rootpath :opencv_arm64)",
    ],
    # features = ["fully_static_link"],
    deps = [
        ":OpencvFramework",
        ":opencv_arm64",  # 显式依赖
    ],
    linkstatic = 1,
    visibility = ["//visibility:public"],
)

# 模块化链接补充（根据实际需要添加）
cc_library(
    name = "opencv_imgproc",
    deps = [":opencv"],
    visibility = ["//visibility:public"],
)