---
title: Azure İzleyici'de Log Analytics çalışma alanına Stream Azure tanılama günlükleri
description: Azure İzleyici'de bir Log Analytics çalışma alanı için Azure tanılama günlükleri akışı yapmayı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 13eb1a8fcea2f74cda5921a51b8c2e8816be975f
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303703"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics-workspace-in-azure-monitor"></a>Azure İzleyici'de Log Analytics çalışma alanına Stream Azure tanılama günlükleri

**[Azure tanılama günlükleri](diagnostic-logs-overview.md)**  portalı, PowerShell cmdlet'leri veya Azure CLI kullanarak Azure İzleyici'de bir Log Analytics çalışma alanına neredeyse gerçek zamanlı akış.

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

Birkaç dakika sonra yeni ayar, bu kaynak için ayarların listesi görüntülenir ve tanılama günlükleri, yeni olay verilerini oluşturulan hemen sonra bu çalışma alanına aktarılır. 15 dakika arasında bir olay olduğunda yayılır ve Log Analytics'te görüntülendiğinde olabilir.

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

## <a name="azure-diagnostics-vs-resource-specific"></a>Azure tanılama ve kaynağa özgü  
Log Analytics hedef Azure tanılama yapılandırması etkinleştirildikten sonra verileri çalışma alanınızda gösterir iki farklı yolu vardır:  
- **Azure tanılama** -bugün Azure hizmetlerinin büyük bölümü tarafından kullanılan eski yöntem budur. Bu modda, tüm veriler herhangi bir tanılama ayarı belirli bir çalışma alanına işaret içinde sona erecek _AzureDiagnostics_ tablo. 
<br><br>Pek çok kaynak aynı tabloya veri göndermek için (_AzureDiagnostics_), bu tablonun şeması şemaları toplanmakta olan tüm farklı veri türleri Süper kümesidir. Örneğin, aşağıdaki veri türleri koleksiyonu için tanılama ayarları oluşturduysanız, tümü aynı çalışma alanına gönderilen:
    - Denetim günlükleri, kaynak (A, B ve C sütunlardan oluşan bir şemaya sahip) 1  
    - Kaynak (D, E ve F sütunlardan oluşan bir şemaya sahip) 2 hata günlükleri  
    - Veri akış günlüklerini kaynak 3 (sütunları G, H ve ben oluşan bir şemaya sahip)  

    AzureDiagnostics tabloda bazı örnek verilerle şu şekilde görünür:  

    | ResourceProvider | Kategori | A | B | C | D | E | F | G | H | I |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | Microsoft.Resource1 | AuditLogs | x1 | Y1 | z1 |
    | Microsoft.Resource2 | Günlüklerini | | | | q1 | W1 | e1 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | J1 | K1 | L1|
    | Microsoft.Resource2 | Günlüklerini | | | | q2 | w2 | E2 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | J3 | K3 | L3|
    | Microsoft.Resource1 | AuditLogs | x5 | Y5 | z5 |
    | ... |

- **Kaynağa özgü** -bu modda, tanılama ayarları yapılandırmada seçilen her kategori başına tek tek tablolar seçilen çalışma alanı oluşturulur. Bu yeni yöntem tamamen açık ayrımı nettir bulmak istediğiniz bulmak çok daha kolay getirir: her kategori için bir tablo. Ayrıca, dinamik türleri için kendi destek avantajları sağlar. Bu mod belirli Azure kaynak türlerini, örneğin zaten görebilirsiniz [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) veya [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor) günlükleri. Sonuç olarak, kaynağa özgü moda geçirmek için her veri türü bekliyoruz. 

    Yukarıdaki örnekte, bu üç tabloda oluşturulan neden olur: 
    - Tablo _AuditLogs_ gibi:

        | ResourceProvider | Kategori | A | B | C |
        | -- | -- | -- | -- | -- |
        | Microsoft.Resource1 | AuditLogs | x1 | Y1 | z1 |
        | Microsoft.Resource1 | AuditLogs | x5 | Y5 | z5 |
        | ... |

    - Tablo _günlüklerini_ gibi:  

        | ResourceProvider | Kategori | D | E | F |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource2 | Günlüklerini | q1 | W1 | e1 |
        | Microsoft.Resource2 | Günlüklerini | q2 | w2 | E2 |
        | ... |

    - Tablo _DataFlowLogs_ gibi:  

        | ResourceProvider | Kategori | G | H | I |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource3 | DataFlowLogs | J1 | K1 | L1|
        | Microsoft.Resource3 | DataFlowLogs | J3 | K3 | L3|
        | ... |

    Kaynağa özgü modu kullanmanın diğer avantajları arasında hem alımı gecikme süresi hem de sorgu süreleri, şemalar ve bunların yapısı, belirli bir tablo ve daha fazlası RBAC hakları olanağı bulunmasını performansı içerir.

### <a name="selecting-azure-diagnostic-vs-resource-specific-mode"></a>Azure tanılama vs kaynağa özgü modu seçme
Çoğu Azure kaynakları için Azure Tanılama veya kaynağa özgü modunu kullanıp kullanmayacağınızı etkinleştirebileceğinizi değil; veri kaynağı kullanmak için seçilmiş bir yöntem aracılığıyla otomatik olarak akar. Lütfen veri modu kullanılırsa Ayrıntılar için Log Analytics'e göndermek için etkinleştirdikten kaynak tarafından sağlanan belgelere bakın. 

Önceki bölümde belirtildiği gibi sonuç olarak tüm hizmetler Azure kullanımda kaynağa özgü modu sağlamak için Azure İzleyici hedefi durumdur. Bu geçişi kolaylaştırmak ve veri parçası olarak bunu, bazı Azure Hizmetleri ekleme Log analytics'e modu bir seçimle sağlayacaksa kayıp olduğundan emin olmak için:  
   ![Tanılama ayarları modu Seçici](media/diagnostic-logs-stream-log-store/diagnostic-settings-mode-selector.png)

Biz **kesin** yol aşağı zor olabilecek geçişleri önlemek için herhangi yeni tanılama ayarlarını kullan kaynak merkezli modu oluşturulan, önerilir.  

Belirli bir Azure kaynak tarafından etkinleştirildikten sonra mevcut tanılama ayarları için geriye dönük olarak Azure Tanılama ' kaynağa özgü moda mümkün olacaktır. Daha önce alınan verileriniz kullanılabilir olmaya devam edecek _AzureDiagnostics_ çalışma alanında, bekletme ayarında yapılandırılan out yaş, ancak herhangi bir yeni veri adanmış tabloya gönderilecek kadar tablo. Tüm sorgular, bunun anlamı olan eski verilerin yaymasına izin ve yeni (eski verileri tam olarak yaş kadar), [birleşim](https://docs.microsoft.com/azure/kusto/query/unionoperator) sorgularınızı işlecinde, iki veri kümeleri birleştirmek için gerekecektir.

Yeni Azure haberleri, üzerinde kaynağa özgü modunda destek günlükleri Hizmetleri için lütfen Ara [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/) blogu!

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>Sınırlama bilinen: AzureDiagnostics sütun sınırı
500'den fazla sütun olmaması herhangi belirli Azure günlüğü tablosu açık bir sınırı yoktur. Bu sınıra ulaşıldıktan sonra ilk 500 dışında herhangi bir sütun ile verileri içeren herhangi bir satır alma zamanında bırakılır. AzureDiagnostics belirli açık olması için bu sınır etkilenen tablodur. Bu genellikle ya da çok çeşitli veri kaynaklarından gönderilir çünkü aynı çalışma alanına veya birden fazla ayrıntılı veri kaynakları aynı çalışma alanına gönderilen olur. 

#### <a name="azure-data-factory"></a>Azure Data Factory  
Azure Data Factory, çok ayrıntılı günlükleri, birtakım nedeniyle bu sınırı tarafından etkilenmiş özellikle bilinen bir kaynaktır. Özellikle, kaynağa özgü önce yapılandırılmış tüm tanılama ayarları için modu etkin veya açıkça A'ya ters uyumluluk açısından kaynağa özgü modunu kullanın:  
- *Herhangi bir etkinlik, işlem hattındaki karşı tanımlanan kullanıcı parametreleri*: etkinliklere karşı her benzersiz olarak adlandırılmış kullanıcı parametresi için oluşturulan yeni bir sütun olur. 
- *Etkinlik giriş ve çıkışları*: Bu etkinliğin etkinlik değişir ve ayrıntılı doğasını nedeniyle çok sayıda sütun oluştur. 
 
Olarak daha geniş geçici çözüm teklifleri ile aşağıdaki, kaynağa özgü modu olabildiğince çabuk kullanmak için günlüklerinizi geçirmek için önerilir. Hemen bunu bulamıyorsanız, geçiş, çalışma alanlarında toplanmakta olan diğer günlük türlerini etkileyen Bu günlükleri olasılığını en aza indirmek için kendi çalışma alanına ADF günlükleri yalıtmak için alternatiftir. 
 
#### <a name="workarounds"></a>Geçici Çözümler
Kısa terimi kadar tüm Azure Hizmetleri kaynağa özgü modunda, herhangi bir hizmeti için etkin değil ancak kaynağa özgü modunu destekleyen, azaltmak için ayrıntılı veri türleri farklı çalışma alanları halinde bu hizmetler tarafından yayımlanan ayırmak için önerilir limitini aştıktan olanağı.  
 
Uzun vadeli, Azure tanılama kaynağa özgü modunu destekleyen tüm Azure hizmetleri taşıma olacaktır. Taşıma olabildiğince çabuk tarafından bu 500 sütun sınırlaması etkilenmesine olasılığını azaltmak için bu moda öneririz.  


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Tanılama Günlükleri](diagnostic-logs-overview.md)

