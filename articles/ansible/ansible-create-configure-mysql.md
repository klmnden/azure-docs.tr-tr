---
title: Ansible'ı kullanarak MySQL için Azure Veritabanı sunucusu oluşturma ve yapılandırma (Önizleme)
description: Ansible'ı kullanarak MySQL için Azure Veritabanı sunucusu oluşturmayı ve yapılandırmayı öğrenin
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, mysql, veritabanı
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/23/2018
ms.openlocfilehash: 508274d11a9693d28a9b3a01bd6ebbd7198e8711
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586709"
---
# <a name="create-and-configure-an-azure-database-for-mysql-server-using-ansible-preview"></a>Ansible'ı kullanarak MySQL için Azure Veritabanı sunucusu oluşturma ve yapılandırma (önizleme)
[MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/), bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmak, yönetmek ve ölçeklendirmek için kullanılan, yönetilen bir hizmettir. Bu Hızlı Başlangıçta, Azure portalını kullanarak yaklaşık beş dakikada nasıl MySQL için Azure Veritabanı sunucusu oluşturacağınız gösterilir. 

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede Ansible'ı kullanarak beş dakika içinde MySQL için Azure Veritabanı sunucusu oluşturma ve güvenlik duvarı kuralını yapılandırma adımları gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.7 gerekir. Ansible 2.7 RC sürümünü `sudo pip install ansible[azure]==2.7.0rc2` çalıştırarak yükleyebilirsiniz. Ansible 2.7, Ekim 2018'de kullanıma sunulacaktır. Bundan sonra, varsayılan sürümü 2.7 olacağı için sürümü burada belirtmeniz gerekmez.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.  

Aşağıdaki örnek **eastus** konumunda **myResourceGroup** adlı bir kaynak grubu oluşturur.

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

Yukarıdaki playbook’u *rg.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook rg.yml
```

## <a name="create-mysql-server-and-database"></a>MySQL sunucusunu ve veritabanını oluşturma
Aşağıdaki örnek **mysqlserveransible** adlı bir MySQL sunucusu ve **mysqldbansible** adlı bir MySQL için Azure Veritabanı oluşturur. Bu, 1 sanal çekirdek içeren, 5. Nesil bir Temel sunucudur. **mysqlserver_name** değerinin benzersiz olması gerektiğini unutmayın. Bölgeler ve katmanlar için geçerli olan değerleri anlamak için lütfen [fiyatlandırma katmanları](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers) belgesini inceleyin. 

Kendi `<server_admin_password>` değerinizi girin:

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

Yukarıdaki playbook’u *mysql_create.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-firewall-rule"></a>Güvenlik duvarı kuralını yapılandırma
Sunucu düzeyindeki bir güvenlik duvarı kuralı, **mysql** komut satırı aracı veya MySQL Workbench gibi bir dış uygulamanın Azure MySQL hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına imkan tanır. Aşağıdaki örnek **extenalaccess** adında ve dış IP adresinden gelen bağlantılara izin veren bir güvenlik duvarı kuralı oluşturur. 

**startIpAddress** ve **endIpAddress** yerine bağlantı kuracağınız IP adresi aralığını girin: 

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
> MySQL için Azure Veritabanı bağlantıları 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.
> 

Bu görevi gerçekleştirmek için REST API'sinin doğrudan kullanılmasını sağlayan **azure_rm_resource** modülü kullanılmıştır.

Yukarıdaki playbook’u *mysql_firewall.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server-using-command-line-tool"></a>Komut satırı aracını kullanarak sunucuya bağlanın
MySQL'i [buradan](https://dev.mysql.com/downloads/) indirerek bilgisayarınıza yükleyebilirsiniz. Bunun yerine kod örneklerindeki **Deneyin** düğmesine veya Azure portalında sağ üstte bulunan `>_` düğmesine tıklayabilir ve **Azure Cloud Shell**'i başlatabilirsiniz.

Aşağıdaki komutları yazın: 

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
Aşağıdaki örnek **myResourceGroup** içindeki MySQL sunucularını ve ardından sunucudaki tüm veritabanlarını sorgular:

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

Yukarıdaki playbook’u *mysql_query*.yml olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook mysql_query.yml
```

Bunu yaptığınızda MySQL sunucusu için şu çıktıyı alırsınız: 
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

Ayrıca MySQL veritabanı için şu çıktıyı alırsınız:
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

Bu kaynaklara ihtiyacınız yoksa, aşağıdaki örneği çalıştırarak silebilirsiniz. **myResourceGroup** adlı kaynak grubunu siler. 

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

Yukarıdaki playbook’u *rg_delete*.yml olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook rg_delete.yml
```

Yalnızca yeni oluşturulan MySQL sunucusunu silmek istiyorsanız aşağıdaki örneği çalıştırarak silebilirsiniz:

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

Yukarıdaki playbook’u *mysql_delete.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook mysql_delete.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)