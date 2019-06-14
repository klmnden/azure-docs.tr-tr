---
title: Azure İzleyici'de Azure Key Vault çözümü | Microsoft Docs
description: Azure anahtar kasası günlükleri gözden geçirmek için Azure İzleyici'de Azure Key Vault çözümünü kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: bwren
ms.openlocfilehash: 481b643f2f7201a2a1745c7aef9ddd81883da020
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60498534"
---
# <a name="azure-key-vault-analytics-solution-in-azure-monitor"></a>Azure İzleyici'de Azure Key Vault Analytics çözümü

![Key Vault simgesi](media/azure-key-vault/key-vault-analytics-symbol.png)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Key Vault AuditEvent günlüklerini gözden geçirmek için Azure İzleyici'de Azure Key Vault çözümünü kullanabilirsiniz.

Çözümü kullanmak için Azure anahtar kasası tanılama günlüğünü etkinleştirme ve Log Analytics çalışma alanına tanılama doğrudan gerekir. Günlükler Azure Blob depolama alanına yazmak gerekli değildir.

> [!NOTE]
> Ocak 2017'de, günlükleri Key Vault'tan Log Analytics'e gönderme desteklenen yönteminizi değiştirdik. Key Vault çözümü kullanıyorsanız gösterir *(kullanım dışı)* başlığında başvurmak [eski Key Vault çözümünden geçiş](#migrating-from-the-old-key-vault-solution) adımları izlemeniz gerekir.
>
>

## <a name="install-and-configure-the-solution"></a>Yükleme ve çözüm yapılandırma
Yükleme ve Azure anahtar kasası çözümü yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Açıklanan işlemi kullanın [Azure İzleyici'yi ekleyin çözüm galeri'sinden](../../azure-monitor/insights/solutions.md) Azure Key Vault çözümü Log Analytics çalışma alanınıza eklemek için.
2. Kullanarak izlemek, Key Vault kaynak için tanılama günlüğünü etkinleştirme [portalı](#enable-key-vault-diagnostics-in-the-portal) veya [PowerShell](#enable-key-vault-diagnostics-using-powershell)

### <a name="enable-key-vault-diagnostics-in-the-portal"></a>Key Vault portalda tanılamayı etkinleştirin

1. Azure portalında, izlemek için Key Vault kaynağına gidin
2. Seçin *tanılama ayarları* aşağıdaki sayfasını açmak için

   ![Azure Key Vault kutucuk görüntüsü](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics01.png)
3. Tıklayın *tanılamayı Aç* aşağıdaki sayfasını açmak için

   ![Azure Key Vault kutucuk görüntüsü](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics02.png)
4. Tanılama ayarı için bir ad verin.
5. Onay kutusunu tıklatın *Log Analytics'e gönderme*
6. Mevcut bir Log Analytics çalışma alanını seçin veya bir çalışma alanı oluşturma
7. Etkinleştirmek için *AuditEvent* günlükleri, günlük onay kutusunu tıklayın
8. Tıklayın *Kaydet* Log Analytics çalışma alanı için tanılama günlüğünü etkinleştirme.

### <a name="enable-key-vault-diagnostics-using-powershell"></a>PowerShell kullanarak Key Vault tanılamayı etkinleştirme
Aşağıdaki PowerShell betiğini nasıl kullanılacağına ilişkin bir örnektir `Set-AzDiagnosticSetting` Key Vault için tanılama günlük kaydını etkinleştirmek için:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'

Set-AzDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Azure Key Vault veri koleksiyonu ayrıntılarını gözden geçirin
Azure Key Vault çözümü, doğrudan Key Vault'tan tanılama günlükleri toplar.
Günlükler Azure Blob depolama alanına yazmak gerekli değildir ve hiçbir aracısı için veri koleksiyonu gereklidir.

Veri toplama yöntemleri ve Azure anahtar kasası için verileri nasıl toplanır hakkında diğer ayrıntıları aşağıdaki tabloda gösterilmektedir.

| Platform | Doğrudan aracı | Systems Center Operations Manager Aracısı | Azure | Operations Manager gerekli? | Operations Manager aracısı veri yönetim grubu gönderilir. | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | geldiğinde |

## <a name="use-azure-key-vault"></a>Azure Key Vault kullanma
Çalıştırdıktan sonra [çözümü yüklemek](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), tıklayarak anahtar kasası verisinin **Key Vault Analytics** kutucuğunda Azure İzleyici'den **genel bakış** sayfası. Bu sayfadan açın **Azure İzleyici** tıklayarak menü **daha fazla** altında **Insights** bölümü. 

![Azure Key Vault kutucuk görüntüsü](media/azure-key-vault/log-analytics-keyvault-tile.png)

Tıkladıktan sonra **Key Vault Analytics** kutucuğunda günlüklerinizi özetlerini görüntüleyin ve ardından aşağıdaki kategorilerde ayrıntıları için ayrıntılara girin:

* Tüm anahtar kasası işlemleri zaman birimi
* İşlem birimleri, zaman içinde başarısız oldu
* İşlem tarafından ortalama işlem gecikme süresi
* 1000 MS'den fazla Süren işlemlerin sayısı ve 1000 MS'den fazla Süren işlemlerin bir listesi işlemler için hizmet kalitesi

![Azure Key Vault panosunun görüntüsü](media/azure-key-vault/log-analytics-keyvault01.png)

![Azure Key Vault panosunun görüntüsü](media/azure-key-vault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Herhangi bir işlem ayrıntılarını görüntülemek için
1. Üzerinde **genel bakış** sayfasında **Key Vault Analytics** Döşe.
2. Üzerinde **Azure anahtar kasası** Pano, dikey pencereleri biriyle özet bilgilerini gözden geçirin ve bir günlük arama sayfasında ilgili ayrıntılı bilgileri görüntülemek için tıklayın.

    Herhangi bir günlük arama sayfası üzerinde sonuç zaman, ayrıntılı sonuçlarını ve günlük arama geçmişinizi görüntüleyebilirsiniz. Ayrıca, sonuçları daraltmak için modelleri göre filtreleyebilirsiniz.

## <a name="azure-monitor-log-records"></a>Azure İzleyici kayıtlarını günlüğe kaydet
Azure Key Vault çözümü bir tür olan kayıtları çözümler **KeyVaults** gelen toplanan [AuditEvent günlüklerini](../../key-vault/key-vault-logging.md) Azure tanılama.  Aşağıdaki tabloda bu kayıtları için özellikleri şunlardır:  

| Özellik | Description |
|:--- |:--- |
| Tür |*AzureDiagnostics* |
| SourceSystem |*Azure* |
| callerIpAddress |İsteği yapan istemcinin IP adresi |
| Kategori | *AuditEvent* |
| CorrelationId |İstemci tarafı günlüklerini hizmet tarafı (Anahtar Kasası) günlükleriyle ilişkilendirmek için istemcinin geçirebileceği isteğe bağlı bir GUID. |
| süre (MS) |Milisaniye cinsinden REST API'si isteğini sunmak için geçen süre. İstemci tarafında ölçtüğünüz süre bu süreyle eşleşmeyebilir için bu kez ağ gecikmesini içermez. |
| httpStatusCode_d |İstek tarafından döndürülen HTTP durum kodu (örneğin, *200*) |
| id_s |İsteğin benzersiz kimliği |
| identity_claim_appid_g | Uygulama kimliği GUID |
| OperationName |Açıklandığı gibi işlem adı [Azure anahtar kasası günlüğü](../../key-vault/key-vault-logging.md) |
| operationVersion |İstemci tarafından istenen REST API sürümü (örneğin *2015-06-01*) |
| requestUri_s |İsteğin URI'si |
| Resource |Anahtar kasasının adı |
| ResourceGroup |Anahtar kasasının kaynak grubu |
| ResourceId |Azure Resource Manager Kaynak Kimliği. Anahtar kasası günlükleri için bu anahtar kasası kaynak kimliğidir. |
| ResourceProvider |*MICROSOFT.KEYVAULT* |
| ResourceType | *KASALARI* |
| resultSignature |HTTP durumu (örneğin, *Tamam*) |
| ResultType |REST API'si isteğinin sonucunu (örneğin, *başarı*) |
| SubscriptionId |Key Vault içeren aboneliği, Azure abonelik kimliği |

## <a name="migrating-from-the-old-key-vault-solution"></a>Eski Key Vault çözümünden geçiş
Ocak 2017'de, günlükleri Key Vault'tan Log Analytics'e gönderme desteklenen yönteminizi değiştirdik. Bu değişiklikler, aşağıdaki avantajları sağlar:
+ Günlükleri bir depolama hesabı kullanmak zorunda kalmadan bir Log Analytics çalışma alanı için doğrudan yazılır
+ Ne zaman bunlara Log Analytics'te almalarının günlükleri üretilir saatten daha az gecikme süresi
+ Daha az yapılandırma adımı
+ Azure tanılama her tür için ortak bir biçimi

Güncel çözümü kullanmak için:

1. [Key Vault'tan bir Log Analytics çalışma alanınıza doğrudan gönderilecek tanılama Yapılandır](#enable-key-vault-diagnostics-in-the-portal)  
2. Açıklanan işlemi kullanarak Azure Key Vault çözümü etkinleştirme [çözüm Galerisi'ndeki çözümlerini Azure izleme Ekle](../../azure-monitor/insights/solutions.md)
3. Tüm kaydedilmiş sorgular, panolar veya yeni veri türü kullanılacağını uyarıları güncelleştirme
   + Tür farklıdır: KeyVaults AzureDiagnostics için. Kaynak türü, anahtar kasası günlükleri için filtre uygulamak için kullanabilirsiniz.
   + Yerine: `KeyVaults`, kullanın `AzureDiagnostics | where ResourceType'=="VAULTS"`
   + Alanlar: (Alan adları büyük-küçük harf duyarlıdır)
   + Bir son eki olan herhangi bir alan için \_s, \_d veya \_g adı küçük harflere ilk karakteri değiştirme
   + Bir son eki olan herhangi bir alan için \_o adı, veriler iç içe geçmiş alanı adlarını temel alarak ayrı ayrı alanlara bölünür. Örneğin, arayanın UPN bir alanda depolanır `identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   + Alan CallerIpAddress CallerIPAddress için değiştirildi.
   + Alan RemoteIPCountry artık mevcut değil
4. Kaldırma *Key Vault Analytics (kullanım dışı)* çözüm. PowerShell kullanıyorsanız, `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Değişiklik yeni çözümde görünür değil. önce toplanan veriler. Alan adları ve eski türünü kullanarak bu verileri sorgulamak devam edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [sorgular Azure İzleyici'de oturum](../../azure-monitor/log-query/log-query-overview.md) ayrıntılı Azure anahtar kasası verilerini görüntülemek için.
