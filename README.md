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

echo "✅ Всё готово, VS Code стоит. Можешь проверять запуск." 
```

Админ
``` bash
sudo deluser student sudo
echo "🔒 Права отозваны. Установка завершена."

```
