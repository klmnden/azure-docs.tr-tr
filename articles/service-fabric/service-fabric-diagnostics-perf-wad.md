---
title: Azure Service Fabric - performans izleme ile Windows Azure Diagnostics uzantısı | Microsoft Docs
description: Azure Service Fabric kümeleri için performans sayaçları toplamak için Windows Azure Diagnostics kullanın.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: dekapur; srrengar
ms.openlocfilehash: a9e8ef7243fcef990dae6ddc6509cd31b3f36e3d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Windows Azure Diagnostics uzantısıyla performans izleme

Bu belge Windows kümeleri için Windows Azure tanılama (WAD) uzantısı aracılığıyla performans sayaçları koleksiyonunu ayarlamak için gerekli adımlar kapsanmaktadır. Linux kümeleri için ayarlanan [OMS Aracısı](service-fabric-diagnostics-oms-agent.md) düğümleriniz için performans sayaçları toplanamadı. 

 > [!NOTE]
> WAD uzantısı, kümeniz için çalışması bu adımları için dağıtılması gerekir. Bu ayarlanmamışsa, üzerinden için head [olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyon](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters).

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

3. İçin toplamak istediğiniz performans sayaçlarını Ekle `PerformanceCounterConfiguration` önceki adımda bildirilen. Toplamak istediğiniz her sayaç ile tanımlanmış bir `counterSpecifier`, `sampleRate`, `unit`, `annotation`ve tüm ilgili `sinks`.

Sayaç için bir yapılandırma örneği *toplam işlemci zamanı* (CPU işlemleri yürütmek için kullanılan saat miktarı) ve *Service Fabric aktör yöntem çağrılarınasaniyede*, Service Fabric özel performans sayaçları biri. Başvurmak [güvenilir aktör performans sayaçları](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) ve [güvenilir hizmeti performans sayaçları](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) Service Fabric özel performans sayaçlarının tam listesi için.

 ```json
 "WadCfg": {
         "DiagnosticMonitorConfiguration": {
           "overallQuotaInMB": "50000",
           "EtwProviders": {
             "EtwEventSourceProviderConfiguration": [
               {
                 "provider": "Microsoft-ServiceFabric-Actors",
                 "scheduledTransferKeywordFilter": "1",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableActorEventTable"
                 }
               },
               {
                 "provider": "Microsoft-ServiceFabric-Services",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableServiceEventTable"
                 }
               }
             ],
             "EtwManifestProviderConfiguration": [
               {
                 "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                 "scheduledTransferLogLevelFilter": "Information",
                 "scheduledTransferKeywordFilter": "4611686018427387904",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricSystemEventTable"
                 }
               }
             ]
           },
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
                     },
                     {
                         "counterSpecifier": "\\Service Fabric Actor Method(*)\\Invocations/Sec",
                         "sampleRate": "PT1M",
                     }
                 ]
             }
         }
       },
  ```

 Sayaç için örnek hızı ihtiyaçlarınıza göre değiştirilebilir. Bu biçimi `PT<time><unit>`, her saniye sayacı istiyorsanız, daha sonra ayarlamanız gerekir böylece `"sampleRate": "PT15S"`.

 >[!NOTE]
 >Kullanabilirsiniz ancak `*` benzer adlandırılmış performans sayaçları gruplarını belirtmek için herhangi bir sayaç (Application Insights'a) bir havuz gönderme bunlar ayrı ayrı bildirilen gerektirir. 

4. Toplanacak gereksinim uygun performans sayaçlarını ekledikten sonra böylece bu değişiklikleri çalışan kümenizdeki yansıtılır küme kaynağı yükseltme yapmanız gerekir. Değiştirilmiş Kaydet `template.json` ve PowerShell açın. Küme kullanarak yükseltme `New-AzureRmResourceGroupDeployment`. Arama kaynak grubu, güncelleştirilmiş şablon dosyası ve parametreler dosyası adı gerektirir ve Resource Manager'ın, güncelleştirilmiş kaynaklarına uygun değişiklik ister. Hesabınızda oturum ve sağ abonelikte sonra yükseltme çalıştırmak için aşağıdaki komutu kullanın:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Yükseltme tamamlandıktan sonra WAD (alır 15-45 dakika arasında), alma performans sayaçlarını toplama olması ve depolama hesabında WADPerformanceCountersTable adlı tabloya göndererek kümenizle ilişkilendirilmiş. Application Insights tarafından performans sayaçları bkz [AI havuz için Resource Manager şablonu ekleme](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template).

## <a name="next-steps"></a>Sonraki adımlar
* Kümeniz için daha fazla performans sayaçlarını toplar. Bkz: [performans ölçümleri](service-fabric-diagnostics-event-generation-perf.md) toplama sayaçları listesi.
* [Kullanımı izleme ve tanılama Windows VM ve Azure Resource Manager şablonları ile](../virtual-machines/windows/extensions-diagnostics-template.md) başka değişiklikler yapmak için `WadCfg`, Tanılama verileri göndermek için ek depolama hesapları yapılandırma dahil olmak üzere.
