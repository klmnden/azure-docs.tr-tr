---
title: Azure Service Fabric güvenilir Hizmetleri uygulama bildirim örnekleri | Microsoft Docs
description: Uygulama yapılandırma ve hizmet için Service Fabric reliable services uygulaması bildirim ayarları hakkında bilgi edinin.
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
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: 6c4c8f0ee6aa12c58e02f71b42312cd6872076aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60719173"
---
# <a name="reliable-services-application-and-service-manifest-examples"></a>Güvenilir hizmetler uygulaması ve hizmet bildirimi örnekleri
Bir ASP.NET Core web ön ucu ve durum bilgisi olan bir arka uç Service Fabric uygulaması için uygulama ve hizmet bildirimleri örnekleri aşağıda verilmiştir. Bu örnekler amacı hangi ayarlar kullanılabilir ve bunların nasıl kullanılacağını göstermektir. Bu uygulama ve hizmet bildirimleri dayalı [Service Fabric .NET hızlı](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) bildirimleri.

Aşağıdaki özellikleri gösterilmiştir:

|Bildirimi|Özellikler|
|---|---|
|[Uygulama bildirimi](#application-manifest)| [Kaynak İdaresi](service-fabric-resource-governance.md), [bir hizmeti bir yerel yönetici farklı çalıştır hesabı](service-fabric-application-runas-security.md), [varsayılan ilke, tüm hizmet kodu paketlere uygulamaları](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages), [kullanıcı ve grup ilkelerioluşturma](service-fabric-application-runas-security.md), bir veri paketi hizmet örnekleri arasında paylaşmak [hizmet uç noktaları geçersiz kıl](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)| 
|FrontEndService hizmet bildirimi| [Hizmetin başlatılması sırasında bir betik çalıştırma](service-fabric-run-script-at-service-startup.md), [bir HTTPS uç noktası tanımlama](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest) | 
|BackEndService hizmet bildirimi| [Yapılandırma paketi bildirmek](service-fabric-application-and-service-manifests.md), [veri paketi bildirmek](service-fabric-application-and-service-manifests.md), [bir uç nokta yapılandırma](service-fabric-service-manifest-resources.md)| 

Bkz: [uygulama bildirim öğeleri](#application-manifest-elements), [VotingWeb hizmeti bildirim öğelerinin](#votingweb-service-manifest-elements), ve [VotingData hizmet bildirim öğeleri](#votingdata-service-manifest-elements) belirli XML hakkında daha fazla bilgi için öğeleri.

## <a name="application-manifest"></a>Uygulama bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="Memory" DefaultValue="4084" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="Port" DefaultValue="8081" />
    <Parameter Name="Protocol" DefaultValue="tcp" />
    <Parameter Name="Type" DefaultValue="internal" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <!-- Override endpoints declared in the service manifest. -->
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="DataEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
      </Endpoints>
    </ResourceOverrides>

    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[Memory]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[Memory]" BlockIOWeight="[BlockIOWeight]" 
                                MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" 
                                MemorySwapInMB="[MemorySwapInMB]"/>

      <!-- Share the data package across multiple instances of the VotingData service-->
      <PackageSharingPolicy PackageRef="VotingDataPkg.Data"/>

      <!-- Give read rights on the "DataEndpoint" endpoint to the Customer2 account.-->
      <SecurityAccessPolicy GrantRights="Read" PrincipalRef="Customer2" ResourceRef="DataEndpoint" ResourceType="Endpoint"/>         
    </Policies>
  </ServiceManifestImport>
  
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Run the setup entry point (defined in the imported service manifest) as the SetupAdminUser account 
      (declared in the following Principals section). -->
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      
    </Policies>

  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <!-- Define users and groups required to run the services and access resources.  Principals are used in the Policies section(s). -->
  <Principals>
    <!-- Declare a set of groups as security principals, which can be referenced in policies. Groups are useful if there are multiple users 
    for different service entry points and they need to have certain common privileges that are available at the group level. -->
    <Groups>
      <!-- Create a group that has administrator privileges. -->
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <!-- Declare a user and add the user to the Administrators system group. The SetupAdminUser account is used to run the 
      setup entry point of the VotingWebPkg code package (described in the preceding Policies section).-->
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
      <!-- Create a user. Local user accounts are created on the machines where the application is deployed. By default, these accounts 
      do not have the same names as those specified here. Instead, they are dynamically generated and have random passwords. -->
      <User Name="Customer1" >
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as a local user with the specified account name and password. Local user accounts are created on the machines 
      where the application is deployed. -->
      <User Name="Customer2" AccountType="LocalUser" AccountName="Customer1" Password="MyPassword">
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as NetworkService. -->
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <!-- Policies applied at the application level. -->
  <Policies>
    <!-- Specify a default user account for all code packages that don’t have a specific RunAsPolicy defined in 
    the ServiceManifestImport section(s). -->
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
    
  </Policies>
</ApplicationManifest>

```

## <a name="votingweb-service-manifest"></a>VotingWeb hizmeti bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <!-- A privileged entry point that by default runs with the same credentials as Service Fabric (typically the NetworkService account) before 
    any other entry point. Use the setup entry point to set system environment variables, give the account running the service (NETWORK SERVICE, by default) 
    access to a certificate's private key, or perform other setup tasks. In the application manifest, you can change the security permissions to run the startup script 
    under a local system account or an administrator account.  -->
    <SetupEntryPoint>
      <ExeHost>
        <!-- The setup script to run. -->
        <Program>Setup.bat</Program>
        <!-- Pass arguments to the script when it runs.-->
        <Arguments>MyValue</Arguments>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
        <!-- Warning! Do not use console redirection in a production application, only use it for local development and debugging. Redirects console output from the startup
        script to an output file in the application folder called "log" on the cluster node where the application is deployed and run. Also set the number of output files
        to retain and the maximum file size (in KB). -->
        <ConsoleRedirection FileRetentionCount="10" FileMaxSizeInKb="20480"/>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- Configure a HTTPS endpoint on port 443. This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>

```

## <a name="votingdata-service-manifest"></a>VotingData hizmet bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingDataPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="VotingDataType"  HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingData.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Declares a folder, named by the Name attribute, under PackageRoot that contains a Settings.xml file. This file contains sections of user-defined, 
  key-value pair settings that the process can read back at run time. During an upgrade, if only the ConfigPackage version has changed, 
  then the running process is not restarted. Instead, a callback notifies the process that configuration settings have changed so they can be reloaded dynamically. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Declares a folder, named by the Name attribute, under PackageRoot which contains static data files to be consumed by the process at run time. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- Define an internal (used for intra-application communication) TCP endpoint. Since no port is specified, one is created and assigned dynamically
           to the service.-->
      <Endpoint Name="DataEndpoint" Protocol="tcp" Type="Internal" />
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

### <a name="resourceoverrides-element"></a>ResourceOverrides öğesi
Hizmet bildirimi kaynakları bildirilen uç noktaları için kaynak geçersiz kılmalarını belirtir. Daha fazla bilgi için [ResourceOverrides öğesi](service-fabric-service-model-schema-elements.md#ResourceOverridesElementResourceOverridesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="endpoints-element"></a>Bitiş öğesi
Geçersiz kılmak için uç nokta. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourceOverridesTypecomplexType)

### <a name="endpoint-element"></a>Uç nokta öğesi
Uç nokta geçersiz kılmak için hizmet bildiriminde bildirilmiş. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

### <a name="policies-element"></a>İlke öğesi
İçeri aktarılan bir hizmet bildiriminde uygulanacak ilkeler (uç nokta bağlama, paket paylaşımı, farklı çalıştır ve güvenlik erişim) açıklar. Daha fazla bilgi için [ilkeleri öğesi](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>ServicePackageResourceGovernancePolicy öğesi
Tüm hizmet paketi düzeyinde mi uygulanacağına kaynak İdaresi İlkesi tanımlar. Daha fazla bilgi için [ServicePackageResourceGovernancePolicy öğesi](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType)

### <a name="resourcegovernancepolicy-element"></a>ResourceGovernancePolicy öğesi
Kaynak sınırları için bir codepackage belirtir. Daha fazla bilgi için [ResourceGovernancePolicy öğesi](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement)

### <a name="packagesharingpolicy-element"></a>PackageSharingPolicy öğesi
Kod, yapılandırma veya veri paketi aynı hizmet türünün hizmet örnekleri arasında paylaşılan olmadığını gösterir. Daha fazla bilgi için [PackageSharingPolicy öğesi](service-fabric-service-model-schema-elements.md#PackageSharingPolicyElementPackageSharingPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexType)

### <a name="securityaccesspolicy-element"></a>SecurityAccessPolicy öğesi
Bir sorumlusu (örneğin, bir uç nokta) kaynak üzerinde bir hizmet bildiriminde tanımlanan izni verir erişin. Genellikle, denetlemek ve güvenlik riskleri en aza indirmek için farklı kaynaklarda Hizmetleri erişimini kısıtlamak çok kullanışlıdır. Bu, uygulamanın farklı geliştiriciler tarafından geliştirilen bir Market Hizmetleri koleksiyonu oluşturulur, özellikle önemlidir. Daha fazla bilgi için [SecurityAccessPolicy öğesi](service-fabric-service-model-schema-elements.md#SecurityAccessPolicyElementSecurityAccessPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInSecurityAccessPolicieselementDefinedInDigestedEndpointelement)

### <a name="runaspolicy-element"></a>RunAsPolicy öğesi
Yerel kullanıcı veya hizmet kod paketinin çalışacağı yerel sistem hesabını belirtir. Etki alanı hesapları Azure Active Directory kullanılabilir olduğu Windows Server dağıtımları üzerinde desteklenir. Varsayılan olarak, uygulamaları altında Fabric.exe işlemini çalıştıran hesabı altında çalışır. Uygulamaları ilkeleri bölümünde bildirilen diğer hesapları olarak da çalıştırabilirsiniz. Bir hizmete bir RunAs ilke uygulayın ve hizmet bildirimi uç noktası HTTP protokolü kaynaklarla bildirir, bağlantı noktaları için ayrılan emin olmak için bir SecurityAccessPolicy da belirtmelisiniz kullanıyorsanız bu uç noktaları erişim denetimi için RunAs listelenen doğru şekilde uygulanır. altında çalışacağı bir kullanıcı hesabı. Bir HTTPS uç noktası için ayrıca istemciye döndürmek için sertifika adını belirtmek için bir EndpointBindingPolicy tanımlamak vardır. Daha fazla bilgi için [RunAsPolicy öğesi](service-fabric-service-model-schema-elements.md#RunAsPolicyElementRunAsPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="defaultservices-element"></a>DefaultServices öğesi
Bu uygulama türüne karşı bir uygulama örneği olduğunda otomatik olarak oluşturulan hizmet örnekleri bildirir. Daha fazla bilgi için [DefaultServices öğesi](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType)

### <a name="service-element"></a>Hizmet öğesi
Uygulama başlatıldığında otomatik olarak oluşturulması için bir hizmet bildirir. Daha fazla bilgi için [hizmet öğesi](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType)

### <a name="statefulservice-element"></a>StatefulService öğesi
Durum bilgisi olan hizmet tanımlar. Daha fazla bilgi için [StatefulService öğesi](service-fabric-service-model-schema-elements.md#StatefulServiceElementStatefulServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)

### <a name="statelessservice-element"></a>StatelessService öğesi
Durum bilgisi olmayan hizmet tanımlar. Daha fazla bilgi için [StatelessService öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)

### <a name="principals-element"></a>İlkeleri öğesi
Bu uygulama hizmetleri ve güvenli kaynaklara çalıştırmak gerekli olan güvenlik sorumlularının (kullanıcılar, gruplar) açıklar. İlkeleri ilkeleri bölümlerde başvurulur. Daha fazla bilgi için [sorumluları öğesi](service-fabric-service-model-schema-elements.md#PrincipalsElementSecurityPrincipalsTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="groups-element"></a>Groups öğesi
Grupları kümesi güvenlik ilkelerini başvurulabilir sorumlularını bildirir. Gruplar farklı hizmet giriş noktaları için birden çok kullanıcı ve grup düzeyinde kullanılabilir olan belirli ortak ayrıcalıklara sahip olması için ihtiyaç duydukları yararlıdır. Daha fazla bilgi için [Groups öğesi](service-fabric-service-model-schema-elements.md#GroupsElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType)

### <a name="group-element"></a>Group öğesi
İlkeleri başvurulabilen bir grup bir güvenlik sorumlusu olarak bildirir. Daha fazla bilgi için [Group öğesi](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInGroupselement)

### <a name="membership-element"></a>Membership öğesi
 Daha fazla bilgi için [Membership öğesi](service-fabric-service-model-schema-elements.md#MembershipElementanonymouscomplexTypeComplexTypeDefinedInGroupelement)

### <a name="systemgroup-element"></a>SystemGroup öğesi
 Daha fazla bilgi için [SystemGroup öğesi](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMembershipelement)

### <a name="users-element"></a>Kullanıcılar öğesi
Bir kullanıcı kümesini güvenlik ilkelerini başvurulabilir sorumlularını bildirir. Daha fazla bilgi için [kullanıcılar öğesi](service-fabric-service-model-schema-elements.md#UsersElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType)

### <a name="user-element"></a>Kullanıcı öğesi
İlkeleri başvurulan bir kullanıcı güvenlik sorumlusu olarak bildirir. Daha fazla bilgi için [kullanıcı öğesi](service-fabric-service-model-schema-elements.md#UserElementanonymouscomplexTypeComplexTypeDefinedInUserselement)

### <a name="memberof-element"></a>İz öğesi
Tüm özellikleri ve bu üyeliği grubun güvenlik ayarları devralmak için kullanıcıların tüm mevcut üyelik grubuna eklenebilir. Üyeliği grubun, farklı hizmetler veya (farklı bir makinede) aynı hizmet tarafından erişilmesi gereken dış kaynakları güvenli hale getirmek için kullanılabilir. Daha fazla bilgi için [MemberOf öğesi](service-fabric-service-model-schema-elements.md#MemberOfElementanonymouscomplexTypeComplexTypeDefinedInUserelement)

### <a name="systemgroup-element"></a>SystemGroup öğesi
Kullanıcı eklemek için sistem grubu.  Sistem grubu grupları bölümünde tanımlanmış olması gerekir. Daha fazla bilgi için [SystemGroup öğesi](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement)

### <a name="group-element"></a>Group öğesi
Kullanıcı eklemek için Grup.  Grup grupları bölümünde tanımlanmış olması gerekir. Daha fazla bilgi için [Group öğesi](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement)

### <a name="policies-element"></a>İlke öğesi
(Günlük toplama, varsayılan Çalıştır, sistem durumu ve güvenlik erişimi) uygulama düzeyinde uygulanacak ilkeler açıklanır. Daha fazla bilgi için [ilkeleri öğesi](service-fabric-service-model-schema-elements.md#PoliciesElementApplicationPoliciesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="defaultrunaspolicy-element"></a>DefaultRunAsPolicy öğesi
Servicemanifestımport bölümünde tanımlanan belirli bir RunAsPolicy olmayan tüm hizmet kod paketleri için bir varsayılan kullanıcı hesabı belirtin. Daha fazla bilgi için [DefaultRunAsPolicy öğesi](service-fabric-service-model-schema-elements.md#DefaultRunAsPolicyElementanonymouscomplexTypeComplexTypeDefinedInApplicationPoliciesTypecomplexType)




## <a name="votingweb-service-manifest-elements"></a>VotingWeb hizmeti bildirim öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest öğesi
Bildirimli olarak hizmet türü ve sürümü açıklanmaktadır. Bu, birlikte bir veya daha fazla hizmet türlerini desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kod, yapılandırma ve veri paketleri listeler. Kaynaklar, tanılama ayarları ve hizmet türü, sistem durumu özellikleri ve ölçümler, Yük Dengeleme gibi hizmet meta verileri ayrıca belirtilir. Daha fazla bilgi için [ServiceManifest öğesi](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes öğesi
Bu bildirimdeki bir CodePackage tarafından desteklenen hangi hizmet türlerini tanımlar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Hizmet türlerini, bildirim düzeyini ve kod paket düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes öğesi](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>StatelessServiceType öğesi
Durum bilgisi olmayan hizmet türünü tanımlar. Daha fazla bilgi için [StatelessServiceType öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage öğesi
Tanımladığınız hizmet türünü destekleyen bir kod paketi açıklar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Sonuçta elde edilen işlemler, çalışma zamanında desteklenen hizmet türlerinin kaydetme beklenir. Birden çok kod paketleri olduğunda, sistemin bildirilen hizmet türlerini herhangi biri için görünür olduğunda, tüm etkinleştirilir. Daha fazla bilgi için [CodePackage öğesi](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="setupentrypoint-element"></a>SetupEntryPoint öğesi
Service Fabric (genellikle NETWORKSERVICE hesabı) aynı kimlik bilgilerini başka bir giriş noktası önce çalışır, varsayılan olarak ayrıcalıklı giriş noktası. Giriş noktası tarafından belirtilen yürütülebilir genellikle uzun süre çalışan hizmet yöneticisidir. Ayrı bir Kurulum giriş noktası varlığını, hizmet ana bilgisayarı uzun sürelerle yüksek ayrıcalıklarla çalıştır gereğini ortadan kaldırır. Daha fazla bilgi için [SetupEntryPoint öğesi](service-fabric-service-model-schema-elements.md#SetupEntryPointElementanonymouscomplexTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>ExeHost öğesi
 Daha fazla bilgi için [ExeHost öğesi](service-fabric-service-model-schema-elements.md#ExeHostElementExeHostEntryPointTypeComplexTypeDefinedInSetupEntryPointelement)

### <a name="program-element"></a>Bir program öğesi
Yürütülebilir adı.  Örneğin, "MySetup.bat" veya "MyServiceHost.exe". Daha fazla bilgi için [Program öğesi](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="arguments-element"></a>Bağımsız değişkenler öğesi
 Daha fazla bilgi için [bağımsız değişkenleri öğesi](service-fabric-service-model-schema-elements.md#ArgumentsElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="workingfolder-element"></a>WorkingFolder öğesi
Kod paketi uygulamanın dağıtıldığı küme düğümü üzerinde işlem için çalışma dizini. Üç değer belirtebilirsiniz: İş (varsayılan), CodePackage veya kod temeli. Kod tabanı çalışma dizini EXE kod paketinde tanımlanır dizinine ayarlandığını belirtir. CodePackage EXE kod paketi dizinde tanımlandığı bağımsız olarak kod paketi kökünde olacak şekilde çalışma dizinini ayarlar. İş, düğüm üzerinde oluşturulan benzersiz bir klasöre çalışma dizinini ayarlar.  Bu klasör, tüm uygulama örneği için aynıdır. Varsayılan olarak, uygulamadaki tüm işlemlerin çalışma dizini, uygulama çalışma klasörü için ayarlanır. Veri işlemleri nerede yazabilirsiniz budur. Bu klasörleri farklı uygulama örnekleri arasında paylaşılan ve silinen verileri kod paketi veya kod tabanına yazma önerilmez. Daha fazla bilgi için [WorkingFolder öğesi](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="consoleredirection-element"></a>ConsoleRedirection öğesi

> [!WARNING]
> Konsol yönlendirmesi, bir üretim uygulamasında kullanmak değil, yalnızca yerel geliştirme ve hata ayıklama için kullanın. Başlangıç betiği konsol çıktısı küme düğümünde burada dağıtılan ve çalıştırma "günlüğü" adlı uygulama klasöründe bir çıktı dosyası için yönlendirir. Daha fazla bilgi için [ConsoleRedirection öğesi](service-fabric-service-model-schema-elements.md#ConsoleRedirectionElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="entrypoint-element"></a>EntryPoint öğesi
Giriş noktası tarafından belirtilen yürütülebilir genellikle uzun süre çalışan hizmet yöneticisidir. Ayrı bir Kurulum giriş noktası varlığını, hizmet ana bilgisayarı uzun sürelerle yüksek ayrıcalıklarla çalıştır gereğini ortadan kaldırır. Giriş noktası tarafından belirtilen yürütülebilir SetupEntryPoint başarıyla çıktıktan sonra çalıştırılır. Sonuçta elde edilen işlem izlenir ve hiç olmadığı kadar sonlandırır veya çöküyor (başlayarak tekrar SetupEntryPoint) yeniden. Daha fazla bilgi için [EntryPoint öğesi](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>ExeHost öğesi
 Daha fazla bilgi için [ExeHost öğesi](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage öğesi
Bir Settings.xml dosyasının içeren PackageRoot altında Name özniteliği tarafından adlı bir klasör bildirir. Bu dosya, çalışma zamanında işlem okuyabilen kullanıcı tanımlı, anahtar-değer çifti ayarları bölümlerini içerir. Yalnızca ConfigPackage sürümü değişti, yükseltme sırasında daha sonra çalışan işlemi yeniden başlatılmaz. Bunun yerine, bunlar dinamik olarak yeniden yüklenebilir, böylece yapılandırma ayarları değişti işlemi bir geri çağırma bildirir. Daha fazla bilgi için [ConfigPackage öğesi](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Kaynaklar öğesi
Derlenmiş kodunu değiştirmeden bildirilen ve hizmet dağıtıldığında değiştirildi. Bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin sorumluları ve ilkeleri bölümleri denetlenir. Daha fazla bilgi için [kaynaklar öğesi](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Bitiş öğesi
Hizmet uç noktalarını tanımlar. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Uç nokta öğesi
Uç nokta geçersiz kılmak için hizmet bildiriminde bildirilmiş. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)



## <a name="votingdata-service-manifest-elements"></a>VotingData hizmet bildirim öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest öğesi
Bildirimli olarak hizmet türü ve sürümü açıklanmaktadır. Bu, birlikte bir veya daha fazla hizmet türlerini desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kod, yapılandırma ve veri paketleri listeler. Kaynaklar, tanılama ayarları ve hizmet türü, sistem durumu özellikleri ve ölçümler, Yük Dengeleme gibi hizmet meta verileri ayrıca belirtilir. Daha fazla bilgi için [ServiceManifest öğesi](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes öğesi
Bu bildirimdeki bir CodePackage tarafından desteklenen hangi hizmet türlerini tanımlar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Hizmet türlerini, bildirim düzeyini ve kod paket düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes öğesi](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statefulservicetype-element"></a>StatefulServiceType öğesi
Durum bilgisi olan hizmet türünü tanımlar. Daha fazla bilgi için [StatefulServiceType öğesi](service-fabric-service-model-schema-elements.md#StatefulServiceTypeElementStatefulServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage öğesi
Tanımladığınız hizmet türünü destekleyen bir kod paketi açıklar. Bu hizmet türlerinden birini karşı bir hizmet örneği oluşturulduğunda, bunların giriş noktaları çalıştırarak bu bildirimde belirtilen tüm kod paketlerinin etkinleştirilir. Sonuçta elde edilen işlemler, çalışma zamanında desteklenen hizmet türlerinin kaydetme beklenir. Birden çok kod paketleri olduğunda, sistemin bildirilen hizmet türlerini herhangi biri için görünür olduğunda, tüm etkinleştirilir. Daha fazla bilgi için [CodePackage öğesi](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>EntryPoint öğesi
Giriş noktası tarafından belirtilen yürütülebilir genellikle uzun süre çalışan hizmet yöneticisidir. Ayrı bir Kurulum giriş noktası varlığını, hizmet ana bilgisayarı uzun sürelerle yüksek ayrıcalıklarla çalıştır gereğini ortadan kaldırır. Giriş noktası tarafından belirtilen yürütülebilir SetupEntryPoint başarıyla çıktıktan sonra çalıştırılır. Sonuçta elde edilen işlem izlenir ve hiç olmadığı kadar sonlandırır veya çöküyor (başlayarak tekrar SetupEntryPoint) yeniden. Daha fazla bilgi için [EntryPoint öğesi](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>ExeHost öğesi
 Daha fazla bilgi için [ExeHost öğesi](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="program-element"></a>Bir program öğesi
Yürütülebilir adı.  Örneğin, "MySetup.bat" veya "MyServiceHost.exe". Daha fazla bilgi için [Program öğesi](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="workingfolder-element"></a>WorkingFolder öğesi
Kod paketi uygulamanın dağıtıldığı küme düğümü üzerinde işlem için çalışma dizini. Üç değer belirtebilirsiniz: İş (varsayılan), CodePackage veya kod temeli. Kod tabanı çalışma dizini EXE kod paketinde tanımlanır dizinine ayarlandığını belirtir. CodePackage EXE kod paketi dizinde tanımlandığı bağımsız olarak kod paketi kökünde olacak şekilde çalışma dizinini ayarlar. İş, düğüm üzerinde oluşturulan benzersiz bir klasöre çalışma dizinini ayarlar.  Bu klasör, tüm uygulama örneği için aynıdır. Varsayılan olarak, uygulamadaki tüm işlemlerin çalışma dizini, uygulama çalışma klasörü için ayarlanır. Veri işlemleri nerede yazabilirsiniz budur. Bu klasörleri farklı uygulama örnekleri arasında paylaşılan ve silinen verileri kod paketi veya kod tabanına yazma önerilmez. Daha fazla bilgi için [WorkingFolder öğesi](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage öğesi
Bir Settings.xml dosyasının içeren PackageRoot altında Name özniteliği tarafından adlı bir klasör bildirir. Bu dosya, çalışma zamanında işlem okuyabilen kullanıcı tanımlı, anahtar-değer çifti ayarları bölümlerini içerir. Yalnızca ConfigPackage sürümü değişti, yükseltme sırasında daha sonra çalışan işlemi yeniden başlatılmaz. Bunun yerine, bunlar dinamik olarak yeniden yüklenebilir, böylece yapılandırma ayarları değişti işlemi bir geri çağırma bildirir. Daha fazla bilgi için [ConfigPackage öğesi](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>Nın DataPackage öğesi
Çalışma zamanında işlem tarafından Tüketilecek statik veri dosyalarını içeren PackageRoot altında Name özniteliği tarafından adlı bir klasör bildirir. Service Fabric, tüm exe ve herhangi bir hizmet bildiriminde listelenen veri paketleri yükseltildiğinde konak ve Destek paketlerinde belirtilen DLLHOSTs dönüşüm. Daha fazla bilgi için [nın DataPackage öğesi](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Kaynaklar öğesi
Derlenmiş kodunu değiştirmeden bildirilen ve hizmet dağıtıldığında değiştirildi. Bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin sorumluları ve ilkeleri bölümleri denetlenir. Daha fazla bilgi için [kaynaklar öğesi](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Bitiş öğesi
Hizmet uç noktalarını tanımlar. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Uç nokta öğesi
Uç nokta geçersiz kılmak için hizmet bildiriminde bildirilmiş. Daha fazla bilgi için [bitiş öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

