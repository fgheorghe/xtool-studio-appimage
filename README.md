Step by step instructions on how to create an AppImage for xTool Studio using Ubuntu.

AppImages allow users of any distro to use the app. I have tested it on Fedora 43 Kinoite and the only error I am getting is when I click Background Refresh (most likely a lib I am failing to install, because it works using the .deb package):
11:12:15.458 (main) › xTool Studio Algorithm Worker call error: xcm api call failed (P2GlobalCorrectImageSync): worker has exit！ related methods not completed
11:12:15.458 › Error occurred in handler for 'xcm': worker has exit！ related methods not completed


Requires the following Ubuntu packages:

sudo apt install fuse libfuse2 appstream

1. Download appimagetool:

wget "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage"

2. Prepare app image creation directory:

mkdir AppDir

cp ../xTool-Studio-x64-1.1.10.deb ./

3. Extract the app's debian package:

dpkg-deb -x xTool-Studio-x64-1.1.10.deb AppDir/

4. Create AppRun launcher:

```
cat > AppDir/AppRun << 'EOF'
#!/bin/bash
SELF=$(readlink -f "$0")
HERE=${SELF%/*}
export LD_LIBRARY_PATH="${HERE}/usr/lib/xtool-studio:${LD_LIBRARY_PATH}"
cd "${HERE}/usr/lib/xtool-studio"
exec "${HERE}/usr/bin/xtool-studio" "$@"
EOF
```

5. Make it executable:

chmod +x AppDir/AppRun

6. Create app shortcut:

```cat > AppDir/xtool-studio.desktop << 'EOF'
[Desktop Entry]
Name=xTool Studio
Exec=xtool-studio
Icon=xtool-studio
Type=Application
Categories=Graphics;
EOF
```

7. Copy app icon:

cp AppDir/usr/share/pixmaps/xtool-studio.png AppDir/ || cp AppDir/usr/share/icons/hicolor/256x256/apps/xtool-studio.png AppDir/ 2>/dev/null

8. Build AppImage:

ARCH=x86_64 ./appimagetool-x86_64.AppImage AppDir

9. Excute and enjoy:

./xTool_Studio-x86_64.AppImage
 
