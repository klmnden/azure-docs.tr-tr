---
title: Azure Service Fabric kapsayıcı uygulaması bildirim örnekleri | Microsoft Docs
description: Uygulama yapılandırma ve hizmet çok kapsayıcılı bir Service Fabric uygulaması için bildirim ayarları hakkında bilgi edinin.
services: service-fabric
documentationcenter: na
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2018
ms.author: pepogors
ms.openlocfilehash: 622e6f7552d91cdb9ccf3668c302496c68a5920f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665993"
---
# <a name="multi-container-application-and-service-manifest-examples"></a>Birden çok kapsayıcılı uygulama ve hizmet bildirimi örnekleri
Çok kapsayıcılı bir Service Fabric uygulaması için uygulama ve hizmet bildirimleri örnekleri aşağıda verilmiştir. Bu örnekler amacı hangi ayarlar kullanılabilir ve bunların nasıl kullanılacağını göstermektir. Bu uygulama ve hizmet bildirimleri dayalı [Windows Server 2016 kapsayıcı örnek](https://github.com/Azure-Samples/service-fabric-containers/tree/master/Windows) bildirimleri.

Aşağıdaki özellikleri gösterilmiştir:

|Bildirim|Özellikler|
|---|---|
|[Uygulama bildirimi](#application-manifest)| [ortam değişkenlerini geçersiz kılmak](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [kapsayıcı bağlantı noktasından konak eşlemeyi yapılandırma](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery), [kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırma](service-fabric-get-started-containers.md#configure-container-registry-authentication), [kaynak İdaresi](service-fabric-resource-governance.md), [yalıtım modunu](service-fabric-get-started-containers.md#configure-isolation-mode), [işletim sistemi derleme özgü kapsayıcı görüntüleri belirtme](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)| 
|[FrontEndService hizmet bildirimi](#frontendservice-service-manifest)| [ortam değişkenlerini ayarlama](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [bir uç nokta yapılandırma](service-fabric-get-started-containers.md#configure-communication), komutları kapsayıcıya geçirmek [sertifika bir kapsayıcıya alma](service-fabric-securing-containers.md)| 
|[BackEndService hizmet bildirimi](#backendservice-service-manifest)|[ortam değişkenlerini ayarlama](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [bir uç nokta yapılandırma](service-fabric-get-started-containers.md#configure-communication), [birim sürücüsü yapılandırma](service-fabric-containers-volume-logging-drivers.md)| 

Bkz: [uygulama bildirim öğeleri](#application-manifest-elements), [FrontEndService hizmet bildirim öğeleri](#frontendservice-service-manifest-elements), ve [BackEndService hizmet bildirim öğeleri](#backendservice-service-manifest-elements) özel hakkında daha fazla bilgi için XML öğeleri.

## <a name="application-manifest"></a>Uygulama bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Container.ApplicationType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="BackEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="FrontEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="MaximumIOps" DefaultValue="20"/>
    <Parameter Name="MemoryFront" DefaultValue="4084" />
    <Parameter Name="MemoryBack" DefaultValue="2048" />
    <Parameter Name="CertThumbprint" DefaultValue=""/>
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="BackEndServicePkg" ServiceManifestVersion="1.0.0" />    
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryFront]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[MemoryFront]" BlockIOWeight="[BlockIOWeight]" MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" MemorySwapInMB="[MemorySwapInMB]"/>
      
      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
        
        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>
        
        <!-- This binds the port the container is listening on (8905 in this sample) to an endpoint resource named "BackEndServiceTypeEndpoint", which is defined in the service manifest.  -->
        <PortBinding ContainerPort="8905" EndpointRef="BackEndServiceTypeEndpoint"/>
        
        <!-- Configure the Azure Files volume plugin.  Bind the source folder on the host VM or a remote share to the destination folder within the running container. -->
        <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
          <!-- Driver options to be passed to driver. The Azure Files volume plugin supports the following driver options:
            shareName (the Azure Files file share that provides the volume for the container), storageAccountName (the Azure storage account
            that contains the Azure Files file share), storageAccountKey (Access key for the Azure storage account that contains the Azure Files file share).
            These three driver options are required. -->
          <DriverOption Name="shareName" Value="" />
          <DriverOption Name="storageAccountName" Value="MY-STORAGE-ACCOUNT-NAME" />
          <DriverOption Name="storageAccountKey" Value="MY-STORAGE-ACCOUNT-KEY" />
        </Volume>
        
        <!-- Windows Server containers may not be compatible across different versions of the OS.  You can specify multiple OS images per container and tag 
        them with the build versions of the OS. Get the build version of the OS by running "winver" at a Windows command prompt. -->
        <ImageOverrides>
          <!-- If the underlying OS is build version 16299 (Windows Server version 1709), Service Fabric picks the container image tagged with Os="16299". -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_1709" Os="16299" />
          
          <!-- An untagged container image is assumed to work across all versions of the OS and overrides the image specified in the service manifest. -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_default" />          
        </ImageOverrides>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>

  <!-- Policies to be applied to the imported service manifest. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    
    <!-- This enables you to provide different values for environment variables when creating a FrontEndService
         Theses environment variables are declared in the FrontEndServiceType service manifest-->
    <EnvironmentOverrides CodePackageRef="Code">
      <EnvironmentVariable Name="BackendServiceName" Value="Container.Application/BackEndService"/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentOverrides>
    
    <!-- This policy maps the  port of the container (80) to the endpoint declared in the service, 
         FrontEndServiceTypeEndpoint which is exposed as port 80 on the host-->    
    <Policies>

      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryBack]"/>

      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">

        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>

        <!-- Binds an endpoint resource (declared in the service manifest) to the exposed container port. -->
        <PortBinding ContainerPort="80" EndpointRef="FrontEndServiceTypeEndpoint"/>

        <!-- Import a certificate into the container.  The certificate must be installed in the LocalMachine store of all the cluster nodes.
          When the application starts, the runtime reads the certificate and generates a PFX file and password (on Windows) or a PEM file (on Linux).
          The PFX file and password are accessible in the container using the Certificates_ServicePackageName_CodePackageName_CertName_PFX and 
          Certificates_ServicePackageName_CodePackageName_CertName_Password environment variables. The PEM file is accessible in the container using the 
          Certificates_ServicePackageName_CodePackageName_CertName_PEM and Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey environment variables.-->
        <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[CertThumbprint]" />

        <!-- If the certificate is already in PFX or PEM form, you can create a data package inside your application and reference that certificate here. -->
        <CertificateRef Name="MyCert2" DataPackageRef="Data" DataPackageVersion="1.0.0" RelativePath="MyCert2.PFX" Password="ENCRYPTED-PASSWORD" IsPasswordEncrypted="true"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
        
    <Service Name="FrontEndService" >
      <StatelessService ServiceTypeName="FrontEndServiceType" InstanceCount="[FrontEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
        <Service Name="BackEndService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="BackEndServiceType" InstanceCount="[BackEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="frontendservice-service-manifest"></a>FrontEndService hizmet bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="FrontEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="FrontEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricfrontendservice:v1</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container or exe.  These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="IsContainer" Value=""/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Data package is the contents of the Data directory under PackageRoot that contains an 
       independently-updateable and versioned static data that's consumed by the process at runtime. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme and port 80 -->
      <Endpoint Name="FrontEndServiceTypeEndpoint" UriScheme="http" Port="80"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="backendservice-service-manifest"></a>BackEndService hizmet bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="BackEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="BackEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricbackendservice:v1</ImageName>
        
        <!-- Pass comma delimited commands to your container. -->
        <Commands> dotnet, myproc.dll, 5 </Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container. These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the host port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme. In this case since no port is specified, one is created and assigned dynamically
           to the service. This dynamically assigned host port is mapped to the container port (8905 in this sample),
            which was specified in the application manifest.-->
      <Endpoint Name="BackEndServiceTypeEndpoint" UriScheme="http" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="application-manifest-elements"></a>Uygulama bildirimi öğesi
### <a name="applicationmanifest-element"></a>ApplicationManifest öğesi
Uygulama türü ve sürümü bildirimli olarak açıklar. Uygulama türü oluşturmak için bir veya daha fazla hizmet bildirimleri bağlı hizmetlerin başvurulur. Bağlı hizmetler, yapılandırma ayarlarını parametreli uygulama ayarları kullanarak geçersiz kılınabilir. Varsayılan Hizmetleri, hizmet şablonları, ilkeleri, ilkeleri, tanılama ayarı ve ayrıca uygulama düzeyinde bildirilen sertifikaları verebilir. Daha fazla bilgi için [ApplicationManifest öğesi](service-fabric-service-model-schema-elements.md#ApplicationManifestElementApplicationManifestTypeComplexType)

### <a name="parameters-element"></a>Parametre öğesi
Bu uygulama bildiriminde kullanılan parametreleri bildirir. Uygulama oluşturulur ve uygulama veya hizmet yapılandırma ayarlarını geçersiz kılmak için kullanılabilir olduğunda bu parametre değeri sağlanabilir. Daha fazla bilgi için [parametreleri öğesi](service-fabric-service-model-schema-elements.md#ParametersElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType)

### <a name="parameter-element"></a>Parameter öğesi
Bu bildirimde kullanılmak üzere bir uygulama parametresi. Parametre değeri uygulama Örnekleme sırasında değiştirilebilir veya herhangi bir değer sağlanmazsa varsayılan değeri kullanılır. Daha fazla bilgi için [Parameter öğesi](service-fabric-service-model-schema-elements.md#ParameterElementanonymouscomplexTypeComplexTypeDefinedInParameterselement)

### <a name="servicemanifestimport-element"></a>Servicemanifestımport öğesi
Hizmet geliştiricisi tarafından oluşturulan bir hizmet bildirimi alır. Uygulamadaki her bir bağlı hizmet için bir hizmet bildirimi aktarılması gerekir. Yapılandırmayı geçersiz kılar ve ilkeleri hizmet bildirimi için bildirilebilir. Daha fazla bilgi için [Servicemanifestımport öğesi](service-fabric-service-model-schema-elements.md#ServiceManifestImportElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType)

### <a name="servicemanifestref-element"></a>ServiceManifestRef öğesi
Hizmet bildirimi, başvuruya göre alır. Şu anda hizmet bildirim dosyasını (ServiceManifest.xml) yapı paketinde bulunmalıdır. Daha fazla bilgi için [ServiceManifestRef öğesi](service-fabric-service-model-schema-elements.md#ServiceManifestRefElementServiceManifestRefTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="policies-element"></a>İlke öğesi
İçeri aktarılan bir hizmet bildiriminde uygulanacak ilkeler (uç nokta bağlama, paket paylaşımı, farklı çalıştır ve güvenlik erişim) açıklar. Daha fazla bilgi için [ilkeleri öğesi](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>ServicePackageResourceGovernancePolicy öğesi
Tüm hizmet paketi düzeyinde mi uygulanacağına kaynak İdaresi İlkesi tanımlar. Daha fazla bilgi için [ServicePackageResourceGovernancePolicy öğesi](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType)

### <a name="resourcegovernancepolicy-element"></a>ResourceGovernancePolicy öğesi
Kod paketi için kaynak sınırları belirtir. Daha fazla bilgi için [ResourceGovernancePolicy öğesi](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement)

### <a name="containerhostpolicies-element"></a>Healthcheck öğesi
Kapsayıcı konağında etkinleştirdiğiniz için ilkeleri belirtir. Daha fazla bilgi için [Healthcheck öğesi](service-fabric-service-model-schema-elements.md#ContainerHostPoliciesElementContainerHostPoliciesTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="repositorycredentials-element"></a>RepositoryCredentials öğesi
Kapsayıcı görüntüsünü çekme görüntülerden depoya için kimlik bilgileri. Daha fazla bilgi için [RepositoryCredentials öğesi](service-fabric-service-model-schema-elements.md#RepositoryCredentialsElementRepositoryCredentialsTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="portbinding-element"></a>PortBinding öğesi
İfşa edilen kapsayıcı bağlantı noktasına bağlamak üzere hangi uç nokta kaynağı belirtir. Daha fazla bilgi için [PortBinding öğesi](service-fabric-service-model-schema-elements.md#PortBindingElementPortBindingTypeComplexTypeDefinedInServicePackageContainerPolicyTypecomplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="volume-element"></a>Birim öğesi
Kapsayıcıya bağlı birimin belirtir. Daha fazla bilgi için [birim öğesi](service-fabric-service-model-schema-elements.md#VolumeElementContainerVolumeTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="driveroption-element"></a>DriverOption öğesi
Sürücü Seçenekleri'sürücüsüne geçirilecek. Daha fazla bilgi için [DriverOption öğesi](service-fabric-service-model-schema-elements.md#DriverOptionElementDriverOptionTypeComplexTypeDefinedInContainerLoggingDriverTypecomplexTypeDefinedInContainerVolumeTypecomplexType)

### <a name="imageoverrides-element"></a>ImageOverrides öğesi
Windows Server kapsayıcıları, işletim Sisteminin farklı sürümleri arasında uyumlu olmayabilir.  Kapsayıcı başına birden çok işletim sistemi görüntüsü belirtin ve bunların işletim Sisteminin derleme sürümleriyle etiketleyin. İşletim sistemi derleme sürümü "winver" bir Windows komut isteminde çalıştırarak alın. Temel işletim sistemi derleme sürümü 16299 (Windows Server 1709 sürümü) varsa, Service Fabric, Os ile etiketlenmiş kapsayıcı görüntüsünü seçer. "16299" =. Etiketsiz bir kapsayıcı görüntüsü, işletim sistemi sürümleri arasında iş kabul edilir ve hizmet bildiriminde belirtilen görüntünün geçersiz kılar. Daha fazla bilgi için [ImageOverrides öğesi](service-fabric-service-model-schema-elements.md#ImageOverridesElementImageOverridesTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="image-element"></a>Görüntü öğesi
Başlatılacak olan işletim sistemi derleme sürüm numarasına karşılık gelen kapsayıcı görüntüsü. İşletim sistemi özniteliği belirtilmezse, kapsayıcı görüntüsünün işletim sistemi sürümleri arasında iş kabul edilir ve hizmet bildiriminde belirtilen görüntünün geçersiz kılar. Daha fazla bilgi için [görüntü öğesi](service-fabric-service-model-schema-elements.md#ImageElementImageTypeComplexTypeDefinedInImageOverridesTypecomplexType)

### <a name="environmentoverrides-element"></a>EnvironmentOverrides öğesi
 Daha fazla bilgi için [EnvironmentOverrides öğesi](service-fabric-service-model-schema-elements.md#EnvironmentOverridesElementEnvironmentOverridesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="environmentvariable-element"></a>Değişkeninin öğesi
ortam değişkeni. Daha fazla bilgi için [değişkeninin öğesi](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="certificateref-element"></a>CertificateRef öğesi
X X509 hakkında bilgiler için kapsayıcı ortamında kullanıma için olan sertifika. Sertifika yüklü olmalıdır tüm küme düğümlerinin LocalMachine deposundaki.
Uygulama başlatıldığında, çalışma zamanı sertifikayı okur ve bir PFX dosyası ve (Windows) parolasını ya da bir PEM dosyası (Linux) oluşturur.
Parola ve PFX dosyasını Certificates_ServicePackageName_CodePackageName_CertName_PFX ve Certificates_ServicePackageName_CodePackageName_CertName_Password ortam değişkenlerini kullanarak kapsayıcıda erişilebilir. PEM dosyasını Certificates_ServicePackageName_CodePackageName_CertName_PEM ve Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey ortam değişkenlerini kullanarak kapsayıcıda erişilebilir. Daha fazla bilgi için [CertificateRef öğesi](service-fabric-service-model-schema-elements.md#CertificateRefElementContainerCertificateTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="defaultservices-element"></a>DefaultServices öğesi
Bu uygulama türüne karşı bir uygulama örneği olduğunda otomatik olarak oluşturulan hizmet örnekleri bildirir. Daha fazla bilgi için [DefaultServices öğesi](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType)

### <a name="service-element"></a>Hizmet öğesi
Uygulama başlatıldığında otomatik olarak oluşturulması için bir hizmet bildirir. Daha fazla bilgi için [hizmet öğesi](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType)

### <a name="statelessservice-element"></a>StatelessService öğesi
Durum bilgisi olmayan hizmet tanımlar. Daha fazla bilgi için [StatelessService öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)


## <a name="frontendservice-service-manifest-elements"></a>FrontEndService hizmet bildirim öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest öğesi
Bildirimli olarak hizmet türü ve sürümü açıklanmaktadır. Bu, birlikte bir veya daha fazla hizmet türlerini desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kod, yapılandırma ve veri paketleri listeler. Kaynaklar, tanılama ayarları ve hizmet türü, sistem durumu özellikleri ve ölçümler, Yük Dengeleme gibi hizmet meta verileri ayrıca belirtilir. Daha fazla bilgi için [ServiceManifest öğesi](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes öğesi
Bu bildirimdeki bir CodePackage tarafından desteklenen hangi hizmet türlerini tanımlar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Hizmet türlerini, bildirim düzeyini ve kod paket düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes öğesi](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>StatelessServiceType öğesi
Durum bilgisi olmayan hizmet türünü tanımlar. Daha fazla bilgi için [StatelessServiceType öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage öğesi
Tanımladığınız hizmet türünü destekleyen bir kod paketi açıklar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Sonuçta elde edilen işlemler, çalışma zamanında desteklenen hizmet türlerinin kaydetme beklenir. Birden çok kod paketleri olduğunda, sistemin bildirilen hizmet türlerini herhangi biri için görünür olduğunda, tüm etkinleştirilir. Daha fazla bilgi için [CodePackage öğesi](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>EntryPoint öğesi
Giriş noktası tarafından belirtilen yürütülebilir genellikle uzun süre çalışan hizmet yöneticisidir. Ayrı bir Kurulum giriş noktası varlığını, hizmet ana bilgisayarı uzun sürelerle yüksek ayrıcalıklarla çalıştır gereğini ortadan kaldırır. Giriş noktası tarafından belirtilen yürütülebilir SetupEntryPoint başarıyla çıktıktan sonra çalıştırılır. Sonuçta elde edilen işlem izlenir ve hiç olmadığı kadar sonlandırır veya çöküyor (başlayarak tekrar SetupEntryPoint) yeniden. Daha fazla bilgi için [EntryPoint öğesi](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="containerhost-element"></a>ContainerHost öğesi
 Daha fazla bilgi için [ContainerHost öğesi](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="imagename-element"></a>IMAGENAME öğesi
Depo ve görüntü https://hub.docker.com veya Azure Container Registry. Daha fazla bilgi için [IMAGENAME öğesi](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="environmentvariables-element"></a>EnvironmentVariables öğesi
Ortam değişkenleri, kapsayıcı veya exe geçirin.  Daha fazla bilgi için [EnvironmentVariables öğesi](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="environmentvariable-element"></a>Değişkeninin öğesi
ortam değişkeni. Daha fazla bilgi için [değişkeninin öğesi](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage öğesi
Bildiren bir klasör adı özniteliği tarafından adlandırılan bir Settings.xml dosyasının içerir. Bu dosya, çalışma zamanında işlem okuyabilen kullanıcı tanımlı, anahtar-değer çifti ayarları bölümlerini içerir. Yalnızca ConfigPackage sürümü değişti, yükseltme sırasında daha sonra çalışan işlemi yeniden başlatılmaz. Bunun yerine, bunlar dinamik olarak yeniden yüklenebilir, böylece yapılandırma ayarları değişti işlemi bir geri çağırma bildirir. Daha fazla bilgi için [ConfigPackage öğesi](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>Nın DataPackage öğesi
Statik veri dosyalarının bulunduğu Name özniteliği tarafından adlı bir klasör, bildirir. Service Fabric, tüm exe ve herhangi bir hizmet bildiriminde listelenen veri paketleri yükseltildiğinde konak ve Destek paketlerinde belirtilen DLLHOSTs dönüşüm. Daha fazla bilgi için [nın DataPackage öğesi](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Kaynaklar öğesi
Derlenmiş kodunu değiştirmeden bildirilen ve hizmet dağıtıldığında değiştirildi. Bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin sorumluları ve ilkeleri bölümleri denetlenir. Daha fazla bilgi için [kaynaklar öğesi](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Bitiş öğesi
Hizmet uç noktalarını tanımlar. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Uç nokta öğesi
Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)


## <a name="backendservice-service-manifest-elements"></a>BackEndService hizmet bildirim öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest öğesi
Bildirimli olarak hizmet türü ve sürümü açıklanmaktadır. Bu, birlikte bir veya daha fazla hizmet türlerini desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kod, yapılandırma ve veri paketleri listeler. Kaynaklar, tanılama ayarları ve hizmet türü, sistem durumu özellikleri ve ölçümler, Yük Dengeleme gibi hizmet meta verileri ayrıca belirtilir. Daha fazla bilgi için [ServiceManifest öğesi](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes öğesi
Bu bildirimdeki bir CodePackage tarafından desteklenen hangi hizmet türlerini tanımlar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Hizmet türlerini, bildirim düzeyini ve kod paket düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes öğesi](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>StatelessServiceType öğesi
Durum bilgisi olmayan hizmet türünü tanımlar. Daha fazla bilgi için [StatelessServiceType öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage öğesi
Tanımladığınız hizmet türünü destekleyen bir kod paketi açıklar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Sonuçta elde edilen işlemler, çalışma zamanında desteklenen hizmet türlerinin kaydetme beklenir. Birden çok kod paketleri olduğunda, sistemin bildirilen hizmet türlerini herhangi biri için görünür olduğunda, tüm etkinleştirilir. Daha fazla bilgi için [CodePackage öğesi](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>EntryPoint öğesi
Giriş noktası tarafından belirtilen yürütülebilir genellikle uzun süre çalışan hizmet yöneticisidir. Ayrı bir Kurulum giriş noktası varlığını, hizmet ana bilgisayarı uzun sürelerle yüksek ayrıcalıklarla çalıştır gereğini ortadan kaldırır. Giriş noktası tarafından belirtilen yürütülebilir SetupEntryPoint başarıyla çıktıktan sonra çalıştırılır. Sonuçta elde edilen işlem izlenir ve hiç olmadığı kadar sonlandırır veya çöküyor (başlayarak tekrar SetupEntryPoint) yeniden. Daha fazla bilgi için [EntryPoint öğesi](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="containerhost-element"></a>ContainerHost öğesi
Daha fazla bilgi için [ContainerHost öğesi](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="imagename-element"></a>IMAGENAME öğesi
Depo ve görüntü https://hub.docker.com veya Azure Container Registry. Daha fazla bilgi için [IMAGENAME öğesi](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="commands-element"></a>Commands öğesi
Virgülle ayrılmış listesini komutları kapsayıcıya geçirin. Daha fazla bilgi için [Commands öğesi](service-fabric-service-model-schema-elements.md#CommandsElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="environmentvariables-element"></a>EnvironmentVariables öğesi
Ortam değişkenleri, kapsayıcı veya exe geçirin.  Daha fazla bilgi için [EnvironmentVariables öğesi](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="environmentvariable-element"></a>Değişkeninin öğesi
ortam değişkeni. Daha fazla bilgi için [değişkeninin öğesi](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage öğesi
Bildiren bir klasör adı özniteliği tarafından adlandırılan bir Settings.xml dosyasının içerir. Bu dosya, çalışma zamanında işlem okuyabilen kullanıcı tanımlı, anahtar-değer çifti ayarları bölümlerini içerir. Yalnızca ConfigPackage sürümü değişti, yükseltme sırasında daha sonra çalışan işlemi yeniden başlatılmaz. Bunun yerine, bunlar dinamik olarak yeniden yüklenebilir, böylece yapılandırma ayarları değişti işlemi bir geri çağırma bildirir. Daha fazla bilgi için [ConfigPackage öğesi](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Kaynaklar öğesi
Derlenmiş kodunu değiştirmeden bildirilen ve hizmet dağıtıldığında değiştirildi. Bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin sorumluları ve ilkeleri bölümleri denetlenir. Daha fazla bilgi için [kaynaklar öğesi](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Bitiş öğesi
Hizmet uç noktalarını tanımlar. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Uç nokta öğesi
 Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

