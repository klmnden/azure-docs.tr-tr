---
title: Silme Azure SQL veritabanı yönetilen örneği sonra VNet delete | Microsoft Docs
description: Sanal ağ siliniyor Azure SQL veritabanı yönetilen örneği sonra silmeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: management
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
manager: craigg
ms.date: 05/07/2019
ms.openlocfilehash: 95d1681c9ff9981990d873a58a2d01833d690e0f
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411994"
---
# <a name="delete-subnet-after-deleting-azure-sql-database-managed-instance"></a>Alt ağ siliniyor Azure SQL veritabanı yönetilen örneği sonra Sil

Bu makalede, alt ağ siliniyor son Azure SQL veritabanı yönetilen örneği içinde bulunan sonra el ile silmeniz konusunda kılavuz bilgiler verilmektedir.

[Sanal küme](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) silinen içermiştir yönetilen örnek saklanır 12 saat boyunca örneği silme. Sanal küme, aynı alt ağdaki yönetilen örnekleri daha hızlı oluşturulmasını etkinleştirmek için tasarım canlı olarak tutulur. Bu süre boyunca sanal kümeyle ilişkili alt ağ silinemiyor.

Boş bir sanal küme tarafından kullanılan alt ağın yayınlanmak el ile silinmesini sanal küme ile mümkündür. Sanal küme silme işlemi, Azure portal veya sanal Küme API ile gerçekleştirilebilir.

> [!NOTE]
> Sanal küme silme başarılı olması için hiçbir yönetilen örnek içermelidir.

## <a name="delete-virtual-cluster-from-azure-portal"></a>Azure portalından sanal küme silme

Azure portalını kullanarak sanal bir kümeyi silmek için yerleşik arama'yı kullanarak sanal küme kaynakları arayın.

![Sanal küme arayın.](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Silmek istediğiniz sanal küme bulduktan sonra bu kaynağı seçin ve Sil seçeneği seçin. Sanal küme silme işlemini onaylamanız istenir.

![Sanal küme silin.](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

Onay sanal küme silindikten sonra Azure portal bildirimleri sağlanır. Sanal küme başarıyla silinmesini, alt ağ için daha fazla yeniden hemen serbest bırakır.

## <a name="delete-virtual-cluster-using-api"></a>API'sini kullanarak sanal küme silme

Silmek için bir sanal Küme API kullanarak belirtilen URI parametreleri [sanal kümeler metodu Sil](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Hakkında bilgi edinin [bağlantı mimarisi yönetilen örneğinde](sql-database-managed-instance-connectivity-architecture.md).
- Bilgi nasıl [yönetilen örneği için bir sanal ağınız değiştirme](sql-database-managed-instance-configure-vnet-subnet.md).
- Sanal ağ oluşturma için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz. [bir Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
