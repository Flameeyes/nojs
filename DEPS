# we need:

# - buildtools
# - build
# - v8
# - build_overrides ?
# - tools?
#   - tools/clang
#   - tools/gyp

# - third_party
# -   third_party/icu
# -   third_party/libc++-static
# -   third_party/libuv
# -   third_party/llvm-build
# -   third_party/webkit/source/core/streams
#     (i.e.: just streams please.)

vars = {
  # Use this googlecode_url variable only if there is an internal mirror for it.
  # If you do not know, use the full path while defining your new deps entry.
  'googlecode_url': 'http://%s.googlecode.com/svn',
  'chromium_git': 'https://chromium.googlesource.com',
  'github_git': 'https://github.com',
  'v8_revision': 'dd6ba785e1081ab3c0e0cdcbd312e2c5a1c101dc',
  'libuv_revision': '9189d241c1fa3c74ad940f06ab9afeb2a45eefea',
  'uv_link_t_revision': '0b9f0ce8d3a44f231a5a77e29ab3a0bed8de6053',
}

# Only these hosts are allowed for dependencies in this DEPS file.
allowed_hosts = [
  'chromium.googlesource.com',
  'boringssl.googlesource.com',
  'pdfium.googlesource.com',
  'android.googlesource.com',
  'github.com',
]

deps = {
  'nojs/third_party/icu':
   Var('chromium_git') + '/chromium/deps/icu.git' + '@' + 'e466f6ac8f60bb9697af4a91c6911c6fc4aec95f',

  'nojs/tools/gyp':
    Var('chromium_git') + '/external/gyp.git' + '@' + 'ed163ce233f76a950dce1751ac851dbe4b1c00cc',

  'nojs/third_party/v8':
    Var('chromium_git') + '/v8/v8.git' + '@' +  Var('v8_revision'),

  'nojs/third_party/scons-2.0.1':
   Var('chromium_git') + '/native_client/src/third_party/scons-2.0.1.git' + '@' + '1c1550e17fc26355d08627fbdec13d8291227067',

  'nojs/third_party/libuv':
   Var('github_git') + '/chrisdickinson/libuv.git' + '@' + Var('libuv_revision'),

  'nojs/third_party/uv_link_t':
   Var('github_git') + '/chrisdickinson/uv_link_t.git' + '@' + Var('uv_link_t_revision'),

  # why, this sure seems noninvasive
  'nojs/third_party/v8/base/trace_event/common':
    Var('chromium_git') + '/chromium/src/base/trace_event/common.git' + '@' + 'c8c8665c2deaf1cc749d9f8e153256d4f67bf1b8',

}

deps_os = {
  'win': {
    'nojs/third_party/cygwin':
     Var('chromium_git') + '/chromium/deps/cygwin.git' + '@' + 'c89e446b273697fadf3a10ff1007a97c0b7de6df',

    'nojs/third_party/psyco_win32':
     Var('chromium_git') + '/chromium/deps/psyco_win32.git' + '@' + 'f5af9f6910ee5a8075bbaeed0591469f1661d868',

    'nojs/third_party/bison':
     Var('chromium_git') + '/chromium/deps/bison.git' + '@' + '083c9a45e4affdd5464ee2b224c2df649c6e26c3',

    'nojs/third_party/gperf':
     Var('chromium_git') + '/chromium/deps/gperf.git' + '@' + 'd892d79f64f9449770443fb06da49b5a1e5d33c1',

    'nojs/third_party/perl':
     Var('chromium_git') + '/chromium/deps/perl.git' + '@' + 'ac0d98b5cee6c024b0cffeb4f8f45b6fc5ccdb78',

    # Parses Windows PE/COFF executable format.
    'nojs/third_party/pefile':
     Var('chromium_git') + '/external/pefile.git' + '@' + '72c6ae42396cb913bcab63c15585dc3b5c3f92f1',

    # GNU binutils assembler for x86-32.
    'nojs/third_party/gnu_binutils':
      Var('chromium_git') + '/native_client/deps/third_party/gnu_binutils.git' + '@' + 'f4003433b61b25666565690caf3d7a7a1a4ec436',
    # GNU binutils assembler for x86-64.
    'nojs/third_party/mingw-w64/mingw/bin':
      Var('chromium_git') + '/native_client/deps/third_party/mingw-w64/mingw/bin.git' + '@' + '3cc8b140b883a9fe4986d12cfd46c16a093d3527',
  },
  'ios': {
    # Code that's not needed due to not building everything
    'nojs/chrome/test/data/perf/canvas_bench': None,
    'nojs/chrome/test/data/perf/frame_rate/content': None,
    'nojs/native_client': None,
    'nojs/third_party/ffmpeg': None,
    'nojs/third_party/hunspell_dictionaries': None,
    'nojs/third_party/webgl': None,
  },
  'mac': {
  },
  'unix': {
  },
}

include_rules = [
  # Everybody can use some things.
  # NOTE: THIS HAS TO STAY IN SYNC WITH third_party/DEPS which disallows these.
  '+base',
  '+build',
  '+ipc',

  # Everybody can use headers generated by tools/generate_library_loader.
  '+library_loaders',

  '+testing',
  '+third_party/icu/source/common/unicode',
  '+third_party/icu/source/i18n/unicode',
  '+url',
]


# checkdeps.py shouldn't check include paths for files in these dirs:
skip_child_includes = [
  'out',
  'v8',
  'win8',
]

hooks = [
  {
    # Update the Windows toolchain if necessary.
    'name': 'win_toolchain',
    'pattern': '.',
    'action': ['python', 'nojs/build/vs_toolchain.py', 'update'],
  },
  {
    # Pull clang if needed or requested via GYP_DEFINES.
    # Note: On Win, this should run after win_toolchain, as it may use it.
    'name': 'clang',
    'pattern': '.',
    'action': ['python', 'nojs/tools/clang/scripts/update.py', '--if-needed'],
  },
  {
    'name': 'gn',
    'pattern': '.',
    'action': ['npm', 'i', 'gn'],
  },
  {
    # Ensure that while generating dependencies lists in .gyp files we don't
    # accidentally reference any .pyc files whose corresponding .py files have
    # already been deleted.
    # We should actually try to avoid generating .pyc files, crbug.com/500078.
    'name': 'remove_stale_pyc_files',
    'pattern': '.',
    'action': [
        'python',
        'nojs/tools/remove_stale_pyc_files.py',
        'nojs/android_webview/tools',
        'nojs/gpu/gles2_conform_support',
        'nojs/infra',
        'nojs/ppapi',
        'nojs/printing',
        'nojs/third_party/closure_compiler/build',
        'nojs/tools',
    ],
  },
]
