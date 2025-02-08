# Домашнее задание к занятию "`Использование Terraform в команде`" - `Маркин Алексей`

### Задание 1

1. Возьмите код:
- из [ДЗ к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/src),
- из [демо к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/demonstration1).
2. Проверьте код с помощью tflint и checkov. Вам не нужно инициализировать этот проект.
3. Перечислите, какие **типы** ошибок обнаружены в проекте (без дублей).

## Решение 1

Код из [ДЗ к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/src),

```
docker run --rm -v $(pwd):/data -t  ghcr.io/terraform-linters/tflint
```

![Вопрос1](https://github.com/Markin-AI/ter-05/blob/main/img/1-1.png)

```
checkov -d ./src || docker run --tty --rm --volume $(pwd):/tf --workdir /tf bridgecrew/checkov --directory /tf
```

![Вопрос1](https://github.com/Markin-AI/ter-05/blob/main/img/1-2.png)

- из [демо к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/demonstration1).

```
docker run --rm -v $(pwd):/data -t  ghcr.io/terraform-linters/tflint
```

![Вопрос1](https://github.com/Markin-AI/ter-05/blob/main/img/1-3.png)

![Вопрос1](https://github.com/Markin-AI/ter-05/blob/main/img/1-4.png)

```
docker run --tty --rm --volume $(pwd):/tf --workdir /tf bridgecrew/checkov --directory /tf
```

![Вопрос1](https://github.com/Markin-AI/ter-05/blob/main/img/1-5.png)

![Вопрос1](https://github.com/Markin-AI/ter-05/blob/main/img/1-6.png)

---

### Задание 2

1. Возьмите ваш GitHub-репозиторий с **выполненным ДЗ 4** в ветке 'terraform-04' и сделайте из него ветку 'terraform-05'.
2. Повторите демонстрацию лекции: настройте YDB, S3 bucket, yandex service account, права доступа и мигрируйте state проекта в S3 с блокировками. Предоставьте скриншоты процесса в качестве ответа.
3. Закоммитьте в ветку 'terraform-05' все изменения.
4. Откройте в проекте terraform console, а в другом окне из этой же директории попробуйте запустить terraform apply.
5. Пришлите ответ об ошибке доступа к state.
6. Принудительно разблокируйте state. Пришлите команду и вывод.

## Решение 2

![Вопрос2](https://github.com/Markin-AI/ter-05/blob/main/img/2-1.png)

![Вопрос2](https://github.com/Markin-AI/ter-05/blob/main/img/2-2.png)

![Вопрос2](https://github.com/Markin-AI/ter-05/blob/main/img/2-3.png)

![Вопрос2](https://github.com/Markin-AI/ter-05/blob/main/img/2-4.png)

![Вопрос2](https://github.com/Markin-AI/ter-05/blob/main/img/2-5.png)

![Вопрос2](https://github.com/Markin-AI/ter-05/blob/main/img/2-6.png)

```
terraform force-unlock dac119cc-5f39-a776-8035-5a02697437ed
```

![Вопрос2](https://github.com/Markin-AI/ter-05/blob/main/img/2-7.png)

---

### Задание 3  

1. Сделайте в GitHub из ветки 'terraform-05' новую ветку 'terraform-hotfix'.
2. Проверье код с помощью tflint и checkov, исправьте все предупреждения и ошибки в 'terraform-hotfix', сделайте коммит.
3. Откройте новый pull request 'terraform-hotfix' --> 'terraform-05'. 
4. Вставьте в комментарий PR результат анализа tflint и checkov, план изменений инфраструктуры из вывода команды terraform plan.
5. Пришлите ссылку на PR для ревью. Вливать код в 'terraform-05' не нужно.

## Решение 3

[Сслыка на pull-request](https://github.com/Markin-AI/ter-04/pull/1)

---

### Задание 4

1. Напишите переменные с валидацией и протестируйте их, заполнив default верными и неверными значениями. Предоставьте скриншоты проверок из terraform console. 

- type=string, description="ip-адрес" — проверка, что значение переменной содержит верный IP-адрес с помощью функций cidrhost() или regex(). Тесты:  "192.168.0.1" и "1920.1680.0.1";
- type=list(string), description="список ip-адресов" — проверка, что все адреса верны. Тесты:  ["192.168.0.1", "1.1.1.1", "127.0.0.1"] и ["192.168.0.1", "1.1.1.1", "1270.0.0.1"].

## Решение 4

```
condition = can(regex("^(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])\\.(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])\\.(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])\\.(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])$", var.ip_address))
```

![Вопрос4](https://github.com/Markin-AI/ter-05/blob/main/img/4-1.png)

```
condition = alltrue([for ip in var.ip_address_list: can(regex("^(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])\\.(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])\\.(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])\\.(25[0-5]|2[0-4][0-9]|[0-1]?[0-9]?[0-9])$", ip))])
```

![Вопрос4](https://github.com/Markin-AI/ter-05/blob/main/img/4-2.png)

---

### Задание 5*
1. Напишите переменные с валидацией:
- type=string, description="любая строка" — проверка, что строка не содержит символов верхнего регистра;
- type=object — проверка, что одно из значений равно true, а второе false, т. е. не допускается false false и true true:
```
variable "in_the_end_there_can_be_only_one" {
    description="Who is better Connor or Duncan?"
    type = object({
        Dunkan = optional(bool)
        Connor = optional(bool)
    })

    default = {
        Dunkan = true
        Connor = false
    }

    validation {
        error_message = "There can be only one MacLeod"
        condition = <проверка>
    }
}
```

## Решение 5*

```
condition = var.in_the_end_there_can_be_only_one.Dunkan !=  var.in_the_end_there_can_be_only_one.Connor
```

![Вопрос5](https://github.com/Markin-AI/ter-05/blob/main/img/5-1.png)


---