---
title: Azure Analysis Services PowerShell ile yönetme | Microsoft Docs
description: PowerShell ile Azure Analysis Services yönetimi.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 06/25/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5c347a024af385e04bfdf3631ddcbaec89df4f40
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937373"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure Analysis Services PowerShell ile yönetme

Bu makalede, Azure Analysis Services sunucusu ve veritabanı yönetim görevlerini gerçekleştirmek için kullanılan PowerShell cmdlet'lerini açıklanmaktadır. 

Sunucu yönetimi görevlerini oluşturma veya bir sunucu silme, askıya almak veya sunucu işlemlerini sürdürme veya hizmet düzeyi (katman) değiştirme gibi Azure Resource Manager (kaynak) cmdlet'lerini ve Analysis Services (sunucu) cmdlet'lerini kullanın. Ekleme veya kaldırma Rol üyeleri gibi veritabanlarını yönetmeyle ilgili diğer görevler işleme veya bölümleme, SQL Server Analysis Services aynı SqlServer modülünde yer cmdlet'lerini kullanın.

## <a name="permissions"></a>İzinler
Çoğu PowerShell görevleri yönettiğiniz Analysis Services sunucusunda yönetici ayrıcalıkları gerektirir. Zamanlanmış PowerShell görevleri katılımsız işlemleridir. Zamanlayıcı'yı çalıştıran hesabı ya da hizmet ilkesi Çözümleme Hizmetleri sunucusunda yönetici ayrıcalıkları olmalıdır. 

AzureRm cmdlet'lerini kullanarak sunucu işlemleri için hesabınızı veya Zamanlayıcı çalıştıran hesabı da kaynağın sahibi rolüne ait olmalıdır [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Kaynak yönetimi işlemleri 
Modül - [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)

|Cmdlet|Açıklama| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Bir sunucu örneği ayrıntılarını alır.|  
|[AzureRmAnalysisServicesServer yeni](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Bir sunucu örneği oluşturur.|   
|[AzureRmAnalysisServicesFirewallConfig yeni](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallconfig)|Yeni bir Analysis Services Güvenlik Duvarı yapılandırma oluşturur.|   
|[AzureRmAnalysisServicesFirewallRule yeni](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallrule)|Yeni bir Analysis Services güvenlik duvarı kuralı oluşturur.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Bir sunucu örneğini kaldırır.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Bir sunucuyu sürdürür.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Bir sunucu örneği askıya alır.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Bir sunucu örneği değiştirir.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Bir sunucu örneğinin var olup olmadığını sınar.| 

## <a name="server-management-operations"></a>Sunucu Yönetimi işlemleri

Modül - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|Açıklama| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azure.analysisservices/add-azureanalysisservicesaccount)|Azure Analysis Services sunucu cmdlet istekleri için kullanılacak bir kimliği doğrulanmış hesabı ekler.| 
|[Dışarı aktarma AzureAnalysisServicesInstance]()|Şu anda oturum açmış ortamında Ekle AzureAnalysisServicesAccount komutu belirtildiği gibi Analysis Services sunucu örneğinden bir günlük verir|  
|[Yeniden başlatma AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Analysis Services sunucu örneği şu anda oturum açmış ortamında yeniden başlatır; Add-AzureAnalysisServicesAccount komutunda belirtilen.|  
|[Eşitleme AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Belirtilen tüm sorgu genişletme örnekleri şu anda oturum açmış ortamında Ekle AzureAnalysisServicesAccount komutunda belirtilen Analysis Services server örneğinin belirtilen bir veritabanında eşitler|  

## <a name="database-operations"></a>Veritabanı işlemleri

Azure Analysis Services veritabanı işlemlerini kullanmak aynı [SqlServer Modülü](https://www.powershellgallery.com/packages/SqlServer) SQL Server Analysis Services olarak. Ancak, tüm cmdlet'leri Azure Analysis Services için desteklenir. Daha fazla bkz öğrenmek için [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

SqlServer modülü görev özgü veritabanı yönetimi cmdlet'leri ve bunun yanı sıra tablolu modeli komut dosyası dili (TMSL) sorgu veya betik kabul genel amaçlı Invoke-ASCmd cmdlet'i sağlar. Aşağıdaki cmdlet SqlServer modülündeki Azure Analysis Services için desteklenir.

  
|Cmdlet|Açıklama|
|------------|-----------------| 
|[Ekleme RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Bir veritabanı rolüne üye ekleme.| 
|[Yedekleme ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Bir Analysis Services veritabanını yedekleyin.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Üye veritabanı rolden kaldırır.|   
|[Çağırma ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|TMSL betiğini yürütün.|
|[Çağırma ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Bir veritabanı işlemi.|  
|[Çağırma ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Bir bölüm işleme.| 
|[Çağırma ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Tablo işlem.|  
|[Bölüm birleştirme](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Bir bölümü birleştirin.|  
|[Geri yükleme ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Bir Analysis Services veritabanını geri yükleyin.| 
  

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell modülünü indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell galerisinde SqlServer Modülü](https://www.powershellgallery.com/packages/SqlServer)    
* [Tablo programlama modeli uyumluluk düzeyi 1200 ve daha yüksek](https://msdn.microsoft.com/library/mt712541.aspx)
