# Настройка CI GitHubActions для gradle java.

1. Создайте новый репозиторий на GitHub и загрузите туда свой проект.
2. Перейдите на GitHub и создайте новый Actions 
![](/images/ActionsButton.png)
3. В поиске впишите `gradle` и выберите `java with gradle`
![](/images/ActionsYmlFileCreate.png)
4. Вставьте в файл `gradle.yml` следующий код
```
name: Java CI with Gradle

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

jobs:
  build:

    runs-on: ubuntu-latest   # образ для сборки

    steps:
    - uses: actions/checkout@v3
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
    - name: Build with Gradle
      uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
      with:
        java-version: '11'
        distribution: 'adopt'
    - name: Run SUT
      run:  java -jar ./artifacts/app-mbank.jar & sleep 10
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew
    - name: Build witch Gradle
      run: ./gradlew test --info
```

Часть `-jar app-mbank.jar` может меняться, но первая часть для вас во всех ДЗ при запуске на вашем ПК будет именно такой.

Для запуска тестов в  режиме headless используйте: `-Dselenide.headless=true`

5. Сохраните изменения.

![](images/startCommit.png)

6. После сборки вашего проекта отобразиться зеленая птичка. 

![](images/check.png)


7. Нажмите на зеленую птичку и выберите "Details"

![](images/details.png)

8. Скопируйте в буфер обмена ссылку на бедж

![](images/new.png)

![](images/saveBageLink.png)

9. Вернитесь в корень репозитория и создайте `README.md` 

![](images/addreadme.png)

10. Вставьте в README.md ссылку, скооперированную на этапе 8 и сохраните изменения. 
После сохранения изменений отображается бедж со статусом сборки 

![](images/bage.png)

11. После сохранения заберите изменения в локальный репозиторий командной  `git pull`
