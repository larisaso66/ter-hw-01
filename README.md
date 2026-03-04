# Домашнее задание к занятию "Введение в Terraform" - ``

---

### Подготовка к выполнению задания

`Cкриншот вывода команды terraform --version`

<img width="884" height="175" alt="Снимок экрана 2026-03-02 в 11 15 30" src="https://github.com/user-attachments/assets/d96f4289-186c-4ca9-9350-223ff6345123" />

### Задание 1

`1. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)`

Согласно .gitignore, личную секретную информацию (логины, пароли, ключи, токены) допустимо сохранять в файле personal.auto.tfvars

```
# own secret vars store.
personal.auto.tfvars
```
`2. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение`

В state-файле секретное содержимое ресурса random_password хранится в ключе result со значением "ebPW5GQDtl2lcVbx"
   
```
{
  "type": "random_password",
  "name": "random_string",
  "instances": [
    {
      "attributes": {
        "result": "ebPW5GQDtl2lcVbx",
        "special": false,
        "upper": true
      }
    }
  ]
}
```
`3. Объясните, в чём заключаются намеренно допущенные ошибки main.tf. Исправьте их`

- **cтрока 29:** *resource "docker_image"* — отсутствует имя ресурса (должно быть nginx);

- **cтрока 34:** *resource "docker_container" "1nginx"* — имя ресурса начинается с цифры (недопустимо, должно быть nginx);

- **cтрока 35:** *image = docker_image.nginx.image_id* — ссылается на несуществующий ресурс (нужно docker_image.nginx);

- **cтрока 36:** *name = "example_${random_password.random_string_FAKE.resulT}"* — ссылается на несуществующий ресурс и неправильное имя переменной

[Ссылка на исправленный main.tf](https://github.com/larisaso66/ter-hw-01/blob/main/src/main.tf)

`4.Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа`

[Ссылка на main.tf с hello_world](https://github.com/larisaso66/ter-hw-01/blob/main/src/main.tf_hello_world)

`Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ?`

Ключ *-auto-approve* опасен тем, что применяет изменения без запроса подтверждения. Это может привести к случайному удалению важных ресурсов и невозможности проверить план изменений перед применением.

Ключ *-auto-approve* применяется, когда нужно запустить Terraform автоматически, без участия человека. Например, в системах автоматической сборки и развертывания, где некому вводить "yes" вручную.

`Вывод команды docker ps`

<img width="964" height="155" alt="Снимок экрана 2026-03-02 в 23 20 00" src="https://github.com/user-attachments/assets/488bf7d5-b40d-4d7d-915f-84c1fda7158a" />

`5. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены`

[Ссылка на terraform.tfstate после destroy](https://github.com/larisaso66/ter-hw-01/blob/main/src/terraform.tfstate_destroy)

`6. Объясните, почему при этом не был удалён docker-образ nginx:latest`

Образ nginx:latest не удалился, потому что в *docker_image.nginx* указан параметр *keep_locally = true*

 Соглано документации terraform провайдера docker: *"keep_locally - (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation"*

То есть образ сохраняется локально даже после terraform destroy.

---

Задания со звездочкой не выполнялись
