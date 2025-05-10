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

# 新增模块化链接配置
cc_library(
    name = "opencv_core",
    srcs = ["opencv2.framework/opencv2"],  # 静态库路径
    hdrs = glob(["opencv2.framework/Headers/**/*.h"]),
    includes = ["opencv2.framework/Headers"],
    linkopts = [
        "-framework Accelerate",
        "-force_load $(location opencv2.framework/opencv2)",
    ],
    visibility = ["//visibility:public"],
)

cc_library(
    name = "opencv_imgproc",
    srcs = ["opencv2.framework/opencv2"],
    deps = [":opencv_core"],  # 显式声明依赖关系
    linkopts = [
        "-framework CoreImage",
        "-force_load $(location opencv2.framework/opencv2)",
    ],
    visibility = ["//visibility:public"],
)