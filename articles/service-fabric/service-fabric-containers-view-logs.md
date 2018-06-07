---
title: Azure Service Fabric kapsayıcıları günlüklerini görüntülemek | Microsoft Docs
description: Service Fabric Explorer kullanarak çalışan bir Service Fabric kapsayıcı hizmetler için kapsayıcı günlüklerini görüntülemeyi açıklar.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2018
ms.author: ryanwi
ms.openlocfilehash: c8b6bc791700e6811f5681ee70329e4d2ac05991
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824620"
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Service Fabric kapsayıcı Hizmeti günlüklerini görüntüle
Azure Service Fabric bir kapsayıcı orchestrator ve her ikisi de destekler [Linux ve Windows kapsayıcıları](service-fabric-containers-overview.md).  Bu makalede, tanılama ve sorun giderme için çalışan bir kapsayıcı hizmeti veya çalışmayan bir kapsayıcı kapsayıcı günlüklerini görünümü açıklar.

## <a name="access-the-logs-of-a-running-container"></a>Çalışan bir kapsayıcı günlüklerine erişim
Kapsayıcı günlükleri kullanarak erişilebilir [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Kümenin yönetim uç noktasından Service Fabric Explorer giderek bir web tarayıcısında açın [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Kapsayıcı günlükleri kapsayıcı hizmeti örneğini çalıştıran küme düğümünde bulunur. Örnek olarak, web ön uç kapsayıcısının günlükler elde [Linux oylama örnek uygulama](service-fabric-quickstart-containers-linux.md). Ağaç görünümünde genişletin **küme**>**uygulamaları**>**VotingType**>**fabric: / oylama / azurevotefront**.  Sonra (d1aa737e-f22a-e347-be16-eec90be24bc1, bu örnekte) bölümünü genişletin ve kapsayıcı küme düğümü üzerinde çalışıp çalışmadığını *_lnxvm_0*.

Kod paketi bulunamadı ağaç görünümünde *_lnxvm_0* genişleterek düğümü **düğümleri**>**_lnxvm_0**>**fabric: / oylama**  > **azurevotfrontPkg**>**kod paketleri**>**kod**.  Ardından **kapsayıcı günlükleri** kapsayıcı günlükleri görüntülemek için seçeneği.

![Service Fabric platformu][Image1]

## <a name="access-the-logs-of-a-dead-or-crashed-container"></a>Ölü veya çöken bir kapsayıcı günlüklerine erişim
İçinde v6.2 başlayarak, ölü veya çöken bir kapsayıcı kullanılarak için günlükleri de getirebilirsiniz [REST API'leri](/rest/api/servicefabric/sfclient-index) veya [Service Fabric CLI (SFCTL)](service-fabric-cli.md) komutları.

### <a name="set-container-retention-policy"></a>Kapsayıcı bekletme ilkesi ayarlama
Service Fabric (6.1 veya üzeri sürümler), kapsayıcı başlatma hatalarının tanılanmasına yardımcı olmak için sonlandırılan veya başlatılamayan kapsayıcıların bekletilmesini destekler. Bu ilke, aşağıdaki kod parçacığında gösterildiği gibi **ApplicationManifest.xml** dosyasında ayarlanabilir:
```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
 ```

**ContainersRetentionCount** ayarı, başarısız olduğunda bekletilecek kapsayıcı sayısını belirtir. Negatif bir değer belirtilirse başarısız olan tüm kapsayıcılar bekletilir. Zaman **ContainersRetentionCount** özniteliği belirtilmezse, kapsayıcı yok korunur. **ContainersRetentionCount** özniteliği, kullanıcıların test ve üretim kümeleri için farklı değerler belirtebilmesi amacıyla Uygulama Parametrelerini destekler. Kapsayıcı hizmetinin diğer düğümlere taşınmasını önlemek için bu özellikler kullanılırken kapsayıcı hizmetinin belirli bir düğümü hedeflemesini sağlamak için yerleştirme kısıtlamaları kullanın. Bu özellik kullanılarak bekletilen tüm kapsayıcılar el ile kaldırılmalıdır.

### <a name="rest"></a>REST
Kullanım [düğümü kapsayıcı günlükleri dağıtılan almak](/rest/api/servicefabric/sfclient-api-getcontainerlogsdeployedonnode) çöken kapsayıcısı günlüklerini alma işlemi. Kapsayıcı çalıştığı düğümün adını, uygulama adı, hizmet bildirim adını ve kod paket adı belirtin.  Belirtin `&Previous=true`. Yanıt kodu paket örneğinin ölü kapsayıcısı için kapsayıcı günlükleri içerir.

İstek URI'si aşağıdaki biçime sahiptir:

```
/Nodes/{nodeName}/$/GetApplications/{applicationId}/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName={ServiceManifestName}&CodePackageName={CodePackageName}&Previous={Previous}
```

Örnek isteği:
```
GET http://localhost:19080/Nodes/_Node_0/$/GetApplications/SimpleHttpServerApp/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName=SimpleHttpServerSvcPkg&CodePackageName=Code&Previous=true  
```

200 yanıt gövdesi:
```json
{   "Content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" } 
```

### <a name="service-fabric-sfctl"></a>Service Fabric (SFCTL)
Kullanım [sfctl hizmeti get-kapsayıcı-günlükleri](service-fabric-sfctl-service.md) çöken bir kapsayıcı için günlükleri getirilemedi komutu.  Kapsayıcı çalıştığı düğümün adını, uygulama adı, hizmet bildirim adını ve kod paket adı belirtin. Belirtin `-previous` bayrağı.  Yanıt kodu paket örneğinin ölü kapsayıcısı için kapsayıcı günlükleri içerir.

```
sfctl service get-container-logs --node-name _Node_0 --application-id SimpleHttpServerApp --service-manifest-name SimpleHttpServerSvcPkg --code-package-name Code –previous
```
Yanıtı:
```json
{   "content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" }
```

## <a name="next-steps"></a>Sonraki adımlar
- Aracılığıyla iş [Linux kapsayıcı uygulama öğretici oluşturma](service-fabric-tutorial-create-container-images.md).
- Daha fazla bilgi edinmek [Service Fabric ve kapsayıcıları](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
