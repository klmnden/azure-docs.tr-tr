---
title: Stream Log analytics'e Azure tanılama günlükleri
description: Azure tanılama günlükleri Log Analytics çalışma alanına akışı yapmayı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: c419a3c44a38f72d56f2b7b362c62e683fc20c7f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993026"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Stream Log analytics'e Azure tanılama günlükleri

**[Azure tanılama günlükleri](monitoring-overview-of-diagnostic-logs.md)**  portal, PowerShell cmdlet'leri veya Azure CLI kullanarak Azure Log analytics'e neredeyse gerçek zamanlı akış.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Tanılama ile neler yapabileceğinizi Log Analytics'te günlüğe kaydeder

Azure Log Analytics, Azure kaynaklarından oluşturulan ham günlük verilerini öngörü sağlayan bir esnek günlük arama ve analiz aracıdır. Bazı özellikler şunlardır:

* **Günlük araması** -günlük verileriniz üzerinde yazma Gelişmiş sorgular, performanstaki günlükleri, çeşitli kaynaklardan ve hatta Azure panonuza sabitlenebilir grafikler oluşturun.
* **Uyarı** -bir veya daha fazla olay belirli sorguyla eşleşen ve bir e-posta veya Web kancası çağrısı ile bildirilmesi algılayabilir.
* **Çözümleri** -önceden oluşturulmuş görünümleri ve günlük verileriniz hakkında anında bilgi vermek panoları'nı kullanın.
* **Gelişmiş analiz** - makine öğrenimi uygulayın ve günlüklerinizi tarafından ortaya olası sorunları belirlemek için eşleşen algoritmalar desen.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Log Analytics için tanılama günlüklerinin akışı etkinleştirme

Tanılama günlüklerini programlı olarak portal, akış veya kullanarak etkinleştirebilirsiniz [Azure İzleyici REST API'leri](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). Tanılama ayarını oluşturduğunuz her iki durumda da bir Log Analytics çalışma alanı ve günlük kategorileri ve bu çalışma alanına göndermek istediğiniz ölçümleri, belirttiğiniz içinde. Bir tanılama **günlüğü kategorisi** kaynak sağlayabilir günlük türüdür.

Log Analytics çalışma ayarı yapılandıran kullanıcının her iki abonelik için uygun RBAC erişimine sahip olduğu sürece, günlükleri yayan kaynak ile aynı abonelikte olması gerekmez.

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Portalı kullanarak Stream tanılama günlükleri
1. Portal, Azure İzleyicisi'ne gidin ve tıklayarak **tanılama ayarları**

    ![Azure İzleyicisi İzleme](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-blade.png)

2. İsteğe bağlı olarak kaynak grubu veya kaynak türe göre listeyi filtreleyin ve kaynağın tanılama ayarını ayarlamak istediğiniz'i tıklatın.

3. Hiçbir ayar kaynağı varsa, istenen bir ayar oluşturmak için seçtiğiniz. "Tanılamayı Aç" tıklayın

   ![Tanılama ayarını - mevcut hiçbir ayar Ekle](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-none.png)

   Kaynakta mevcut ayarları varsa, bu kaynakta zaten yapılandırılmış ayarların bir listesini görürsünüz. "Tanılama ayarı Ekle" ye tıklayın

   ![Tanılama ayarını - var olan ayarları Ekle](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-multiple.png)

3. Ayarı, bir ad verin ve kutusunu işaretlemeniz **Log Analytics'e gönderme**, sonra bir Log Analytics çalışma alanı seçin.

   ![Tanılama ayarını - var olan ayarları Ekle](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-configure.png)

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni ayar, bu kaynak için ayarların listesi görüntülenir ve tanılama günlükleri, yeni olay verilerini oluşturulan hemen sonra bu çalışma alanına aktarılır. Beş dakika arasında bir olay olduğunda yayılır ve Log Analytics'te görüntülendiğinde olabileceğini unutmayın.

### <a name="via-powershell-cmdlets"></a>PowerShell cmdlet'leri
Aracılığıyla akışını etkinleştirmek için [Azure PowerShell cmdlet'leri](insights-powershell-samples.md), kullanabileceğiniz `Set-AzureRmDiagnosticSetting` cmdlet şu parametrelerle:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Tam Azure kaynak kimliği değil çalışma alanı kimliği/anahtarı Log Analytics Portalı'nda gösterilen çalışma alanının çalışma alanı kimliği özelliği aldığını unutmayın.

### <a name="via-azure-cli"></a>Azure CLI

Aracılığıyla akışını etkinleştirmek için [Azure CLI](insights-cli-samples.md), kullanabileceğiniz [az İzleyici diagnostic-settings oluşturma](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) komutu.

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

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Log analytics'te verileri nasıl sorgu?

Portalında veya Gelişmiş analiz deneyimi Log analytics'in bir parçası olarak günlük arama dikey penceresinde, günlük yönetimi çözümü AzureDiagnostics tablonun altında bir parçası olarak tanılama günlükleri sorgulayabilir. Ayrıca [Azure kaynakları için çeşitli çözümler](../log-analytics/log-analytics-add-solutions.md) günlük verileri anında Öngörüler Log Analytics'e gönderme, alma için yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Tanılama Günlükleri](monitoring-overview-of-diagnostic-logs.md)
