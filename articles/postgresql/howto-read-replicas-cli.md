---
title: Salt okunur çoğaltmalar için Azure clı'dan PostgreSQL için Azure veritabanı yönetme
description: Azure CLI üzerinden çoğaltmaları okuma PostgreSQL için Azure veritabanı yönetmeyi öğrenin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/29/2019
ms.openlocfilehash: 4c8502aad40662766b038205eb19dd3302f601b7
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632512"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli"></a>Oluşturma ve Azure CLI üzerinden salt okunur çoğaltmalar yönetme

Bu makalede, oluşturma ve Azure clı'dan PostgreSQL için Azure veritabanı'nda salt okunur çoğaltmalar yönetme konusunda bilgi edinin. Salt okunur çoğaltmalar hakkında daha fazla bilgi için bkz: [genel bakış](concepts-read-replicas.md).

> [!IMPORTANT]
> Salt okunur çoğaltma özelliği genel Önizleme aşamasındadır.

## <a name="prerequisites"></a>Önkoşullar
- Bir [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-up-azure-cli.md) ana sunucu olarak.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 


## <a name="prepare-the-master-server"></a>Ana sunucu hazırlama
Bu adımlar, genel amaçlı veya bellek için iyileştirilmiş katmanlarındaki ana sunucu hazırlamak için kullanılmalıdır.

`azure.replication_support` Parametresi ayarlanmalıdır **çoğaltma** ana sunucu üzerinde. Bu statik parametre değiştiğinde, değişikliğin etkili olması için sunucunun yeniden başlatılması gereklidir.

1. Ayarlama `azure.replication_support` çoğaltmaya.

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. Sunucuya değişikliği uygulamak için yeniden başlatın.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma

`az mysql server replica create` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Çoğaltma sunucusu oluşturulacağı kaynak grubu.  |
| ad | mydemoserver-çoğaltma | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Çoğaltma kaynağı adı veya kaynak kimliği mevcut ana sunucu. |

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Ayarlamadıysanız `azure.replication_support` parametresi **çoğaltma** üzerinde bir genel amaçlı veya ana sunucu ve sunucu yeniden bellek için iyileştirilmiş, bir hata alırsınız. Bir çoğaltma oluşturmadan önce bu iki adımı tamamlayın.

Çoğaltma Yöneticisi olarak aynı sunucu yapılandırmasını kullanarak oluşturulur. Bir çoğaltma oluşturulduktan sonra birkaç ayar bağımsız olarak ana sunucu ile değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı da ayrı ayrı değiştirilebilir ya da temel katmandan hariç.

> [!IMPORTANT]
> Bir ana sunucu yapılandırması için yeni değerleri güncelleştirilmeden önce çoğaltma yapılandırması eşit veya daha fazla değerlerle güncelleştirin. Bu eylem, çoğaltma ana dala yapılan değişiklikler ile koruyabilirsiniz sağlar.

## <a name="list-replicas"></a>Liste çoğaltmalar
Ana sunucu çoğaltmalarını listesini görüntüleyebilirsiniz.

```azurecli-interactive
az postgres server replica stop --server-name mydemoserver --resource-group myresourcegroup 
```

## <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma
Bir ana sunucu ve bir salt okunur çoğaltma arasında çoğaltmayı durdurabilirsiniz.

Çoğaltma için ana sunucu ve salt okunur bir çoğaltması durdurduktan sonra geri alınamaz. Salt okunur çoğaltma hem okuma hem de yazma işlemleri destekleyen bir tek başına sunucuya olur. Tek başına sunucu ile bir çoğaltma yeniden yapılamıyor.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

## <a name="delete-a-master-or-replica-server"></a>Bir ana veya çoğaltma sunucusu silme
Bir ana veya çoğaltma sunucusu silmek için bir PostgreSQL sunucusu için Azure veritabanı başına silmek için aynı komutunu kullanabilirsiniz. 

Ana sunucu sildiğinizde tüm salt okunur çoğaltmalar için çoğaltma durdurulur. Salt okunur çoğaltmaların artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucuları olur.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [PostgreSQL için Azure veritabanı'nda çoğaltmaları okuma](concepts-read-replicas.md).