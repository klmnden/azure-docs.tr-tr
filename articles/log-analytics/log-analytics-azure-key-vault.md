---
title: "Günlük analizi Azure anahtar kasası çözümde | Microsoft Docs"
description: "Azure anahtar kasası günlükleri gözden geçirmek için günlük analizi Azure anahtar kasası çözüm kullanabilirsiniz."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 651586e0846ffb22a23e64b73c2cc614980d9b92
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a>Günlük analizi Azure anahtar kasası Analytics çözümde

![Anahtar kasası simgesi](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

Azure Key Vault AuditEvent günlüklerini incelemek için Log Analytics içindeki Azure Key Vault çözümünü kullanabilirsiniz.

Çözümü kullanmak için Azure anahtar kasası tanılama günlük kaydını etkinleştirmek ve tanılama günlük analizi çalışma alanı'na doğrudan gerekir. Günlükleri Azure Blob depolama alanına yazmak gerekli değildir.

> [!NOTE]
> Ocak 2017 ' günlükleri için günlük analizi anahtar Kasası'nı gönderme desteklenen şekilde değiştirildi. Anahtar kasası çözümü kullanıyorsanız gösterir *(kullanım dışı)* başlığında başvurmak [eski anahtar kasası çözüm'den geçiş](#migrating-from-the-old-key-vault-solution) adımları izlemeniz gerekir.
>
>

## <a name="install-and-configure-the-solution"></a>Yükleyin ve yapılandırın
Yükleme ve Azure anahtar kasası çözümü yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Azure anahtar kasası çözümden etkinleştirmek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. Kullanarak izlemek, anahtar kasası kaynaklar için tanılama günlüğünü etkinleştirme [portal](#enable-key-vault-diagnostics-in-the-portal) veya [PowerShell](#enable-key-vault-diagnostics-using-powershell)

### <a name="enable-key-vault-diagnostics-in-the-portal"></a>Anahtar kasası tanılama portalda etkinleştir

1. Azure portalında izlemek için anahtar kasası kaynak gidin
2. Seçin *tanılama günlükleri* aşağıdaki sayfasını açmak için

   ![Azure anahtar kasası döşeme görüntüsü](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. Tıklatın *tanılamayı açın* aşağıdaki sayfasını açmak için

   ![Azure anahtar kasası döşeme görüntüsü](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. Tanılama'yı açmak için tıklatın *üzerinde* altında *durumu*
5. Onay kutusunu tıklatın *için günlük analizi Gönder*
6. Varolan bir günlük analizi çalışma alanını seçin veya bir çalışma alanı oluşturma
7. Etkinleştirmek için *AuditEvent* günlükleri, günlük altında onay kutusunu tıklatın
8. Tıklatın *kaydetmek* günlük analizi için tanılama günlük kaydını etkinleştirmek için

### <a name="enable-key-vault-diagnostics-using-powershell"></a>Anahtar kasası tanılamayı PowerShell kullanarak etkinleştirme
Aşağıdaki PowerShell betiğini nasıl kullanılacağını gösteren bir örnek sağlar `Set-AzureRmDiagnosticSetting` anahtar kasası için tanılama günlük kaydını etkinleştirmek için:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Azure anahtar kasası veri toplama ayrıntılarını gözden geçirin
Azure anahtar kasası çözüm tanılama günlükleri doğrudan anahtar Kasası'nı toplar.
Günlükleri Azure Blob depolama alanına yazmak gerekli değildir ve aracı için veri toplama gereklidir.

Aşağıdaki tabloda, veri toplama yöntemleri ve Azure anahtar kasası için verileri nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracısı | Systems Center Operations Manager Aracısı | Azure | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | geldiğinde |

## <a name="use-azure-key-vault"></a>Azure Key Vault kullanma
Çalıştırdıktan sonra [çözümü yüklemek](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), tıklayarak anahtar kasası verisinin **Azure anahtar kasası** gelen döşeme **genel bakış** günlük analizi sayfası.

![Azure anahtar kasası döşeme görüntüsü](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Tıklattıktan sonra **genel bakış** döşeme, günlüklerinizi özetlerini görüntüleyin ve aşağıdaki kategorilerde ayrıntıları için detaya:

* Tüm anahtar kasası işlemleri zaman içinde hacmi
* İşlem birimleri zaman içinde başarısız oldu
* İşlem tarafından işletimsel ortalama gecikme süresi
* Birden fazla 1000 ms ele işlemlerinin sayısı ve birden fazla 1000 ms ele işlemleri listesini işlemleri için hizmet kalitesi

![Azure anahtar kasası Pano görüntüsü](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Azure anahtar kasası Pano görüntüsü](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Herhangi bir işlem ayrıntılarını görüntülemek için
1. Üzerinde **genel bakış** sayfasında, **Azure anahtar kasası** döşeme.
2. Üzerinde **Azure anahtar kasası** Pano, dikey pencereleri birinde özet bilgilerini inceleyin ve bir günlük arama sayfasında ilgili ayrıntılı bilgileri görüntülemek için tıklatın.

    Herhangi bir günlük arama sayfası üzerinde sonuçları zaman, ayrıntılı sonuçları ve günlük arama geçmişi görüntüleyebilirsiniz. Sonuçları daraltmak için modelleri göre de filtre uygulayabilirsiniz.

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Azure anahtar kasası çözüm türünü içeren kayıtları çözümler **KeyVaults** gelen toplanan [AuditEvent günlükleri](../key-vault/key-vault-logging.md) Azure tanılama.  Aşağıdaki tabloda bu kayıtları için özellikleri şunlardır:  

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*AzureDiagnostics* |
| SourceSystem |*Azure* |
| CallerIpAddress |İsteği yapan istemcinin IP adresi |
| Kategori | *AuditEvent* |
| CorrelationId |İstemci tarafı günlüklerini hizmet tarafı (Anahtar Kasası) günlükleriyle ilişkilendirmek için istemcinin geçirebileceği isteğe bağlı bir GUID. |
| DurationMs |Milisaniye cinsinden REST API'si isteğini sunmak için geçen süre. İstemci tarafında ölçmek süre bu süreyle eşleşmeyebilir şekilde bu kez ağ gecikmesini içermez. |
| httpStatusCode_d |İstek tarafından döndürülen HTTP durum kodu (örneğin, *200*) |
| id_s |İsteğin benzersiz kimliği |
| identity_claim_appid_g | Uygulama kimliği için GUID |
| OperationName |Açıklandığı gibi işlemin adı [Azure anahtar kasası günlüğü](../key-vault/key-vault-logging.md) |
| OperationVersion |İstemci tarafından istenen REST API sürümü (örneğin *2015-06-01*) |
| requestUri_s |İsteğin URI'si |
| Kaynak |Anahtar kasasının adı |
| ResourceGroup |Anahtar kasasının kaynak grubu |
| ResourceId |Azure Resource Manager Kaynak Kimliği. Anahtar kasası günlükleri için bu anahtar kasası kaynak kimliğidir. |
| ResourceProvider |*MICROSOFT. KEYVAULT* |
| ResourceType | *KASALARI* |
| ResultSignature |HTTP durumu (örneğin, *Tamam*) |
| ResultType |REST API isteğinin sonucunu (örneğin, *başarı*) |
| SubscriptionId |Anahtar kasası içeren abonelik Azure abonelik kimliği |

## <a name="migrating-from-the-old-key-vault-solution"></a>Eski anahtar kasası çözüm'den geçiş
Ocak 2017 ' günlükleri için günlük analizi anahtar Kasası'nı gönderme desteklenen şekilde değiştirildi. Bu değişiklikler, aşağıdaki avantajları sağlar:
+ Günlükleri doğrudan günlük analizi depolama hesabı kullanmak zorunda kalmadan yazılır
+ Günlükleri bunları günlük analizi kullanılabilir olması için ne zaman oluşturulur zamandan daha az gecikme süresi
+ Daha az yapılandırma adımları
+ Azure tanılama tüm türleri için ortak bir biçimi

Güncellenen çözümü kullanmak için:

1. [Anahtar Kasası'nı doğrudan günlük analizi için gönderilecek tanılama Yapılandır](#enable-key-vault-diagnostics-in-the-portal)  
2. Azure anahtar kasası çözüm açıklanan işlemi kullanarak etkinleştirin [Çözümleri Galerisi çözümlerinden günlük analizi Ekle](log-analytics-add-solutions.md)
3. Tüm kayıtlı sorgu, panolar veya yeni veri türünü kullanmak için uyarıları güncelleştirme
  + Değişikliği türüdür: AzureDiagnostics KeyVaults. Kaynak türü, anahtar kasası günlükleri için filtre uygulamak için kullanabilirsiniz.
  - Yerine: `Type=KeyVaults`, kullanın`Type=AzureDiagnostics ResourceType=VAULTS`
  + Alanlar: (alan adları büyük küçük harfe duyarlı)
  - Sonekine sahip herhangi bir alan için \_s, \_d veya \_g adı küçük harflere ilk karakter değiştirme
  - Sonekine sahip herhangi bir alan için \_adında, verileri o iç içe geçmiş alan adlarını temel alarak tek tek alanlara bölünür. Örneğin, çağıran UPN bir alanda depolanır`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   - Alan CallerIpAddress CallerIPAddress için değiştirildi
   - Alan RemoteIPCountry artık mevcut değil
4. Kaldırma *anahtar kasası Analytics (kullanım dışı)* çözümü. PowerShell kullanıyorsanız, kullanın`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Değişiklik yeni çözümde görünür değil önce veri toplanmadı. Eski türü ve alan adları kullanarak bu verileri sorgulamak devam edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı Azure anahtar kasası verileri görüntülemek için.
