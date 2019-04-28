---
title: Hızlı Başlangıç - basit bir Azure CLI komutunu - az mysql ayarlama (Önizleme) kullanarak MySQL için Azure veritabanı oluşturma
description: Azure veritabanı Azure CLI (komut satırı arabirimi) kullanarak MySQL sunucusu için komutu oluşturmak için Hızlı Başlangıç Kılavuzu.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 3/18/2019
ms.custom: mvc
ms.openlocfilehash: aa0d2a9e990faa8d99355744824f34e26aeb519e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61231089"
---
# <a name="quickstart-create-an-azure-database-for-mysql-using-a-simple-azure-cli-command---az-mysql-up-preview"></a>Hızlı Başlangıç: Basit bir Azure CLI komutunu - az mysql ayarlama (Önizleme) kullanarak MySQL için Azure veritabanı oluşturma

> [!IMPORTANT]
> [Az mysql'i ayarlama](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) Azure CLI komutunu önizlemededir.

MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta nasıl kullanılacağı gösterilmektedir [az mysql'i ayarlama](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) Azure CLI kullanarak MySQL için Azure veritabanı oluşturmak için komutu. Sunucu oluşturmanın yanı sıra `az mysql up` komut bir örnek veritabanı, bir kök kullanıcı veritabanında oluşturur, Azure Hizmetleri için Güvenlik Duvarı'nı açar ve varsayılan istemci bilgisayar için güvenlik duvarı kuralları oluşturur. Bu, geliştirme süreci hızlandırmak için yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu makalede, Azure CLI Sürüm 2.0 veya sonraki çalıştırdığınızı gerektirir. yerel olarak. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

Kullanarak hesabınızda oturum açmanız gerekir [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) komutu. Komut çıktısındaki ilgili abonelik adına karşılık gelen **id** özelliğinin değerini not edin.

```azurecli
az login
```

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Yedek **abonelik kimliği** özelliğinden **az login** aboneliğinizin abonelik kimliği yer çıktı.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma

Bu komutları kullanmak için yükleme [db yukarı](/cli/azure/ext/db-up) uzantısı. Bir hata döndürülürse, Azure CLI'nin en son sürümünü yüklediğinizden emin olun. Bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli).

```azurecli
az extension add --name db-up
```

Aşağıdaki komutu kullanarak MySQL için Azure veritabanı oluşturma:

```azurecli
az mysql up
```

(, El ile geçersiz kılmadıkça) sunucusu aşağıdaki varsayılan değerlerle oluşturulur:

**Ayar** | **Varsayılan değer** | **Açıklama**
---|---|---
server-name | Sistem tarafından oluşturulan | Azure veritabanınızı MySQL sunucusuna tanıtan benzersiz bir ad.
resource-group | Sistem tarafından oluşturulan | Yeni bir Azure kaynak grubu.
sku-name | GP_Gen5_2 | Sku'nun adı. Kısaca {fiyatlandırma katmanı}\_{işlem nesli}\_{sanal çekirdek sayısı} kuralına uyar. Bir genel amaçlı 5. nesil 2 sanal çekirdek sunucusuna varsayılandır. Bkz. bizim [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/mysql/) katmanları hakkında daha fazla bilgi için.
backup-retention | 7 | Yedeklemenin ne kadar süreyle tutulacağı. Birim olarak gün kullanılır.
geo-redundant-backup | Devre dışı | Coğrafi olarak yedekli yedeklemelerin bu sunucu için etkinleştirilip etkinleştirilmeyeceği.
location | westus2 | Sunucu için Azure konumu.
ssl-enforcement | Devre dışı | Bu sunucu için ssl'in etkinleştirilip etkinleştirilmeyeceği.
storage-size | 5120 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır).
version | 5.7 | MySQL ana sürümü.
admin-user | Sistem tarafından oluşturulan | Yöneticinin oturum açma kullanıcı adı.
admin-password | Sistem tarafından oluşturulan | Yönetici kullanıcının parolası.

> [!NOTE]
> Hakkında daha fazla bilgi için `az mysql up` komut ve ek, parametreleri [Azure CLI belgeleri](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up).

Sunucunuz oluşturulduktan sonra aşağıdaki ayarlarla birlikte gelir:

- "Devbox" adlı bir güvenlik duvarı kuralı oluşturulur. Azure CLI, makinenin IP adresini algılamaya `az mysql up` komutu çalıştırın ve beyaz listelere bu IP adresidir.
- "Azure hizmetlerine erişime izin ver" açık olarak ayarlandı. Bu ayar, sunucunun güvenlik duvarı, aboneliğinizde olmayan kaynaklar dahil olmak üzere tüm Azure kaynaklarını gelen bağlantıları kabul edecek şekilde yapılandırır.
- `wait_timeout` Parametresi 8 saat olarak ayarlanır
- "Sampledb" adlı boş bir veritabanı oluşturulur
- "Sampledb" ayrıcalıkları olan "Kök" adlı yeni bir kullanıcı oluşturulduğunda

> [!NOTE]
> MySQL için Azure veritabanı, 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlantı kurduğunuzda, 3306 numaralı bağlantı noktası üzerinden giden trafiğe ağınızın güvenlik duvarı tarafından izin verilmiyor. BT departmanınızdan sunucunuza bağlanmak için 3306 numaralı bağlantı noktasını açmasını vardır.

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sonra `az mysql up` komut tamamlandığında, popüler programlama dillerini bağlantı dizelerinin listesini size geri döndürülür. Yeni oluşturulan Azure veritabanınızı MySQL sunucusuna belirli özellikleri ile önceden yapılandırılmış Bu bağlantı dizeleridir.

Kullanabileceğiniz [az mysql show-connection-string](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-show-connection-string) Bu bağlantı dizelerini yeniden listelemek için komutu.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aşağıdaki komutu kullanarak hızlı başlangıçta oluşturduğunuz tüm kaynakları temizleyin. Bu komut, MySQL için Azure veritabanı ve kaynak grubunu siler.

```azurecli
az mysql down --delete-group
```

Yalnızca yeni oluşturulan sunucuyu silmek istiyorsanız, çalıştırabileceğiniz [aşağı az mysql](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-down) komutu.

```azurecli
az mysql down
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI ile bir MySQL Veritabanı tasarlama](./tutorial-design-database-using-cli.md)
