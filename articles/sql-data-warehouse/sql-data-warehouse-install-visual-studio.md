---
title: "SQL Data Warehouse için Visual Studio'yu ve SSDT'yi yükleme | Microsoft Docs"
description: "Azure SQL Data Warehouse için Visual Studio'yu ve SQL Server Veri Araçları'nı (SSDT) Yükleme"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 03/30/2017
ms.author: anvang;barbkess
ms.openlocfilehash: f7023b78c241a7bc8014276cd0bfa455165b42cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>SQL Data Warehouse için Visual Studio ve SSDT yükleme
SQL Data Warehouse için uygulama geliştirmek için en son sürümünü Visual Studio ile SQL Server veri Araçları (SSDT) en son sürümünü kullanmanızı öneririz.  Geriye dönük uyumluluk için SSDT ile Visual Studio 2013 Güncelleştirme 5 de desteklenir.  

Visual Studio'yu SSDT ile kullanmak; sorgu çalıştırmanın yanı sıra, SQL Server Nesne Gezgini'ni kullanarak SQL Data Warehouse'unuzda bulunan tabloları, görünümleri, saklı yordamları ve daha birçok nesneyi görsel olarak araştırmanıza olanak sağlar.

> [!NOTE]
> SQL Data Warehouse, henüz Visual Studio Veritabanı Projelerini desteklemiyor.  Bu özellik sonraki bir sürümde eklenecek.
> 
> 

## <a name="step-1-install-visual-studio"></a>1. adım: Visual Studio yükleme
Karşıdan yükleyip Visual Studio'yu yüklemek için aşağıdaki bağlantıları izleyin. Visual Studio 2013 zaten varsa veya sonraki bir sürümü yüklüyse, adım 2'ye atlayabilirsiniz SSDT yükleyin.

1. [Visual Studio indirme][].
2. İzleyin [Visual Studio'yu yükleme] [ Installing Visual Studio] Kılavuzu MSDN'de ve varsayılan yapılandırmaları seçin.

## <a name="step-2-install-ssdt"></a>2. Adım: SSDT'yi yükleme
Visual Studio için SSDT yüklemek üzere aşağıdaki adımları izleyerek Visual Studio'da bir SSDT güncelleştirmesinin olup olmadığını kontrol edin.

1. Visual Studio tıklayın **Araçları** / **Uzantılar ve güncelleştirmeler...** / **Güncelleştirmeleri**
2. **Ürün Güncelleştirmeleri**'ni seçip **Veritabanı araçları için Microsoft SQL Server Güncelleştirmesi** olup olmadığına bakın.

Güncelleştirme yoksa en son sürümün yüklü olduğu anlamına gelir.  SSDT'nin yüklendiğini doğrulamak **Yardım** / **Microsoft Visual Studio Hakkında**'ya tıklayın ve SQL Server Veri Araçları'nın listede olup olmadığına bakın.  14.0.60525.0 SSDT'nin en son sürümüdür.  Yükleme seçeneğini Visual Studio'dan kullanılabilir durumda değilse, ziyaret edebilirsiniz [SSDT indirme] [ SSDT Download] indirmek ve SSDT'yi el ile yüklemek için sayfa.

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
