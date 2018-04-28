---
title: Günlük analizi ile Azure PaaS kaynak ölçümleri toplamak | Microsoft Docs
description: Bekletme ve günlük analizi analizi için PowerShell kullanarak Azure PaaS kaynak ölçümleri koleksiyonunun nasıl etkinleştirileceği öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: magoedte
ms.openlocfilehash: 8a2c04c2f79f310b7e70e7add7a8d5f318f056d2
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="configure-collection-of-azure-paas-resource-metrics-with-log-analytics"></a>Günlük analizi ile Azure PaaS kaynak ölçümleri koleksiyonunu yapılandırma

Azure SQL ve Web siteleri (Web uygulamaları) gibi bir hizmet (PaaS) kaynaklar olarak Azure platformu performans ölçümleri verilerini yerel olarak günlük analizi yayma. Bu komut dosyası, kullanıcıların belirli bir kaynak grubunun veya aboneliğin tümü arasında dağıtılmış PaaS kaynaklar için günlüğü ölçümlerini etkinleştirmeniz olanak tanır. 

Bugün, ölçümleri kaynakları Azure portalı üzerinden PaaS için günlüğü etkinleştirmek için bir yolu yoktur. Bu nedenle, bir PowerShell betiğini kullanmanız gerekir. Günlük analizi izleme, yanı sıra bu yerel ölçümleri günlüğe kaydetme özelliğine ölçekte Azure kaynaklarını izlemenize olanak tanır. 

## <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce bilgisayarınızda yüklü aşağıdaki Azure Resource Manager modüllerini doğrulayın:

- AzureRM.Insights
- AzureRM.OperationalInsights
- AzureRM.Resources
- AzureRM.profile

>[!NOTE]
>Tüm Azure Resource Manager modüllerini Powershell'den Azure Resource Manager komutları çalıştırdığınızda uyumluluğundan emin olmak için aynı sürüme sahip öneririz.
>
Bilgisayarınıza Azure Resource Manager modüllerini en son sürümünü yüklemek için bkz: [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.1#update-azps).  

## <a name="enable-azure-diagnostics"></a>Azure tanılama etkinleştir  
Azure tanılama PaaS kaynaklar için yapılandırma gerçekleştirilir betik yürüterek **etkinleştir AzureRMDiagnostics.ps1**, kullanılabilir olduğu [PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-AzureRMDiagnostics/2.52/DisplayScript).  Komut dosyası aşağıdaki senaryoları destekler:
  
* Bir Abonelikteki bir veya daha fazla kaynak gruplarıyla ilgili bir kaynak belirtme  
* Bir abonelik belirli bir kaynak grubunda ilgili bir kaynak belirtme  
* Farklı bir çalışma alanına iletmek için bir kaynak yeniden yapılandırın

Azure tanılama verilerini toplama Ölçümleriyle destekler ve doğrudan günlük analizi için Gönder yalnızca kaynakları desteklenir.  Ayrıntılı bir listesi için gözden [toplamak Azure Hizmeti günlüklerini ve günlük analizi kullanmak için ölçümleri](log-analytics-azure-storage.md) 

Karşıdan yüklemek ve komut dosyasını çalıştırmak için aşağıdaki adımları gerçekleştirin.

1.  Windows başlangıç ekranından yazın **PowerShell** ve PowerShell Arama sonuçlarından sağ tıklatın.  Select menüsünde **yönetici olarak çalıştır**.   
2. Kaydet **etkinleştir AzureRMDiagnostics.ps1** komut dosyası yerel olarak aşağıdaki komutu çalıştırarak ve komut dosyasını depolamak için bir yol sağlar.    

    ```
    PS C:\> save-script -Name Enable-AzureRMDiagnostics -Path "C:\users\<username>\desktop\temp"
    ```

3. Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.   
4. Aşağıdaki betiği çalıştırın `.\Enable-AzureRmDiagnostics.ps1` aboneliğinizde veya parametresi ile belirli bir kaynaktan veri toplamayı etkinleştirmek için herhangi bir parametre olmadan `-ResourceGroup <myResourceGroup>` belirli bir kaynak grubunda bir kaynak belirtmek için.   
5. Birden çok değer girerek varsa, uygun aboneliği listeden seçin.<br><br> ![Komut dosyası tarafından döndürülen abonelik seçin](./media/log-analytics-collect-azurepass-posh/script-select-subscription.png)<br> Aksi durumda, tek bir abonelik kullanılabilir otomatik olarak seçer.
6. Ardından, komut dosyası bir abonelikte kayıtlı günlük analizi çalışma alanlarının listesini döndürür.  Listeden uygun olanı seçin.<br><br> ![Komut dosyası tarafından döndürülen bir çalışma alanı seçin](./media/log-analytics-collect-azurepass-posh/script-select-workspace.png)<br> 
7. Koleksiyondan etkinleştirmek istediğiniz Azure kaynağı seçin. Örneğin, 5 yazarsanız, SQL Azure veritabanları için veri toplama etkinleştirin.<br><br> ![Komut dosyası tarafından döndürülen select kaynak türü](./media/log-analytics-collect-azurepass-posh/script-select-resource.png)<br>
   Yalnızca Azure tanılama ve doğrudan bir günlük analizi gönderme toplama Ölçümleriyle Destek kaynakları seçebilirsiniz.  Komut dosyası değerini gösterecektir **True** altında **ölçümleri** sütun abonelik ya da belirtilen kaynak grubunun bulduğu kaynaklar listesi için.    
8. Seçiminizi onaylamanız istenir.  Girin **Y** tüm ölçümleri günlüğe kaydetmeyi etkinleştirmek için seçili Abonelikteki tüm SQL veritabanları, örneğimizde kaynaklar tanımlanan, kapsam için.  

Komut dosyası, seçilen ölçütlerle eşleşen her kaynak karşı çalıştırın ve ölçümleri koleksiyonu için bunları etkinleştirin. Bu işlem tamamlandıktan sonra yapılandırma tamamlandıktan belirten bir ileti görürsünüz.  

Günlük analizi deponuzun Azure PaaS kaynak verileri görmek kısa bir süre içinde tamamlandıktan sonra başlar.  Türünde bir kayıt `AzureMetrics` oluşturulur ve bu kayıtları çözümleme tarafından desteklendiği [Azure SQL analizi](log-analytics-azure-sql.md) ve [Azure Web Apps Analytics](log-analytics-azure-web-apps-analytics.md) yönetim çözümleri.   

## <a name="update-a-resource-to-send-data-to-another-workspace"></a>Başka bir çalışma alanına veri göndermek için bir kaynak güncelleştirme
Verileri bir günlük analizi çalışma alanına zaten gönderme bir kaynağa sahip ve daha sonra başka bir çalışma alanı başvurmak için yeniden yapılandırmak karar verirseniz, komut dosyasıyla çalıştırabilirsiniz `-Update` parametresi.  

**Örnek:** 
`PS C:\users\<username>\Desktop\temp> .\Enable-AzureRMDiagnostics.ps1 -Update`

İlk yapılandırmasını gerçekleştirmek için komut dosyasını çalıştırdığınızda aynı bilgileri yanıtlamanız istenir.  

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için. 

* Kullanım [özel alanlar](log-analytics-custom-fields.md)(olay kayıtlarını tek tek alanlarına ayrıştırılamadı.

* Gözden geçirme [günlük analizi kullanmak için özel bir pano oluşturun](log-analytics-dashboards.md) günlüğünüzün görselleştirmek nasıl anlamak için kuruluş için anlamlı şekillerde arar.
