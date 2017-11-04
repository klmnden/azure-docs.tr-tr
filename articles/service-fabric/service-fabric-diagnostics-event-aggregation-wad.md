---
title: "Windows Azure Service Fabric olay toplama Azure tanılama | Microsoft Docs"
description: "Toplama ve izleme ve tanılama Azure Service Fabric kümeleri için WAD kullanarak olay toplama hakkında bilgi edinin."
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
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 5773361fdec4cb8ee54fa2856f6aa969d5dac4e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyonu
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Azure Service Fabric kümesi çalıştırırken, merkezi bir konumda tüm düğümlerdeki günlükleri toplamak için iyi bir fikirdir. Merkezi bir konumda günlükler sahip çözümlemek ve sorunları kümenizdeki veya bu kümede çalışan hizmetler ve uygulamalar sorunları gidermenize yardımcı olur.

Karşıya yükleme ve günlükleri toplamak için bir yol günlükleri Azure Storage'a yükler ve ayrıca Azure Application Insights veya olay hub'ları için günlükleri gönderme seçeneği içeren Windows Azure tanılama (WAD) uzantısı kullanmaktır. Olayları depolama alanından okuyun ve bunları bir analiz platformu üründe gibi yerleştirmek için bir dış işlem kullanabilirsiniz [OMS günlük analizi](../log-analytics/log-analytics-service-fabric.md) veya başka bir çözüm günlük ayrıştırma.

## <a name="prerequisites"></a>Ön koşullar
Bu araçlar, bu belgede bazı işlemleri gerçekleştirmek için kullanılır:

* [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) (Azure bulut Hizmetleri ile ilgili ancak iyi bilgi ve örnekler var)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager istemci](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager şablonu](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a>Günlük ve olay kaynakları

### <a name="service-fabric-platform-events"></a>Service Fabric platform olayları
' Da anlatıldığı gibi [bu makalede](service-fabric-diagnostics-event-generation-infra.md), Service Fabric ayarlayan, hangisinin aşağıdaki kanallar kolayca yapılandırılır izleme göndermek için WAD ve tanılama verilerini depolama tabloya birkaç Giden kutusu günlük kanallar ile veya başka bir yerde:
  * Çalışma olaylarını: Service Fabric platformundan gerçekleştirir üst düzey işlem. Örnek uygulamalar ve hizmetler, düğüm durumu değişiklikleri ve yükseltme bilgileri oluşturulmasını verilebilir. Bu olay için Windows izleme (ETW) günlükleri olarak gösterilen
  * [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
  * [Programlama modeli olaylarının güvenilir hizmetler](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a>Uygulama olayları
 Uygulamaların ve hizmetlerin koddan yayılan ve Visual Studio şablonları sağlanan EventSource yardımcı sınıfı kullanılarak yazılan olaylar. EventSource günlüklerine uygulamanızdan yazma hakkında daha fazla bilgi için bkz: [İzleyici ve yerel makine geliştirme Kurulum Hizmetleri'nde tanılamak](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>Tanılama uzantısını dağıtma
Günlükleri toplamayı ilk adımı, Service Fabric kümesindeki VM'lerin her birinde tanılama uzantısını dağıtmaktır. Tanılama uzantısını her VM günlükleri toplar ve bunları belirttiğiniz depolama hesabı yükler. Adımlar Azure portalında veya Azure Resource Manager kullanıp biraz göre farklılık gösterir. Adımlar ayrıca dağıtım küme oluşturmanın bir parçası değil veya zaten bir küme için göre değişir. Her senaryo için adımları bakalım.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Azure portal ile küme oluşturma bir parçası olarak tanılama uzantısını dağıtma
Tanılama uzantısını kümedeki sanal makineleri küme oluşturmanın bir parçası dağıtmak için tanılama Ayarları panelini kullanın. aşağıdaki görüntüde - gösterilen tanılama ayarlandığından emin olun **üzerinde** (varsayılan ayar). Kümeyi oluşturduktan sonra portal kullanarak bu ayarları değiştiremezsiniz.

![Küme oluşturma için Portalı'nda Azure tanılama ayarları](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

Portalı kullanarak bir küme oluştururken, şablonu indirme öneririz **Tamam'ı tıklatmadan önce** küme oluşturmak için. Ayrıntılar için başvurmak [bir Azure Resource Manager şablonu kullanarak bir Service Fabric kümesi ayarlayın](service-fabric-cluster-creation-via-arm.md). Portalı kullanarak bazı değişiklik yapılamıyor çünkü daha sonra değişiklikler yapmak için şablonu gerekir.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak küme oluşturmanın bir parçası olarak tanılama uzantısını dağıtma
Kaynak Yöneticisi'ni kullanarak bir küme oluşturmak için küme oluşturmadan önce tam küme Resource Manager şablonu JSON tanılama yapılandırması eklemeniz gerekir. Resource Manager şablonu örneklerimizi parçası olarak eklenecek tanılama yapılandırması içeren bir örnek beş VM küme Resource Manager şablonu sunuyoruz. Azure Örnekler Galerisi bu konumda bkz: [beş düğümlü kümeyi tanılama Resource Manager şablonu örneği ile](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Resource Manager şablonu tanılama ayarında görmek için azuredeploy.json dosyasını açın ve arama **IaaSDiagnostics**. Bu şablonu kullanarak bir küme oluşturmak için seçin **Azure'a Dağıt** düğmesini önceki bağlantıda kullanılabilir.

Alternatif olarak, Resource Manager örnek indirebilir, değişiklik ve kullanarak bir küme ile değiştirilen şablon oluşturma `New-AzureRmResourceGroupDeployment` bir Azure PowerShell penceresinde komutu. Aşağıdaki kod, komuta geçirdiğiniz parametreler için bkz. PowerShell kullanarak bir kaynak grubu dağıtma hakkında ayrıntılı bilgi için bkz: [Azure Resource Manager şablonu ile bir kaynak grubu dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Varolan bir kümeye tanılama uzantısını dağıtma
Dağıtılan tanılama yok varolan bir kümeye varsa veya var olan yapılandırmasını değiştirmek istiyorsanız, ekleyebilir veya güncelleştirebilir. Var olan küme oluşturmak veya daha önce açıklandığı gibi portaldan şablonu karşıdan yüklemek için kullanılan Resource Manager şablonu değiştirin. Aşağıdaki görevleri gerçekleştirerek template.json dosyasını değiştirin.

Yeni bir depolama kaynağı kaynaklar bölümüne ekleyerek şablonuna ekleyin.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Ardından, parametreleri bölüme yalnızca depolama hesabı tanımlarını sonra arasında eklemek `supportLogStorageAccountName` ve `vmNodeType0Name`. Yer tutucu metni değiştirmek *depolama hesabı adı buraya* depolama hesabı adı.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Ardından, güncelleştirme `VirtualMachineProfile` uzantıları dizi içinde aşağıdaki kodu ekleyerek template.json dosyasının bölümü. Başında veya burada eklenen bağlı olarak bitiş virgül eklediğinizden emin olun.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
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
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Template.json dosyasını açıklandığı şekilde değiştirdikten sonra Resource Manager şablonunu yeniden yayımlayın. Şablonu dışarı aktarılmışsa deploy.ps1 dosyasını çalıştıran şablonu yeniden yayımlar. Dağıttıktan sonra emin **ProvisioningState** olan **başarılı**.

## <a name="collect-health-and-load-events"></a>Sistem durumu toplama ve olayları yükleme

Service Fabric 5.4 sürümünden itibaren sistem durumu ve yük ölçüm olayları koleksiyonu için kullanılabilir. Bu olaylar Sistem kullanarak sistem veya kodunuzu tarafından oluşturulan olayları yansıtmak veya Raporlama API'leri gibi yük [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) veya [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Bu, toplama ve zaman içinde sistem durumu görüntüleme ve sistem durumu veya yük olaylara dayanarak uyarı verme sağlar. Visual Studio'nun Tanılama Olay Görüntüleyicisi'nde bu olayları görüntülemek için Ekle "Microsoft-ServiceFabric:4:0x4000000000000008" ETW sağlayıcılar listesi.

Olaylarını toplamak için dahil etmek için Resource Manager şablonu değiştirin.

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="collect-reverse-proxy-events"></a>Ters proxy olaylarını Topla

Service Fabric 5.7 sürümünden başlayarak [ters proxy](service-fabric-reverseproxy.md) olayları koleksiyonu için kullanılabilir.
Ters proxy olayları bir istek hataları ve tüm istekler hakkında ayrıntılı olayları içeren başka bir işleme yansıtarak içeren hata olayları ters proxy işlenen iki kanalı halinde yayar. 

1. Hatası olaylarını Topla: görüntülemek için bu olayları Visual Studio'nun Tanılama Olay Görüntüleyicisi'nde Ekle "Microsoft-ServiceFabric:4:0x4000000000000010" ETW sağlayıcılar listesi.
Azure kümelerdeki olaylarını toplamak için dahil etmek için Resource Manager şablonu değiştirin

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

2. Toplama tüm olayları işlemeyi istek: içinde Visual Studio'nun Tanılama Olay Görüntüleyicisi'ni ETW sağlayıcı listesine güncelleştirme Microsoft ServiceFabric girişinde "Microsoft-ServiceFabric:4:0x4000000000000020".
Azure Service Fabric kümeleri için dahil etmek için resource manager şablonu değiştirme

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```
> Bu ters proxy aracılığıyla tüm trafiği toplar ve depolama kapasitesini hızlıca tüketebileceği gibi bu kanal toplama olaylarından dikkatli etkinleştirilmesi önerilir.

Azure Service Fabric kümeleri için tüm düğümler olaylardan toplanır ve SystemEventTable bir araya getirilir.
Ayrıntılı ters proxy olaylarını sorun giderme için başvuruda [ters proxy tanılama Kılavuzu](service-fabric-reverse-proxy-diagnostics.md).

## <a name="collect-from-new-eventsource-channels"></a>Yeni EventSource kanaldan Topla

Yeni bir uygulama, olduğunuz hakkında dağıtmak için gerçekleştirmenizi aynı temsil eden yeni EventSource kanaldan günlükleri toplamak için tanılama güncelleştirmek için varolan bir tanılama kurulumu için daha önce açıklanan adımları küme.

Güncelleştirme `EtwEventSourceProviderConfiguration` yapılandırma uygulamadan önce yeni EventSource kanalları kullanarak güncelleştirmek için girdileri eklemek için template.json dosyasını bölümünde `New-AzureRmResourceGroupDeployment` PowerShell komutu. Olay kaynağı adı, kodunuz Visual Studio tarafından oluşturulan ServiceEventSource.cs dosyasındaki bir parçası olarak tanımlanır.

Örneğin, olay kaynağı Eventsource My adlandırılmışsa Eventsource My olayların MyDestinationTableName adlı bir tabloya yerleştirmek için aşağıdaki kodu ekleyin.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Performans sayaçlarını veya olayları toplamak için Resource Manager şablonu sağlanan örnekler kullanarak değiştirme [bir Azure Resource Manager şablonu kullanarak bir Windows sanal makine izleme ve tanılama oluşturmak](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ardından, Resource Manager şablonunu yeniden yayımlayın.

## <a name="collect-performance-counters"></a>Performans sayaçlarını Topla

Kümenizden performans ölçümlerini derleme için "WadCfg > DiagnosticMonitorConfiguration" kümeniz için Resource Manager şablonunda performans sayaçları ekleyin. Bkz: [Service Fabric performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) performans sayaçları için toplama öneririz.

Örneğin, burada örneklenen 15 dakikada bir performans sayacı ayarlarız (Bu değiştirilebilir ve biçimi izler "PT\<zaman >\<birim >", örneğin, PT3M üç dakikalık aralıklarla örnek) ve aktarılan için uygun depolama tablo her bir dakika.

  ```json
  "PerformanceCounters": {
      "scheduledTransferPeriod": "PT1M",
      "PerformanceCounterConfiguration": [
          {
              "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
              "sampleRate": "PT15S",
              "unit": "Percent",
              "annotation": [
              ],
              "sinks": ""
          }
      ]
  }
  ```
  
Aşağıdaki bölümde açıklandığı gibi bir Application Insights havuz kullanıyorsanız ve bu ölçümleri Application Insights'ta gösterilmesini istiyorsanız sonra yukarıda gösterildiği gibi "havuzlarını" bölümünde havuz adı eklediğinizden emin olun. Ayrıca, bunlar etkinleştirdiğiniz diğer günlük kanaldan gelen veriler kullanıma yönelik kitle olmayan şekilde, performans sayaçları için göndermek için ayrı bir tablo oluşturmayı düşünün.


## <a name="send-logs-to-application-insights"></a>Günlükleri Application Insights'a gönderme

Uygulama Öngörüler (AI) izleme ve Tanılama verileri gönderme WAD yapılandırmasının bir parçası yapılabilir. Olay çözümleme ve görselleştirme için AI kullanmaya karar verirseniz, okuma [olay çözümleme ve görselleştirme Application Insights ile](service-fabric-diagnostics-event-analysis-appinsights.md) , "WadCfg" bir parçası olarak bir AI havuzunu kurmak için.

## <a name="next-steps"></a>Sonraki adımlar

Azure tanılama doğru şekilde yapılandırdıktan sonra ETW ve EventSource günlüklerinden, depolama tablolardaki verileri görürsünüz. OMS, Kibana veya doğrudan Resource Manager şablonunda yapılandırılmamış başka bir veri analizi ve görselleştirme platform kullanmayı seçerseniz, bu depolama tablolardaki verileri okumak için seçtiğiniz platform ayarlamak emin olun. Bunu yapmak için OMS görece önemsiz ve içinde açıklanan [OMS olay ve Günlük çözümlemesi](service-fabric-diagnostics-event-analysis-oms.md). Application Insights biraz bu bağlamda bir özel durum, bu nedenle başvurmak tanılama uzantı yapılandırmasını bir parçası olarak yapılandırıldıktan sonra [uygun makale](service-fabric-diagnostics-event-analysis-appinsights.md) AI kullanmayı tercih ederseniz.

>[!NOTE]
>Şu anda filtre veya tabloya gönderilen olaylar bölümlendirmek mümkün değildir. Olayları tablodan kaldırmak için bir işlem uygulayın yok, tablo büyümeye devam edecek. Şu anda çalışan bir veri temizleme hizmeti örneği yok [izleme örnek](https://github.com/Azure-Samples/service-fabric-watchdog-service), ve 30 veya 90 günlük süre kaydettiği depolamak için iyi bir neden olmadıkça kendiniz için bir tane de yazma önerilir.

* [Tanılama uzantısını kullanarak performans sayaçlarını veya günlükleri toplamak öğrenin](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Olay çözümleme ve görselleştirme Application Insights ile](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Olay çözümleme ve OMS Görselleştirme](service-fabric-diagnostics-event-analysis-oms.md)