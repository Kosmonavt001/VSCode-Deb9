# Solll

``` bash
#!/bin/bash
# === Установка портативного VS Code на Astra Linux CE 2.12 ===

set -e

echo ">>> Удаляем старые/битые установки VS Code..."
sudo apt --fix-broken install -y >/dev/null 2>&1 || true
sudo apt remove --purge code code-oss -y >/dev/null 2>&1 || true
sudo apt autoremove -y >/dev/null 2>&1 || true

echo ">>> Скачиваем VS Code 1.61.2 (совместимую версию)..."
cd /tmp
wget -q https://update.code.visualstudio.com/1.61.2/linux-x64/stable -O vscode-1.61.2.tar.gz

echo ">>> Распаковываем в /opt..."
sudo tar -xzf vscode-1.61.2.tar.gz -C /opt

echo ">>> Создаём ярлык /usr/local/bin/code..."
sudo ln -sf /opt/VSCode-linux-x64/code /usr/local/bin/code

echo ">>> Добавляем иконку в меню..."
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

echo ">>> Установка завершена!"
echo "Запускать можно командой: code"
echo "Или через меню -> Программы -> Разработка -> Visual Studio Code"
```
