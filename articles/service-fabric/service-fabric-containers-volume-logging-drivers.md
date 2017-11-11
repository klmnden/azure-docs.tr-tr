---
title: "Azure Service Fabric Docker Compose Önizleme | Microsoft Docs"
description: "Azure Service Fabric Service Fabric kullanarak exsiting kapsayıcıları düzenlemek kolay hale getirmek için Docker Compose'u biçimi kabul eder. Bu destek, şu anda önizlemede değil."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 955f84e5656bbf568234cbaf69faa4dd0a741206
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="using-volume-plugins-and-logging-drivers-in-your-container"></a>Birim eklenti ve günlük sürücüleri, kapsayıcıdaki kullanma
Service Fabric destekleyen belirtme [Docker birim eklentileri](https://docs.docker.com/engine/extend/plugins_volume/) ve [Docker günlük sürücüleri](https://docs.docker.com/engine/admin/logging/overview/) kapsayıcı hizmetiniz için.  Bu, verilerinizi kalıcı hale getirmek olanak tanır [Azure dosyaları](https://azure.microsoft.com/en-us/services/storage/files/) , kapsayıcı taşınmış veya farklı bir ana bilgisayarda yeniden olsa bile.

Şu anda, yalnızca birim sürücüleri aşağıda gösterildiği gibi Linux kapsayıcıları için vardır.  Windows kapsayıcıları kullanıyorsanız, bir birim için bir Azure dosyaları eşlemek olası [SMB3 paylaşımı](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) Windows Server'ın en son 1709 sürümünü kullanan bir birimin sürücü olmadan. Bu, sanal makinelerinizi kümenizdeki Windows Server 1709 sürüme güncelleştirilmesi gerekir.


## <a name="install-volumelogging-driver"></a>Birim/günlük sürücüsünü yükleyin

Docker birim/günlük sürücü makinede yüklü değilse, el ile RDP/SSH-lık makinesine aracılığıyla yüklemeye bir [VMSS başlangıç betiği](https://azure.microsoft.com/en-us/resources/templates/201-vmss-custom-script-windows/) veya kullanarak bir [SetupEntryPoint](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-model#describe-a-service) komut dosyası. Belirtilen yöntemlerden birini seçerek, yüklemek için bir komut dosyası yazabilirsiniz [Docker birim sürücüsü Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/):


```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

## <a name="specify-the-plugin-or-driver-in-the-manifest"></a>Eklenti veya sürücü bildiriminde belirtin
Eklenti uygulama bildiriminde aşağıdaki bildiriminde gösterildiği gibi belirtilir:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
        </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

Önceki örnekte `Source` etiketinde `Volume` kaynak klasörü gösterir. Kaynak klasörü kapsayıcıları ya da kalıcı bir uzak depo barındıran VM bir klasörde olabilir. `Destination` Etiketi konumudur, `Source` çalışan kapsayıcıda eşlenir.  Bu nedenle, hedefiniz, kapsayıcı içinde zaten varolan bir konumu olamaz.

Bir birim eklentisi belirtirken, Service Fabric belirtilen parametreleri kullanarak birimi otomatik olarak oluşturur. `Source` Etiketi birim adıdır ve `Driver` etiketi birimin sürücü eklentisi belirtir. Seçenekleri kullanılarak belirtilebilir `DriverOption` etiketi aşağıdaki kod parçacığında gösterildiği gibi:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Docker günlük sürücü belirtilirse, kümede günlükleri işlemek için aracıları (veya kapsayıcıları) dağıtmak gereklidir.  `DriverOption` Etiketi de günlüğü sürücü seçeneklerini belirtmek için kullanılabilir.

Service Fabric kümesine kapsayıcıları dağıtmak için aşağıdaki makalelere bakın:


[Service Fabric bir kapsayıcıda dağıtma](service-fabric-deploy-container.md)

