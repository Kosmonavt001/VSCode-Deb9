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
#!/bin/bash
# === Установка и передача Visual Studio Code (portable) от admin1 -> student ===
# Подходит для Astra Linux CE 2.12 (Debian 9 / Orel)
# Запускать от имени root или sudo

set -e

ADMIN_USER="admin1"
STUDENT_USER="student"

echo ">>> Проверяем наличие пользователей..."
id $ADMIN_USER >/dev/null 2>&1 || { echo "❌ Пользователь $ADMIN_USER не найден"; exit 1; }
id $STUDENT_USER >/dev/null 2>&1 || { echo "❌ Пользователь $STUDENT_USER не найден"; exit 1; }

echo ">>> Удаляем старые и битые установки VS Code..."
sudo apt --fix-broken install -y >/dev/null 2>&1 || true
sudo apt remove --purge code code-oss -y >/dev/null 2>&1 || true
sudo apt autoremove -y >/dev/null 2>&1 || true

echo ">>> Скачиваем совместимую версию VS Code (1.61.2)..."
cd /tmp
wget -q https://update.code.visualstudio.com/1.61.2/linux-x64/stable -O vscode-1.61.2.tar.gz

echo ">>> Распаковываем в /opt..."
sudo tar -xzf vscode-1.61.2.tar.gz -C /opt
sudo chown -R root:root /opt/VSCode-linux-x64

echo ">>> Создаём глобальную ссылку для всех пользователей..."
sudo ln -sf /opt/VSCode-linux-x64/code /usr/local/bin/code

echo ">>> Создаём ярлык для всех пользователей..."
sudo tee /usr/share/applications/code-portable.desktop > /dev/null <<'EOF'
[Desktop Entry]
Name=Visual Studio Code (portable)
Comment=Visual Studio Code (portable)
Exec=/usr/local/bin/code %F
Icon=/opt/VSCode-linux-x64/resources/app/resources/linux/code.png
Terminal=false
Type=Application
Categories=Development;IDE;
EOF

echo ">>> Копируем настройки и расширения с $ADMIN_USER на $STUDENT_USER..."
sudo -u $STUDENT_USER mkdir -p /home/$STUDENT_USER/.config
sudo -u $STUDENT_USER mkdir -p /home/$STUDENT_USER/.vscode

if [ -d /home/$ADMIN_USER/.config/Code ]; then
    sudo cp -r /home/$ADMIN_USER/.config/Code /home/$STUDENT_USER/.config/
fi

if [ -d /home/$ADMIN_USER/.vscode ]; then
    sudo cp -r /home/$ADMIN_USER/.vscode /home/$STUDENT_USER/
fi

sudo chown -R $STUDENT_USER:$STUDENT_USER /home/$STUDENT_USER/.config/Code /home/$STUDENT_USER/.vscode

echo ">>> Проверяем доступ для запуска..."
sudo -u $STUDENT_USER bash -c "command -v code >/dev/null 2>&1 && echo '✅ VS Code доступен для $STUDENT_USER' || echo '⚠️ Что-то не так с путём code'"

echo
echo ">>> Готово!"
echo "Теперь пользователь $STUDENT_USER может запустить VS Code:"
echo "    code"
echo "или через меню -> Программы -> Разработка -> Visual Studio Code"

sudo mkdir -p /home/student/.config/Code
sudo mkdir -p /home/student/.vscode/extensions

sudo chown -R student:student /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true

sudo chmod -R u+w /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true

echo "✅ Права исправлены. Теперь попробуй запустить VS Code под пользователем student командой:"
echo "code"

sudo mkdir -p /home/student/.config/Code/Backups
sudo mkdir -p /home/student/.vscode/extensions

sudo chown -R student:student /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true
sudo chmod -R u+rwx /home/student/.config /home/student/.vscode /home/student /run/user/1002 2>/dev/null || true

echo "✅ Всё готово! Теперь войди под пользователем student и запусти:"
echo "code"
```
