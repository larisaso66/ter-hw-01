# Домашнее задание к занятию "Введение в Terraform" - `Фамилия и имя студента`

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
      "mode": "managed",
      "type": "random_password",
      "name": "random_string",
      "provider": "provider[\"registry.terraform.io/hashicorp/random\"]",
      "instances": [
        {
          "schema_version": 3,
          "attributes": {
            "bcrypt_hash": "$2a$10$AALDwHXfqUklsq.tibtvUO/r9Nmj6HyPWCPoZVj/VpgcsT7G7hk.2",
            "id": "none",
            "keepers": null,
            "length": 16,
            "lower": true,
            "min_lower": 1,
            "min_numeric": 1,
            "min_special": 0,
            "min_upper": 1,
            "number": true,
            "numeric": true,
            "override_special": null,
            "result": "ebPW5GQDtl2lcVbx",
            "special": false,
            "upper": true
          },
```
`3. Объясните, в чём заключаются намеренно допущенные ошибки main.tf. Исправьте их`

- **cтрока 29:** *resource "docker_image"* — отсутствует имя ресурса (должно быть "nginx");

- **cтрока 34:** *resource "docker_container" "1nginx"* — имя ресурса начинается с цифры (недопустимо, должно быть "nginx");

- **cтрока 35:** *image = docker_image.nginx.image_id* — ссылается на несуществующий ресурс (нужно docker_image.nginx);

- **cтрока 36:** *name = "example_${random_password.random_string_FAKE.resulT}"* — ссылается на несуществующий ресурс и неправильное имя переменной

**Исправленный main.tf**

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
    }
  }
  required_version = "~>1.12.0" /*Многострочный комментарий.
 Требуемая версия terraform */
}
provider "docker" {}

#однострочный комментарий

resource "random_password" "random_string" {
  length      = 16
  special     = false
  min_upper   = 1
  min_lower   = 1
  min_numeric = 1
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"

  ports {
    internal = 80
    external = 9090
  }
}
```
`4.Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа`

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
    }
  }
  required_version = ">=1.12.0" 
}
provider "docker" {}

resource "random_password" "random_string" {
  length      = 16
  special     = false
  min_upper   = 1
  min_lower   = 1
  min_numeric = 1
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "hello_world"

  ports {
    internal = 80
    external = 9090
  }
}
```
`Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ?`

Ключ -auto-approve опасен тем, что применяет изменения без запроса подтверждения. Это может привести к случайному удалению важных ресурсов и невозможности проверить план изменений перед применением.

Ключ -auto-approve применяется, когда нужно запустить Terraform автоматически, без участия человека. Например, в системах автоматической сборки и развертывания, где некому вводить "yes" вручную.

<img width="964" height="155" alt="Снимок экрана 2026-03-02 в 23 20 00" src="https://github.com/user-attachments/assets/488bf7d5-b40d-4d7d-915f-84c1fda7158a" />



---

### Задание 2

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


---

### Задание 3

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

### Задание 4

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
