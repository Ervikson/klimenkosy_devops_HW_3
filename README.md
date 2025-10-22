

1.1
  Скачал файлы
  <img width="1320" height="360" alt="HW_3_1-1" src="https://github.com/user-attachments/assets/cf33b09d-d7b0-41c1-9e50-65901d9d2f97" />

1.2
  Сикреты необходимо хранть в personal.auto.tfvars т.к.:
    - Не попадает в коммиты
    - Не отправляется в удалённый репозиторий
    - Доступен только локально

  Тогда как: 
    - terraform.tfvars — не игнорируется, попадает в репозиторий
    - *.tfstate — файлы состояния, а не конфигурации
    - .terraformrc — конфиг Terraform, но он НЕ игнорируется (есть исключение !.terraformrc)

1.3
  Выполнил код проекта:
  <img width="866" height="937" alt="HW_3_1-3_1" src="https://github.com/user-attachments/assets/23d4bec6-d346-4e94-8e5a-a385d8d0f061" />

  Ключ:Значение
    "result": "Aw3ixRltgWauBWl4"

1.4
  terrafom validate
  <img width="866" height="937" alt="HW_3_1-4_1" src="https://github.com/user-attachments/assets/6086bd37-b0ea-49e9-a5a0-1d9183a37479" />

  -  Первая ошибка говорит о том, что в блоке ресурса docker_image должно быть указано 2 обязательных лейбла - Тип и Имя
  -  Вторая ошибка синтаксическая, в лейбле имени "1nginx" - необходимо указать "nginx"

     так же, terraform validate нам об этом не говорит, но:
     
  -  В блоке required_providers не указан провайдер random, но используется ресурс random_password
  -  Используется random_password.random_string_FAKE.resulT с ошибками:
       random_string_FAKE вместо random_string
       resulT вместо result

1.5
  Выполнение исправленного кода:
  <img width="1556" height="937" alt="HW_3_1-5" src="https://github.com/user-attachments/assets/f58d8a14-d2ca-46ee-b1e6-79dc611a26f5" />

1.6
  Поменяли имя контейнера:
  <img width="1556" height="937" alt="HW_3_1-6" src="https://github.com/user-attachments/assets/790ff662-f66a-49d5-a5b5-0cc8533518a3" />

  Опасности:
    - Нет проверки изменений - Terraform применяет все изменения без возможности предварительного просмотра и анализа
    - Риск потери данных - Может случайно удалить или изменить критичные ресурсы
    - Автоматическое применение ошибок - Если в коде есть ошибки, они сразу попадут в продакшен
    - Нет контроля времени - Изменения применяются сразу, без возможности выбрать подходящий момент

  Полезности:
    - CI/CD пайплайны - Для автоматического деплоя в тестовых/стейджинг окружениях
    - Ночные деплои - Для автоматического применения изменений по расписанию
    - Скрипты автоматизации - Когда Terraform запускается неинтерактивно
    - Тестовые среды - Где быстрое развертывание важнее стабильности

1.7
  terraform destroy:
    <img width="1556" height="937" alt="HW_3_1-7" src="https://github.com/user-attachments/assets/50d327e2-c776-495f-825c-c30c5e11bb87" />

1.8
  В официальной документации провайдера kreuzwerker/docker для ресурса docker_image указано:
    keep_locally - (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation.

  Параметр keep_locally = true явно указывает Terraform не удалять Docker-образ при выполнении команды terraform destroy
