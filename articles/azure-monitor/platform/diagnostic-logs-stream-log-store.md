---
title: Azure İzleyici'de Log Analytics çalışma alanına Stream Azure tanılama günlükleri
description: Azure İzleyici'de bir Log Analytics çalışma alanı için Azure tanılama günlükleri akışı yapmayı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 33d8f2e7c65a786d1ecb389574fe186efb6fb705
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630782"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics-workspace-in-azure-monitor"></a>Azure İzleyici'de Log Analytics çalışma alanına Stream Azure tanılama günlükleri

**[Azure tanılama günlükleri](diagnostic-logs-overview.md)**  portal, PowerShell cmdlet'leri veya Azure CLI kullanarak Azure İzleyici'de bir Log Analytics çalışma alanına neredeyse gerçek zamanlı akış.

## <a name="what-you-can-do-with-diagnostics-logs-in-a-log-analytics-workspace"></a>Log Analytics çalışma alanında günlükleri Tanılama ile yapabilecekleriniz

Azure İzleyici, Azure kaynaklarından oluşturulan ham günlük verilerini kavramanıza olanak tanıyan bir esnek günlük sorgu ve analiz aracı sağlar. Bazı özellikler şunlardır:

* **Günlük sorgusu** -yazma günlük verilerinizi çeşitli performanstaki günlüklerinden kaynakları ve Oluştur üzerinden grafikleri Gelişmiş sorgular Azure panonuza sabitlenmiş.
* **Uyarı** -bir veya daha fazla olay belirli sorguyla eşleşen ve Azure İzleyici uyarıları kullanarak bir e-posta veya Web kancası çağrısı ile bildirilmesi algılayabilir.
* **Gelişmiş analiz** - makine öğrenimi uygulayın ve günlüklerinizi tarafından ortaya olası sorunları belirlemek için eşleşen algoritmalar desen.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics-workspace"></a>Log Analytics çalışma alanına tanılama günlüklerinin akışı etkinleştirme

Tanılama günlüklerini programlı olarak portal, akış veya kullanarak etkinleştirebilirsiniz [Azure İzleyici REST API'leri](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). Tanılama ayarını oluşturduğunuz her iki durumda da bir Log Analytics çalışma alanı ve günlük kategorileri ve bu çalışma alanına göndermek istediğiniz ölçümleri, belirttiğiniz içinde. Bir tanılama **günlüğü kategorisi** kaynak sağlayabilir günlük türüdür.

Log Analytics çalışma ayarı yapılandıran kullanıcının her iki abonelik için uygun RBAC erişimine sahip olduğu sürece, günlükleri yayan kaynak ile aynı abonelikte olması gerekmez.

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay Hub'ındaki 'Gelen iletiler' ölçümü temelinde araştırılıp bir kuyruk düzeyi. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Portalı kullanarak Stream tanılama günlükleri
1. Portal, Azure İzleyicisi'ne gidin ve tıklayarak **tanılama ayarları** içinde **ayarları** menüsü.


2. İsteğe bağlı olarak kaynak grubu veya kaynak türe göre listeyi filtreleyin ve kaynağın tanılama ayarını ayarlamak istediğiniz'i tıklatın.

3. Hiçbir ayar kaynağı varsa, istenen bir ayar oluşturmak için seçtiğiniz. "Tanılamayı Aç" tıklayın

   ![Tanılama ayarını - mevcut hiçbir ayar Ekle](media/diagnostic-logs-stream-log-store/diagnostic-settings-none.png)

   Kaynakta mevcut ayarları varsa, bu kaynakta zaten yapılandırılmış ayarların bir listesini görürsünüz. "Tanılama ayarı Ekle" ye tıklayın

   ![Tanılama ayarını - var olan ayarları Ekle](media/diagnostic-logs-stream-log-store/diagnostic-settings-multiple.png)

3. Ayarı, bir ad verin ve kutusunu işaretlemeniz **Log Analytics'e gönderme**, sonra bir Log Analytics çalışma alanı seçin.

   ![Tanılama ayarını - var olan ayarları Ekle](media/diagnostic-logs-stream-log-store/diagnostic-settings-configure.png)

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni ayar, bu kaynak için ayarların listesi görüntülenir ve tanılama günlükleri, yeni olay verilerini oluşturulan hemen sonra bu çalışma alanına aktarılır. Beş dakika arasında bir olay olduğunda yayılır ve Log Analytics'te görüntülendiğinde olabileceğini unutmayın.

### <a name="via-powershell-cmdlets"></a>PowerShell cmdlet'leri

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Aracılığıyla akışını etkinleştirmek için [Azure PowerShell cmdlet'leri](../../azure-monitor/platform/powershell-quickstart-samples.md), kullanabileceğiniz `Set-AzDiagnosticSetting` cmdlet şu parametrelerle:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Tam Azure kaynak kimliği değil çalışma alanı kimliği/anahtarı Log Analytics Portalı'nda gösterilen çalışma alanının çalışma alanı kimliği özelliği aldığını unutmayın.

### <a name="via-azure-cli"></a>Via Azure CLI

Aracılığıyla akışını etkinleştirmek için [Azure CLI](../../azure-monitor/platform/cli-samples.md), kullanabileceğiniz [az İzleyici diagnostic-settings oluşturma](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) komutu.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Tanılama günlüğüne olarak geçirilen JSON dizisi sözlükleri ekleyerek ek kategori ekleyebilirsiniz `--logs` parametresi.

`--resource-group` Bağımsız değişken, yalnızca gerekli if `--workspace` bir nesne kimliği değil.

## <a name="how-do-i-query-the-data-from-a-log-analytics-workspace"></a>Bir Log Analytics çalışma alanı verilerini nasıl sorgu?

Azure İzleyicisi portalındaki günlükleri dikey penceresinde, günlük yönetimi çözümü AzureDiagnostics tablonun altında bir parçası olarak tanılama günlükleri sorgulayabilir. Ayrıca [Azure kaynakları için çeşitli izleme çözümleri](../../azure-monitor/insights/solutions.md) günlük verileri anında Öngörüler Azure İzleyici ile gönderdiğiniz almak için yükleyebilirsiniz.

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>Sınırlama bilinen: AzureDiagnostics sütun sınırı
Veri türleri gönderilen tüm aynı tabloya birçok kaynağa göndermek için (_AzureDiagnostics_), bu tablonun şeması şemaları toplanmakta olan tüm farklı veri türleri Süper kümesidir. Örneğin, aşağıdaki veri türleri koleksiyonu için tanılama ayarları oluşturduysanız, tümü aynı çalışma alanına gönderilen:
- Denetim günlükleri, kaynak (A, B ve C sütunlardan oluşan bir şemaya sahip) 1  
- Kaynak (D, E ve F sütunlardan oluşan bir şemaya sahip) 2 hata günlükleri  
- Veri akış günlüklerini kaynak 3 (sütunları G, H ve ben oluşan bir şemaya sahip)  
 
AzureDiagnostics tabloda bazı örnek verilerle şu şekilde görünür:  
 
| ResourceProvider | Kategori | A | B | C | D | E | C | G | H | I |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Microsoft.Resource1 | AuditLogs | x1 | Y1 | z1 |
| Microsoft.Resource2 | Günlüklerini | | | | q1 | W1 | e1 |
| Microsoft.Resource3 | DataFlowLogs | | | | | | | J1 | K1 | L1|
| Microsoft.Resource2 | Günlüklerini | | | | q2 | w2 | E2 |
| Microsoft.Resource3 | DataFlowLogs | | | | | | | J3 | K3 | L3|
| Microsoft.Resource1 | AuditLogs | x5 | Y5 | z5 |
| ... |
 
500'den fazla sütun olmaması herhangi belirli Azure günlüğü tablosu açık bir sınırı yoktur. Bu sınıra ulaşıldıktan sonra ilk 500 dışında herhangi bir sütun ile verileri içeren herhangi bir satır alma zamanında bırakılır. AzureDiagnostics belirli açık olması için bu sınır etkilenen tablodur. Bu genellikle ya da çok çeşitli veri kaynaklarından gönderilir çünkü aynı çalışma alanına veya birkaç çok ayrıntılı veri kaynakları aynı çalışma alanına gönderilen olur. 
 
#### <a name="azure-data-factory"></a>Azure Data Factory  
Azure Data Factory, çok ayrıntılı günlükleri, birtakım nedeniyle bu sınırı tarafından etkilenmiş özellikle bilinen bir kaynaktır. Özellikle:  
- *Herhangi bir etkinlik, işlem hattındaki karşı tanımlanan kullanıcı parametreleri*: etkinliklere karşı her benzersiz olarak adlandırılmış kullanıcı parametresi için oluşturulan yeni bir sütun olur. 
- *Etkinlik giriş ve çıkışları*: Bu etkinliğin etkinlik değişir ve ayrıntılı doğasını nedeniyle, büyük bir miktarını oluşturur. 
 
Olarak daha geniş geçici çözüm teklifleri ile aşağıdaki, bu günlükler, çalışma alanlarında toplanmakta olan diğer günlük türlerini etkileyen olasılığını en aza indirmek için kendi çalışma alanına ADF günlükleri yalıtmak için önerilir. Mid-Nisan 2019 tarafından Azure Data Factory için kullanılabilir günlükleri seçkin isteriz.
 
#### <a name="workarounds"></a>Geçici Çözümler
500-sütun sınırını tanımlandı kadar kısa vadede, ayrıntılı veri türleri limitini aştıktan olasılığını azaltmak için ayrı çalışma alanları halinde ayırmak önerilir.
 
Uzun vadeli, Azure tanılama uzağa birleşik, seyrek bir şema her veri türü başına tek tek tablolar taşınmasını; dinamik türler için destek ile eşlendiğinde, bu Azure tanılama mekanizma aracılığıyla Azure günlüklerine gelen veri kullanılabilirliğini önemli ölçüde artırır. Zaten bu seçim Azure kaynak türlerini, örneğin gördüğünüz [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) veya [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor) günlükleri. Lütfen Azure üzerinde bu seçkin günlükleri destekleyen yeni kaynak türleri hakkında daha fazla haber arayın [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/) blogu!


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Tanılama Günlükleri](diagnostic-logs-overview.md)

