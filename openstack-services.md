# 1.Описать взаимодействие основных компонентов платформы OpenStack(привести схемы).

## Схема взаимодействия сервисов Openstack

! [Схема взаимодействия сервисов Openstack] (./scheme.png)

## Основные сервисы Openstack:

Служба идентификации OpenStack Keystone представляет собой централизованный каталог пользователей и сервисов, к которым они имеют доступ. 
Keystone выступает в виде единой системы аутентификации.

1. Keystone проверяет действительность учетных записей пользователей, сопоставление пользователей проектам OpenStack и ролям и в случае успеха выдает токен на доступ к другим сервисам. 
Также Keystone ведет каталог служб.

2. Glance ведет каталог образов вир- туальных машин, которые пользователи могут использовать как шаблоны для запуска экземпляров виртуальных машин в облаке. 
Также данный сервис предоставляет функционал резервного копирования и создания моментальных снимков.

3. Nova Этот сервис устанавливается на всех вычислительных узлах кластера. 
Он предоставляет уровень абстракции виртуального оборудования (процессоры, память, блочные устройства, сетевые адаптеры). 
Nova обеспечивает управление экземплярами виртуальных машин, обращаясь к гипервизору и отдавая такие коман- ды, как, например, их запуск и остановку.

4. Neutron отвечает за сетевую связанность. Пользователи могут самостоятельно создавать виртуальные сети, маршрутизаторы, назначать IP-адреса.

5. Cinder управляет блочным хранилищем, которое могут использовать запущенные экземпляры виртуальных машин. 
Это постоянное хранилище информации для виртуальных машин.

6. Celiometer представляет собой централизованный источник информации по метрикам облака и данным мониторинга. 
Этот компонент обеспечивает возможность биллинга для OpenStack.

7. Horizon позволяющий управлять ресурсами облака через веб-консоль.

8. OpenStack Object Storage (в качестве бекенда может быть использован - Swift, Ceph S3, Minio) - Сервис представляет собой объектное хранилище, позволяющее пользователям хранить файлы. 


## Для чего нужен cloud-init?
cloud-init - инструмент, который автоматически применяет пользовательские данные к вашей ВМ.
При помощи cloud-init экземпляры виртуальных машин при старте могут настраивать различные параметры, такие как имя узла, открытый ssh-ключ для аутентификации, имя хоста и др. 
Инстансы ВМ получают эту информацию во время загрузки, обращаясь на адрес агента метаданных Neutron: http://169.254.169.254. 
Агент метаданных проксирует соответствующие запросы к openstack-nova-api при помощи пространства имен маршрутизатора или DHCP.
Конфигурационная информация передается в виртуальную машину при помощи параметров --user-data во время запуска.
Так же метаданные можно передать при старте виртуальной машины из Horizon на вкладке Post-Creation.