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
ms.openlocfilehash: cf7b0dd3a81c35be4907dbba85b72ce4f87e3a9f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="using-volume-plugins-and-logging-drivers-in-your-container"></a>Birim eklenti ve günlük sürücüleri, kapsayıcıdaki kullanma

Service Fabric destekleyen belirtme [Docker birim eklentileri](https://docs.docker.com/engine/extend/plugins_volume/) ve [Docker günlük sürücüleri](https://docs.docker.com/engine/admin/logging/overview/) kapsayıcı hizmetiniz için. 

## <a name="install-volumelogging-driver"></a>Birim/günlük sürücüsünü yükleyin

Docker birim/günlük sürücü makinede yüklü değilse, makinede RDP/SSH-lık veya VMSS başlangıç komut dosyası aracılığıyla el ile yükleyin. Örneğin, içinde sipariş Docker birim sürücüsü SSH makinede yüklemek ve çalıştırmak için:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/17.09.0-ce-azure1  \
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

Önceki örnekte `Source` etiketinde `Volume` kaynak klasörü gösterir. Kaynak klasörü kapsayıcıları ya da kalıcı bir uzak depo barındıran VM bir klasörde olabilir. `Destination` Etiketi konumudur, `Source` çalışan kapsayıcıda eşlenir. 

Bir birim eklentisi belirtirken, Service Fabric belirtilen parametreleri kullanarak birimi otomatik olarak oluşturur. `Source` Etiketi birim adıdır ve `Driver` etiketi birimin sürücü eklentisi belirtir. Seçenekleri kullanılarak belirtilebilir `DriverOption` etiketi aşağıdaki kod parçacığında gösterildiği gibi:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Docker günlük sürücü belirtilirse, kümede günlükleri işlemek için aracıları (veya kapsayıcıları) dağıtmak gereklidir.  `DriverOption` Etiketi de günlüğü sürücü seçeneklerini belirtmek için kullanılabilir.

Service Fabric kümesine kapsayıcıları dağıtmak için aşağıdaki makalelere bakın:


[Service Fabric bir kapsayıcıda dağıtma](service-fabric-deploy-container.md)

