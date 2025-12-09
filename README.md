mkdir AppDir
cp ../xTool-Studio-x64-1.1.10.deb ./
dpkg-deb -x xTool-Studio-x64-1.1.10.deb AppDir/
cat > AppDir/AppRun << 'EOF'
#!/bin/bash
SELF=$(readlink -f "$0")
HERE=${SELF%/*}
export LD_LIBRARY_PATH="${HERE}/usr/lib/xtool-studio:${LD_LIBRARY_PATH}"
cd "${HERE}/usr/lib/xtool-studio"
exec "${HERE}/usr/bin/xtool-studio" "$@"
EOF

chmod +x AppDir/AppRun
cat > AppDir/xtool-studio.desktop << 'EOF'
[Desktop Entry]
Name=xTool Studio
Exec=xtool-studio
Icon=xtool-studio
Type=Application
Categories=Graphics;
EOF

cp AppDir/usr/share/pixmaps/xtool-studio.png AppDir/ || cp AppDir/usr/share/icons/hicolor/256x256/apps/xtool-studio.png AppDir/ 2>/dev/null
ARCH=x86_64 ./appimagetool-x86_64.AppImage AppDir

./xTool_Studio-x86_64.AppImage
 
