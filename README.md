# Solll

### Установка VS CODE на Debain 9
``` bash
wget -q https://update.code.visualstudio.com/1.61.2/linux-x64/stable -O vscode-1.61.2.tar.gz
sudo tar -xzf vscode-1.61.2.tar.gz -C /opt
sudo ln -sf /opt/VSCode-linux-x64/code /usr/local/bin/code
mkdir -p ~/.local/share/applications
cat > ~/.local/share/applications/code-portable.desktop <<'EOF'
[Desktop Entry]
Name=Visual Studio Code (portable)
Comment=Visual Studio Code (portable)
Exec=/usr/local/bin/code %F
Icon=/opt/VSCode-linux-x64/resources/app/resources/linux/code.png
Terminal=false
Type=Application
Categories=Development;IDE;
EOF
chmod +x ~/.local/share/applications/code-portable.desktop
ADMIN_USER="admin1"
STUDENT_USER="student"
sudo -u $STUDENT_USER mkdir -p /home/$STUDENT_USER/.config
sudo -u $STUDENT_USER mkdir -p /home/$STUDENT_USER/.vscode
if [ -d /home/$ADMIN_USER/.config/Code ]; then
    sudo cp -r /home/$ADMIN_USER/.config/Code /home/$STUDENT_USER/.config/
fi
if [ -d /home/$ADMIN_USER/.vscode ]; then
    sudo cp -r /home/$ADMIN_USER/.vscode /home/$STUDENT_USER/
fi
sudo chown -R $STUDENT_USER:$STUDENT_USER /home/$STUDENT_USER/.config/Code /home/$STUDENT_USER/.vscode
sudo -u $STUDENT_USER bash -c "command -v code >/dev/null 2>&1 && echo '✅ VS Code доступен для $STUDENT_USER' || echo '⚠️ Что-то не так с путём code'"
echo
sudo mkdir -p /home/student/.config/Code
sudo mkdir -p /home/student/.vscode/extensions
sudo chown -R student:student /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true
sudo chmod -R u+w /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true
sudo mkdir -p /home/student/.config/Code/Backups
sudo mkdir -p /home/student/.vscode/extensions
sudo chown -R student:student /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true
sudo chmod -R u+rwx /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true
sudo rm -rf /home/student/.config/Code 2>/dev/null
sudo mkdir -p /home/student/.config/Code
sudo mkdir -p /home/student/.vscode/extensions
sudo chown -R student:student /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true
sudo chmod -R 755 /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true
ls -ld /home/student/.config /home/student/.config/Code /home/student/.vscode /run/user/1002
sudo chown -R student:student /home/student/.config
sudo chmod -R 755 /home/student/.config
sudo rm -rf /home/student/.config/Code
sudo mkdir -p /home/student/.config/Code/Backups
sudo mkdir -p /home/student/.vscode/extensions
sudo chown -R student:student /home/student/.config /home/student/.vscode
sudo chmod -R 755 /home/student/.config /home/student/.vscode
ls -ld /home/student/.config /home/student/.config/Code /home/student/.vscode
```
