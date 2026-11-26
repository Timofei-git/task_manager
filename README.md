# task-manager-cicd

Учебный проект, восстановленный из all_code Java.docx.pdf. Java 21, Spring Boot 3.5.16, Surefire/Failsafe 3.6.1, JaCoCo 0.8.15 — версии из методических пособий.

## Локальная проверка

Установить JDK 21, Maven и Git, открыть каталог проекта в IntelliJ IDEA как Maven-проект. В PowerShell из корня:

```powershell
java -version
mvn -version
git --version
mvn -B -ntp clean verify
mvn -B -ntp surefire-report:report-only
```

Ожидается 14 модульных и 11 интеграционных тестов. JAR: `target/task-manager-cicd-1.0.0.jar`. Покрытие: `target/site/jacoco/index.html`. XML: `target/surefire-reports` и `target/failsafe-reports`.

Запуск: `java -jar target/task-manager-cicd-1.0.0.jar`. API: http://localhost:8080/api/tasks.

## CI

Создать собственный GitHub-репозиторий, загрузить содержимое каталога в ветки main и feature/task-priority. Jenkinsfile рассчитан на Pipeline script from SCM и Windows Agent с меткой windows-java21-agent. GitHub Actions запускается при push в main/feature/**, pull_request в main и вручную. Docker-расширение не включено, индивидуальные задания не выполнены.

pom.xml и конфигурации CI восстановлены по методичкам; зависимости и версии сохранены по источнику, их доступность и реальная сборка не проверены. Добавлена тестовая зависимость httpclient5 для HTTP PATCH. Переносы строк из PDF исправлены. Для Jenkins потребуется собственный адрес репозитория и подключенный агент.
