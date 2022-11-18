# Домашнее задание к занятию "14.2 Синхронизация секретов с внешними сервисами. Vault"

## Задача 1: Работа с модулем Vault

Запустить модуль Vault конфигураций через утилиту kubectl в установленном minikube

```
kubectl apply -f 14.2/vault-pod.yml
```
## Ответ: запустил модуль

![image](https://user-images.githubusercontent.com/92969676/202632869-7e8b82a5-5d17-4b17-8c30-4d152cec3177.png)


Получить значение внутреннего IP пода

```
kubectl get pod 14.2-netology-vault -o json | jq -c '.status.podIPs'
```
## Ответ: поставил утилиту и получил ip пода:

![image](https://user-images.githubusercontent.com/92969676/202633097-9c00a605-13de-4283-ba02-0804b9685123.png)

Примечание: jq - утилита для работы с JSON в командной строке

Утилиту поставил командой ```apt-get install -y jq```

Запустить второй модуль для использования в качестве клиента

```
kubectl run -i --tty fedora --image=fedora --restart=Never -- sh
```

## Ответ: второй модуль запущен


Установить дополнительные пакеты

```
dnf -y install pip
pip install hvac
```

Запустить интепретатор Python и выполнить следующий код, предварительно
поменяв IP и токен

```
import hvac
client = hvac.Client(
    url='http://10.10.133.71:8200',
    token='aiphohTaa0eeHei'
)
client.is_authenticated()

# Пишем секрет
client.secrets.kv.v2.create_or_update_secret(
    path='hvac',
    secret=dict(netology='Big secret!!!'),
)

# Читаем секрет
client.secrets.kv.v2.read_secret_version(
    path='hvac',
)
```
