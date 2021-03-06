---
title: Öğretici - Ansible kullanarak MySQL için Azure veritabanı'nda veritabanlarını yapılandırma | Microsoft Docs
description: MySQL sunucusu için Azure Veritabanı oluşturmak ve yapılandırmak için Ansible'ı kullanmayı öğrenin
keywords: ansible, azure, devops, bash, playbook, mysql, veritabanı
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 1170ae9d609a07dbdaebf50e145de65faefa60ec
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230921"
---
# <a name="tutorial-configure-databases-in-azure-database-for-mysql-using-ansible"></a>Öğretici: Ansible'ı kullanarak MySQL için Azure veritabanı'nda veritabanlarını yapılandırın

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[MySQL için Azure veritabanı](/azure/mysql/overview) bir ilişkisel veritabanı hizmeti üzerinde MySQL Community sürümünü temel alır. MySQL için Azure veritabanı, web uygulamalarınızda MySQL veritabanlarını yönetmenizi sağlar.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * MySql sunucusu oluşturma
> * MySql veritabanı oluşturma
> * Bir dış uygulama sunucunuza bağlanabilmesi filewall kuralı yapılandırma
> * Azure cloud shell'de, MySql sunucusuna bağlanma
> * Sorgu, kullanılabilir MySQL sunucuları
> * Tüm veritabanları, bağlı sunucular listesi

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Bu bölümdeki playbook kodu bir Azure kaynak grubu oluşturur. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.  

Aşağıdaki playbook'u `rg.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

* Adlı bir kaynak grubu `myResourceGroup` oluşturulur.
* Kaynak grubu oluşturulur `eastus` konumu:

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook rg.yml
```

## <a name="create-a-mysql-server-and-database"></a>MySQL sunucusu ve veritabanı oluşturma

Bu bölümde playbook kodda bir MySQL server ve MySQL örneği için Azure veritabanı oluşturur. Yeni MySQL server ile bir sanal çekirdek Gen 5 temel amacı sunucusudur ve adlı `mysqlserveransible`. Veritabanı örneği adlı `mysqldbansible`.

Fiyatlandırma katmanları hakkında daha fazla bilgi için bkz. [fiyatlandırma katmanları MySQL için Azure veritabanı](/azure/mysql/concepts-pricing-tiers). 

Aşağıdaki playbook'u `mysql_create.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus
    mysqlserver_name: mysqlserveransible
    mysqldb_name: mysqldbansible
    admin_username: mysqladmin
    admin_password: <server_admin_password> 
  tasks:
    - name: Create MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        sku:
          name: B_Gen5_1
          tier: Basic
        location: "{{ location }}"
        version: 5.6
        enforce_ssl: True
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        storage_mb: 51200
    - name: Create instance of MySQL Database
      azure_rm_mysqldatabase:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
        name: "{{ mysqldb_name }}"
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

* İçinde `vars` bölümü değerini `mysqlserver_name` benzersiz olması gerekir.
* İçinde `vars` bölümünde, değiştirin `<server_admin_password>` parolayla.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-a-firewall-rule"></a>Güvenlik duvarı kuralını yapılandırma

Sunucu düzeyinde güvenlik duvarı kuralı, bir dış uygulamanın Azure MySQL hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına izin verir. Dış uygulamalar örnekler `mysql` komut satırı aracı ve MySQL Workbench.

Bu bölümde playbook kodda adlı bir güvenlik duvarı kuralı oluşturur `extenalaccess` , herhangi bir dış IP adresinden bağlantılara izin verir. 

Aşağıdaki playbook'u `mysql_firewall.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
  - name: Open firewall to access MySQL Server from outside
    azure_rm_resource:
      api_version: '2017-12-01'
      resource_group: "{{ resource_group }}"
      provider: dbformysql
      resource_type: servers
      resource_name: "{{ mysqlserver_name }}"
      subresource:
        - type: firewallrules
          name: externalaccess
      body:
        properties:
          startIpAddress: "0.0.0.0"
          endIpAddress: "255.255.255.255"
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

* Değişken bölümünde değiştirin `startIpAddress` ve `endIpAddress`. Bağlanmakta aralığın karşılık gelen IP adresi aralığı kullanın.
* MySQL için Azure Veritabanı bağlantıları 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu örnekte, BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.
* Playbook'u kullanan `azure_rm_resource` modülü REST API'sini doğrudan kullanımına izin verir.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server"></a>Sunucuya bağlanma

Bu bölümde, daha önce oluşturduğunuz sunucuya bağlanmak için Azure cloud shell kullanın.

1. Seçin **deneyin** aşağıdaki kodda düğmesi:

    ```azurecli-interactive
    mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
    ```

1. İstemde, sunucu durumunu sorgulamak için aşağıdaki komutu girin:

    ```sql
    mysql> status
    ```
    
    Her şey yolunda giderse, aşağıdaki sonuçları benzer bir çıktı görürsünüz:
    
    ```
    demo@Azure:~$ mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65233
    Server version: 5.6.39.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> status
    --------------
    mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64) using  EditLine wrapper
    
    Connection id:          65233
    Current database:
    Current user:           mysqladmin@13.76.42.93
    SSL:                    Cipher in use is AES256-SHA
    Current pager:          stdout
    Using outfile:          ''
    Using delimiter:        ;
    Server version:         5.6.39.0 MySQL Community Server (GPL)
    Protocol version:       10
    Connection:             mysqlserveransible.mysql.database.azure.com via TCP/IP
    Server characterset:    latin1
    Db     characterset:    latin1
    Client characterset:    utf8
    Conn.  characterset:    utf8
    TCP port:               3306
    Uptime:                 36 min 21 sec
    
    Threads: 5  Questions: 559  Slow queries: 0  Opens: 96  Flush tables: 3  Open tables: 10  Queries per second avg: 0.256
    --------------
    ```
    
## <a name="query-mysql-servers"></a>Sorgu MySQL sunucuları

Bu bölümdeki playbook kod MySQL sunucuları sorgular `myResourceGroup` ve bulunan sunucular veritabanlarını listeler.

Aşağıdaki playbook'u `mysql_query.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Query MySQL Servers in current resource group
      azure_rm_mysqlserver_facts:
        resource_group: "{{ resource_group }}"
      register: mysqlserverfacts

    - name: Dump MySQL Server facts
      debug:
        var: mysqlserverfacts

    - name: Query MySQL Databases
      azure_rm_mysqldatabase_facts:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
      register: mysqldatabasefacts

    - name: Dump MySQL Database Facts
      debug:
        var: mysqldatabasefacts
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook mysql_query.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```json
"servers": [
    {
        "admin_username": "mysqladmin",
        "enforce_ssl": false,
        "fully_qualified_domain_name": "mysqlserveransible.mysql.database.azure.com",
        "id": "/subscriptions/685ba005-af8d-4b04-8f16-a7bf38b2eb5a/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysqlserveransible",
        "location": "eastus",
        "name": "mysqlserveransible",
        "resource_group": "myResourceGroup",
        "sku": {
            "capacity": 1,
            "family": "Gen5",
            "name": "B_Gen5_1",
            "tier": "Basic"
        },
        "storage_mb": 5120,
        "user_visible_state": "Ready",
        "version": "5.6"
    }
]
```

Ayrıca MySQL veritabanı için aşağıdaki çıktıyı görürsünüz:

```json
"databases": [
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "information_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysql",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransibler"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysqldbansible",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "performance_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Aşağıdaki playbook'u `cleanup.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)