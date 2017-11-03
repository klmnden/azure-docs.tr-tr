---
title: "Azure Otomasyonu DSC OMS günlük analizi için raporlama verilerini iletmek | Microsoft Docs"
description: "Bu makalede, istenen durum yapılandırması (DSC) ek bilgiler sunmak için Microsoft Operations Management Suite günlük analizi ve yönetimi için raporlama verilerini göndermek gösterilmiştir."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: eslesar
ms.openlocfilehash: 316031c5297a0201c8db4a9e177298c78962c673
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="forward-azure-automation-dsc-reporting-data-to-oms-log-analytics"></a>Azure Otomasyonu DSC OMS günlük analizi veri raporlama ilet

Automation DSC düğüm durumu verilerinin, Microsoft Operations Management Suite (OMS) günlük analizi çalışma alanına gönderebilirsiniz.  
Uyumluluk durumu Azure portalında veya PowerShell DSC düğüm yapılandırmaları kaynaklarında tek tek ve düğümler için görülebilir. Günlük analizi ile şunları yapabilirsiniz:

* Yönetilen düğümler ve ayrı ayrı kaynaklar için uyumluluk bilgilerini alma
* Bir e-posta veya uyumluluk durumuna dayalı uyarı Tetikle
* Yönetilen düğümlere gelişmiş sorguları yazma
* Automation hesapları arasında uyumluluk durumu ilişkilendirmek
* Zaman içinde düğüm uyumluluk geçmişinizi Görselleştirme

## <a name="prerequisites"></a>Ön koşullar

Automation DSC raporlarınızı günlük analizi için göndermeye başlayın, gerekir:

* Kasım 2016 veya sonraki sürümünün [Azure PowerShell](/powershell/azure/overview) (v2.3.0).
* Azure Otomasyonu hesabı. Daha fazla bilgi için bkz: [Azure Automation ile çalışmaya başlama](automation-offering-get-started.md)
* Günlük analizi çalışma alanı ile bir **otomasyon ve Denetim** hizmet teklifi. Daha fazla bilgi için bkz: [günlük Analytics ile çalışmaya başlama](../log-analytics/log-analytics-get-started.md).
* En az bir Azure Otomasyonu DSC düğümü. Daha fazla bilgi için bkz: [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md) 

## <a name="set-up-integration-with-log-analytics"></a>Günlük analizi ile tümleştirmeyi ayarlamak

Azure Automation DSC'den günlük analizi veri almaya başlamak için aşağıdaki adımları tamamlayın:

1. PowerShell Azure hesabınızda oturum açın. Bkz: [oturum oturum Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azurermps-4.0.0)
1. Alma _ResourceId_ aşağıdaki PowerShell komutunu çalıştırarak Otomasyon hesabınızın: (birden fazla Otomasyon hesabı varsa, seçin _ResourceId_ yapılandırmak istediğiniz hesap için).

  ```powershell
  # Find the ResourceId for the Automation Account
  Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
  ```
1. Alma _ResourceId_ aşağıdaki PowerShell komutunu çalıştırarak günlük analizi çalışma alanınızın: (birden çok çalışma alanı varsa, seçin _ResourceId_ yapılandırmak istediğiniz çalışma alanı için).

  ```powershell
  # Find the ResourceId for the Log Analytics workspace
  Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
  ```
1. Aşağıdaki PowerShell komutunu çalıştırın değiştirme `<AutomationResourceId>` ve `<WorkspaceResourceId>` ile _ResourceId_ değerleri önceki adımların her biri:

  ```powershell
  Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories "DscNodeStatus"
  ```

Verileri Azure Automation DSC'den günlük analizi alma işlemini durdurmak istiyorsanız, aşağıdaki PowerShell komutunu çalıştırın.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories "DscNodeStatus"
```

## <a name="view-the-dsc-logs"></a>DSC günlükleri görüntüle

Automation DSC verileriniz için günlük analizi ile tümleştirme ayarladıktan sonra bir **günlük arama** düğmesi görüntülenecek **DSC düğümleri** dikey Otomasyon hesabınızın. Tıklatın **günlük arama** DSC düğümü verileri için günlükleri görüntülemek için düğmeye.

![Günlük Ara düğmesi](media/automation-dsc-diagnostics/log-search-button.png)

**Günlük arama** dikey penceresi açılır ve gördüğünüz bir **DscNodeStatusData** her DSC düğümü için işlemi ve **DscResourceStatusData** işlemi her [DSC kaynağı](https://msdn.microsoft.com/powershell/dsc/resources) bu düğüme uygulanan düğüm yapılandırması adı verilen.

**DscResourceStatusData** işlemi başarısız DSC kaynakları için hata bilgilerini içerir.

Bu işlem için verileri görmek için listedeki her bir işlemin'ı tıklatın.

[Günlük analizi arayarak. günlükleri görüntüleyebilirsiniz Bkz: [Bul günlük aramaları kullanarak verileri](../log-analytics/log-analytics-log-searches.md).
DSC günlüklerinizi bulmak için aşağıdaki sorguyu yazın:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category = "DscNodeStatus"`

Ayrıca, sorgu işlemi adıyla da daraltabilirsiniz. Örneğin: ' türü AzureDiagnostics ResourceProvider = = "MICROSOFT. OTOMASYON"kategorisi"DscNodeStatus"OperationName ="DscNodeStatusData"=

### <a name="send-an-email-when-a-dsc-compliance-check-fails"></a>DSC uyumluluk denetimi başarısız olduğunda bir e-posta Gönder

Bizim üst müşteri isteklerinden biri, bir şeyler bir DSC yapılandırması yanlış gittiğinde bir e-posta veya bir metin gönderme olanağı içindir.   

Bir uyarı kuralı oluşturmak için bir günlük arama uyarı çağıracağı DSC rapor kayıtlar için oluşturarak başlayın.  Tıklatın **uyarı** oluşturmak ve uyarı kuralı yapılandırmak için düğmesi.

1. Günlük analizi genel bakış sayfasında **günlük arama**.
1. Uyarınız için günlük arama sorgusunu sorgu alanına aşağıdaki arama yazarak oluşturun:`Type=AzureDiagnostics Category=DscNodeStatus NodeName_s=DSCTEST1 OperationName=DscNodeStatusData ResultType=Failed`

  Günlüklerini birden fazla Otomasyon hesabı veya abonelik alanınıza ayarladıysanız, uyarılarınızı aboneliği ve Automation hesabı göre gruplandırabilirsiniz.  
  Otomasyon hesabı adı DscNodeStatusData arama kaynak alanından elde edilebilir.  
1. Açmak için **uyarı kuralı Ekle** ekranında **uyarı** sayfanın üst kısmındaki. Uyarı yapılandırma seçenekleri hakkında daha fazla bilgi için bkz: [günlük analizi uyarılarını](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Tüm düğümleri arasında başarısız DSC kaynakları bulun

Günlük analizi kullanmanın bir avantajı, düğümlere başarısız olup olmadığını denetler arayabilirsiniz ' dir.
Başarısız DSC kaynakları tüm örneklerini bulmak için.

1. Günlük analizi genel bakış sayfasında **günlük arama**.
1. Uyarınız için günlük arama sorgusunu sorgu alanına aşağıdaki arama yazarak oluşturun:`Type=AzureDiagnostics Category=DscNodeStatus OperationName=DscResourceStatusData ResultType=Failed`

### <a name="view-historical-dsc-node-status"></a>Geçmiş DSC düğüm durumu görüntüle

Son olarak, zaman içinde DSC düğüm durumu geçmişi görselleştirmek isteyebilirsiniz.  
DSC düğümü durumunuzu durumunun zamanla aramak için bu sorguyu kullanın.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Bu düğüm durumu bir grafik zaman içinde görüntüler.

## <a name="log-analytics-records"></a>Log Analytics kayıtları

Azure Otomasyonu tanılama günlük analizi kayıtları iki kategorisi oluşturur.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Tarih ve saat uyumluluk denetimi ne zaman çalıştırıldığı. |
| OperationName |DscNodeStatusData |
| ResultType |Düğüm uyumlu olup olmadığı. |
| NodeName_s |Yönetilen düğümünün adı. |
| NodeComplianceStatus_s |Düğüm uyumlu olup olmadığı. |
| DscReportStatus |Uyumluluğu denetle olup olmadığını başarıyla çalıştırıldı. |
| ConfigurationMode | Nasıl yapılandırması düğüme uygulanır. Olası değerler şunlardır: __"ApplyOnly"__,__"ApplyandMonitior"__, ve __"ApplyandAutoCorrect"__. <ul><li>__ApplyOnly__: DSC yapılandırmasını uygular ve yeni bir yapılandırma hedef düğüme veya bir sunucudan yeni bir yapılandırma çekilen itildiği sürece başka hiçbir şey yapmaz. Yeni yapılandırma ilk uygulamadan sonra DSC önceden yapılandırılmış bir durumdan kayması kontrol etmez. DSC çalışır önce başarılı olana kadar yapılandırmayı uygulamak __ApplyOnly__ etkisi alır. </li><li> __ApplyAndMonitor__: Bu varsayılan değerdir. LCM'yi yeni tüm yapılandırmalar için geçerlidir. Hedef düğüm istenen durumundan drifts yeni yapılandırma ilk uygulamadan sonra günlükleri tutarsızlık DSC bildirir. DSC çalışır önce başarılı olana kadar yapılandırmayı uygulamak __ApplyAndMonitor__ etkisi alır.</li><li>__ApplyAndAutoCorrect__: DSC tüm yeni yapılandırmaları uygular. Yeni yapılandırma ilk uygulamadan sonra hedef düğüm istenen durumundan drifts DSC günlükleri tutarsızlık raporları ve geçerli yapılandırmasını yeniden uygular.</li></ul> |
| HostName_s | Yönetilen düğümünün adı. |
| IP adresi | Yönetilen düğümü IPv4 adresi. |
| Kategori | DscNodeStatus |
| Kaynak | Azure Otomasyonu hesabının adı. |
| Tenant_g | Arayanlar için Kiracı tanımlayan GUID. |
| NodeId_g |Yönetilen düğümü tanıtan GUID. |
| DscReportId_g |Rapor tanımlar GUID. |
| LastSeenTime_t |Tarih ve saat raporun son görüntülenen zaman. |
| ReportStartTime_t |Tarih ve saat raporun başlatıldığı. |
| ReportEndTime_t |Tarih ve saat raporun tamamlanmış olduğunda. |
| NumberOfResources_d |Düğüme uygulanan yapılandırmasında DSC kaynakları sayısı çağrılır. |
| SourceSystem | Günlük analizi nasıl veriler toplanır. Her zaman *Azure* Azure tanılama için. |
| ResourceId |Azure Otomasyonu hesabını belirtir. |
| ResultDescription | Bu işlem açıklaması. |
| SubscriptionId | Otomasyon hesabının Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabının kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Uyumluluk raporu bağıntı kimliği GUID. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Tarih ve saat uyumluluk denetimi ne zaman çalıştırıldığı. |
| OperationName |DscResourceStatusData|
| ResultType |Kaynak uyumlu olup olmadığı. |
| NodeName_s |Yönetilen düğümünün adı. |
| Kategori | DscNodeStatus |
| Kaynak | Azure Otomasyonu hesabının adı. |
| Tenant_g | Arayanlar için Kiracı tanımlayan GUID. |
| NodeId_g |Yönetilen düğümü tanıtan GUID. |
| DscReportId_g |Rapor tanımlar GUID. |
| DscResourceId_s |DSC kaynağı örneğinin adı. |
| DscResourceName_s |DSC kaynağı adı. |
| DscResourceStatus_s |DSC kaynağı uyumlu olup olmadığı. |
| DscModuleName_s |DSC kaynağı içeren PowerShell modülü adı. |
| DscModuleVersion_s |DSC kaynağı içeren PowerShell modülü sürümü. |
| DscConfigurationName_s |Düğüme uygulanan yapılandırmasının adı. |
| ErrorCode_s | Kaynak başarısız olursa hata kodu. |
| ErrorMessage_s |Kaynak başarısız olursa hata iletisi. |
| DscResourceDuration_d |DSC kaynağı çalıştığı saniye cinsinden süre. |
| SourceSystem | Günlük analizi nasıl veriler toplanır. Her zaman *Azure* Azure tanılama için. |
| ResourceId |Azure Otomasyonu hesabını belirtir. |
| ResultDescription | Bu işlem açıklaması. |
| SubscriptionId | Otomasyon hesabının Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabının kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Uyumluluk raporu bağıntı kimliği GUID. |

## <a name="summary"></a>Özet

Automation DSC verileriniz için günlük analizi göndererek, Automation DSC düğümleri tarafından durumunu daha iyi bir anlayış alabilirsiniz:

* Bir sorun olduğunda bildirimde bulunacak uyarılar ayarlama
* Runbook sonuçlarınızı görselleştirmek için özel görünümleri ve arama sorguları kullanarak, runbook iş durumu ve diğer anahtar göstergeleri veya ölçümleri ilgili.  

Günlük analizi Automation DSC verilerinize büyük işletimsel görünürlük sağlar ve adres olaylar daha hızlı yardımcı olabilir.  

## <a name="next-steps"></a>Sonraki adımlar

* Farklı arama sorguları oluşturmak ve günlük analizi ile Automation DSC günlükleri gözden geçirmek hakkında daha fazla bilgi için bkz: [oturum aramaları günlük analizi](../log-analytics/log-analytics-log-searches.md)
* Azure Otomasyonu DSC kullanma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu DSC ile çalışmaya başlama](automation-dsc-getting-started.md)
* OMS Log Analytics ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için bkz. [Log Analytics’te Azure depolama verileri toplamaya genel bakış](../log-analytics/log-analytics-azure-storage.md)

