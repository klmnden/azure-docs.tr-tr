---
title: "Azure Service Fabric - performans izleme ile Windows Azure Diagnostics uzantısı | Microsoft Docs"
description: "Azure Service Fabric kümeleri için performans sayaçları toplamak için Windows Azure Diagnostics kullanın."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/05/2017
ms.author: dekapur
ms.openlocfilehash: f71c483eba201eae91c15b84a2ed42fcb68ae575
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="performance-monitoring-with-windows-azure-diagnostics-extension"></a>Windows Azure Diagnostics uzantısıyla performans izleme

Bu belge Windows kümeleri için Windows Azure tanılama (WAD) uzantısı aracılığıyla performans sayaçları koleksiyonunu ayarlamak için gerekli adımlar kapsanmaktadır. Linux kümeleri için ayarlanan [OMS Aracısı](service-fabric-diagnostics-oms-agent.md) düğümleriniz için performans sayaçları toplanamadı. 

 > [!NOTE]
> WAD uzantısı, kümeniz için çalışması bu adımları için dağıtılması gerekir. Bunu ayarlanmadı, üzerinden için head [olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyon](service-fabric-diagnostics-event-aggregation-wad.md) ve adımları *tanılama uzantısını dağıtma*.

## <a name="collect-performance-counters-via-the-wadcfg"></a>Performans sayaçları WadCfg Topla

Performans sayaçları WAD toplamak için kümenin Resource Manager şablonu yapılandırmasında uygun şekilde değiştirmeniz gerekir. Şablonunuza toplamak ve Resource Manager kaynak yükseltme çalıştırmak istediğiniz bir performans sayacı eklemek için aşağıdaki adımları izleyin.

1. Kümenizin şablonunda WAD yapılandırması bulunamıyor - bulma `WadCfg`. Altında toplamak için performans sayaçlarını ekleme `DiagnosticMonitorConfiguration`.

2. Aşağıdaki bölümde ekleyerek performans sayaçları toplanamadı yapılandırmanızı ayarlayın, `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    `scheduledTransferPeriod` Nasıl toplanır sayaçların değerleri, Azure depolama tabloya ve herhangi bir aktarılır frquently havuz yapılandırılmış tanımlar. 

3. İçin toplamak istediğiniz performans sayaçlarını Ekle `PerformanceCounterConfiguration` önceki adımda bildirilen. Toplamak istediğiniz her sayaç ile tanımlanmış bir `counterSpecifier`, `sampleRate`, `unit`, `annotation`ve tüm ilgili `sinks`. Performans sayacı için bir örneği burada verilmiştir *toplam işlemci zamanı* (CPU işlemleri yürütmek için kullanılan saat miktarı):

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": [
            {
                "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                "sampleRate": "PT1M",
                "unit": "Percent",
                "annotation": [
                ],
                "sinks": ""
            }
        ]
    }
    ```

    Sayaç için örnek hızı ihtiyaçlarınıza göre değiştirilebilir. Bu biçimi `PT<time><unit>`, her saniye sayacı istiyorsanız, daha sonra ayarlamanız gerekir böylece `"sampleRate": "PT15S"`.

    Listesine ekleyerek diğer birkaç performans sayaçları, benzer şekilde, toplayabilirsiniz `PerformanceCounterConfiguration`. Kullanabilirsiniz ancak `*` benzer adlandırılmış performans sayaçları gruplarını belirtmek için herhangi bir sayaç (Application Insights'a) bir havuz gönderme bunlar ayrı ayrı bildirilen gerektirir. 

4. Size toplanacak gereksinim uygun performans sayaçlarını eklendiğinde, böylece bu değişiklikleri çalışan kümenizdeki yansıtılır küme kaynağı yükseltme yapmanız gerekir. Değiştirilmiş Kaydet `template.json` ve PowerShell açın. Küme kullanarak yükseltme `New-AzureRmResourceGroupDeployment`. Arama kaynak grubu, güncelleştirilmiş şablon dosyası ve parametreler dosyası adı gerektirir ve Resource Manager'ın, güncelleştirilmiş kaynaklarına uygun değişiklik ister. Hesabınızda oturum ve sağ abonelikte sonra yükseltme çalıştırmak için aşağıdaki komutu kullanın:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Yükseltme tamamlandıktan sonra (alır 15-45 dakika arasında), WAD çalışırken olması toplama performans sayaçları ve içinde bildirilen depolama hesabındaki bir tabloya göndererek `WadCfg`.

## <a name="next-steps"></a>Sonraki adımlar
* Application Insights ekleyerek, performans sayaçları Application Insights havuz için bkz, `WadCfg`. Okuma *AI havuz için Resource Manager şablonu Ekle* bölümüne [olay çözümleme ve görselleştirme Application Insights ile ](service-fabric-diagnostics-event-analysis-appinsights.md) Application Insights havuzunu yapılandırmak için.
* Kümeniz için daha fazla performans sayaçlarını toplar. Bkz: [performans ölçümleri](service-fabric-diagnostics-event-generation-perf.md) toplama sayaçları listesi.
* [Kullanımı izleme ve tanılama Windows VM ve Azure Resource Manager şablonları ile](../virtual-machines/windows/extensions-diagnostics-template.md) başka değişiklikler yapmak için `WadCfg`, Tanılama verileri göndermek için ek depolama hesapları yapılandırma dahil olmak üzere.