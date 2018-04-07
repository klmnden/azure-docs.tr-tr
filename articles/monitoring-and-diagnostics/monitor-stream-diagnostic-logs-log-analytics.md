---
title: Akış günlük analizi için Azure tanılama günlüklerini | Microsoft Docs
description: Günlük analizi çalışma alanı için Azure tanılama günlüklerini akış öğrenin.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2018
ms.author: johnkem
ms.openlocfilehash: 82011126375a3c5016e110aac9ce6bc1b2d59cdf
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Akış günlük analizi için Azure tanılama günlükleri

**[Azure tanılama günlüklerini](monitoring-overview-of-diagnostic-logs.md)**  portalı, PowerShell cmdlet'leri veya Azure CLI 2.0 kullanarak Azure günlük analizi için yakın gerçek zamanlı akış.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Günlük analizi günlüklerini Tanılama ile yapabilecekleriniz

Azure günlük analizi Azure kaynaklarından oluşturulan ham günlük verileri kavramanıza olanak tanıyan bir esnek günlük arama ve analizi aracıdır. Bazı özellikleri şunlardır:

* **Günlük arama** -yazma gelişmiş sorguları günlük verilerinizi, correlate günlükleri çeşitli kaynaklardan ve hatta Azure panonuza sabitlenir grafikleri oluşturun.
* **Uyarı** -bir veya daha fazla olayları belirli bir sorgu eşleşen ve bir e-posta veya Web kancası çağrısı ile bildirim hale algılayabilir.
* **Çözümleri** -önceden oluşturulmuş görünümler ve günlük verileriniz hakkında anında bilgi vermek panoları kullanın.
* **Gelişmiş analizler** - machine learning uygulanır ve günlüklerinizi tarafından açığa olası sorunları tanımlamak için eşleşen algoritmaları desen.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Günlük analizi için tanılama günlükleri akışını etkinleştirmek

Tanılama günlüklerini portalı yoluyla programlı olarak akış veya kullanarak etkinleştirebilirsiniz [Azure İzleyici REST API'lerini](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). Her iki durumda da, oluşturduğunuz bir tanılama ayarını günlük analizi çalışma alanı ve günlük kategorileri ve bu çalışma alanına göndermek istediğiniz ölçümleri, belirttiğiniz içinde. Bir tanılama **günlük kategori** kaynak sağlayabilir günlük türüdür.

Günlük analizi çalışma alanı ayar yapılandıran kullanıcı her iki aboneliğin uygun RBAC erişime sahip olduğu sürece günlükleri yayma kaynak ile aynı abonelikte olması gerekmez.

> [!NOTE]
> Çok boyutlu ölçümleri tanılama ayarları aracılığıyla gönderme şu anda desteklenmiyor. Ölçümleri boyutlarla boyut değerleri toplanan düzleştirilmiş tek boyutlu ölçümleri olarak dışarı aktarılır.
>
> *Örneğin*: bir olay hub'ındaki 'Gelen iletileri' Ölçüm incelediniz ve üzerinde grafiğinin bir sıra gerçekleştiriliyordu. Ancak, ölçüm gelen tüm iletilerin tüm temsil edilir tanılama ayarları aracılığıyla dışarı aktardığınızda olay hub'ı sıralar.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Portalı kullanarak akış tanılama günlükleri
1. Portal, Azure izleyicisine gidin ve tıklayın **tanılama ayarları**

    ![Azure İzleyicisi İzleme bölümü](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-blade.png)

2. İsteğe bağlı olarak kaynak grubu veya kaynak türü listeyi filtrelemek sonra tanılama ayarını ayarlamak istediğiniz kaynak'ı tıklatın.

3. Hiçbir ayar kaynağı varsa, sizden istenir bir ayar oluşturmak için seçtiğiniz. "Tanılamayı açın."'i tıklatın

   ![Tanılama ayarını - ayar Ekle](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-none.png)

   Kaynak üzerinde var olan ayarları varsa, bu kaynak üzerinde zaten yapılandırılmış ayarları listesini görürsünüz. "Tanılama ayarını Ekle" yi tıklatın.

   ![Tanılama ayarını ayarlar varolan - Ekle](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-multiple.png)

3. Ayarı, bir ad verin ve için kutuyu **için günlük analizi Gönder**, günlük analizi çalışma alanı seçin.

   ![Tanılama ayarını ayarlar varolan - Ekle](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-configure.png)

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni ayar, bu kaynak için ayarları listesi görüntülenir ve yeni olay verilerini oluşturulan hemen tanılama günlükleri, çalışma alanına akışa alınır. En fazla beş dakikada bir olay olduğunda yayılan ve günlük analizi görüntülendiğinde arasında olabileceğini unutmayın.

### <a name="via-powershell-cmdlets"></a>PowerShell cmdlet'leri
Aracılığıyla akışını etkinleştirmek için [Azure PowerShell cmdlet'leri](insights-powershell-samples.md), kullanabileceğiniz `Set-AzureRmDiagnosticSetting` Bu parametreler cmdlet'iyle:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Workspaceıd özelliği olmayan çalışma alanı kimliği/anahtar günlük analizi portalında gösterilen çalışma alanı tam Azure kaynak Kimliğini alır.

### <a name="via-azure-cli-20"></a>Via Azure CLI 2.0

Aracılığıyla akışını etkinleştirmek için [Azure CLI 2.0](insights-cli-samples.md), kullanabileceğiniz [az İzleyici tanılama ayarlarını oluştur](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) komutu.

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

Ek kategoriler tanılama günlük olarak geçirilen JSON dizisine sözlükler ekleyerek ekleyebileceğiniz `--logs` parametresi.

`--resource-group` Bağımsız değişken, yalnızca gerekli olduğunda `--workspace` bir nesne kimliği değil

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Günlük analizi verileri nasıl sorgu?

Portal ya da günlük analizi bir parçası olarak Advanced Analytics deneyimi günlük arama dikey penceresinde, günlük yönetim çözümü AzureDiagnostics tablonun altında bir parçası olarak tanılama günlüklerini sorgulayabilirsiniz. Ayrıca [Azure kaynakları için çeşitli çözümler](../log-analytics/log-analytics-add-solutions.md) günlük analizi gönderiyorsunuz daha iyi günlük verilerini almak için yükleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure tanılama günlükleri hakkında daha fazla bilgi](monitoring-overview-of-diagnostic-logs.md)
