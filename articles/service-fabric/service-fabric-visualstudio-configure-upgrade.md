---
title: Service Fabric uygulama yükseltme yapılandırma | Microsoft Docs
description: Microsoft Visual Studio kullanarak bir Service Fabric uygulama yükseltme için ayarları yapılandırma konusunda bilgi edinin.
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: faf7fd137d5c1efcd425cf28fd4860c62a719a67
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34211659"
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Visual Studio'da bir Service Fabric uygulama yükseltme yapılandırın
Azure Service Fabric için Visual Studio Araçları, yerel veya uzak kümelerine yayımlama için yükseltme desteği sağlar. Uygulamanızı test ve hata ayıklama sırasında uygulama değiştirme yerine daha yeni bir sürüme yükseltmek istediğiniz üç senaryo vardır:

* Yükseltme sırasında uygulama veri kaybı olmayacaktır.
* Olmayacaktır bu nedenle yükseltme sırasında herhangi bir hizmet kesintisi yükseltme etki alanları arasında yeterli hizmet örnekleri yaymak ise yüksek kullanılabilirlik kalır.
* Bunu yükseltilirken testleri bir uygulamaya karşı çalıştırabilirsiniz.

## <a name="parameters-needed-to-upgrade"></a>Yükseltme için gereken parametreleri
İki dağıtım türlerinden birini seçebilirsiniz: normal veya yükseltme. Bir yükseltme dağıtımı, korur ancak normal dağıtım tüm önceki dağıtım bilgileri ve verileri küme üzerinde siler. Visual Studio'da bir Service Fabric uygulaması yükselttiğinizde, uygulama yükseltme parametreleri ve sistem durumu ilkelerini kontrol sağlamanız gerekir. Uygulama yükseltme parametreleri sistem durumu denetimi ilkeleri yükseltme başarılı olup olmadığını belirlenirken yükseltme denetlemeye yardımcı olmak. Bkz: [Service Fabric uygulama yükseltme: yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) daha fazla ayrıntı için.

Üç yükseltme modu vardır: *izlenen*, *UnmonitoredAuto*, ve *UnmonitoredManual*.

* İzlenen yükseltme yükseltme otomatikleştirir ve uygulama sistem durumu denetimi.
* UnmonitoredAuto yükseltme yükseltme otomatikleştirir, ancak uygulama sistem durumu denetimi atlar.
* UnmonitoredManual yükseltme yaptığınızda, el ile yükseltme her etki alanı yükseltme yapmanız gerekir.

Her yükseltme modu parametreleri farklı kümelerini gerektirir. Bkz: [uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) kullanılabilir yükseltme seçenekleri hakkında daha fazla bilgi edinmek için.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Visual Studio'da bir Service Fabric uygulama yükseltme
Service Fabric uygulaması yükseltmek için Visual Studio Service Fabric araçları kullanıyorsanız, normal dağıtım yerine yükseltme denetleyerek olması için yayımlama işlemi belirtebilirsiniz **uygulama yükseltme** onay kutusu.

### <a name="to-configure-the-upgrade-parameters"></a>Yükseltme parametrelerini yapılandırmak için
1. Tıklatın **ayarları** yanındaki onay kutusunu düğmesi. **Yükseltme parametreleri Düzenle** iletişim kutusu görüntülenir. **Yükseltme parametreleri Düzenle** iletişim kutusu izlenen, UnmonitoredAuto ve UnmonitoredManual yükseltme modlarını destekler.
2. Out parametresi kılavuz doldurun ve isteyen yükseltme modu seçin.

    Her parametre varsayılan değerlerine sahip. İsteğe bağlı bir parametre *DefaultServiceTypeHealthPolicy* bir karma tablosu girişi alır. İşte bir örnek için karma tablo giriş biçimi *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* aşağıdaki biçimde bir karma tablosu girdi alır başka bir isteğe bağlı parametresi:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Gerçekçi örneği aşağıdadır:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. UnmonitoredManual yükseltme modu seçerseniz, devam etmek ve yükseltme işlemini bitirmek için bir PowerShell konsolunda el ile başlatmanız gerekir. Başvurmak [Service Fabric uygulama yükseltme: Gelişmiş konular](service-fabric-application-upgrade-advanced.md) nasıl el ile yükseltme öğrenmek için çalışır.

## <a name="upgrade-an-application-by-using-powershell"></a>PowerShell kullanarak uygulama yükseltme
Service Fabric uygulaması yükseltmek için PowerShell cmdlet'lerini kullanabilirsiniz. Bkz: [Service Fabric uygulama yükseltme öğretici](service-fabric-application-upgrade-tutorial.md) ve [başlangıç ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) ayrıntılı bilgi için.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Bir sistem durumu denetimi İlkesi uygulama bildirim dosyasında belirtin
Her bir Service Fabric uygulaması hizmetinde varsayılan değerleri geçersiz kılmak kendi sistem durumu ilkesi parametrelerine sahip olabilirsiniz. Bu parametre değerleri uygulama bildirim dosyasının sağlayabilir.

Aşağıdaki örnek, uygulama bildiriminde her hizmet için bir benzersiz sistem durumu denetimi ilkesi uygulamak gösterilmiştir.

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Sonraki adımlar
Uygulama yükseltme hakkında daha fazla bilgi için bkz: [Visual Studio kullanarak uygulama yükseltme](service-fabric-application-upgrade-tutorial.md).
