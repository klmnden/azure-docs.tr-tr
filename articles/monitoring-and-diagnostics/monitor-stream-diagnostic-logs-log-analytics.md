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
ms.date: 11/01/2017
ms.author: johnkem
ms.openlocfilehash: 517ce3547f471dd1b40c79b2f087b02ad7f51b85
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Akış günlük analizi için Azure tanılama günlükleri
**[Azure tanılama günlüklerini](monitoring-overview-of-diagnostic-logs.md)**  portalı, PowerShell cmdlet'leri veya Azure CLI kullanarak Azure günlük analizi için yakın gerçek zamanlı akış.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Günlük analizi günlüklerini Tanılama ile yapabilecekleriniz

Azure günlük analizi Azure kaynaklarından oluşturulan ham günlük verileri kavramanıza olanak tanıyan bir esnek günlük arama ve analizi aracıdır. Bazı özellikleri şunlardır:

* **Günlük arama** -yazma gelişmiş sorguları günlük verilerinizi, correlate günlükleri çeşitli kaynaklardan ve hatta Azure panonuza sabitlenir grafikleri oluşturun.
* **Uyarı** -bir veya daha fazla olayları belirli bir sorgu eşleşen ve bir e-posta veya Web kancası çağrısı ile bildirim hale algılayabilir.
* **Çözümleri** -önceden oluşturulmuş görünümler ve günlük verileriniz hakkında anında bilgi vermek panoları kullanın.
* **Gelişmiş analizler** - machine learning uygulanır ve günlüklerinizi tarafından açığa olası sorunları tanımlamak için eşleşen algoritmaları desen.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Günlük analizi için tanılama günlükleri akışını etkinleştirmek
Tanılama günlüklerini portalı yoluyla programlı olarak akış veya kullanarak etkinleştirebilirsiniz [Azure İzleyici REST API'lerini](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). Her iki durumda da, oluşturduğunuz bir tanılama ayarını günlük analizi çalışma alanı ve günlük kategorileri ve bu çalışma alanına göndermek istediğiniz ölçümleri, belirttiğiniz içinde. Bir tanılama **günlük kategori** kaynak sağlayabilir günlük türüdür.

Günlük analizi çalışma alanı ayar yapılandıran kullanıcı her iki aboneliğin uygun RBAC erişime sahip olduğu sürece günlükleri yayma kaynak ile aynı abonelikte olması gerekmez.

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

### <a name="via-azure-cli"></a>Via Azure CLI
Aracılığıyla akışını etkinleştirmek için [Azure CLI](insights-cli-samples.md), kullanabileceğiniz `insights diagnostic set` komut şöyle:

```azurecli
azure insights diagnostic set --resourceId <resourceID> --workspaceId <workspace resource ID> --categories <list of categories> --enabled true
```

Workspaceıd özelliği olmayan çalışma alanı kimliği/anahtar günlük analizi portalında gösterilen çalışma alanı tam Azure kaynak Kimliğini alır.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Günlük analizi verileri nasıl sorgu?

Portal ya da günlük analizi bir parçası olarak Advanced Analytics deneyimi günlük arama dikey penceresinde, günlük yönetim çözümü AzureDiagnostics tablonun altında bir parçası olarak tanılama günlüklerini sorgulayabilirsiniz. Ayrıca [Azure kaynakları için çeşitli çözümler](../log-analytics/log-analytics-add-solutions.md) günlük analizi gönderiyorsunuz daha iyi günlük verilerini almak için yükleyebilir.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure tanılama günlükleri hakkında daha fazla bilgi](monitoring-overview-of-diagnostic-logs.md)
