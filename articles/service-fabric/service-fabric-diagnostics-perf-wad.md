---
title: Azure Service Fabric - performans izleme ile Windows Azure tanılama uzantısı | Microsoft Docs
description: Azure Service Fabric kümeleriniz için performans sayaçları toplamak için Windows Azure Tanılama'yı kullanın.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: srrengar
ms.openlocfilehash: f99206fe673f69c78bf130026207ed58344ccea5
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324434"
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Windows Azure tanılama uzantısı ile performans izleme

Bu belge Windows kümeleri için Windows Azure tanılama (WAD) uzantısı aracılığıyla performans sayaçlarını toplamayı ayarlamak için gerekli adımları kapsar. Linux kümeleri için ayarlanan [OMS Aracısı](service-fabric-diagnostics-oms-agent.md) performans sayaçları düğümlerinize için toplanacak. 

 > [!NOTE]
> WAD uzantısı, sizin için işe için bu adımları için kümenizde dağıtılmalıdır. Ayarlandığına değil, attıktan [olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyon](service-fabric-diagnostics-event-aggregation-wad.md).  

## <a name="collect-performance-counters-via-the-wadcfg"></a>WadCfg performans sayaçlarını Topla

WAD ile performans sayaçları toplamak için kümenin Resource Manager şablonu yapılandırmada uygun şekilde değiştirmeniz gerekir. Şablonunuza toplamak ve bir Resource Manager kaynak yükseltmesi çalıştırmak için istediğiniz performans sayaç eklemek için aşağıdaki adımları izleyin.

1. WAD yapılandırması kümenizin şablonunda bulma - bulma `WadCfg`. Altında Toplanacak performans sayaçlarını ekleme `DiagnosticMonitorConfiguration`.

2. Aşağıdaki bölümüne ekleyerek, performans sayaçları toplamak için yapılandırmasını ayarlayın, `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    `scheduledTransferPeriod` Frquently toplanan sayaçlarını değerlerini aktarılır, Azure depolama tablosuna ve herhangi bir havuz nasıl yapılandırılacağı tanımlar. 

3. İçin toplamak istediğiniz performans sayaçlarını Ekle `PerformanceCounterConfiguration` önceki adımda bildirildi. Toplamak istediğiniz her bir sayacın ile tanımlanmış bir `counterSpecifier`, `sampleRate`, `unit`, `annotation`ve tüm ilgili `sinks`.

İşte bir örnek için sayaç ile yapılandırmasının *toplam işlemci zamanı* (CPU işleme için kullanılan saat miktarı) ve *Service Fabric aktör yöntem çağrılarısaniyede*, bir Service Fabric özel performans sayaçları. Başvurmak [güvenilir aktör performans sayaçları](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) ve [güvenilir hizmeti performans sayaçları](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) Service Fabric özel performans sayaçlarının tam listesi için.

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

 Sayaç için örnek hızı ihtiyaçlarınıza göre değiştirilebilir. Bu biçimi `PT<time><unit>`, her saniye sayacı isterseniz, daha sonra ayarlamalısınız `"sampleRate": "PT15S"`.

 >[!NOTE]
 >Kullanabilirsiniz ancak `*` benzer adlandırılmış performans sayaçlarının grupları belirlemek için tüm sayaçları bir havuz (Application Insights) gönderme bunlar ayrı olarak bildirilen gerektirir. 

4. Toplanması gereken uygun performans sayaçlarını ekledikten sonra böylece bu değişiklikler, çalışan kümenizin yansıtılır, küme kaynağı yükseltmeniz gerekir. Değiştirdiğiniz Kaydet `template.json` ve PowerShell açın. Küme kullanarak yükseltebilirsiniz `New-AzureRmResourceGroupDeployment`. Arama, kaynak grubunu, güncelleştirilmiş şablon dosyası ve parametreleri dosyası adı gerektirir ve Resource Manager'ın güncelleştirdiğiniz kaynaklara gerekli değişiklikleri yapmanızı ister. Hesabınızda oturum açmış ve doğru abonelikte sonra yükseltmeyi gerçekleştirmek için aşağıdaki komutu kullanın:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Yükseltme tamamlandıktan sonra WAD (alır 15-45 dakika arasında), sıralı performans sayaçlarını toplama olması ve kümenizle ilişkili depolama hesabında WADPerformanceCountersTable adlı tabloda gönderme. Application Insights ile performans Sayaçlarınızı bkz [Resource Manager şablonuna AI havuz ekleme](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template).

## <a name="next-steps"></a>Sonraki adımlar
* Kümenizi daha fazla performans sayacını toplar. Bkz: [performans ölçümlerini](service-fabric-diagnostics-event-generation-perf.md) toplama sayaçları listesi.
* [Kullanımı izleme ve Tanılama'yı bir Windows VM ve Azure Resource Manager şablonları ile](../virtual-machines/windows/extensions-diagnostics-template.md) başka değişiklikler yapmak için `WadCfg`, Tanılama verileri göndermesini ek depolama hesaplarını yapılandırma dahil olmak üzere.
