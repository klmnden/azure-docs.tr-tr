---
title: Oluşturma ve MySQL için Azure veritabanı'nda salt okunur çoğaltmalar yönetme
description: Bu makalede, ayarlama ve Azure CLI kullanarak MySQL için Azure veritabanı'nda salt okunur çoğaltmalar yönetmek açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: a9ca34953827c1f94e2696eb4f09163be335d2f4
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510693"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-cli"></a>Nasıl oluşturmak ve yönetmek, Azure CLI kullanarak MySQL için Azure veritabanı çoğaltmalarını okuyun

Bu makalede, oluşturmak ve yönetmek için aynı Azure bölgesindeki Azure CLI kullanarak MySQL hizmeti için Azure veritabanı yöneticisi olarak okundu çoğaltmaları öğreneceksiniz.

> [!NOTE]
> Azure CLI'yı henüz oluştururken çoğaltmaları ana sunucudan farklı bir bölgede desteklemez. Bölgeler arası çoğaltma oluşturmak için kullanın [Azure portalında]( howto-read-replicas-portal.md) yerine.

## <a name="prerequisites"></a>Önkoşullar

- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Bir [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md) ana sunucu olarak kullanılır. 

> [!IMPORTANT]
> Salt okunur çoğaltma özelliği yalnızca için Azure veritabanı genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanları MySQL sunucuları için kullanılabilir. Bu fiyatlandırma katmanlarından birini ana sunucusu olduğundan emin olun.

## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma

Salt okunur çoğaltma sunucusu, aşağıdaki komutu kullanarak oluşturulabilir:

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

`az mysql server replica create` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusu oluşturulacağı kaynak grubu.  |
| ad | mydemoreplicaserver | Oluşturulan yeni çoğaltma sunucusunun adı. |
| source-server | mydemoserver | Adı veya çoğaltma kaynağı için mevcut ana sunucu kimliği. |

> [!NOTE]
> Okuma çoğaltmaları aynı sunucu yapılandırma yöneticisi olarak oluşturulur. Çoğaltma sunucusu yapılandırması, oluşturulduktan sonra değiştirilebilir. Çoğaltma sunucusunun yapılandırmasını çoğaltma ana ayak olduğundan emin olmak için ana daha eşit veya daha fazla değerlerinde tutulması gereken önerilir.

## <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma

> [!IMPORTANT]
> Bir sunucuya çoğaltma durdurma işlemi geri alınamaz. Bir ana ve çoğaltma arasında çoğaltmayı durdurdu sonra geri alınamaz. Çoğaltma sunucusu bir tek başına sunucu olur ve artık hem okuma hem de yazma işlemleri destekler. Bu sunucu bir yinelemeye yeniden yapılamıyor.

Aşağıdaki komutu kullanarak bir salt okunur çoğaltma sunucusuna çoğaltma durdurulabilir:

```azurecli-interactive
az mysql server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

`az mysql server replica stop` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusunun mevcut olduğu kaynak grubu.  |
| ad | mydemoreplicaserver | Üzerindeki çoğaltma durdurma çoğaltma sunucusunun adı. |

## <a name="delete-a-replica-server"></a>Çoğaltma sunucusunu Sil

Salt okunur çoğaltma sunucusu silme yapılabilir çalıştırarak **[az mysql server delete](/cli/azure/mysql/server)** komutu.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

## <a name="delete-a-master-server"></a>Bir ana sunucu silme

> [!IMPORTANT]
> Ana sunucu silme tüm çoğaltma sunucuları için çoğaltma durdurulur ve ana sunucusunu siler. Artık hem okuma hem de yazma işlemleri destekleyen tek başına sunucular çoğaltma sunucusu olur.

Ana sunucu silmek için çalıştırabileceğiniz **[az mysql server delete](/cli/azure/mysql/server)** komutu.

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="list-replicas-for-a-master-server"></a>Ana sunucu için liste çoğaltmaları

Verilen bir ana sunucu için tüm çoğaltmaları görüntülemek için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az mysql server replica list --server-name mydemoserver --resource-group myresourcegroup
```

`az mysql server replica list` Komut takip eden parametreleri gerektiriyor:

| Ayar | Örnek değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Çoğaltma sunucusu oluşturulacağı kaynak grubu.  |
| server-name | mydemoserver | Adı veya ana sunucu kimliği. |

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [çoğaltmaları okuyun](concepts-read-replicas.md)
