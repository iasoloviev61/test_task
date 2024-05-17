## 2.Подготовить пошаговую обезличенную  инструкцию по созданию нового инстанса на платформе OpenStack. 

1. Создать проект в Openstack:

```
openstack project create test-project
openstack user create --project test-project --password test123 testuser
openstack role add --user testuser --project TENANT_ID member
```

2. Создать openrc файл для создания ресурсов в проекте

```
cat testuser-openrc
export PS1='[\u@\h \W(testuser)]\$ '
export OS_AUTH_TYPE=password
export OS_PROJECT_DOMAIN_NAME=Default
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_NAME=test-project
export OS_USERNAME=testuser
export OS_PASSWORD=test123
export OS_AUTH_URL=http://public_ip:5000/v3
export OS_REGION_NAME=RegionOne
export OS_IDENTITY_API_VERSION=3
export OS_IMAGE_API_VERSION=2
export OS_INTERFACE=public
```
3. Применить openrc

```
source testuser-openrc
```

4. Создать image, ssh keypair, network, subnet

```
openstack image create --disk-format raw --file images/cirros.img test-image
openstack keypair create --type ssh test-keypair
openstack network create test-net
openstack subnet create test-subnet --subnet-range 10.1.0.0/24 \
--network test-net --dns-nameserver 8.8.8.8 \
--dns-nameserver 8.8.4.4
```

5. Создать instance

```
openstack server create --image test-image\
 --flavor test-flavor\
 --key-name test-keypair\
 --network test-net\
 test_instance
```

6. Проверить созданный инстанс:

```
opensta server show test_instance
```
