---
title: Tek başına Azure Service Fabric küme yapılandırmasını yükseltme | Microsoft Docs
description: Tek başına Service Fabric kümesi çalıştıran yapılandırma yükseltmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: f99c1ebb64bf881bcd42f15e13bb81b96ccfa064
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60387137"
---
# <a name="upgrade-the-configuration-of-a-standalone-cluster"></a>Tek başına küme yapılandırmasını Yükselt 

Herhangi bir modern sistemi için yükseltme olanağı ürününüzden uzun süreli başarının anahtarıdır. Bir Azure Service Fabric kümesine sahip olduğunuz bir kaynaktır. Bu makalede, tek başına Service Fabric kümenizi yapılandırma ayarlarını yükseltileceği açıklanır.

## <a name="customize-cluster-settings-in-the-clusterconfigjson-file"></a>Küme ayarları ClusterConfig.json dosyasındaki özelleştirme
Tek başına kümeler yapılandırılır *ClusterConfig.json* dosya. Farklı ayarlar hakkında daha fazla bilgi için bkz: [tek başına bir Windows kümesi için yapılandırma ayarlarını](service-fabric-cluster-manifest.md).

Ekleme, güncelleştirme veya ayarlarında kaldırma `fabricSettings` bölümüne [küme özelliklerini](./service-fabric-cluster-manifest.md#cluster-properties) konusundaki *ClusterConfig.json*. 

Örneğin, aşağıdaki JSON yeni bir ayar ekler *MaxDiskQuotaInMB* için *tanılama* bölümüne `fabricSettings`:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

ClusterConfig.json dosyanızda ayarlar değiştirildikten sonra [küme yapılandırmasını test](#test-the-cluster-configuration) ardından [küme yapılandırmasını yükseltme](#upgrade-the-cluster-configuration) kümenize ayarları uygulamak için. 

## <a name="test-the-cluster-configuration"></a>Küme yapılandırmasını test etme
Yapılandırma Yükseltmeyi başlatmadan önce yeni küme yapılandırma JSON tek başına paketteki aşağıdaki PowerShell betiğini çalıştırarak test edebilirsiniz:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>
```

Veya bu betiği kullanın:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>
```

Bazı yapılandırmalar olamaz, uç noktaları gibi küme adı, düğüm IP yükseltilmiş vb. Yeni küme yapılandırma JSON karşı eski bir test edilir ve bir sorun olduğunda hataları PowerShell penceresinde oluşturur.

## <a name="upgrade-the-cluster-configuration"></a>Küme yapılandırmasını Yükselt
Küme yapılandırması yükseltme yükseltmek için çalıştırmanız [başlangıç ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade). Yükseltme etki alanı tarafından işlenen yükseltme etki alanı yapılandırma yükseltmedir.

```powershell
Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

## <a name="upgrade-cluster-certificate-configuration"></a>Küme sertifikası yapılandırmasını Yükselt
Bir küme sertifikası, küme düğümleri arasında kimlik doğrulaması için kullanılır. Küme düğümler arasında iletişim hatası engellediği için sertifika geçişi ile çok dikkatli gerçekleştirilmelidir.

Dört seçenekten desteklenir:  

* Tek bir sertifika yükseltme: Yükseltme yolu (birincil) -> sertifika B (birincil) sertifikasıdır sertifika C (birincil) -> ->...

* Çift sertifika yükseltme: Sertifika (birincil) -> sertifika (birincil) ve B (ikincil) -> sertifika B (birincil) yükseltme yolu olan sertifika B (birincil) -> ve C (ikincil) -> sertifika C (birincil) ->...

* Sertifika türü yükseltme: Sertifika parmak izi tabanlı yapılandırma <> – CommonName tabanlı sertifika yapılandırması. Örneğin, sertifika parmak izi (birincil) ve parmak izi B (ikincil) -> sertifika CommonName C.

* Sertifika verenin parmak izi yükseltme: Sertifikanın CN yükseltme yoludur = A, IssuerThumbprint IT1 = (birincil) sertifika CN -> = A, IssuerThumbprint IT1, IT2 = (birincil) sertifika CN -> = A, IssuerThumbprint IT2 = (birincil).


## <a name="next-steps"></a>Sonraki adımlar
* Bazı özelleştirmeyi öğrenin [Service Fabric küme ayarlarını](service-fabric-cluster-fabric-settings.md).
* Bilgi edinmek için nasıl [kümenizin ölçeğini daraltma ve genişletme](service-fabric-cluster-scale-up-down.md).
* Hakkında bilgi edinin [uygulama yükseltmeleri](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
