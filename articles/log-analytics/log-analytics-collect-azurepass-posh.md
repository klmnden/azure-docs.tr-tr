---
title: Log Analytics ile Azure PaaS kaynak ölçümleri toplamak | Microsoft Docs
description: Bekletme ve Log analytics'te analiz için PowerShell kullanarak Azure PaaS kaynak ölçümleri toplamayı etkinleştirmeyi öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: a9635a7c9bad9079814750dc4be945701ba80451
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632322"
---
# <a name="configure-collection-of-azure-paas-resource-metrics-with-log-analytics"></a>Log Analytics ile Azure PaaS kaynak ölçümleri koleksiyonunu yapılandırma

Azure SQL ve Web sitelerini (Web uygulamaları) gibi bir hizmet (PaaS) kaynak olarak Azure platformu, performans ölçüm verileri Log Analytics için yerel olarak gönderebilir. Bu betik, kullanıcıların belirli bir kaynak grubu ister bütün bir abonelik zaten dağıtılmış PaaS kaynakları için günlüğü ölçümlerini etkinleştirmeniz olanak tanır. 

Bugün, PaaS için Azure portalı üzerinden kaynakları günlüğü ölçümleri etkinleştirmek üzere hiçbir yolu yoktur. Bu nedenle, bir PowerShell Betiği kullanmanız gerekir. Log Analytics, izleme ile birlikte bu yerel ölçümleri günlüğe kaydetme özelliğini uygun ölçekte Azure kaynakları izlemek etkinleştirin. 

## <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce bilgisayarınızda yüklü aşağıdaki Azure Resource Manager modüllerini doğrulayın:

- AzureRM.Insights
- AzureRM.OperationalInsights
- AzureRM.Resources
- AzureRM.profile

>[!NOTE]
>Tüm Azure Resource Manager modüllerini Powershell'den Azure Resource Manager komutları çalıştırdığınızda uyumluluğu sağlamak için aynı sürümde olduğundan öneririz.
>
Azure Resource Manager modüllerini en son sürümünü bilgisayarınıza yüklemek için bkz: [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.1#update-azps).  

## <a name="enable-azure-diagnostics"></a>Azure tanılamayı etkinleştirin  
PaaS kaynakları Azure tanılamayı yapılandırma gerçekleştirilir betik yürüterek **etkinleştir AzureRMDiagnostics.ps1**, kullanılabilir olduğu [PowerShell Galerisi](https://www.powershellgallery.com/packages/Enable-AzureRMDiagnostics/2.52).  Betik aşağıdaki senaryoları destekler:
  
* Bir Abonelikteki bir veya daha fazla kaynak grupları ilgili kaynak belirtme  
* Belirli bir kaynak grubu bir abonelik için ilgili bir kaynak belirtme  
* Farklı bir çalışma alanına iletmek için bir kaynağı yeniden yapılandırın

Azure tanılama verilerini toplama Ölçümleriyle destekler ve doğrudan Log Analytics'e gönderme yalnızca kaynakları desteklenir.  Ayrıntılı bir listesi için gözden [toplamak Azure hizmeti günlükleri ve ölçümleri Log analytics'teki kullanım için](log-analytics-azure-storage.md) 

İndir ve Yürüt komut dosyası için aşağıdaki adımları gerçekleştirin.

1.  Windows başlangıç ekranından yazın **PowerShell** ve PowerShell Arama sonuçlarından sağ tıklayın.  Menüden seçim **yönetici olarak çalıştır**.   
2. Kaydet **etkinleştir AzureRMDiagnostics.ps1** betiği yerel olarak aşağıdaki komutu çalıştırarak ve komut dosyasını depolamak için bir yol sağlama.    

    ```
    PS C:\> save-script -Name Enable-AzureRMDiagnostics -Path "C:\users\<username>\desktop\temp"
    ```

3. Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.   
4. Aşağıdaki betiği çalıştırın `.\Enable-AzureRmDiagnostics.ps1` aboneliğinizdeki veya parametresiyle belirli bir kaynaktan veri toplamayı etkinleştirmek için herhangi bir parametre olmadan `-ResourceGroup <myResourceGroup>` belirli kaynak grubunda bir kaynak belirtmek için.   
5. Birden fazla doğru değer girerek varsa uygun aboneliği listeden seçin.<br><br> ![Komut dosyası tarafından döndürülen bir abonelik seçin](./media/log-analytics-collect-azurepass-posh/script-select-subscription.png)<br> Aksi takdirde, tek bir abonelik kullanılabilir otomatik olarak seçer.
6. Ardından, betiği aboneliğinde kayıtlı bir Log Analytics çalışma alanlarının bir listesini döndürür.  Listeden uygun olanı seçin.<br><br> ![Komut dosyası tarafından döndürülen çalışma alanı seçin](./media/log-analytics-collect-azurepass-posh/script-select-workspace.png)<br> 
7. Koleksiyondan etkinleştirmek istediğiniz Azure kaynağını seçin. Örneğin, 5 yazarsanız, SQL Azure veritabanları için veri toplamayı etkinleştirin.<br><br> ![Komut dosyası tarafından döndürülen select kaynak türü](./media/log-analytics-collect-azurepass-posh/script-select-resource.png)<br>
   Azure Tanılama'yı ve doğrudan Log Analytics'e gönderme ile toplama ölçümleri destekleyen kaynaklar yalnızca seçebilirsiniz.  Komut dosyası değerini gösterir **True** altında **ölçümleri** bulur, abonelik veya belirtilen kaynak grubunda kaynaklar listesi için sütun.    
8. Seçiminizi onaylayın istenir.  Girin **Y** seçili Abonelikteki tüm SQL veritabanları, örneğimizde kaynaklar tanımlandığı, kapsamı için tüm ölçümleri günlüğe kaydetmeyi etkinleştirmek için.  

Betik seçilen ölçütlerle eşleşen her kaynak karşı çalıştırma ve bunlara yönelik ölçümleri toplamayı etkinleştirin. Tamamlandıktan sonra yapılandırma tamamlandıktan belirten bir ileti görürsünüz.  

Log Analytics deponuzdaki Azure PaaS kaynak verilerini görmek kısa bir süre içinde tamamlandıktan sonra başlar.  Türünde bir kayıt `AzureMetrics` oluşturulur ve bu kayıtları analiz tarafından desteklenmektedir [Azure SQL Analytics](log-analytics-azure-sql.md) ve [Azure Web Apps Analytics](log-analytics-azure-web-apps-analytics.md) yönetim çözümleri.   

## <a name="update-a-resource-to-send-data-to-another-workspace"></a>Başka bir çalışma alanına veri göndermek için kaynak güncelleştirme
Zaten bir Log Analytics çalışma alanına veri gönderen bir kaynak varsa ve daha sonra başka bir çalışma alanı başvurmak için yeniden yapılandırmaya karar, komut dosyasını çalıştırabilirsiniz `-Update` parametresi.  

**Örnek:** 
`PS C:\users\<username>\Desktop\temp> .\Enable-AzureRMDiagnostics.ps1 -Update`

İlk yapılandırmayı gerçekleştiren betiği çalıştırdığınızda, aynı bilgileri yanıtlamanız istenir.  

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [günlük aramaları](log-analytics-queries.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 

* Kullanım [özel alanlar](log-analytics-custom-fields.md)(olay kayıtları tek tek alanlarına ayrıştırılamadı.

* Gözden geçirme [Log Analytics'te kullanım için özel bir pano oluşturma](../azure-monitor/platform/dashboards.md) günlüğünüzün görselleştirmek nasıl anlamak için kuruluş için anlamlı şekillerde arar.
