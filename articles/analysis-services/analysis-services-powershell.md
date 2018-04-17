---
title: Azure Analysis Services PowerShell ile yönetme | Microsoft Docs
description: PowerShell ile Azure Analysis Services yönetimi.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: reference
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c7315835bca446c4cae592f4bdd58a733b203655
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Azure Analysis Services PowerShell ile yönetme

Bu makalede, Azure Analysis Services sunucusu ve veritabanı yönetim görevlerini gerçekleştirmek için kullanılan PowerShell cmdlet'lerini açıklanmaktadır. 

Sunucu yönetimi görevlerini oluşturma veya bir sunucu silme, askıya almak veya sunucu işlemlerini sürdürme veya hizmet düzeyi (katman) değiştirme gibi Azure Resource Manager (AzureRM) cmdlet'lerini kullanın. Ekleme veya kaldırma Rol üyeleri gibi veritabanlarını yönetmeyle ilgili diğer görevler işleme veya bölümleme, SQL Server Analysis Services aynı SqlServer modülünde yer cmdlet'lerini kullanın.

## <a name="permissions"></a>İzinler
Çoğu PowerShell görevleri yönettiğiniz Analysis Services sunucusunda yönetici ayrıcalıkları gerektirir. Zamanlanmış PowerShell görevleri katılımsız işlemleridir. Zamanlayıcı'yı çalıştıran hesabı Çözümleme Hizmetleri sunucusunda yönetici ayrıcalıkları olmalıdır. 

AzureRm cmdlet'lerini kullanarak sunucu işlemleri için hesabınızı veya Zamanlayıcı çalıştıran hesabı da kaynağın sahibi rolüne ait olmalıdır [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md). 

## <a name="server-operations"></a>Sunucu işlemleri 
Azure Analysis Services cmdlet'leri dahil edilmiştir [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) Bileşen Modülü. AzureRM cmdlet modüllerini yüklemek için bkz: [Azure Resource Manager cmdlet'lerini](/powershell/azure/overview) PowerShell galerisinde.

|Cmdlet|Açıklama| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azurerm.analysisservices/add-azureanalysisservicesaccount)|Azure Analysis Services sunucu cmdlet istekleri için kullanılacak bir kimliği doğrulanmış hesabı ekler.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Bir sunucu örneği ayrıntılarını alır.|  
|[AzureRmAnalysisServicesServer yeni](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Bir sunucu örneği oluşturur.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Bir sunucu örneğini kaldırır.|  
|[Yeniden başlatma AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Analysis Services sunucu örneği şu anda oturum açmış ortamında yeniden başlatır; Add-AzureAnalysisServicesAccount komutunda belirtilen.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Bir sunucuyu sürdürür.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Bir sunucu örneği askıya alır.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Bir sunucu örneği değiştirir.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Bir sunucu örneğinin var olup olmadığını sınar.| 

## <a name="database-operations"></a>Veritabanı işlemleri

Azure Analysis Services veritabanı işlemlerini kullanmak aynı [SqlServer](https://www.powershellgallery.com/packages/SqlServer) modülü SQL Server Analysis Services olarak. Ancak, tüm cmdlet'leri Azure Analysis Services için desteklenir. 

SqlServer modülü görev özgü veritabanı yönetimi cmdlet'leri ve bunun yanı sıra tablolu modeli komut dosyası dili (TMSL) sorgu veya betik kabul genel amaçlı Invoke-ASCmd cmdlet'i sağlar. Aşağıdaki cmdlet SqlServer modülündeki Azure Analysis Services için desteklenir.

  
|Cmdlet|Açıklama|
|------------|-----------------| 
|[Ekleme RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Bir veritabanı rolüne üye ekleme.| 
|[Yedekleme ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Bir Analysis Services veritabanını yedekleyin.|  
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Üye veritabanı rolden kaldırır.|   
|[Çağırma ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|TMSL betiğini yürütün.|
|[Çağırma ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Bir veritabanı işlemi.|  
|[Çağırma ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|Bir bölüm işleme.| 
|[Çağırma ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Tablo işlem.|  
|[Bölüm birleştirme](https://msdn.microsoft.com/library/hh479576.aspx)|Bir bölümü birleştirin.|  
|[Geri yükleme ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Bir Analysis Services veritabanını geri yükleyin.| 
  

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell modülünü indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell galerisinde SqlServer Modülü](https://www.powershellgallery.com/packages/SqlServer)    
* [Tablo programlama modeli uyumluluk düzeyi 1200 ve daha yüksek](https://msdn.microsoft.com/library/mt712541.aspx)
