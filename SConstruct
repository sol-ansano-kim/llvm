import os
import sys
import excons


env = excons.MakeBaseEnv()

out_incdir = excons.OutputBaseDirectory() + "/include"
out_libdir = excons.OutputBaseDirectory() + "/lib"

debug = (excons.GetArgument("debug", 0, int) != 0)

exc = (excons.GetArgument("llvm-enable-eh", 0, int) != 0)
if exc:
  rtti = True
else:
  rtti = (excons.GetArgument("llvm-enable-rtti", 0, int) != 0)

targets = excons.GetArgument("llvm-targets", "X86")

cmake_opts = {"LLVM_DEPENDENCY_DEBUGGING": 0,
              "LLVM_BUILD_GLOBAL_ISEL": 0,
              "LLVM_INSTALL_UTILS": 0,
              "LLVM_INSTALL_TOOLCHAIN_ONLY": 0,
              "LLVM_USE_FOLDERS": 1,
              "LLVM_APPEND_VC_REV": 0,
              "BUILD_SHARED_LIBS": 0,
              "LLVM_ENABLE_BACKTRACES": 1,
              "LLVM_ENABLE_CRASH_OVERRIDES": 1,
              "LLVM_ENABLE_FFI": 0,
              "LLVM_ENABLE_TERMINFO": 1,
              "LLVM_ENABLE_LIBEDIT": 1,
              "LLVM_ENABLE_THREADS": 1,
              "LLVM_ENABLE_ZLIB": 0,
              "LLVM_ENABLE_PIC": 1,
              "LLVM_ENABLE_RTTI": (1 if rtti else 0),
              "LLVM_ENABLE_EH": (1 if exc else 0),
              "LLVM_ENABLE_WARNINGS": 1,
              "LLVM_ENABLE_MODULES": 0,
              "LLVM_ENABLE_MODULE_DEBUGGING": (1 if sys.platform == "darwin" else 0),
              "LLVM_ENABLE_LOCAL_SUBMODULE_VISIBILITY": (0 if sys.platform == "darwin" else 1),
              "LLVM_ENABLE_CXX1Y": 0,
              "LLVM_ENABLE_LIBCXX": 0,
              "LLVM_ENABLE_LLD": 0,
              "LLVM_ENABLE_PEDANTIC": 1,
              "LLVM_ENABLE_WERROR": 0,
              "LLVM_ENABLE_ASSERTIONS": 0,
              "LLVM_ENABLE_EXPENSIVE_CHECKS": 0,
              "LLVM_FORCE_USE_OLD_HOST_TOOLCHAIN": 0,
              "LLVM_USE_INTEL_JITEVENTS": 0,
              "LLVM_USE_OPROFILE": 0,
              "LLVM_EXTERNALIZE_DEBUGINFO": 0,
              "LLVM_USE_SPLIT_DWARF": 0,
              "LLVM_POLLY_BUILD": 0,
              "LLVM_POLLY_LINK_INTO_TOOLS": 0,
              "LLVM_INCLUDE_TOOLS": 1,
              "LLVM_BUILD_TOOLS": 1,
              "LLVM_INCLUDE_UTILS": 0,
              "LLVM_BUILD_UTILS": 0,
              "LLVM_BUILD_RUNTIME": 1,
              "LLVM_INCLUDE_EXAMPLES": 0,
              "LLVM_BUILD_EXAMPLES": 0,
              "LLVM_INCLUDE_TESTS": 0,
              "LLVM_BUILD_TESTS": 0,
              "LLVM_INCLUDE_DOCS": 0,
              "LLVM_INCLUDE_GO_TESTS": 0,
              "LLVM_BUILD_DOCS": 0,
              "LLVM_ENABLE_DOXYGEN": 0,
              "LLVM_ENABLE_SPHINX": 0,
              "LLVM_ENABLE_OCAMLDOC": 0,
              "LLVM_BUILD_EXTERNAL_COMPILER_RT": 0,
              "LLVM_LINK_LLVM_DYLIB": 0,
              "LLVM_BUILD_LLVM_C_DYLIB": 0,
              "LLVM_BUILD_LLVM_DYLIB": 0,
              "LLVM_OPTIMIZED_TABLEGEN": 0,
              "LLVM_ADD_NATIVE_VISUALIZERS_TO_SOLUTION": 0,
              "LLVM_TARGETS_TO_BUILD": targets,
              # Does that changes anything?
              "CMAKE_BUILD_TYPE": ("Debug" if debug else "Release"),
              "LLVM_LIBDIR_SUFFIX": ""}

prjs = [
   {  "name": "llvm",
      "type": "cmake",
      "cmake-opts": cmake_opts,
      "cmake-cfgs": excons.CollectFiles(["include", "lib"], patterns=["CMakeLists.txt"], recursive=True),
      "cmake-srcs": excons.CollectFiles(["lib"], patterns=["*.cpp"], recursive=True)
   }
]

excons.AddHelpOptions(llvm="""LLVM OPTIONS
  llvm-targets=<str>    : ; separated list of targets to build or "all". ["X86"]
  llvm-enable-rtti=0|1  : Enable C++ Runtime Type Information.           [0]
  llvm-enable-eh=0|1    : Enable C++ Exceptions (implies RTTI).          [0]""")

excons.DeclareTargets(env, prjs)

