## Домашнее задание к занятию "13.4 инструменты для упрощения написания конфигурационных файлов. Helm и Jsonnet"
В работе часто приходится применять системы автоматической генерации конфигураций. Для изучения нюансов использования разных инструментов нужно попробовать упаковать приложение каждым из них.

### Задание 1: подготовить helm чарт для приложения
Необходимо упаковать приложение в чарт для деплоя в разные окружения. Требования:
* каждый компонент приложения деплоится отдельным deployment’ом/statefulset’ом;
* в переменных чарта измените образ приложения для изменения версии.

Установил NFS сервер через helm и создал pv - ourspace: </br>
`helm repo add stable https://charts.helm.sh/stable && helm repo update` </br>
`helm install nfs-server stable/nfs-server-provisioner` </br>
Установить helm-chart пакет в namespace - stage-ns-devops6: </br>
`helm install app devops6-chart --namespace stage-ns-devops6 --create-namespace` </br>
![app_install](https://github.com/murzinvit/screen_1/blob/c58a03582cc9157acf9b6e094526e6a529a058d2/Kuber_helm_app_install.jpg) </br>
Установить helm-chart пакет в namespace - test-ns-devops6: </br>
`helm install app devops6-chart --namespace test-ns-devops6 --create-namespace` </br>

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

-----------------------------------------------
Ссылки: </br>
https://www.digitalocean.com/community/tutorials/an-introduction-to-helm-the-package-manager-for-kubernetes-ru </br>
https://habr.com/ru/company/flant/blog/423239/ </br>
https://habr.com/ru/company/flant/blog/420437/ </br>
