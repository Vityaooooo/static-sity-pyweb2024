# Создание и развертывание статического сайта

Для выполнения данного задания были придприняты следующие шаги

1. Установка ```hugo```
    установка дистрибутива hugo_extended_0.139.2_windows-amd64
2. Создание сайта 
    ```hugo new site pyweb```
3. Добавление тема 
    - переходим в директория сайта 
    ```cd pyweb```
    - инициализируем git
    ```git init```
    - добавляем тему (например, ananke)
    ```git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke```
4. Редактируем hugo.toml (добавляем название темы и имя удаленного репозитория)
    ```
    baseURL = 'https://vityaooooo.github.io/static-sity-pyweb2024/' 
    languageCode = 'ru-ru'
    title = 'Проектирование и развертывание веб-решений в эко-системе Python'
    theme = 'ananke'
    ```
5. Создаем посты (контент) для нашего сайта
    - в папке content создаем папку posts
    ```mkdir posts```
    - добавляем файл md с постом
    - добавляем в начале файла информацию для Hugo для публикации
    ```
    ---
    title: "Пианино на JavaScript"
    date: 2024-11-28T14:00:00+02:00
    draft: false
    ---
    ```
6. Проверяем что все работает
    - запускаем команду 
    ```hugo server```
    - переходим по локальному адресу и проверяем что посты отображаются
7. Создаем репозиторий на GitHub
8. Добавляем ветку gh-pages
9. Добавляем secrets в репозитории
    - переходим в настройки -> secrets and variables -> Actions -> new repository secrets
    - создаем secrets (например, HUGO-DEPLOY) c Personal access tokens
10. Создаем yml файл для автоматизации деплоя 
```
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main
    pull_request:
jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: "latest"
        extended: true
    
    - name: Build the site
      run: hugo --minify
    
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      if: github.ref == 'refs/heads/main'
      with:
        github_token: ${{ secrets.HUGO_DEPLOY }}
        publish_dir: ./public
```
11. Делаем push в удаленный репозиторий
12. При успешном выполнении всех шагов по адресу https://<username>.github.io/<repository-name>/ будет доступен наш статический сайт
