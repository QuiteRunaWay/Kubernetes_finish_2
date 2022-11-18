# Домашнее задание к занятию "14.2 Синхронизация секретов с внешними сервисами. Vault"

## Задача 1: Работа с модулем Vault

Запустить модуль Vault конфигураций через утилиту kubectl в установленном minikube

```
kubectl apply -f 14.2/vault-pod.yml
```
## Ответ: запустил модуль

![image](https://user-images.githubusercontent.com/92969676/202632869-7e8b82a5-5d17-4b17-8c30-4d152cec3177.png)

![image](https://user-images.githubusercontent.com/92969676/202635988-57f34e1e-5cf1-43e8-a9da-3e2a0b367997.png)

Получить значение внутреннего IP пода

```
kubectl get pod 14.2-netology-vault -o json | jq -c '.status.podIPs'
```
## Ответ: поставил утилиту и получил ip пода:

![image](https://user-images.githubusercontent.com/92969676/202633097-9c00a605-13de-4283-ba02-0804b9685123.png)

Примечание: jq - утилита для работы с JSON в командной строке

### Утилиту поставил командой ```apt-get install -y jq```

Запустить второй модуль для использования в качестве клиента

```
kubectl run -i --tty fedora --image=fedora --restart=Never -- sh
```

## Ответ: второй модуль запущен

![image](https://user-images.githubusercontent.com/92969676/202633606-22cfaa3d-a9a6-429e-9270-125749237abc.png)


Установить дополнительные пакеты

```
dnf -y install pip
pip install hvac
```
## Ответ: пакеты установлены

![image](https://user-images.githubusercontent.com/92969676/202633835-46ca98fa-aa2f-46ef-8b50-8b94c5a4d932.png)

![image](https://user-images.githubusercontent.com/92969676/202633922-21bb818e-95ac-435a-8198-5895a84f005e.png)

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

## Ответ: ip был изменен, токен был использован из примера, файл 14.2/vault-pod.yml

```
import hvac
client = hvac.Client(
    url='http://172.17.0.3:8200',
    token='aiphohTaa0eeHei'
)
client.is_authenticated()
```

![image](https://user-images.githubusercontent.com/92969676/202637383-5c9b5bac-3d3e-425e-9b29-cb138d9205da.png)

```
# Пишем секрет
client.secrets.kv.v2.create_or_update_secret(path='hvac',secret=dict(netology='Big secret!!!'),)
```

![image](https://user-images.githubusercontent.com/92969676/202637610-5619a437-e0ce-41f1-8952-eedecfb5ef58.png)

```
# Читаем секрет
client.secrets.kv.v2.read_secret_version(path='hvac',)
```

![image](https://user-images.githubusercontent.com/92969676/202637698-0c1f5281-2263-4166-b764-128a59340d3c.png)




