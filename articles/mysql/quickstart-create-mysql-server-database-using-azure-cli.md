---
title: "Hızlı Başlangıç: MySQL sunucusu için Azure Veritabanı Oluşturma - Azure CLI | Microsoft Docs"
description: "Bu hızlı başlangıçta, Azure CLI aracını kullanarak bir Azure kaynak grubunda MySQL sunucusu için nasıl Azure Veritabanı sunucusu oluşturabileceğiniz açıklanır."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.workload: 
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 574299dd64120d75a1a36cb2ded0fdd269292570
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Azure CLI aracını kullanarak MySQL sunucusu için Azure Veritabanı oluşturma
Bu hızlı başlangıçta, Azure CLI aracını kullanarak bir Azure kaynak grubunda MySQL sunucusu için yaklaşık beş dakikada nasıl Azure Veritabanı sunucusu oluşturabileceğiniz açıklanır. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır.

Bu hızlı başlangıcı tamamlamak için en son [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) sürümünü yüklediğinizden emin olun. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```azurecli
az login
```
Komut istemi yönergelerini izleyerek tarayıcınızda https://aka.ms/devicelog adresini açın ve **komut isteminde** oluşturulan kodu girin.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[az group create](https://docs.microsoft.com/cli/azure/group#create) komutunu kullanarak bir [Azure kaynak grubu](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `westus` konumunda `mycliresource` adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure Veritabanı oluşturma
**az mysql server create** komutunu kullanarak MySQL sunucusu için Azure Veritabanı oluşturun. Bir sunucu birden çok veritabanını yönetebilir. Genellikle her proje veya kullanıcı için farklı bir veritabanı kullanılır.

Aşağıdaki örnekte, `westus` bölgesinde bulunan `mycliresource` kaynak grubundaki `mycliserver` adlı MySQL sunucusu için bir Azure Veritabanı oluşturulur. Sunucunun `myadmin` şeklinde bir oturum adı ve `Password01!` şeklinde bir parolası vardır. Sunucu **Temel** performans katmanıyla oluşturulmuştur ve sunucudaki tüm veritabanları **50** işlem birimini ortak olarak kullanır. Uygulama gereksinimlerine bağlı olarak işlem ve depolama ölçeğini büyütebilir veya küçültebilirsiniz.

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver--location westus --user myadmin --password Password01! --performance-tier Basic --compute-units 50
```

![Azure CLI aracını kullanarak MySQL sunucusu için Azure Veritabanı oluşturma](./media/quickstart-create-mysql-server-database-using-azure-cli/3_az-mysq-server-create.png)

## <a name="configure-firewall-rule"></a>Güvenlik duvarı kuralını yapılandırma
**az mysql server firewall-rule create** komutunu kullanarak MySQL sunucusu için Azure Veritabanı düzeyinde bir güvenlik duvarı kuralı oluşturun. Sunucu düzeyindeki bir güvenlik duvarı kuralı, **mysql.exe** komut satırı aracı veya MySQL Workbench gibi bir dış uygulamanın Azure MySQL hizmetinin güvenlik duvarı üzerinden sunucunuza bağlanmasına imkan tanır. 

Aşağıdaki örnekte önceden tanımlanmış bir adres aralığı için bir güvenlik duvarı kuralı oluşturulmaktadır. Bu örnek için aralık, olası tüm IP adresleri aralığıdır.

```azurecli
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>SSL ayarlarını yapılandırma
Varsayılan olarak sunucunuz ile istemci uygulamaları arasında SSL bağlantıları zorunlu tutulur.  Bu, İnternet üzerinden veri akışını şifreleyerek “hareket halindeki” verilerinizin güvenli olmasını sağlar.  Bu hızlı başlangıcı kolaylaştırmak amacıyla sunucunuz için SSL bağlantılarını devre dışı bırakacağız.  Üretim sunucuları için bunu yapmanız önerilmez.  Daha ayrıntılı bilgi edinmek için bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md).

Aşağıdaki örnekte, MySQL sunucunuzda SSL’yi zorunlu tutma ayarı devre dışı bırakılır.
 
 ```azurecli
 az mysql server update --resource-group mycliresource --name mycliserver -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.

```azurecli
az mysql server show --resource-group mycliresource --name mycliserver
```

Sonuç JSON biçimindedir. **fullyQualifiedDomainName** ve **administratorLogin** bilgilerini not alın.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>mysql.exe komut satırı aracını kullanarak sunucuya bağlanma
Sunucunuza **mysql.exe** komut satırı aracını kullanarak bağlanmak için bilgisayarınızda MySQL yüklemesinin olduğundan emin olun.  MySQL’yi [buradan](https://dev.mysql.com/downloads/) indirebilirsiniz.

Komut istemini açın ve şunu girin: 

1. **mysql** komut satırı aracını kullanarak sunucuya bağlanın:
```dos
 mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

2. Sunucu durumunu görüntüleyin:
```dos
 mysql> status
```
Her şey yolunda giderse, komut satırı aracı aşağıdaki çıkışı oluşturmalıdır:

```dos
C:\Users\v-chenyh>mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mycliserver.database.windows.net via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Ek komutlar için bkz. [MySQL 5.6 Başvuru Kılavuzu - Bölüm 4.5.1](https://dev.mysql.com/doc/refman/5.6/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>MySQL Workbench GUI aracını kullanarak sunucuya bağlanma
1.    İstemci bilgisayarınızda MySQL Workbench uygulamasını başlatın. MySQL Workbench uygulamasını [buradan](https://dev.mysql.com/downloads/workbench/) indirip yükleyebilirsiniz.

2.    **Yeni Bağlantı Oluştur** iletişim kutusundaki **Parametreler** sekmesine aşağıdaki bilgileri girin:

| **Parametreler** | **Açıklama** |
|----------------|-----------------|
|    *Bağlantı Adı* | Bu bağlantı için bir ad belirtin (herhangi bir ad olabilir) |
| *Bağlantı Yöntemi* | Standart (TCP/IP) seçeneğini belirleyin |
| *Ana Bilgisayar Adı* | mycliserver.database.windows.net (daha önce not aldığınız SERVER NAME değeri) |
| *Bağlantı Noktası* | 3306 |
| *Kullanıcı Adı* | myadmin@mycliserver (daha önce not aldığınız SERVER ADMIN LOGIN değeri) |
| *Parola* | Yönetici hesabı parolasını kasada depolayabilirsiniz |

![yeni bağlantı oluştur](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

3.    Tüm parametrelerin doğru yapılandırılıp yapılandırılmadığını test etmek için **Bağlantıyı Sına**’ya tıklayın.

4.    Şimdi, yeni oluşturduğunuz bağlantıya tıklayarak sunucuya başarıyla bağlanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara başka bir hızlı başlangıç kılavuzu/öğretici için gereksinim duymuyorsanız, aşağıdakileri yaparak bu kaynakları silebilirsiniz: 

```azurecli
az group delete --name mycliresource
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI ile bir MySQL Veritabanı tasarlayın](./tutorial-design-database-using-cli.md).

