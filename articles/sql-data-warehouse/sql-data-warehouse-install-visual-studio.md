---
title: SQL Data Warehouse için Visual Studio'yu ve SSDT'yi yükleme | Microsoft Docs
description: Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: a2f01424dedb977000d0e4150f4a31c1a9a21cfb
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>SQL Data Warehouse için Visual Studio ve SSDT yükleme
SQL Data Warehouse için uygulama geliştirmek için en son sürümünü Visual Studio ile SQL Server veri Araçları (SSDT) en son sürümünü kullanmanızı öneririz.  Geriye dönük uyumluluk için SSDT ile Visual Studio 2013 Güncelleştirme 5 de desteklenir.  

SSDT ile Visual Studio kullanarak görsel olarak tabloları, görünümleri, saklı yordamları ve daha birçok nesneyi SQL veri ambarı keşfedin yanı sıra sorguları çalıştırmak için SQL Server Nesne Gezgini kullanmanıza olanak sağlar.

> [!NOTE]
> SQL Data Warehouse, henüz Visual Studio Veritabanı Projelerini desteklemiyor. Bu özellikle ilgili düzenli güncelleştirmeleri almak için lütfen oy [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>1. adım: Visual Studio yükleme
Karşıdan yükleyip Visual Studio'yu yüklemek için aşağıdaki bağlantıları izleyin. Visual Studio 2013 zaten varsa veya sonraki bir sürümü yüklüyse, adım 2'ye atlayabilirsiniz SSDT yükleyin.

1. [Visual Studio indirme][].
2. İzleyin [Visual Studio'yu yükleme] [ Installing Visual Studio] Kılavuzu MSDN'de ve varsayılan yapılandırmaları seçin.

## <a name="step-2-install-ssdt"></a>2. Adım: SSDT'yi yükleme
Visual Studio için SSDT yüklemek için önce Visual Studio içinde bir SSDT güncelleştirmesinin için aşağıdaki adımları izleyerek denetleyin.

1. Visual Studio tıklayın **Araçları** / **Uzantılar ve güncelleştirmeler...** / **Güncelleştirmeleri**
2. **Ürün Güncelleştirmeleri**'ni seçip **Veritabanı araçları için Microsoft SQL Server Güncelleştirmesi** olup olmadığına bakın.

Güncelleştirme yoksa en son sürümün yüklü olduğu anlamına gelir.  SSDT'nin yüklendiğini doğrulamak **Yardım** / **Microsoft Visual Studio Hakkında**'ya tıklayın ve SQL Server Veri Araçları'nın listede olup olmadığına bakın. 14.0.60525.0 SSDT'nin en son sürümüdür. Yükleme seçeneğini Visual Studio'dan kullanılabilir durumda değilse, ziyaret edebilirsiniz [SSDT indirme] [ SSDT Download] indirmek ve SSDT'yi el ile yüklemek için sayfa.

## <a name="next-steps"></a>Sonraki adımlar
SSDT'nin en son sürümüne sahip olduğunuza göre hazır [bağlanmak] [ connect] SQL veri ambarı için.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio indirme]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
