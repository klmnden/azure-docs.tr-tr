---
title: Azure Analysis Services PowerShell ile yönetme | Microsoft Docs
description: PowerShell ile Azure Analysis Services yönetimi.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 07/01/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a958c33e173c881a3ad09a49fe9f71ddb0c9df56
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508951"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure Analysis Services PowerShell ile yönetme

Bu makalede, Azure Analysis Services sunucusu ve veritabanı yönetim görevlerini gerçekleştirmek için kullanılan PowerShell cmdlet'lerini açıklanır. 

Oluşturma veya sunucu silme, askıya alma veya sürdürme sunucu işlemleri veya hizmet düzeyi (katman) değiştirme gibi sunucu kaynak yönetimi görevlerinde Azure Analysis Services cmdlet'lerini kullanın. Veritabanını yönetme ile ilgili diğer görevler, ekleme veya Rol üyeleri, işleme veya dahil edilen SQL Server Analysis Services ile aynı SqlServer modülündeki cmdlet'ler bölümleme gibi.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>İzinler

Çoğu PowerShell görevleri, yönettiğiniz Analysis Services sunucusunda yönetici ayrıcalıkları gerektirir. Zamanlanmış PowerShell görevleri katılımsız işlemlerdir. Hesap veya hizmet sorumlusu Zamanlayıcı tarafından çalıştırılan Analysis Services sunucusunda yönetici ayrıcalıkları olmalıdır. 

Azure PowerShell cmdlet'lerini kullanarak sunucu işlemleri için hesabınızı veya Zamanlayıcı çalıştıran hesabı da kaynak sahibi rolüne ait olmalıdır [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-and-server-operations"></a>Kaynak ve sunucu işlemleri 

Modül - yükleme [Az.AnalysisServices](https://www.powershellgallery.com/packages/Az.AnalysisServices)   
Belgeleri - [Az.AnalysisServices başvurusu](/powershell/module/az.analysisservices)

## <a name="database-operations"></a>Veritabanı işlemleri

Azure Analysis Services veritabanı işlemleri aynı SqlServer modülündeki SQL Server Analysis Services'i kullanın. Ancak, tüm cmdlet'leri, Azure Analysis Services için desteklenir. 

SqlServer modülündeki bir Tablosal Model betik dili (TMSL) sorgu veya betik kabul eden genel amaçlı Invoke-ASCmd cmdlet yanı sıra görev özgü veritabanı yönetimi cmdlet'leri sağlar. Aşağıdaki SqlServer modülündeki cmdlet'ler, Azure Analysis Services için desteklenir.

Modül - yükleme [SqlServer](https://www.powershellgallery.com/packages/SqlServer)   
Belgeleri - [SqlServer başvurusu](/powershell/module/sqlserver)

### <a name="supported-cmdlets"></a>Desteklenen cmdlet'leri

|Cmdlet|Açıklama|
|------------|-----------------| 
|[RoleMember ekleyin](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Üye veritabanı rolüne ekleyin.| 
|[Yedekleme ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Bir Analysis Services veritabanını yedekleyin.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Üye veritabanı rolden kaldırma.|   
|[Çağırma ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|TMSL betiğini yürütün.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Bir veritabanı işlemi.|  
|[Çağırma ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Bir bölüm işleyin.| 
|[Çağırma ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|İşlem bir tablo.|  
|[Bölüm birleştirme](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Bir bölümü birleştirin.|  
|[Geri yükleme-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Bir Analysis Services veritabanını geri yükleyin.| 
  

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell)      
* [SQL Server PowerShell modülünü indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS'yi indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell galerisinde SqlServer modülündeki](https://www.powershellgallery.com/packages/SqlServer)    
* [Tablosal Model programlama uyumluluk düzeyi 1200 ve üzeri](/sql/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)
