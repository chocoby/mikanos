# See here for image contents: https://github.com/sarisia/mikanos-docker
ARG VARIANT=latest
FROM ghcr.io/sarisia/mikanos:${VARIANT}

# Cross-compile fix for running natively on Apple Silicon (arm64).
# Required to build the x64 UEFI bootloader (MikanLoaderPkg / EDK II).
# Patch the template; edksetup.sh copies it to Conf/tools_def.txt on first run.
# See: https://github.com/sarisia/mikanos-docker (tools_def.txt.diff)
RUN sed -i \
      -e '/DEFINE CLANG38_X64_TARGET/ s/x86_64-pc-linux-gnu/x86_64-linux-gnu/' \
      -e '/^DEBUG_CLANG38_X64_CC_FLAGS/ s|$| -I/usr/x86_64-linux-gnu/include|' \
      /home/vscode/edk2/BaseTools/Conf/tools_def.template

# Make git's bundled diff-highlight available on PATH (some ~/.gitconfig
# pagers reference it; the image ships it non-executable and outside PATH).
RUN install -m 0755 /usr/share/doc/git/contrib/diff-highlight/diff-highlight \
      /usr/local/bin/diff-highlight

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-here>
