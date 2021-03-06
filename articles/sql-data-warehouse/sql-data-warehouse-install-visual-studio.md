---
title: SQL Data Warehouse için Visual Studio ve SSDT'yi yükleme | Microsoft Docs
description: Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/05/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: b2e34f1f72b1b0aa76d4a3031102d052118dae5f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66304125"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>SQL Data Warehouse için Visual Studio ve SSDT yükleme
SQL veri ambarı için uygulama geliştirmek üzere Visual Studio 2019'ı kullanın. Şu anda Visual Studio 2019 SSDT, SQL veri ambarı için desteklenmiyor. 

SSDT ile Visual Studio kullanarak tablolar, görünümler, saklı yordamlar ve daha birçok nesneyi görsel olarak, SQL veri ambarı'nda keşfetmek için SQL Server Nesne Gezgini kullanmanıza olanak tanır. Ayrıca sorguları çalıştırmanıza olanak sağlar.

> [!NOTE]
> SQL Data Warehouse, henüz Visual Studio Veritabanı Projelerini desteklemiyor. Bu özelliğe ilişkin düzenli güncelleştirmeler almak için lütfen üzerinden oy [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>1\. adım: Visual Studio yükleme
Karşıdan yükleyip Visual Studio'yu yüklemek için aşağıdaki bağlantıları izleyin. Visual Studio 2013 zaten var veya sonraki bir sürümü yüklüyse, adım 2'ye atlayabilirsiniz SSDT'yi yükleme.

1. [Visual Studio'yu indirin][].
2. İzleyin [Visual Studio'yu] [ Installing Visual Studio] Kılavuzu MSDN'de ve varsayılan yapılandırmaları seçin.

## <a name="step-2-install-ssdt"></a>2\. adım: SSDT'yi yükleme
Visual Studio için SSDT yüklemek için önce Visual Studio'da bir SSDT güncelleştirmesinin aşağıdaki adımları izleyerek denetleyin.

1. Visual Studio'da tıklayın **Araçları** / **Uzantılar ve güncelleştirmeler...** / **Güncelleştirmeleri**
2. **Ürün Güncelleştirmeleri**'ni seçip **Veritabanı araçları için Microsoft SQL Server Güncelleştirmesi** olup olmadığına bakın.

Bir güncelleştirme bulunamazsa yüklü en son sürümü olması gerekir. SSDT'nin yüklendiğini doğrulamak **Yardım** / **Microsoft Visual Studio Hakkında**'ya tıklayın ve SQL Server Veri Araçları'nın listede olup olmadığına bakın. Visual Studio'da yükleme seçeneği kullanılamıyorsa, ziyaret edebilirsiniz [SSDT indirme] [ SSDT Download] sayfa indirin ve SSDT'yi el ile yükleyin.

## <a name="next-steps"></a>Sonraki adımlar
Artık SSDT'nin en son sürümüne sahip olduğunuza göre şunları yapmaya hazırsınız [bağlanma] [ connect] SQL veri ambarınıza.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio'yu indirin]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
