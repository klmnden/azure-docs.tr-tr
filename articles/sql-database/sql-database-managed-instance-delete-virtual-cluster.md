---
title: Bir alt ağ siliniyor Azure SQL veritabanı yönetilen örneği sonra Sil | Microsoft Docs
description: Yönetilen örnek bir Azure SQL veritabanını silme sonra bir Azure sanal ağı silmek öğrenin.
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
ms.openlocfilehash: ec5d99e160e739f59e2bf2ea369fe83e9900a1f1
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295310"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Bir alt ağ siliniyor Azure SQL veritabanı yönetilen örneği sonra Sil

Bu makalede, bir alt ağ siliniyor son Azure SQL veritabanı yönetilen örneği içinde bulunan sonra el ile silmeniz konusunda kılavuz bilgiler verilmektedir.

SQL veritabanı kullanan bir [sanal küme](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) silinen yönetilen örnek içerecek. Sanal küme 12 saat sonra yönetilen örnekleri aynı alt ağda hızlı bir şekilde oluşturmanızı sağlamak için örnek silme işlemi kalıcıdır. Boş bir sanal küme tutmak için ücret alınmaz. Bu süre boyunca sanal kümeyle ilişkili alt ağ silinemiyor.

12 saat bekleyin ve sanal küme ve onun alt kümesini hemen silmek tercih istemiyorsanız, bunu el ile yapabilirsiniz. Azure portalı veya API sanal kümeleri kullanarak sanal küme el ile silin.

> [!NOTE]
> Sanal küme silme başarılı olması için hiçbir yönetilen örnek içermelidir.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Azure portalından sanal küme silme

Azure portalını kullanarak bir sanal kümede silmek için sanal küme kaynakları arayın.

![Arama kutusu vurgulandığı Azure portal ekran görüntüsü](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Silmek istediğiniz sanal küme bulduktan sonra bu kaynak seçip **Sil**. Sanal küme silme işlemini onaylamanız istenir.

![Sanal Azure portalının ekran görüntüsü panoyu Sil seçeneğinin vurgulandığı kümeleri.](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

Azure portal bildirimleri alanında sanal küme silindikten sonra onay gösterilir. Sanal küme başarılı silme işlemi hemen yeniden kullanım için alt serbest bırakır.

## <a name="delete-virtual-cluster-by-using-the-api"></a>API'yi kullanarak sanal küme silme

Belirtilen URI parametreleri API'si üzerinden sanal bir kümesini silmek için kullanın [sanal kümeler metodu Sil](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Hakkında bilgi edinin [bağlantı mimarisi yönetilen örneğinde](sql-database-managed-instance-connectivity-architecture.md).
- Bilgi nasıl [yönetilen örneği için bir sanal ağınız değiştirme](sql-database-managed-instance-configure-vnet-subnet.md).
- Sanal ağ oluşturma için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz. [bir Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
