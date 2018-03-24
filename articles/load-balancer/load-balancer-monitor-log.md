---
title: Yük Dengeleyici için işlemleri, olayları ve sayaçları izleme | Microsoft Docs
description: Uyarı olayları etkinleştir ve Azure yük dengeleyici için sistem durumu günlüğü araştırma hakkında bilgi edinin
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: dabf4bcae957559978e731636bb13554f1a68b73
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="log-analytics-for-azure-load-balancer"></a>Azure Load Balancer için günlük analizi

>[!NOTE] 
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Temel Yük Dengeleyici anlatılmaktadır. Standart yük dengeleyici hakkında daha fazla bilgi için bkz: [standart yük dengeleyici genel bakış](load-balancer-standard-overview.md).

Azure'da günlükleri farklı türlerde yönetmek ve yük dengeleyici sorunlarını gidermek için kullanabilirsiniz. Bu günlükler bazıları Portalı aracılığıyla erişilebilir. Tüm günlükleri Azure blob depolama alanından ayıklanan ve farklı araçlar, Excel ve Powerbı gibi görüntülenebilir. Aşağıdaki listeden günlükleri farklı türleri hakkında daha fazla bilgi edinebilirsiniz.

* **Denetim günlüklerini:** kullanabileceğiniz [Azure denetim günlükleri](../monitoring-and-diagnostics/insights-debugging-with-events.md) (eskiden İşlem günlükleri olarak bilinir) Azure Abonelikleriniz ve durumlarını gönderilen tüm işlemleri görüntülemek için. Denetim günlüklerini varsayılan olarak etkindir ve Azure Portalı'nda görüntülenebilir.
* **Uyarı olayı günlükleri:** yük dengeleyici tarafından oluşturulan uyarılar görüntülemek için bu günlük kullanabilirsiniz. Yük Dengeleyici için durum her beş dakikada toplanır. Bu günlük yalnızca bir yük dengeleyici uyarı olayı tetiklenir yazılır.
* **Sistem durumu araştırma günlüklerini:** , arka uç havuzundaki istekleri sistem durumu araştırma hataları nedeniyle yük dengeleyiciden gösterilmeyen örnek sayısı gibi sistem durumu araştırma tarafından algılanan sorunları görüntülemek için bu günlük kullanabilirsiniz. Sistem durumu araştırma durumundaki bir değişiklik olduğunda bu günlük yazılır.

> [!IMPORTANT]
> Günlük analizi yük dengeleyici yalnızca Internet'e yönelik için şu anda çalışıyor. Günlükleri yalnızca Resource Manager dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynakların günlükleri kullanamazsınız. Dağıtım modelleri hakkında daha fazla bilgi için bkz: [anlama Resource Manager dağıtımını ve klasik dağıtımı](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Denetim günlüğü, her Resource Manager kaynak için otomatik olarak etkinleştirilir. Olay ve bu günlükleri kullanılabilir veri toplama başlatmak için araştırma sistem günlüğü etkinleştirmeniz gerekir. Günlük kaydını etkinleştirmek için aşağıdaki adımları kullanın.

Oturum açma için [Azure portal](http://portal.azure.com). Bir yük dengeleyici, henüz yoksa [bir yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) devam etmeden önce.

1. Portalı'nda tıklatın **Gözat**.
2. Seçin **yük dengeleyici**.

    ![Portal - yük dengeleyici](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Mevcut bir Yük Dengeleyiciyi seçin >> **tüm ayarları**.
4. Yük Dengeleyici adı altında iletişim kutusunun sağ tarafta kaydırın **izleme**, tıklatın **tanılama**.

    ![Portal - yük dengeleyici ayarları](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. İçinde **tanılama** bölmesi altında **durum**seçin **üzerinde**.
6. Tıklatın **depolama hesabı**.
7. Altında **GÜNLÜKLERİ**, mevcut bir depolama hesabını seçin veya yeni bir tane oluşturun. Olay verilerini kaç gün olay günlüklerinde depolanacak belirlemek için kaydırıcıyı kullanın. 
8. **Kaydet**’e tıklayın.

    ![Portal - tanılama günlükleri](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Denetim günlükleri ayrı bir depolama hesabı gerektirmez. Kullanım olay ve sistem durumu depolama araştırma günlük kaydı hizmeti ücret uygulanabilir.

## <a name="audit-log"></a>Denetleme günlüğü

Denetim günlüğü varsayılan olarak oluşturulur. Günlükleri 90 gün boyunca Azure'nın olay günlüklerini deposunda saklanır. Bu günlükler hakkında daha fazla bilgi okuyarak [olayları görüntülemek ve Denetim günlükleri](../monitoring-and-diagnostics/insights-debugging-with-events.md) makale.

## <a name="alert-event-log"></a>Uyarı olay günlüğü

Bu günlük yalnızca üzerinde etkinleştirdikten, oluşturulan bir yük dengeleyici temelinde. Olayları JSON biçiminde günlüğe kaydedilen ve günlüğe kaydetme etkinleştirildiğinde, belirtilen depolama hesabında depolanır. Bir olayın bir örnek verilmiştir.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

JSON çıktısını gösterir *eventname* yük dengeleyici nedenini açıklayan özelliğini bir uyarı oluşturdu. Bu durumda, oluşturulan uyarı TCP bağlantı noktası Tükenme nedeniyle IP NAT kaynağı tarafından sınırları (SNAT) neden oldu.

## <a name="health-probe-log"></a>Sistem durumu araştırma günlük

Bu günlük yalnızca üzerinde etkinleştirdikten, oluşturulan bir yük dengeleyici temel olarak ayrıntılı yukarıdaki başına. Veri günlük kaydı etkinleştirildiğinde, belirtilen depolama hesabında depolanır. 'Öngörüler günlükleri loadbalancerprobehealthstatus' adlı bir kapsayıcı oluşturulur ve aşağıdaki veriler günlüğe kaydedilir:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

JSON çıktısını özellikleri alanında araştırma sistem durumu için temel bilgileri gösterir. *DipDownCount* özelliği, ağ trafiğini başarısız araştırma yanıtları nedeniyle gösterilmeyen uç üzerinde örnekleri toplam sayısını gösterir.

## <a name="view-and-analyze-the-audit-log"></a>Görüntüleme ve denetim günlüğü çözümleme

Görüntüleme ve aşağıdaki yöntemlerden birini kullanarak denetim günlüğü verilerini çözümleme:

* **Azure Araçları:** bilgi almanızı Azure PowerShell, Azure komut satırı arabirimi (CLI), Azure REST API veya Azure preview portal aracılığıyla denetim günlükleri. Her yöntem için adım adım yönergeler ayrıntılı olarak [denetim işlemleri Resource Manager ile](../azure-resource-manager/resource-group-audit.md) makalesi.
* **Power BI:** zaten yoksa bir [Power BI](https://powerbi.microsoft.com/pricing) hesabı deneyebilirsiniz, ücretsiz. Kullanarak [Azure denetim günlükleri paketi Power BI için içerik](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), önceden yapılandırılmış panolar verilerinizle analiz edebilirsiniz veya görünümler gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Görüntüleme ve durum araştırması ve olay günlüğü çözümleme

Depolama hesabınıza bağlanın ve olay ve sistem durumu araştırma günlükleri için JSON günlük girişlerini almak gerekir. JSON dosyaları yükledikten sonra CSV ve Excel, Powerbı veya başka bir veri görselleştirme araç görünümünde dönüştürebilirsiniz.

> [!TIP]
> Visual Studio ve sabitleri ve değişkenleri C# değerlerini değiştirmenin temel kavramları biliyorsanız, kullanabileceğiniz [Dönüştürücü Araçları oturum](https://github.com/Azure-Samples/networking-dotnet-log-converter) github'dan kullanılabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure denetim günlükleri Power BI ile görselleştirme](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog postası.
* [Görüntüleme ve Azure denetim günlükleri Power BI ve daha fazla çözümleme](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog postası.

## <a name="next-steps"></a>Sonraki adımlar

[Yük dengeleyici araştırmalarını anlama](load-balancer-custom-probe-overview.md)
