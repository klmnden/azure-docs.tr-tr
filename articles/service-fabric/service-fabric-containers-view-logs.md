---
title: Azure Service Fabric'te kapsayıcı günlüklerini görüntüleme | Microsoft Docs
description: Service Fabric Explorer kullanarak çalışan bir Service Fabric kapsayıcı Hizmetleri için kapsayıcı günlüklerini görüntülemeyi açıklar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2018
ms.author: aljo
ms.openlocfilehash: 0408010a49b8ec83aa02c74887139f663788ad80
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662831"
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Bir Service Fabric kapsayıcı hizmeti için günlükleri görüntüleyin
Azure Service Fabric kapsayıcı Düzenleyicisi ve her ikisi de destekler [Linux ve Windows kapsayıcıları](service-fabric-containers-overview.md).  Bu makalede, böylece tanılayın ve sorunlarını giderme ölü kapsayıcı ya da çalışan bir kapsayıcı hizmeti kapsayıcı günlüklerini görüntülemeyi açıklar.

## <a name="access-the-logs-of-a-running-container"></a>Çalışan bir kapsayıcının günlüklerine erişme
Kapsayıcı günlüklerini kullanarak erişilebilir [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Bir web tarayıcısında Service Fabric Explorer'a giderek kümenin yönetim uç noktasından açın [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Kapsayıcı günlüklerini kapsayıcı hizmet örneği üzerinde çalıştığı küme düğümünü bulunur. Örneğin, web ön uç kapsayıcısının günlükleri alma [Linux Voting örnek uygulamasını](service-fabric-quickstart-containers-linux.md). Ağaç görünümünde genişletin **küme**>**uygulamaları**>**VotingType**>**fabric: / Voting / azurevotefront**.  Ardından (d1aa737e-f22a-e347-be16-eec90be24bc1, bu örnekte) bölümü genişletin ve kapsayıcı küme düğümü üzerinde çalışıp çalışmadığını *_lnxvm_0*.

Ağaç görünümünde, kod paketi bulmak *_lnxvm_0* düğümünü genişleterek **düğümleri**>**_lnxvm_0**>**fabric: / Voting**  > **azurevotfrontPkg**>**kod paketleri**>**kod**.  Ardından **kapsayıcı günlüklerini** kapsayıcı günlüklerini görüntülemek için seçeneği.

![Service Fabric platformu][Image1]

## <a name="access-the-logs-of-a-dead-or-crashed-container"></a>Ölü veya çöken bir kapsayıcının günlüklerine erişme
İçinde v6.2 itibaren kullanarak ölü veya çöken kapsayıcı için günlükleri de getirebilirsiniz [REST API'leri](/rest/api/servicefabric/sfclient-index) veya [Service Fabric CLI (SFCTL)](service-fabric-cli.md) komutları.

### <a name="set-container-retention-policy"></a>Kapsayıcı bekletme ilkesi ayarlama
Service Fabric (6.1 veya üzeri sürümler), kapsayıcı başlatma hatalarının tanılanmasına yardımcı olmak için sonlandırılan veya başlatılamayan kapsayıcıların bekletilmesini destekler. Bu ilke, aşağıdaki kod parçacığında gösterildiği gibi **ApplicationManifest.xml** dosyasında ayarlanabilir:
```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
 ```

**ContainersRetentionCount** ayarı, başarısız olduğunda bekletilecek kapsayıcı sayısını belirtir. Negatif bir değer belirtilirse başarısız olan tüm kapsayıcılar bekletilir. Zaman **ContainersRetentionCount** özniteliği belirtilmezse, hiçbir kapsayıcı bekletilmez. **ContainersRetentionCount** özniteliği, kullanıcıların test ve üretim kümeleri için farklı değerler belirtebilmesi amacıyla Uygulama Parametrelerini destekler. Kapsayıcı hizmetinin diğer düğümlere taşınmasını önlemek için bu özellikler kullanılırken kapsayıcı hizmetinin belirli bir düğümü hedeflemesini sağlamak için yerleştirme kısıtlamaları kullanın. Bu özellik kullanılarak bekletilen tüm kapsayıcılar el ile kaldırılmalıdır.

Ayar **RunInteractive** Docker's karşılık gelen `--interactive` ve `tty` [bayrakları](https://docs.docker.com/engine/reference/commandline/run/#options). Bu ayar ayarlandığında bu bayraklar kapsayıcı başlatmak için true bildirimi dosyasındaki kullanılır.  

### <a name="rest"></a>REST
Kullanım [düğüm kapsayıcı günlüklerini dağıtılan alma](/rest/api/servicefabric/sfclient-api-getcontainerlogsdeployedonnode) çöken bir kapsayıcı için günlükleri alma işlemi. Kapsayıcıyı çalıştıran düğümün adı, uygulama adı, hizmet bildirim adını ve kod paket adı belirtin.  Belirtin `&Previous=true`. Kapsayıcı günlüklerini ölü kapsayıcısı kod paketi örneği için yanıt içerir.

İstek URI'si aşağıdaki biçime sahiptir:

```
/Nodes/{nodeName}/$/GetApplications/{applicationId}/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName={ServiceManifestName}&CodePackageName={CodePackageName}&Previous={Previous}
```

Örnek istek:
```
GET http://localhost:19080/Nodes/_Node_0/$/GetApplications/SimpleHttpServerApp/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName=SimpleHttpServerSvcPkg&CodePackageName=Code&Previous=true  
```

200 yanıt gövdesi:
```json
{   "Content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" } 
```

### <a name="service-fabric-sfctl"></a>Service Fabric (SFCTL)
Kullanım [sfctl hizmet get-container-logs](service-fabric-sfctl-service.md) çöken bir kapsayıcı için günlükleri alma komutu.  Kapsayıcıyı çalıştıran düğümün adı, uygulama adı, hizmet bildirim adını ve kod paket adı belirtin. Belirtin `--previous` bayrağı.  Kapsayıcı günlüklerini ölü kapsayıcısı kod paketi örneği için yanıt içerir.

```
sfctl service get-container-logs --node-name _Node_0 --application-id SimpleHttpServerApp --service-manifest-name SimpleHttpServerSvcPkg --code-package-name Code –-previous
```
Yanıt:
```json
{   "content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" }
```

## <a name="next-steps"></a>Sonraki adımlar
- Çalışmak [bir Linux kapsayıcı uygulaması Öğreticisi oluşturma](service-fabric-tutorial-create-container-images.md).
- Daha fazla bilgi edinin [Service Fabric ve kapsayıcılar](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
