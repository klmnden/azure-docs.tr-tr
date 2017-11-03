---
title: "Oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure CLI kullanarak yönetme | Microsoft Docs"
description: "Bu makalede, oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure CLI komut satırını kullanarak yönetme açıklar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 66d192287eeaaaa82c0f61f8aa13b8bf7bf8cd47
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-cli"></a>Oluşturma ve Azure CLI kullanarak Azure veritabanı için MySQL güvenlik duvarı kurallarını yönetme
Sunucu düzeyinde güvenlik duvarı kuralları yöneticilerin belirli bir IP adresi veya bir IP adresi aralığı bir Azure veritabanına erişim için MySQL Server yönetmesine izin verin. Uygun Azure CLI komutları kullanarak, oluşturabilir, güncelleştirme, silin, listeleyin ve sunucunuzu yönetmek için güvenlik duvarı kuralları gösterir. Genel Bakış Azure veritabanı için MySQL güvenlik duvarları için bkz: [Azure veritabanı için MySQL server güvenlik duvarı kuralları](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Ön koşullar
* [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).
* Azure Python SDK PostgreSQL ve MySQL hizmetlerini yükleyin.
* PostgreSQL ve MySQL Hizmetleri için Azure CLI bileşenini yükleyin.
* MySQL sunucusu için bir Azure veritabanı oluşturun.

## <a name="firewall-rule-commands"></a>Güvenlik duvarı kuralı komutlar:
**Az mysql server güvenlik duvarı kuralı** komut oluşturma, silme, liste, Göster ve güvenlik duvarı kurallarını güncelleştir için Azure CLI üzerinden kullanılır.

Komutlar:
- **oluşturma**: bir Azure MySQL server güvenlik duvarı kuralı oluşturun.
- **silme**: Azure MySQL server güvenlik duvarı kuralını siler.
- **Liste**: Azure MySQL server güvenlik duvarı kuralları listesi.
- **Göster**: güvenlik duvarı kuralı Azure MySQL server ayrıntılarını gösterin.
- **Güncelleştirme**: bir Azure MySQL server güvenlik duvarı kuralı güncelleştirin.

## <a name="log-in-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Azure'da oturum açma ve Azure veritabanı için MySQL sunucuları listesi
Azure CLI kullanarak güvenli bir şekilde Azure hesabınızla bağlanmak **az oturum açma** komutu.

1. Komut satırından aşağıdaki komutu çalıştırın:
```azurecli
az login
```
Bu komut sonraki adımda kullanmak için bir kod çıkarır.

2. Sayfasını açmak için bir web tarayıcısı kullanma [https://aka.ms/devicelogin](https://aka.ms/devicelogin)ve ardından kodunu girin.

3. İstendiğinde, Azure kimlik bilgilerinizi kullanarak oturum açın.

4. Oturumunuzla yetkilendirildikten sonra Aboneliklerin listesini konsolda yazdırılır. Kullanılacak geçerli aboneliği ayarlamak için istenen abonelik Kimliğini kopyalayın.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Adlarını değilseniz MySQL sunucuları abonelik ve kaynak grubunuz için Azure veritabanlarını listeler.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Üzerinde çalışmak üzere MySQL server belirtmek zorunda listenin adı özniteliği unutmayın. Gerekirse, bu sunucu için ayrıntıları doğrulayın ve doğru olduğundan emin olmak için name özniteliği kullanma:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı güvenlik duvarı kuralları listesi 
Sunucu adı ve kaynak grubu adı kullanarak, sunucu üzerinde var olan sunucunun güvenlik duvarı kuralları listesi. Sunucu adı özniteliği belirtilen dikkat edin **--server** geçiş ve de **--ad** geçin.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
İçindeki bir JSON (varsayılan) biçimlendirmek istiyorsanız çıkış kuralları listeler. Kullanabileceğiniz **--çıktı tablosu** , daha okunabilir bir tablo biçiminde sonuçlarını çıkarmak için anahtar.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mysql-server"></a>Bir güvenlik duvarı kuralı Azure veritabanı için MySQL sunucusu oluşturun.
Azure MySQL sunucu adını ve kaynak grubu adı kullanarak, sunucu üzerinde yeni bir güvenlik duvarı kuralı oluşturun. Başlangıç IP yanı sıra, kural için bir ad ve kural için (bir IP adresi aralığı erişim sağlamak için) IP bitmelidir.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Tek bir IP adresi için erişime izin vermek için bu örnekte olduğu gibi aynı IP adresi IP başlangıç ve bitiş IP olarak sağlayın.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Başarılı, komut çıktısı JSON biçiminde (varsayılan) oluşturduğunuz güvenlik duvarı kuralı ayrıntılarını listeler. Çıkış hatası varsa, hata iletisi metni yerine gösterir.

## <a name="update-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı üzerinde bir güvenlik duvarı kuralı güncelleştirme 
Azure MySQL sunucu adı ve kaynak grubu adı kullanarak, sunucuda mevcut bir güvenlik duvarı kuralı güncelleştirin. Var olan güvenlik duvarı kuralı adı güncelleştirmek için IP ve bitiş IP öznitelikler giriş yanı sıra başlangıç sağlar.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Başarılı, komut çıktısı JSON biçiminde (varsayılan) güncelleştirdiniz güvenlik duvarı kuralı ayrıntılarını listeler. Çıkış hatası varsa, hata iletisi metni yerine gösterir.

> [!NOTE]
> Güvenlik duvarı kuralı mevcut değilse, kural güncelleştirme komutu tarafından oluşturulur.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Güvenlik duvarı kural ayrıntılarını Azure veritabanı için MySQL Server Göster
Azure MySQL sunucu adını ve kaynak grubu adı kullanarak, var olan Güvenlik Duvarı'nı sunucudan kural ayrıntılarını gösterir. Var olan güvenlik duvarı kuralının adını girdi olarak sağlayın.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Başarılı, komut çıktısı JSON biçiminde (varsayılan) belirttiğiniz güvenlik duvarı kuralı ayrıntılarını listeler. Çıkış hatası varsa, hata iletisi metni yerine gösterir.

## <a name="delete-a-firewall-rule-on-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı üzerinde bir güvenlik duvarı kuralını siler
Azure MySQL sunucu adı ve kaynak grubu adı kullanarak, mevcut bir güvenlik duvarı kuralı sunucudan kaldırın. Var olan güvenlik duvarı kuralının adını sağlayın.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Başarı hiçbir çıktısı yok. Başarısızlık durumunda, hata iletisi metni görüntüler.

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında daha iyi anlamak [Azure veritabanı için MySQL Server güvenlik duvarı kuralları](./concepts-firewall-rules.md).
- [Oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure Portalı'nı kullanarak yönetme](./howto-manage-firewall-using-portal.md).
