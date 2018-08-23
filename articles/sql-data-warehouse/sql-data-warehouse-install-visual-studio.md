---
title: SQL Data Warehouse için Visual Studio ve SSDT'yi yükleme | Microsoft Docs
description: Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 11e4e6e887f7c182c1c321d8837e11f07afc2f6b
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444734"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>SQL Data Warehouse için Visual Studio ve SSDT yükleme
SQL veri ambarı için uygulama geliştirmek üzere Visual Studio'nun en son sürümü ile SQL Server veri Araçları (SSDT) en son sürümü kullanmanızı öneririz.  Geriye dönük uyumluluk için SSDT ile Visual Studio 2013 Güncelleştirme 5 de desteklenir.  

SSDT ile Visual Studio kullanarak görsel olarak tablolar, görünümler, saklı yordamlar ve daha birçok nesneyi, SQL veri ambarı'nda keşfedin yanı sıra sorguları çalıştırmak için SQL Server Nesne Gezgini kullanmanıza olanak tanır.

> [!NOTE]
> SQL Data Warehouse, henüz Visual Studio Veritabanı Projelerini desteklemiyor. Bu özelliğe ilişkin düzenli güncelleştirmeler almak için lütfen üzerinden oy [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>1. adım: Visual Studio yükleme
Karşıdan yükleyip Visual Studio'yu yüklemek için aşağıdaki bağlantıları izleyin. Visual Studio 2013 zaten var veya sonraki bir sürümü yüklüyse, adım 2'ye atlayabilirsiniz SSDT'yi yükleme.

1. [Visual Studio'yu indirin][].
2. İzleyin [Visual Studio'yu] [ Installing Visual Studio] Kılavuzu MSDN'de ve varsayılan yapılandırmaları seçin.

## <a name="step-2-install-ssdt"></a>2. Adım: SSDT'yi yükleme
Visual Studio için SSDT yüklemek için önce Visual Studio'da bir SSDT güncelleştirmesinin aşağıdaki adımları izleyerek denetleyin.

1. Visual Studio'da tıklayın **Araçları** / **Uzantılar ve güncelleştirmeler...** / **Güncelleştirmeleri**
2. **Ürün Güncelleştirmeleri**'ni seçip **Veritabanı araçları için Microsoft SQL Server Güncelleştirmesi** olup olmadığına bakın.

Güncelleştirme yoksa en son sürümün yüklü olduğu anlamına gelir.  SSDT'nin yüklendiğini doğrulamak **Yardım** / **Microsoft Visual Studio Hakkında**'ya tıklayın ve SQL Server Veri Araçları'nın listede olup olmadığına bakın. 14.0.60525.0 SSDT'nin en son sürümüdür. Visual Studio'da yükleme seçeneği kullanılamıyorsa, ziyaret edebilirsiniz [SSDT indirme] [ SSDT Download] sayfa indirin ve SSDT'yi el ile yükleyin.

## <a name="next-steps"></a>Sonraki adımlar
SSDT'nin en son sürümüne sahip olduğunuza göre hazır olduğunuz [bağlanma] [ connect] SQL veri ambarınıza.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio'yu indirin]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
