Админ
``` bash
# Даем студенту право использовать sudo
sudo usermod -aG sudo student
echo "✅ Права выданы. Переходи на аккаунт студента."
```

Студента:
``` bash
# 1. Качаем и распаковываем
wget -q https://update.code.visualstudio.com/1.61.2/linux-x64/stable -O vscode.tar.gz
sudo tar -xzf vscode.tar.gz -C /opt
sudo ln -sf /opt/VSCode-linux-x64/code /usr/local/bin/code

# 2. Создаем ярлык на рабочий стол (Desktop или Рабочий стол)
mkdir -p ~/Desktop
cat <<EOF > ~/Desktop/vscode.desktop
[Desktop Entry]
Name=Visual Studio Code
Exec=code --no-sandbox %F
Icon=/opt/VSCode-linux-x64/resources/app/resources/linux/code.png
Type=Application
Terminal=false
EOF
chmod +x ~/Desktop/vscode.desktop

# 3. Создаем папки конфигов (теперь они сразу будут с правами студента)
mkdir -p ~/.config/Code
mkdir -p ~/.vscode/extensions
# 1. Исправляем права на основные папки
sudo chown -R student:student \~/.config/Code
sudo chown -R student:student \~/.vscode

# 2. Создаём все нужные подпапки вручную
mkdir -p \~/.config/Code/{Backups,User,logs,CachedData}
mkdir -p \~/.vscode/extensions

# 3. Даём полные права
chmod -R 755 \~/.config/Code
chmod -R 755 \~/.vscode
# Смотрим, кто владеет папками
ls -ld \~/.config/Code \~/.config \~/.vscode

# Исправляем владельца
sudo chown -R student:student \~/.config/Code
sudo chown -R student:student \~/.vscode

# Создаём все нужные папки
mkdir -p \~/.config/Code/{Backups,User,logs,CachedData,CrashReports}
mkdir -p \~/.vscode/extensions

# Даём правильные права
chmod -R u+rwX \~/.config/Code
chmod -R u+rwX \~/.vscode

echo "✅ Права исправлены. Теперь попробуй запустить VS Code."
echo "✅ Права исправлены"
echo "✅ Всё готово, VS Code стоит. Можешь проверять запуск." 
```

Админ
``` bash
sudo deluser student sudo
echo "🔒 Права отозваны. Установка завершена."

```
