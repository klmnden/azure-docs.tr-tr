---
title: Bir Service Fabric uygulamasının yükseltmesini yapılandırma | Microsoft Docs
description: Microsoft Visual Studio kullanarak Service Fabric uygulaması yükseltme ayarlarını yapılandırmayı öğrenin.
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
ms.openlocfilehash: 79120371ca2a62e5ef9f2bf38476635db12e9fcc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082862"
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Visual Studio'da bir Service Fabric uygulamasının yükseltmesini yapılandırma
Azure Service Fabric için Visual Studio Araçları, yerel veya uzak kümelerine yayımlama için yükseltme desteği sağlar. Uygulamanızı test ve hata ayıklama sırasında uygulama değiştirmek yerine daha yeni bir sürüme yükseltmek istediğiniz üç senaryo vardır:

* Yükseltme sırasında uygulama veri kaybı olmayacaktır.
* Olmayacaktır bu nedenle yükseltme işlemi sırasında herhangi bir hizmet kesintisi olması durumunda yeterli hizmet örnekleri yükseltme etki alanına yaymak kullanılabilirlik yüksek olarak kalır.
* Bunu yükseltilirken testleri bir uygulamayı karşı çalıştırabilirsiniz.

## <a name="parameters-needed-to-upgrade"></a>Yükseltme için gereken parametreleri
İki dağıtım türünden seçebilirsiniz: normal veya yükseltme. Bir yükseltme dağıtımı, korur ancak bir normal dağıtım herhangi bir önceki dağıtım bilgileri ve veri kümesinde siler. Visual Studio'da Service Fabric uygulaması yükseltme yaptığınızda, uygulama yükseltme parametreleri ve sistem durumu ilkeleri kontrol sağlamanız gerekir. Uygulama yükseltme parametreleri, sistem durumu denetimi ilkeleri yükseltmenin başarılı olup olmadığını belirler. yükseltme, Denetim yardımcı olur. Bkz: [Service Fabric uygulama yükseltme: yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) daha fazla ayrıntı için.

Üç yükseltme modu vardır: *İzlenen*, *UnmonitoredAuto*, ve *UnmonitoredManual*.

* İzlenen yükseltme yükseltme otomatikleştirir ve uygulama sistem durumu denetimi.
* UnmonitoredAuto yükseltme yükseltme otomatikleştirir, ancak uygulama sistem durumu denetimini atlar.
* UnmonitoredManual yükseltme yaptığınızda, her bir yükseltme etki alanı el ile yükseltmeniz gerekir.

Her yükseltme modu farklı parametre kümesi gerektirir. Bkz: [uygulama yükseltme parametreleri](service-fabric-application-upgrade-parameters.md) kullanılabilir yükseltme seçenekleri hakkında daha fazla bilgi edinmek için.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Visual Studio'da Service Fabric uygulaması yükseltme
Service Fabric uygulaması yükseltme için Visual Studio Service Fabric Araçları'nı kullanıyorsanız, bir normal dağıtım yerine yükseltme denetleyerek olacak şekilde bir yayımlama işlemi belirtebilirsiniz **uygulamayı Yükselt** onay kutusu.

### <a name="to-configure-the-upgrade-parameters"></a>Yükseltme parametreleri yapılandırmak için
1. Tıklayın **ayarları** yanındaki onay kutusunu düğmesini. **Yükseltme parametreleri Düzenle** iletişim kutusu görüntülenir. **Yükseltme parametreleri Düzenle** iletişim kutusu, izlenen ve UnmonitoredAuto UnmonitoredManual yükseltme modu destekler.
2. Kullanın ve ardından parametre Kılavuzu doldurmak istediğiniz yükseltme modu seçin.

    Her parametrenin varsayılan değeri var. İsteğe bağlı parametre *DefaultServiceTypeHealthPolicy* bir karma tablo girdi alır. İşte bir örnek için karma tablo giriş biçimi *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* şu biçimde bir karma tablo giriş aldığı başka bir isteğe bağlı parametresi:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Gerçek zamanlı konuşmaların örnek aşağıda verilmiştir:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. UnmonitoredManual yükseltme modu seçerseniz, devam etmek ve yükseltme işlemini tamamlamak için bir PowerShell Konsolu el ile başlatmanız gerekir. Başvurmak [Service Fabric uygulama yükseltme: Gelişmiş konular](service-fabric-application-upgrade-advanced.md) nasıl el ile yükseltme öğrenmek için çalışır.

## <a name="upgrade-an-application-by-using-powershell"></a>PowerShell kullanarak bir uygulamayı yükseltme
Service Fabric uygulaması yükseltme için PowerShell cmdlet'lerini kullanabilirsiniz. Bkz: [Service Fabric uygulaması yükseltme Öğreticisi](service-fabric-application-upgrade-tutorial.md) ve [başlangıç ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) ayrıntılı bilgi için.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Uygulama bildirim dosyasına bir sistem durumu onay ilkesine belirtin
Her hizmetin bir Service Fabric uygulamasında varsayılan değerlerini geçersiz kılması kendi sistem durumu ilke parametreleri olabilir. Bu parametre değerlerini uygulama bildirimi dosyasındaki sağlayabilir.

Aşağıdaki örnek uygulama bildiriminde her hizmet için benzersiz sistem durumu onay ilkesine uygulanacağını gösterir.

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
Bir uygulamayı yükseltme hakkında daha fazla bilgi için bkz. [Visual Studio kullanarak uygulama yükseltme](service-fabric-application-upgrade-tutorial.md).
