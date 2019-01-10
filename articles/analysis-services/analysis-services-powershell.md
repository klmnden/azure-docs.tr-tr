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
ms.openlocfilehash: 9e7683883963db2cf1911405225fcdbf289de2bb
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54187549"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure Analysis Services PowerShell ile yönetme

Bu makalede, Azure Analysis Services sunucusu ve veritabanı yönetim görevlerini gerçekleştirmek için kullanılan PowerShell cmdlet'lerini açıklanır. 

Oluşturma veya sunucu silme, askıya alma veya sürdürme sunucu işlemleri veya hizmet düzeyi (katman) değiştirme gibi sunucu yönetimi görevleri, Azure Resource Manager (kaynak) cmdlet'leri ve Analysis Services (sunucu) cmdlet'lerini kullanın. Veritabanını yönetme ile ilgili diğer görevler, ekleme veya Rol üyeleri, işleme veya dahil edilen SQL Server Analysis Services ile aynı SqlServer modülündeki cmdlet'ler bölümleme gibi.

## <a name="permissions"></a>İzinler

Çoğu PowerShell görevleri, yönettiğiniz Analysis Services sunucusunda yönetici ayrıcalıkları gerektirir. Zamanlanmış PowerShell görevleri katılımsız işlemlerdir. Hesap veya hizmet sorumlusu Zamanlayıcı tarafından çalıştırılan Analysis Services sunucusunda yönetici ayrıcalıkları olmalıdır. 

AzureRm cmdlet'leri kullanarak sunucu işlemleri için hesabınızı veya Zamanlayıcı çalıştıran hesabı da kaynak sahibi rolüne ait olmalıdır [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Kaynak yönetimi işlemleri 

Modül - [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)

|Cmdlet|Açıklama| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Sunucu örneğinin ayrıntılarını alır.|  
|[Yeni-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Bir sunucu örneği oluşturur.|   
|[Yeni AzureRmAnalysisServicesFirewallConfig](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallconfig)|Yeni bir Analysis Services güvenlik duvarı yapılandırması oluşturur.|   
|[Yeni AzureRmAnalysisServicesFirewallRule](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallrule)|Yeni bir Analysis Services güvenlik duvarı kuralı oluşturur.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Bir sunucu örneğini kaldırır.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Bir sunucu örneğini sürdürür.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Bir sunucu örneği askıya alır.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Bir sunucu örneği değiştirir.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Sunucu örneğinin varlığını test eder.| 

## <a name="server-management-operations"></a>Sunucu Yönetimi işlemleri

Modül - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Açıklama| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azure.analysisservices/add-azureanalysisservicesaccount)|Azure Analysis Services sunucu cmdlet'i istekleri için kullanılacak bir yetkili hesabı ekler.| 
|[Dışarı aktarma AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Analysis Services sunucusu şu anda oturum açmış ortam Add-AzureAnalysisServicesAccount komutunda belirtilen örneğinden günlüğünü dışarı aktarır|  
|[Yeniden başlatma AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Analysis Services sunucu örneği şu anda oturum açmış ortamında yeniden başlatılır; Add-AzureAnalysisServicesAccount komutunda belirtilen.|  
|[Eşitleme AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Belirtilen bir Analysis Services sunucusu şu anda oturum açmış ortam Add-AzureAnalysisServicesAccount komut belirtildiği gibi tüm sorgu genişleme örnekleri belirtilen örneği veritabanında eşitler|  

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
* [Tablosal Model programlama uyumluluk düzeyi 1200 ve üzeri](https://msdn.microsoft.com/library/mt712541.aspx)
