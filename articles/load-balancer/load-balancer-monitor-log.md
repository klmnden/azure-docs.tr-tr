---
title: İzleme işlemleri, olayları ve genel temel yük dengeleyici için sayaçlar
titlesuffix: Azure Load Balancer
description: Uyarı olayları etkinleştirmektedir ve genel temel yük dengeleyici için sistem durumu günlüğü araştırma hakkında bilgi edinin
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2018
ms.author: kumud
ms.openlocfilehash: 0d7c792c5230a5d82e97f4598a5dcfb864cead74
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60861189"
---
# <a name="azure-monitor-logs-for-public-basic-load-balancer"></a>Genel temel yük dengeleyici için Azure izleme günlükleri

>[!IMPORTANT] 
>Azure Load Balancer iki farklı türü destekler: Temel ve Standart. Bu makalede Temel Yük Dengeleyici anlatılmaktadır. Standard Load Balancer hakkında daha fazla bilgi için bkz: [standart Load Balancer'a genel bakış](load-balancer-standard-overview.md) telemetri üzerinden çok boyutlu ölçümler Azure İzleyici'de kullanıma sunar.

Günlükleri farklı türde, yönetme ve sorun giderme temel yük Dengeleyiciler için Azure'da kullanabilirsiniz. Bu günlükler bazıları, portal üzerinden erişilebilir. Tüm günlükler Azure blob depolama alanından ayıklanır ve Excel ve Power BI gibi farklı Araçları'nda görüntülenebilir. Günlükleri aşağıdaki listeden farklı türleri hakkında daha fazla bilgi edinebilirsiniz.

* **Denetim günlükleri:** Kullanabileceğiniz [Azure denetim günlükleri](../monitoring-and-diagnostics/insights-debugging-with-events.md) (önceden işlem günlükleri olarak da bilinir) Azure Abonelikleriniz ve durumlarını gönderilen tüm işlemleri görüntülemek için. Denetim günlükleri varsayılan olarak etkindir ve Azure portalında görüntülenebilir.
* **Uyarı olay günlükleri:** Bu günlük, yük dengeleyici tarafından oluşturulan uyarılar görüntülemek için kullanabilirsiniz. Yük Dengeleyici için durum beş dakikada toplanır. Bu günlük yalnızca bir yük dengeleyici uyarı olayı tetiklenir yazılır.
* **Sistem durumu araştırma günlükleri:** Bu günlük, arka uç havuzundaki istekleri sistem durumu araştırma hatası nedeniyle yük dengeleyiciden almıyor örneklerinin gibi sistem durumu araştırması tarafından algılanan sorunları görüntülemek üzere kullanabilirsiniz. Sistem durumu araştırması durumundaki bir değişiklik olduğunda bu günlüğe yazılır.

> [!IMPORTANT]
> Azure İzleyici şu anda yalnızca genel temel yük Dengeleyiciler çalışır günlüğe kaydeder. Günlükleri yalnızca Resource Manager dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynakların günlükleri'ni kullanamazsınız. Dağıtım modelleri hakkında daha fazla bilgi için bkz. [anlama Resource Manager dağıtımını ve klasik dağıtımı](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Denetim günlüğü, her bir Resource Manager kaynağı için otomatik olarak etkinleştirilir. Olay ve bu günlükleri kullanılabilir veri toplamaya başlamak için durum araştırması günlüğe kaydetme etkinleştirmeniz gerekir. Günlüğe kaydetmeyi etkinleştirmek için aşağıdaki adımları kullanın.

Oturum açma için [Azure portalında](https://portal.azure.com). Bir yük dengeleyici henüz yoksa [yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) devam etmeden önce.

1. Portalında **Gözat**.
2. Seçin **yük Dengeleyiciler**.

    ![Portalı - yük dengeleyici](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Mevcut bir Yük Dengeleyiciyi seçin >> **tüm ayarlar**.
4. Yük dengeleyicinin adı altında iletişim kutusunun sağ tarafında kaydırarak **izleme**, tıklayın **tanılama**.

    ![Portalı - yük dengeleyici ayarları](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. İçinde **tanılama** bölmesi altında **durumu**seçin **üzerinde**.
6. Tıklayın **depolama hesabı**.
7. Altında **GÜNLÜKLERİ**, mevcut bir depolama hesabını seçin veya yeni bir tane oluşturun. Kaç gün olay verileri olay günlüklerinde depolanacak belirlemek için kaydırıcıyı kullanın. 
8. **Kaydet**’e tıklayın.

Tanılama belirtilen depolama hesabında tablo depolamaya kaydedilir. Günlükleri kaydediliyor değil, ilgili günlük üretilmiş olmasıdır.

![Portalı - tanılama günlükleri](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Denetim günlükleri ayrı bir depolama hesabı gerektirmez. Kullanım depolama olay sistem durumu araştırması günlüğe kaydetme hizmeti ücret uygulanabilir.

## <a name="audit-log"></a>Denetim günlüğü

Denetim günlüğü varsayılan olarak oluşturulur. Günlükleri, Azure'nın olay günlüklerini deposunda 90 gün boyunca korunur. Bu günlükler hakkında daha fazla bilgi edinmek [olayları görüntülemek ve Denetim günlükleri](../monitoring-and-diagnostics/insights-debugging-with-events.md) makalesi.

## <a name="alert-event-log"></a>Uyarı olay günlüğü

Bunu şirket etkinleştirdiyseniz bu günlük yalnızca oluşturulan bir yük dengeleyici temelinde. Olayları JSON biçiminde günlüğe kaydedilen ve günlüğe kaydetme etkinleştirildiğinde, belirtilen depolama hesabında depolanır. Bir olayın bir örnek verilmiştir.

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

JSON çıktısını gösterir *eventname* yük dengeleyici nedenini açıklayan özelliği bir uyarı oluşturulur. Bu durumda, oluşturulan uyarı TCP bağlantı noktası tükenmesi nedeniyle kaynak IP NAT tarafından sınırları (SNAT) neden oldu.

## <a name="health-probe-log"></a>Sistem durumu araştırması günlüğü

Bunu şirket etkinleştirdiyseniz bu günlük yalnızca oluşturulan bir yük dengeleyici olarak ayrıntılı olarak yukarıdaki. Veri günlük kaydı etkinleştirildiğinde, belirtilen depolama hesabında depolanır. 'Insights günlükleri loadbalancerprobehealthstatus' adlı bir kapsayıcı oluşturulur ve aşağıdaki veriler günlüğe kaydedilir:

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

JSON çıkışını özellikleri alanın sistem durumu araştırması için temel bilgileri gösterir. *DipDownCount* özellik üzerinde hangi makinenin başarısız araştırma yanıtları nedeniyle ağ trafiği almıyor uç örneklerinin toplam sayısı gösterilmiştir.

## <a name="view-and-analyze-the-audit-log"></a>Görüntüleme ve analiz denetim günlüğü

Görüntüleyebilir ve aşağıdaki yöntemlerden birini kullanarak denetim günlüğü verilerini analiz edin:

* **Azure Araçları:** Azure PowerShell, Azure komut satırı arabirimi (CLI), Azure REST API'si veya Azure preview portal aracılığıyla denetim günlüklerinden bilgi alın. Her yöntem için adım adım yönergeleri ayrıntılı olarak [Resource Manager denetim işlemleri](../azure-resource-manager/resource-group-audit.md) makalesi.
* **Power BI:** Zaten yoksa bir [Power BI](https://powerbi.microsoft.com/pricing) hesabı deneyebilirsiniz, ücretsiz. Kullanarak [Azure denetim günlükleri içerik paketi Power BI için](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), önceden yapılandırılmış Panolar ile verilerinizi analiz edin veya görünümleri gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Durum araştırması ve olay günlüğünü görüntüleme ve çözümleme

Depolama hesabınıza bağlanın ve olay ve sistem durumu araştırma günlükleri için JSON günlük girişlerini almak gerekir. JSON dosyalarını yükledikten sonra CSV ve Excel, Powerbı veya herhangi bir veri görselleştirme aracını görünümünde dönüştürme yapabilirsiniz.

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Power BI ile Azure denetim günlüklerinizi görselleştirin](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog gönderisi.
* [Görüntüleme ve Power BI ve Azure denetim günlükleri analiz](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog gönderisi.

## <a name="next-steps"></a>Sonraki adımlar

[Yük dengeleyici araştırmalarını anlama](load-balancer-custom-probe-overview.md)
