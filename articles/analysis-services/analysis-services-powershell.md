---
title: Azure Analysis Services PowerShell ile yönetme | Microsoft Docs
description: PowerShell ile Azure Analysis Services yönetimi.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 1f9c30f1c914f6c8d42967e014d967ba0d5b85cc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58893852"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure Analysis Services PowerShell ile yönetme

Bu makalede, Azure Analysis Services sunucusu ve veritabanı yönetim görevlerini gerçekleştirmek için kullanılan PowerShell cmdlet'lerini açıklanır. 

Oluşturma veya sunucu silme, askıya alma veya sürdürme sunucu işlemleri veya hizmet düzeyi (katman) değiştirme gibi sunucu yönetimi görevleri, Azure Resource Manager (kaynak) cmdlet'leri ve Analysis Services (sunucu) cmdlet'lerini kullanın. Veritabanını yönetme ile ilgili diğer görevler, ekleme veya Rol üyeleri, işleme veya dahil edilen SQL Server Analysis Services ile aynı SqlServer modülündeki cmdlet'ler bölümleme gibi.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>İzinler

Çoğu PowerShell görevleri, yönettiğiniz Analysis Services sunucusunda yönetici ayrıcalıkları gerektirir. Zamanlanmış PowerShell görevleri katılımsız işlemlerdir. Hesap veya hizmet sorumlusu Zamanlayıcı tarafından çalıştırılan Analysis Services sunucusunda yönetici ayrıcalıkları olmalıdır. 

Azure PowerShell cmdlet'lerini kullanarak sunucu işlemleri için hesabınızı veya Zamanlayıcı çalıştıran hesabı da kaynak sahibi rolüne ait olmalıdır [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Kaynak yönetimi işlemleri 

Modül - [Az.AnalysisServices](/powershell/module/az.analysisservices)

|Cmdlet|Açıklama| 
|------------|-----------------| 
|[Get-AzAnalysisServicesServer](/powershell/module/az.analysisservices/get-azanalysisservicesserver)|Sunucu örneğinin ayrıntılarını alır.|  
|[New-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver)|Bir sunucu örneği oluşturur.|   
|[Yeni AzAnalysisServicesFirewallConfig](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallconfig)|Yeni bir Analysis Services güvenlik duvarı yapılandırması oluşturur.|   
|[Yeni AzAnalysisServicesFirewallRule](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallrule)|Yeni bir Analysis Services güvenlik duvarı kuralı oluşturur.|   
|[Remove-AzAnalysisServicesServer](/powershell/module/az.analysisservices/remove-azanalysisservicesserver)|Bir sunucu örneğini kaldırır.|  
|[Resume-AzAnalysisServicesServer](/powershell/module/az.analysisservices/resume-azanalysisservicesserver)|Bir sunucu örneğini sürdürür.|  
|[Suspend-AzAnalysisServicesServer](/powershell/module/az.analysisservices/suspend-azanalysisservicesserver)|Bir sunucu örneği askıya alır.| 
|[Set-AzAnalysisServicesServer](/powershell/module/az.analysisservices/set-azanalysisservicesserver)|Bir sunucu örneği değiştirir.|   
|[Test-AzAnalysisServicesServer](/powershell/module/az.analysisservices/test-azanalysisservicesserver)|Sunucu örneğinin varlığını test eder.| 

## <a name="server-management-operations"></a>Sunucu Yönetimi işlemleri

Modül - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Açıklama| 
|------------|-----------------| 
|[Add-AzAnalysisServicesAccount](/powershell/module/az.analysisservices/add-AzAnalysisServicesaccount)|Azure Analysis Services sunucu cmdlet'i istekleri için kullanılacak bir yetkili hesabı ekler.| 
|[Dışarı aktarma AzAnalysisServicesInstance](/powershell/module/az.analysisservices/export-AzAnalysisServicesinstancelog)|Analysis Services sunucusu şu anda oturum açmış ortam AzAnalysisServicesAccount Ekle komutunda belirtilen örneğinden günlüğünü dışarı aktarır|  
|[Yeniden başlatma AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|Analysis Services sunucu örneği şu anda oturum açmış ortamında yeniden başlatılır; Add-AzAnalysisServicesAccount komutunda belirtilen.|  
|[Eşitleme AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|Belirtilen bir Analysis Services sunucusu şu anda oturum açmış ortam AzAnalysisServicesAccount Ekle komutunda belirtilen konumdaki tüm sorgu genişleme örnekleri için belirtilen örneği veritabanında eşitler|  

## <a name="database-operations"></a>Veritabanı işlemleri

Azure Analysis Services veritabanı işlemleri kullanmak aynı [SqlServer modülündeki](https://www.powershellgallery.com/packages/SqlServer) olarak SQL Server Analysis Services. Ancak, tüm cmdlet'leri, Azure Analysis Services için desteklenir. Daha fazla bilgi için bkz. [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

SqlServer modülündeki bir Tablosal Model betik dili (TMSL) sorgu veya betik kabul eden genel amaçlı Invoke-ASCmd cmdlet yanı sıra görev özgü veritabanı yönetimi cmdlet'leri sağlar. Aşağıdaki SqlServer modülündeki cmdlet'ler, Azure Analysis Services için desteklenir.

  
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

* [SQL Server PowerShell modülünü indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS'yi indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell galerisinde SqlServer modülündeki](https://www.powershellgallery.com/packages/SqlServer)    
* [Tablosal Model programlama uyumluluk düzeyi 1200 ve üzeri](/sql/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)
