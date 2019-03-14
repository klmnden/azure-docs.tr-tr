---
title: Oluşturma ve MySQL için Azure veritabanı, ansible ürününü kullanarak yapılandırma
description: MySQL sunucusu için Azure Veritabanı oluşturmak ve yapılandırmak için Ansible'ı kullanmayı öğrenin
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, mysql, veritabanı
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/23/2018
ms.openlocfilehash: 23530dbda06ba99a9c9b2e1665abb09afd8161b1
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791078"
---
# <a name="create-and-configure-an-azure-database-for-mysql-server-by-using-ansible"></a>Oluşturma ve MySQL için Azure veritabanı, ansible ürününü kullanarak yapılandırma
[MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/), bulutta yüksek kullanılabilirlikte MySQL veritabanları çalıştırmak, yönetmek ve ölçeklendirmek için kullanılan yönetilen bir hizmettir. Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. 

Bu hızlı başlangıçta MySQL sunucusu için Azure Veritabanı oluşturmak ve bu veritabanının güvenlik duvarı kuralını yapılandırmak için Ansible kullanma gösterilmektedir. Bu görevleri Azure portalını kullanarak yaklaşık beş dakika içinde tamamlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.7 gerekir. 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.  

Aşağıdaki örnek **eastus** konumunda **myResourceGroup** adlı bir kaynak grubu oluşturur:

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

Önceki playbook'u **rg.yml** olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook rg.yml
```

## <a name="create-a-mysql-server-and-database"></a>MySQL sunucusu ve veritabanı oluşturma
Aşağıdaki örnek **mysqlserveransible** adlı bir MySQL sunucusu ve **mysqldbansible** adlı bir MySQL için Azure Veritabanı örneği oluşturur. Bu, 1 Sanal Çekirdek içeren 5. Nesil bir Temel Amaçlı sunucudur. 

**mysqlserver_name** değerinin benzersiz olması gerekir. Bölgelere ve katmanlara göre geçerli olan değerleri anlamak için lütfen [fiyatlandırma katmanları](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers) belgelerini inceleyin. `<server_admin_password>` parolasını bir parolayla değiştirin.

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

Önceki playbook'u **mysql_create.yml** olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-a-firewall-rule"></a>Güvenlik duvarı kuralını yapılandırma
Sunucu düzeyinde bir güvenlik duvarı kuralı, dış bir uygulamanın Azure MySQL hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına izin verir. **mysql** komut satırı aracı veya MySQL Workbench dış uygulama örnekleridir.
Aşağıdaki örnek **extenalaccess** adında ve dış IP adresinden gelen bağlantılara izin veren bir güvenlik duvarı kuralı oluşturur. 

**startIpAddress** ve **endIpAddress** için kendi değerlerinizi girin. Bağlanacağınız yere karşılık gelen IP adres aralığını kullanın. 

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

> [!NOTE]
> MySQL için Azure Veritabanı bağlantıları 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu örnekte, BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.
> 

Burada bu görevi gerçekleştirmek için **azure_rm_resource** modülü kullanılmaktadır. REST API'sinin doğrudan kullanılmasına izin verir.

Önceki playbook'u **mysql_firewall.yml** olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server-by-using-the-command-line-tool"></a>Komut satırı aracını kullanarak sunucuya bağlanma
[MySQL'ı indirebilir](https://dev.mysql.com/downloads/) ve bilgisayarınıza yükleyebilirsiniz. Veya kod örneklerindeki **Deneyin** düğmesini ya da Azure portalında sağ köşedeki araç çubuğundan **>_** düğmesini seçebilir ve **Azure Cloud Shell**'i açabilirsiniz.

Aşağıdaki komutları girin: 

1. **mysql** komut satırı aracını kullanarak sunucuya bağlanın:
```azurecli-interactive
 mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
```

2. Sunucu durumunu görüntüleyin:
```sql
 mysql> status
```

Her şey yolunda giderse komut satırı aracı aşağıdaki metni oluşturmalıdır:

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

## <a name="using-facts-to-query-mysql-servers"></a>MySQL sunucularını sorgulamak için olguları kullanma
Aşağıdaki örnek **myResourceGroup** içindeki MySQL sunucularını ve ardından sunuculardaki tüm veritabanlarını sorgular:

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

Önceki playbook'u **mysql_query.yml** olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook mysql_query.yml
```

Bundan sonra MySQL sunucusu için aşağıdaki çıktıyı görürsünüz: 
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

Ayrıca MySQL veritabanı için de aşağıdaki çıktıyı görürsünüz:
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

Bu kaynaklara ihtiyacınız yoksa, bunları aşağıdaki örneği çalıştırarak silebilirsiniz. **myResourceGroup** adlı kaynak grubunu siler. 

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

Önceki playbook'u **rg_delete.yml** olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook rg_delete.yml
```

Yalnızca yeni oluşturulan MySQL sunucusunu silmek istiyorsanız aşağıdaki örneği çalıştırın:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Delete MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        state: absent
```

Önceki playbook'u **mysql_delete.yml** olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook mysql_delete.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)
