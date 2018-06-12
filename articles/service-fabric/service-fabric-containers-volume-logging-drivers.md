---
title: Service Fabric Azure dosyaları birim sürücüsü (Önizleme) | Microsoft Docs
description: Azure dosyaları için yedekleme birimleri, kapsayıcıdan kullanarak Service Fabric destekler. Bu, şu anda önizlemede değil.
services: service-fabric
documentationcenter: other
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: other
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/10/2018
ms.author: subramar
ms.openlocfilehash: d6195eda43dfd6ad249e82dabd0b314fc162b8c6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301091"
---
# <a name="service-fabric-azure-files-volume-driver-preview"></a>Service Fabric Azure dosyaları birim sürücüsü (Önizleme)
Azure dosyaları birim eklentidir bir [Docker birim eklentisi](https://docs.docker.com/engine/extend/plugins_volume/) sağlayan [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) birimleri Docker kapsayıcıları için temel. Bu Docker birim eklentisi Service Fabric kümelerine dağıtılabilir bir Service Fabric uygulaması olarak paketlenir. Buna ait amacı, birimler kümeye dağıtılan diğer Service Fabric kapsayıcı uygulamaları için Azure dosya tabanlı sağlamaktır.

> [!NOTE]
> Azure dosyaları birim eklentisi 6.255.389.9494 sürümü bu belgeyle birlikte kullanılabilen bir önizleme sürümüdür. Önizleme sürümü olduğu **değil** üretim ortamlarında kullanmak için desteklenir.
>

## <a name="prerequisites"></a>Önkoşullar
* Azure dosyaları birim eklentisi Windows sürümü çalışan [Windows Server sürüm 1709](https://docs.microsoft.com/en-us/windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10 sürüm 1709](https://docs.microsoft.com/en-us/windows/whats-new/whats-new-windows-10-version-1709) veya sonraki işletim sistemleri yalnızca. Azure dosyaları birim eklentisi Linux sürümü Service Fabric tarafından desteklenen tüm işletim sistemi sürümlerinde çalışır.

* Azure dosyaları birim eklentisi, yalnızca Service Fabric 6.2 ve daha yeni sürüm çalışır.

* ' Ndaki yönergeleri izleyin [Azure dosyaları belgelerine](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share) birimi olarak kullanmak Service Fabric kapsayıcı uygulama için bir dosya paylaşımı oluşturmak için.

* İhtiyacınız olacak [Powershell Service Fabric modülü ile](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started) veya [SFCTL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cli) yüklü.

## <a name="deploy-the-service-fabric-azure-files-application"></a>Service Fabric Azure dosyaları uygulama dağıtma

Birimleri için kapsayıcı sağlayan Service Fabric uygulaması aşağıdakiler arasından indirilebilir [bağlantı](https://aka.ms/sfvolume). Uygulama kümeye dağıtılabilir [PowerShell](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications), [CLI](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-lifecycle-sfctl) veya [FabricClient API'leri](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications-fabricclient).

1. Komut satırını kullanarak, indirilen uygulama paketi kök dizinine dizini değiştirin.

    ```powershell
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. [ImageStoreConnectionString] ve [ApplicationPackagePath] için uygun değer ile aşağıdaki komutu çalıştırın görüntü deposu için uygulama paketi kopyalayın:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Uygulama türünü kaydetme

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. Uygulama oluşturmak için Not komutta uygulaması oluşturma **ListenPort** uygulama parametresi. Bu uygulama parametresi için belirtilen bu değer Azure dosyaları birim eklentisi Docker arka plan programı gelen istekleri için dinlediği bağlantı noktasıdır. Uygulama için sağlanan bağlantı noktası küme veya uygulamalarınızı kullanın diğer bağlantı noktasıyla çakışmadığından emin olmak önemlidir.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]

> Windows Server 2016 Datacenter eşleme SMB başlatmalar kapsayıcılara desteklemez ([yalnızca sürüm 1709 Windows Server'da desteklenen](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-storage)). Bu sınırlama, ağ birimi eşlemenin ve Azure dosyaları birim sürücülerin 1709 eski sürümlerinde engeller.
>   

### <a name="deploy-the-application-on-a-local-development-cluster"></a>Bir yerel geliştirme küme üzerindeki uygulama dağıtma
Azure dosyaları birim eklentisi uygulama için varsayılan hizmet örnek sayısı kümedeki her düğüm için dağıtılan hizmet örneği anlamına gelir -1 ' dir. Ancak, yerel bir geliştirme kümesi üzerinde Azure dosyaları birim eklentisi Uygulama dağıtırken, hizmet örnek sayısı 1 olarak belirtilmesi gerekir. Bu aracılığıyla yapılabilir **Instancecount** uygulama parametresi. Bu nedenle, yerel bir geliştirme kümesi üzerinde Azure dosyaları birim eklentisi uygulama dağıtmak için bir komut şöyledir:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```
## <a name="configure-your-applications-to-use-the-volume"></a>Uygulamalarınızı birimi kullanmak için yapılandırma
Aşağıdaki kod parçacığında, bir Azure tabanlı dosyaları birim, uygulamanızın uygulama bildiriminde nasıl belirtilebilir gösterir. İlgilendiğiniz belirli öğe **birim** etiketi:

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
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
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

Azure dosyaları birim eklentisi için sürücü adı **sfazurefile**. Bu değer için ayarlanmış **sürücü** özniteliği **birim** uygulama bildiriminde öğesi.

İçinde **birim** yukarıdaki parçacığında, Azure dosyaları birim eklentisi öğesinde aşağıdaki etiketlerin gerektirir:
- **Kaynak** -birimin adıdır. Kullanıcı kendi birim için herhangi bir ad seçin.
- **Hedef** -bu etiketi birimin çalışan kapsayıcıda eşlenmiş konumdur. Bu nedenle, hedefinizi kapsayıcı içinde zaten bir konumu olamaz

Gösterildiği gibi **DriverOption** öğeleri parçacığında bulunan yukarıdaki Azure dosyaları birim eklentisi aşağıdaki sürücü seçenekleri destekler:

- **shareName** -birim için kapsayıcı sağlar. Azure dosyaları dosya paylaşımının adı
- **storageAccountName** - Name Azure dosyaları dosyasını içeren Azure depolama hesabını paylaşmak
- **storageAccountKey** -Azure dosyaları dosya paylaşımı içeren Azure depolama hesabının erişim anahtarı

Yukarıdaki sürücü seçeneklerin tümü gereklidir.

## <a name="using-your-own-volume-or-logging-driver"></a>Kendi birim veya günlük sürücü kullanma
Service Fabric, kendi özel kullanımını da sağlar [birim](https://docs.docker.com/engine/extend/plugins_volume/) veya [günlüğü](https://docs.docker.com/engine/admin/logging/overview/) sürücüleri. Docker birim/günlük sürücü kümede yüklü değilse, onu el ile RDP/SSH protokolleri kullanarak yükleyebilirsiniz. Yükleme işlemine bu protokolleri aracılığıyla gerçekleştirebileceğiniz bir [sanal makine ölçek kümesi başlangıç komut dosyası](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) veya bir [SetupEntryPoint betik](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service).

Yüklemek için komut dosyası örneği [Docker birim sürücüsü Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) aşağıdaki gibidir:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

Uygulamalarınızda, birim veya yüklediğiniz, günlük sürücü kullanmak için uygun değerleri belirtmek olurdu **birim** ve **LogConfig** altında öğelerin  **ContainerHostPolicies** uygulama bildiriminizi de.

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

Bir birim eklenti belirtirken, Service Fabric belirtilen parametreleri kullanarak birimi otomatik olarak oluşturur. **Kaynak** etiketinde **birim** öğesi birim adıdır ve **sürücü** etiketi birimin sürücü eklentisi belirtir. **Hedef** etiketi konumudur, **kaynak** çalışan kapsayıcıda eşlenir. Bu nedenle, hedefinizi kapsayıcı içinde zaten bir konumu olamaz. Seçenekleri kullanarak belirtilebilir **DriverOption** şu şekilde etiketleyin:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Uygulama parametreler, birimler için desteklenir, önceki bildirim parçacığında gösterildiği gibi (Ara `MyStorageVar` bir örnek için kullanın).

Docker günlük sürücü belirtilirse, günlükleri işlemek için aracıları (veya kapsayıcıları) kümede dağıtmak zorunda. **DriverOption** etiketi, günlük sürücü seçeneklerini belirtmek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Birim sürücüsü dahil olmak üzere, kapsayıcı örnekleri görmek için lütfen ziyaret [Service Fabric kapsayıcı örnekleri](https://github.com/Azure-Samples/service-fabric-containers)
* Makale bir Service Fabric kümesi kapsayıcıları dağıtın bakın [Service Fabric bir kapsayıcıda dağıtma](service-fabric-deploy-container.md)
