## Домашнее задание к занятию "13.4 инструменты для упрощения написания конфигурационных файлов. Helm и Jsonnet"
В работе часто приходится применять системы автоматической генерации конфигураций. Для изучения нюансов использования разных инструментов нужно попробовать упаковать приложение каждым из них.

### Задание 1: подготовить helm чарт для приложения
Необходимо упаковать приложение в чарт для деплоя в разные окружения. Требования:
* каждый компонент приложения деплоится отдельным deployment’ом/statefulset’ом;
* в переменных чарта измените образ приложения для изменения версии.

Установил NFS сервер через helm-chart: </br>
`helm repo add stable https://charts.helm.sh/stable && helm repo update` </br>
`helm install nfs-server stable/nfs-server-provisioner` </br>
Создать pvc для front, back и database: </br>
 ```
 kubectl apply -f - <<EOF
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
      name: front-volume
    spec:
      storageClassName: "nfs"
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 100Mi
 EOF
 ```
 ![helm_make_pvc](https://github.com/murzinvit/screen_1/blob/f0e08f45cad230d631d78e33071929ef5bc3fac5/Kuber_helm_make_pvc.jpg) </br>
Установить приложение, перейти в папку с helm-chart и выполнить команду: </br>
`helm install devops6-app .` </br>
[devops6-chart](https://github.com/murzinvit/13.04_kubernetes_config_helm/tree/main/devops6-chart) </br>
![app_install](https://github.com/murzinvit/screen_1/blob/4eed8c1dd05717430e86ec6ac771ea54b85bc535/Kuber_helm_installdevops6_app1.jpg) </br>
Сервисы подняты в deployments: </br>
![app_deployment](https://github.com/murzinvit/screen_1/blob/e61fda2e4392c432d6c9e4d199d9a53b69739900/Kuber_helm_app_deployment.jpg) </br>
Смена версии контейнера в файле values.yaml. Заменил latest на ver1: </br>
![container_value](https://github.com/murzinvit/screen_1/blob/62c8610f89f339fb690caa02978e81d80cd4d1a5/Kuber_change_container_value.jpg) </br>

### Задание 2: запустить 2 версии в разных неймспейсах
Подготовив чарт, необходимо его проверить. Попробуйте запустить несколько копий приложения:
* одну версию в namespace=app1;
* вторую версию в том же неймспейсе;
* третью версию в namespace=app2.

### Задание 3 (*): повторить упаковку на jsonnet
Для изучения другого инструмента стоит попробовать повторить опыт упаковки из задания 1, только теперь с помощью инструмента jsonnet. </br>

Раочие заметки: </br>
-----------------------------------------------
Установка NFS сервера через helm: </br>
    установить helm: `curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash` </br>
    добавить репозиторий чартов: `helm repo add stable https://charts.helm.sh/stable && helm repo update` </br>
    установить nfs-server через helm: `helm install nfs-server stable/nfs-server-provisioner` </br>

chart — это набор файлов </br>
Очистка всех доступных значений конфигурации пакета: `helm inspect values chart-name` </br>
При установке объединяется конфигурация из --set, values.yaml, templates </br>
Helm позволяет легко возвращаться к предыдущим редакциям релиза </br>
Создать пример каталога пакетов: helm create chart-name </br>
Файлы, необходимые для генерации манифестов Kubernetes-ресурсов: </br>
`Chart.yaml` - метаданные(name,version, etc..) </br>
`templates` - файлы манифеста Kubernetes(шаблоны) </br>
`charts` - </br>
`values.yaml` - переменные конфигурации из манифестов(значения по умолчанию) </br>
`requirements.yaml` - зависимости </br>
Установка пакета: `helm install app devops6-chart --namespace stage-ns-devops6 --create-namespace` </br>

-----------------------------------------------
Ссылки: </br>
https://www.digitalocean.com/community/tutorials/an-introduction-to-helm-the-package-manager-for-kubernetes-ru </br>
https://habr.com/ru/company/flant/blog/423239/ </br>
https://habr.com/ru/company/flant/blog/420437/ </br>
