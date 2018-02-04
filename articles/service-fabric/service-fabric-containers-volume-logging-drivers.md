---
title: "Azure Service Fabric Docker Compose (Önizleme) | Microsoft Docs"
description: "Azure Service Fabric Service Fabric kullanarak var olan kapsayıcıları düzenlemek daha kolay hale getirmek için Docker Compose biçimi kabul eder. Docker Compose desteği şu anda önizlemede değil."
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
ms.openlocfilehash: cbe7e338ac7da9dc7e8d03cb1bb07a69af70cb17
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="use-docker-volume-plug-ins-and-logging-drivers-in-your-container"></a>Docker birim eklentiler ve günlük sürücüleri, kapsayıcıdaki kullanın
Azure Service Fabric destekleyen belirtme [Docker birim eklentileri](https://docs.docker.com/engine/extend/plugins_volume/) ve [Docker günlük sürücüleri](https://docs.docker.com/engine/admin/logging/overview/) kapsayıcı hizmetiniz için. Verilerinizi kalıcı [Azure dosyaları](https://azure.microsoft.com/services/storage/files/) zaman, kapsayıcı taşınmış veya farklı bir ana bilgisayarda yeniden.

Yalnızca birim sürücüleri Linux kapsayıcıları için şu anda desteklenir. Windows kapsayıcıları kullanıyorsanız, bir birim için bir Azure dosyaları eşleyebilirsiniz [SMB3 paylaşımı](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) bir birimin sürücü olmadan. Bu eşleme için sanal makineleri (VM'ler) kümenizdeki en son Windows Server 1709 sürüme güncelleştirin.


## <a name="install-the-docker-volumelogging-driver"></a>Docker birim/günlük sürücüsünü yükleyin

Docker birim/günlük sürücü makinede yüklü değilse, onu el ile RDP/SSH protokolleri kullanarak yükleyebilirsiniz. Yükleme işlemine bu protokolleri aracılığıyla gerçekleştirebileceğiniz bir [sanal makine ölçek kümesi başlangıç komut dosyası](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) veya bir [SetupEntryPoint betik](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service).

Yüklemek için komut dosyası örneği [Docker birim sürücüsü Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) aşağıdaki gibidir:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

> [!NOTE]
> Windows Server 2016 Datacenter eşleme SMB başlatmalar kapsayıcılara desteklemez ([yalnızca sürüm 1709 Windows Server'da desteklenen](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/container-storage)). Bu sınırlama, ağ birimi eşlemenin ve Azure dosyaları birim sürücülerin 1709 eski sürümlerinde engeller. 
>   


## <a name="specify-the-plug-in-or-driver-in-the-manifest"></a>Eklenti belirtin veya bildiriminde sürücüsü
Eklentiler uygulama bildiriminde aşağıdaki gibi belirtilir:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
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
        <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
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

**Kaynak** etiketinde **birim** öğesi, kaynak klasöre başvuruyor. Kaynak klasör kapsayıcıları ya da kalıcı bir uzak depo barındıran VM bir klasörde olabilir. **Hedef** etiketi konumudur, **kaynak** çalışan kapsayıcıda eşlenir. Bu nedenle, hedefinizi kapsayıcı içinde zaten bir konumu olamaz.

Uygulama parametreler, birimler için desteklenir, önceki bildirim parçacığında gösterildiği gibi (Ara `MyStoreVar` bir örnek için kullanın).

Bir birim eklenti belirtirken, Service Fabric belirtilen parametreleri kullanarak birimi otomatik olarak oluşturur. **Kaynak** etiketi birim adıdır ve **sürücü** etiketi birimin sürücü eklentisi belirtir. Seçenekleri kullanarak belirtilebilir **DriverOption** şu şekilde etiketleyin:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Docker günlük sürücü belirtilirse, günlükleri işlemek için aracıları (veya kapsayıcıları) kümede dağıtmak zorunda. **DriverOption** etiketi, günlük sürücü seçeneklerini belirtmek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Makale bir Service Fabric kümesi kapsayıcıları dağıtın bakın [Service Fabric üzerinde bir kapsayıcıyı dağıtmak](service-fabric-deploy-container.md).
