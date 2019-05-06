---
title: 'Hızlı Başlangıç: Ayarlama - tek bir CLI komutu az postgres sunucusuyla olan PostgreSQL için Azure veritabanı oluşturma'
description: Komutu - Azure CLI (komut satırı arabirimi) kullanarak tek bir sunucu PostgreSQL için Azure veritabanı oluşturmak için Hızlı Başlangıç Kılavuzu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 05/06/2019
ms.openlocfilehash: 49f71c199a2832d763bb3c19d878fade47dfb8e4
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65069086"
---
# <a name="quickstart-use-an-azure-cli-command-az-postgres-up-preview-to-create-an-azure-database-for-postgresql---single-server"></a>Hızlı Başlangıç: -Tek bir sunucu PostgreSQL için Azure veritabanı oluşturmak için bir Azure CLI komutunu, az postgres ayarlama (Önizleme) kullanma

> [!IMPORTANT]
> [Az postgres'kurmak](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) Azure CLI komutunu önizlemededir.

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta nasıl kullanılacağı gösterilmektedir [az postgres'kurmak](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) Azure CLI kullanarak PostgreSQL için Azure veritabanı oluşturmak için komutu. Sunucu oluşturmanın yanı sıra `az postgres up` komut bir örnek veritabanı, bir kök kullanıcı veritabanında oluşturur, Azure Hizmetleri için Güvenlik Duvarı'nı açar ve varsayılan istemci bilgisayar için güvenlik duvarı kuralları oluşturur. Bu varsayılanlar, geliştirme süreci hızlandırmak için yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu makalede, Azure CLI Sürüm 2.0 veya sonraki çalıştırdığınızı gerektirir. yerel olarak. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

Kullanarak hesabınızda oturum açmak ihtiyacınız olacak [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) komutu. Not **kimliği** komut çıktısındaki ilgili abonelik adına için özellik.

```azurecli
az login
```

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Yedek **abonelik kimliği** özelliğinden **az login** aboneliğinizin abonelik kimliği yer çıktı.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Bu komutları kullanmak için yükleme [db yukarı](/cli/azure/ext/db-up) uzantısı. Bir hata döndürülürse, Azure CLI'nin en son sürümünü yüklediğinizden emin olun. Bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli).

```azurecli
az extension add --name db-up
```

Aşağıdaki komutu kullanarak PostgreSQL için Azure veritabanı oluşturma:

```azurecli
az postgres up
```

(, El ile geçersiz kılmadıkça) sunucusu aşağıdaki varsayılan değerlerle oluşturulur:

**Ayar** | **Varsayılan değer** | **Açıklama**
---|---|---
server-name | Sistem tarafından oluşturulan | PostgreSQL için Azure Veritabanı sunucunuzu tanıtan benzersiz bir ad.
resource-group | Sistem tarafından oluşturulan | Yeni bir Azure kaynak grubu.
sku-name | GP_Gen5_2 | Sku'nun adı. Kısaca {fiyatlandırma katmanı}\_{işlem nesli}\_{sanal çekirdek sayısı} kuralına uyar. Bir genel amaçlı 5. nesil 2 sanal çekirdek sunucusuna varsayılandır. Bkz. bizim [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/postgresql/) katmanları hakkında daha fazla bilgi için.
backup-retention | 7 | Nasıl uzun bir yedekleme korunur. Birim olarak gün kullanılır.
geo-redundant-backup | Devre dışı | Coğrafi olarak yedekli yedeklemelerin bu sunucu için etkinleştirilip etkinleştirilmeyeceği.
location | westus2 | Sunucu için Azure konumu.
ssl-enforcement | Devre dışı | Bu sunucu için ssl'in etkinleştirilip etkinleştirilmeyeceği.
storage-size | 5120 | Sunucunun depolama kapasitesi (birim olan megabayt kullanılır).
version | 10 | PostgreSQL ana sürümü.
admin-user | Sistem tarafından oluşturulan | Yönetici kullanıcı adı.
admin-password | Sistem tarafından oluşturulan | Yönetici kullanıcının parolası.

> [!NOTE]
> Hakkında daha fazla bilgi için `az postgres up` komut ve ek, parametreleri [Azure CLI belgeleri](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up).

Sunucunuz oluşturulduktan sonra aşağıdaki ayarlarla birlikte gelir:

- "Devbox" adlı bir güvenlik duvarı kuralı oluşturulur. Azure CLI, makinenin IP adresini algılamaya `az postgres up` komutu çalıştırın ve beyaz listelere bu IP adresidir.
- "Azure hizmetlerine erişime izin ver" açık olarak ayarlandı. Bu ayar, sunucunun güvenlik duvarı, aboneliğinizde olmayan kaynaklar dahil olmak üzere tüm Azure kaynaklarını gelen bağlantıları kabul edecek şekilde yapılandırır.
- "Sampledb" adlı boş bir veritabanı oluşturulur
- "Sampledb" ayrıcalıkları olan "Kök" adlı yeni bir kullanıcı oluşturulduğunda

> [!NOTE]
> PostgreSQL için Azure veritabanı, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanıyorsanız ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. BT departmanınızdan sunucunuza bağlanmak için 5432 numaralı bağlantı noktasını açmasını vardır.

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Sonra `az postgres up` komut tamamlandığında, popüler programlama dillerini bağlantı dizelerinin listesini size geri döndürülür. Yeni oluşturulan Azure veritabanınızı PostgreSQL sunucusuna belirli özellikleri ile önceden yapılandırılmış Bu bağlantı dizeleridir.

Kullanabileceğiniz [az postgres show-connection-string](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-show-connection-string) Bu bağlantı dizelerini yeniden listelemek için komutu.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aşağıdaki komutu kullanarak hızlı başlangıçta oluşturduğunuz tüm kaynakları temizleyin. Bu komut, PostgreSQL sunucusu için Azure veritabanı ve kaynak grubunu siler.

```azurecli
az postgres down --delete-group
```

Yalnızca yeni oluşturulan sunucuyu silmek istiyorsanız, çalıştırabileceğiniz [aşağı az postgres](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-down) komutu.

```azurecli
az postgres down
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
