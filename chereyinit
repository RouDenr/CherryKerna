#!/bin/bash

# Переменные
script_dir=$(dirname "$0")
script_name=$(basename "$0")
ignore_file="$script_dir/.cherrycoreignore"
files_to_ignore=("$script_name" ".gitignore" "README.md")
bashrc_file="$HOME/.bashrc"
placeholder="# Placeholder for custom script execution"

# Функция для создания файла игнорирования с шаблоном
create_ignore_file() {
    touch "$ignore_file"
    for file in "${files_to_ignore[@]}"; do
        echo "$file" >> "$ignore_file"
    done
}

# Функция для добавления выполнения скрипта в .bashrc
add_to_bashrc() {
    echo "" >> "$bashrc_file"
    echo "$placeholder" >> "$bashrc_file"
    echo "source \"$script_dir/$script_name\"" >> "$bashrc_file"
}

# Функция для проверки исполняемости файлов
check_executability() {
    files=$(ls "$script_dir" | grep -v -f "$ignore_file")
    for file in $files; do
        if [ -x "$script_dir/$file" ]; then
            echo "$file is executable"
        else
            echo "$file is not executable"
        fi
    done
}

# Функция для установки
install() {
    # Получаем путь к папке, где находится установщик
    script_dir=$(dirname "$(readlink -f "$0")")

    # Создаем папку для установки
    install_dir="$script_dir/myapp"
    mkdir -p "$install_dir"

    # Копируем исполняемые файлы в папку установки
    cp -r "$script_dir/bin"/* "$install_dir"

    # Добавляем папку установки в PATH
    echo "export PATH=\"$install_dir:\$PATH\"" >> ~/.bashrc
    source ~/.bashrc

    echo "Установка завершена. Исполняемые файлы доступны по командам."
}

# Функция для удаления
uninstall() {
    # Получаем путь к папке, где находится установщик
    script_dir=$(dirname "$(readlink -f "$0")")

    # Удаляем папку установки
    install_dir="$script_dir/myapp"
    rm -rf "$install_dir"

    # Удаляем путь из PATH
    sed -i "/$install_dir/d" ~/.bashrc
    source ~/.bashrc

    echo "Удаление завершено."
}


# Добавляем выполнение скрипта в .bashrc, если нет заглушки
if ! grep -q "$placeholder" "$bashrc_file"; then
    add_to_bashrc
fi

# Проверяем исполняемость файлов
check_executability
#!/bin/bash

# Проверяем количество аргументов
if [ $# -eq 1 ]; then
    # Выполняем действие в зависимости от переданного аргумента
    case "$1" in
        "install")
            install
            ;;
        "uninstall")
            uninstall
            ;;
        *)
            echo "Недопустимая команда: $1. Используйте 'install' или 'uninstall'."
            exit 1
            ;;
    esac

    echo "Использование: $0 [install | uninstall]"
    exit 0
fi
