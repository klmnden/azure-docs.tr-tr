---
title: Windows Azure Service Fabric olay toplama Azure tanılama | Microsoft Docs
description: Toplama ve izleme ve tanılama Azure Service Fabric kümeleri için WAD kullanarak olay toplama hakkında bilgi edinin.
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
ms.date: 04/03/2018
ms.author: dekapur;srrengar
ms.openlocfilehash: 3e897ecb4d42fe2165457c34faa4c1178178e4d3
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Olay toplama ve Windows Azure Tanılama'yı kullanarak koleksiyonu
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Azure Service Fabric kümesi çalıştırırken, merkezi bir konumda tüm düğümlerdeki günlükleri toplamak için iyi bir fikirdir. Merkezi bir konumda günlükler sahip çözümlemek ve sorunları kümenizdeki veya bu kümede çalışan hizmetler ve uygulamalar sorunları gidermenize yardımcı olur.

Karşıya yükleme ve günlükleri toplamak için bir yol günlükleri Azure Storage'a yükler ve ayrıca Azure Application Insights veya olay hub'ları için günlükleri gönderme seçeneği içeren Windows Azure tanılama (WAD) uzantısı kullanmaktır. Olayları depolama alanından okuyun ve bunları bir analiz platformu üründe gibi yerleştirmek için bir dış işlem kullanabilirsiniz [günlük analizi](../log-analytics/log-analytics-service-fabric.md) veya başka bir çözüm günlük ayrıştırma.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki araçlar, bu makaledeki kullanılır:

* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager şablonu](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-platform-events"></a>Service Fabric platform olayları
Service Fabric ayarlayan, birkaç ile [Giden kutusu günlük kanalları](service-fabric-diagnostics-event-generation-infra.md), aşağıdaki kanallar izleme göndermek için uzantı ve tanılama verilerini depolama tabloya veya başka bir yerde ile önceden yapılandırılmış olan biri:
  * [Çalışma olaylarını](service-fabric-diagnostics-event-generation-operational.md): Service Fabric platformundan gerçekleştirir üst düzey işlem. Örnek uygulamalar ve hizmetler, düğüm durumu değişiklikleri ve yükseltme bilgileri oluşturulmasını verilebilir. Bu olay için Windows izleme (ETW) günlükleri olarak gösterilen
  * [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
  * [Programlama modeli olaylarının güvenilir hizmetler](service-fabric-reliable-services-diagnostics.md)

## <a name="deploy-the-diagnostics-extension-through-the-portal"></a>Portal üzerinden tanılama uzantısını dağıtma
Günlükleri toplamayı ilk adımı, Service Fabric kümesindeki sanal makine ölçek kümesi düğümlerinde tanılama uzantısını dağıtmaktır. Tanılama uzantısını her VM günlükleri toplar ve bunları belirttiğiniz depolama hesabı yükler. Aşağıdaki adımlar, Azure portalı ve Azure Resource Manager şablonları ile yeni ve var olan kümeleri için bunu gerçekleştirmek nasıl verilmiştir.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Azure portal ile küme oluşturma bir parçası olarak tanılama uzantısını dağıtma
İsteğe bağlı ayarları'nı genişletin ve tanılama ayarlandığından emin olun, Küme Küme yapılandırma adımda oluştururken, **üzerinde** (varsayılan ayar).

![Küme oluşturma için Portalı'nda Azure tanılama ayarları](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics-new.png)

Şablonu İndirme önerilir **Oluştur'u önce** son adımda. Ayrıntılar için başvurmak [bir Azure Resource Manager şablonu kullanarak bir Service Fabric kümesi ayarlayın](service-fabric-cluster-creation-via-arm.md). Veri toplamaya (yukarıda listelenen) hangi kanallar üzerinde değişiklik yapmak için şablonu gerekir.

![Küme şablonu](media/service-fabric-diagnostics-event-aggregation-wad/download-cluster-template.png)

Azure depolama olaylarında toplayarak göre [günlük analizi ayarlamak](service-fabric-diagnostics-oms-setup.md) Öngörüler elde edin ve günlük analizi portalında sorgulamak için

>[!NOTE]
>Şu anda filtre veya tablolara gönderilen olaylar bölümlendirmek mümkün değildir. Olayları tablodan kaldırmak için bir işlem uygulayın yok, tablo büyümeye devam edecek (varsayılan ucun 50 GB'dir). Bu öğeler değiştirmek yönergeler [bu makalede daha aşağıda](service-fabric-diagnostics-event-aggregation-wad.md#update-storage-quota). Ayrıca, çalışan bir veri temizleme hizmeti örneği yok [izleme örnek](https://github.com/Azure-Samples/service-fabric-watchdog-service), ve 30 veya 90 günlük süre kaydettiği depolamak için iyi bir neden olmadıkça kendiniz için bir tane de yazma önerilir.

## <a name="deploy-the-diagnostics-extension-through-azure-resource-manager"></a>Azure Resource Manager aracılığıyla tanılama uzantısını dağıtma

### <a name="create-a-cluster-with-the-diagnostics-extension"></a>Tanılama uzantılı bir küme oluşturun
Kaynak Yöneticisi'ni kullanarak bir küme oluşturmak için küme oluşturmadan önce tüm Resource Manager şablonu JSON tanılama yapılandırması eklemeniz gerekir. Resource Manager şablonu örneklerimizi parçası olarak eklenecek tanılama yapılandırması içeren bir örnek beş VM küme Resource Manager şablonu sunuyoruz. Azure Örnekler Galerisi bu konumda bkz: [beş düğümlü kümeyi tanılama Resource Manager şablonu örneği ile](https://azure.microsoft.com/en-in/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/).

Resource Manager şablonu tanılama ayarında görmek için azuredeploy.json dosyasını açın ve arama **IaaSDiagnostics**. Bu şablonu kullanarak bir küme oluşturmak için seçin **Azure'a Dağıt** düğmesini önceki bağlantıda kullanılabilir.

Alternatif olarak, Resource Manager örnek indirebilir, değişiklik ve kullanarak bir küme ile değiştirilen şablon oluşturma `New-AzureRmResourceGroupDeployment` bir Azure PowerShell penceresinde komutu. Aşağıdaki kod, komuta geçirdiğiniz parametreler için bkz. PowerShell kullanarak bir kaynak grubu dağıtma hakkında ayrıntılı bilgi için bkz: [Azure Resource Manager şablonu ile bir kaynak grubu dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="add-the-diagnostics-extension-to-an-existing-cluster"></a>Tanılama uzantısını varolan bir kümeye ekleme
Dağıtılan tanılama yoksa mevcut bir kümeniz varsa, ekleyin veya küme şablonu güncelleştirin. Var olan küme oluşturmak veya daha önce açıklandığı gibi portaldan şablonu karşıdan yüklemek için kullanılan Resource Manager şablonu değiştirin. Aşağıdaki görevleri gerçekleştirerek template.json dosyasını değiştirin:

Yeni bir depolama kaynağı kaynaklar bölümüne ekleyerek şablonuna ekleyin.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "sku": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Ardından, parametreleri bölüme yalnızca depolama hesabı tanımlarını sonra arasında eklemek `supportLogStorageAccountName`. Yer tutucu metni değiştirmek *depolama hesabı adı buraya* gibi depolama hesabı adı.

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
      "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
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

> [!TIP]
> Kümenize kapsayıcıları dağıtın kullanacaksanız, docker istatistikleri için bunu ekleyerek seçmek WAD etkinleştirmek, **WadCfg > DiagnosticMonitorConfiguration** bölümü.
>
>```json
>"DockerSources": {
>    "Stats": {
>        "enabled": true,
>        "sampleRate": "PT1M"
>    }
>},
>```

### <a name="update-storage-quota"></a>Depolama kotası güncelleştir

Genişletme tarafından doldurulmuş tabloları itibaren büyür kota isabet kadar kota boyutunu azaltmak düşünmek isteyebilirsiniz. Varsayılan değer 50 GB'tır ve şablondaki altında yapılandırılabilir `overallQuotainMB` altında `DiagnosticMonitorConfiguration`

```json
"overallQuotaInMB": "50000",
```

## <a name="log-collection-configurations"></a>Günlük toplama yapılandırmaları
Ek kanalları günlüklerinden de koleksiyonu için kullanılabilir olan, Azure'da çalışan kümeler için şablonda yapabileceğiniz yapılandırmaların çoğu bazıları aşağıda verilmiştir.

* İşlemsel kanal - Base: varsayılan, Service Fabric ve gelen dağıtılmakta olan yeni bir uygulama, bir düğüm için olaylar dahil olmak üzere, küme tarafından gerçekleştirilen üst düzey işlemler veya yükseltme bir geri alma tarafından etkinleştirilmiş vb. Olaylar listesi için bkz [çalışma kanal olaylarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-operational).
  
```json
      scheduledTransferKeywordFilter: "4611686018427387904"
  ```
* İşlemsel kanal - ayrıntılı: Bu sistem durumu raporları ve Yük Dengeleme kararları artı temel işletimsel kanal her şeyi içerir. Bu olaylar Sistem kullanarak sistem veya kodunuzu tarafından oluşturulan veya Raporlama API'leri gibi yük [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) veya [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Visual Studio'nun Tanılama Olay Görüntüleyicisi'nde bu olayları görüntülemek için Ekle "Microsoft-ServiceFabric:4:0x4000000000000008" ETW sağlayıcılar listesi.

```json
      scheduledTransferKeywordFilter: "4611686018427387912"
  ```

* Veri ve mesajlaşma kanalı - Base: kritik günlüklerini ve olayları (şu anda yalnızca ReverseProxy) Mesajlaşma ve veri yolu ayrıca ayrıntılı işletimsel kanal günlüklerine oluşturulan. Bu, işlenen isteklerin yanı sıra, istek hataları ve diğer kritik sorunlar ReverseProxy işleme olaylardır. **Kapsamlı günlüğü için Bizim önerimiz budur**. Visual Studio'nun Tanılama Olay Görüntüleyicisi'nde bu olayları görüntülemek için Ekle "Microsoft-ServiceFabric:4:0x4000000000000010" ETW sağlayıcılar listesi.

```json
      scheduledTransferKeywordFilter: "4611686018427387928"
  ```

* Veri & Mesajlaşma kanalı - ayrıntılı: veri ve küme ve ayrıntılı işlem kanal Mesajlaşma tüm kritik olmayan kayıtları içeren kapsamlı kanal. Ayrıntılı tüm ters proxy olaylarını sorun giderme için başvurmak [ters proxy tanılama Kılavuzu](service-fabric-reverse-proxy-diagnostics.md).  Visual Studio'nun Tanılama Olay Görüntüleyicisi'nde bu olayları görüntülemek için Ekle "Microsoft-ServiceFabric:4:0x4000000000000020" ETW sağlayıcılar listesi.

```json
      scheduledTransferKeywordFilter: "4611686018427387944"
  ```

>[!NOTE]
>Bu kanal olayları çok yüksek hacimli sahipse, bu etkinleştirme olay toplama çok hızlı bir şekilde oluşturulmakta izlemeleri kanal sonuçlarında ayrıntılı ve depolama kapasitesini kullanmasını sağlayabilirsiniz. Yalnızca bu kesinlikle gerekli olduğunda etkinleştirin.


Etkinleştirmek için **temel veri ve mesajlaşma kanalı** kapsamlı günlüğü için Bizim önerimiz `EtwManifestProviderConfiguration` içinde `WadCfg` şablonunuzun şu şekilde görünür:

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
                "scheduledTransferKeywordFilter": "4611686018427387928",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricSystemEventTable"
                }
              }
            ]
          }
        }
      },
```

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

Kümenizden performans ölçümlerini derleme için "WadCfg > DiagnosticMonitorConfiguration" kümeniz için Resource Manager şablonunda performans sayaçları ekleyin. Bkz: [WAD ile performans izleme](service-fabric-diagnostics-perf-wad.md) değiştirme adımları için `WadCfg` belirli performans sayaçları toplanamadı. Başvuru [Service Fabric performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) listesini performans sayaçları için toplama öneririz.
  
Aşağıdaki bölümde açıklandığı gibi bir Application Insights havuz kullanıyorsanız ve bu ölçümleri Application Insights'ta gösterilmesini istiyorsanız sonra yukarıda gösterildiği gibi "havuzlarını" bölümünde havuz adı eklediğinizden emin olun. Bu işlem, ayrı ayrı yapılandırılır performans sayaçlarını Application Insights kaynağınıza otomatik olarak gönderir.


## <a name="send-logs-to-application-insights"></a>Günlükleri Application Insights'a gönderme

Uygulama Öngörüler (AI) izleme ve Tanılama verileri gönderme WAD yapılandırmasının bir parçası yapılabilir. Olay çözümleme ve görselleştirme için AI kullanmaya karar verirseniz, okuma [AI havuzunu kurmak nasıl](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template) , "WadCfg" nın bir parçası olarak.

## <a name="next-steps"></a>Sonraki adımlar

Azure tanılama doğru şekilde yapılandırdıktan sonra ETW ve EventSource günlüklerinden, depolama tablolardaki verileri görürsünüz. Günlük analizi, Kibana veya doğrudan Resource Manager şablonunda yapılandırılmamış başka bir veri analizi ve görselleştirme platform kullanmayı seçerseniz, bu depolama tablolardaki verileri okumak için seçtiğiniz platform ayarlamak emin olun. Bunu yapmak için günlük analizi görece önemsiz ve içinde açıklanan [olay ve günlük analizi](service-fabric-diagnostics-event-analysis-oms.md). Application Insights biraz bu bağlamda bir özel durum, bu nedenle başvurmak tanılama uzantı yapılandırmasını bir parçası olarak yapılandırıldıktan sonra [uygun makale](service-fabric-diagnostics-event-analysis-appinsights.md) AI kullanmayı tercih ederseniz.

>[!NOTE]
>Şu anda filtre veya tabloya gönderilen olaylar bölümlendirmek mümkün değildir. Olayları tablodan kaldırmak için bir işlem uygulayın yok, tablo büyümeye devam edecek. Şu anda çalışan bir veri temizleme hizmeti örneği yok [izleme örnek](https://github.com/Azure-Samples/service-fabric-watchdog-service), ve 30 veya 90 günlük süre kaydettiği depolamak için iyi bir neden olmadıkça kendiniz için bir tane de yazma önerilir.

* [Tanılama uzantısını kullanarak performans sayaçlarını veya günlükleri toplamak öğrenin](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Olay çözümleme ve görselleştirme Application Insights ile](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Olay çözümleme ve görselleştirme günlük analizi](service-fabric-diagnostics-event-analysis-oms.md)
