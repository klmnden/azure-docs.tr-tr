---
title: "Bulut Hizmetleri rolünü config XPath kopya sayfası | Microsoft Docs"
description: "Çeşitli XPath ayarları bulut hizmeti rolünü yapılandırma ayarlarını bir ortam değişkeni göstermek için kullanabilirsiniz."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: fd6efac829d3fd9e2840362b8d2ff423add566d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Rol yapılandırma ayarlarını XPath olan bir ortam değişkeni olarak kullanıma sunma
Bulut hizmeti çalışan ya da web rolü hizmet tanımı dosyası ortam değişkenleri olarak çalışma zamanı yapılandırma değerlerini getirebilir. Aşağıdaki XPath değerleri (hangi API değerlerine karşılık gelen) desteklenir.

Bu XPath değerleri de aracılığıyla kullanılabilir [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) kitaplığı. 

## <a name="app-running-in-emulator"></a>Öykünücüde çalışan uygulama
Uygulamayı öykünücüde çalışmadığını gösterir.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@emulated" |
| Kod |var x RoleEnvironment.IsEmulated; = |

## <a name="deployment-id"></a>Dağıtım kimliği
Örneğin dağıtım kimliği alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/Deployment/@id" |
| Kod |var Deploymentıd = RoleEnvironment.DeploymentId; |

## <a name="role-id"></a>Rol Kimliği
Örnek için geçerli rol kimliği alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@id" |
| Kod |var kimliği = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>etki alanı güncelleştirme
Güncelleştirme etki alanı örneği alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kod |var ud RoleEnvironment.CurrentRoleInstance.UpdateDomain; = |

## <a name="fault-domain"></a>hata etki alanı
Hata etki alanı örneği alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kod |var fd RoleEnvironment.CurrentRoleInstance.FaultDomain; = |

## <a name="role-name"></a>Rol adı
Bu rol adı örneği alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/RoleEnvironment/CurrentInstance/@roleName" |
| Kod |var rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Yapılandırma ayarı
Belirtilen yapılandırma ayarının değeri alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/ConfigurationSettings/ConfigurationSetting [@name'Setting1' =]/@value" |
| Kod |değişken ayarı RoleEnvironment.GetConfigurationSettingValue("Setting1"); = |

## <a name="local-storage-path"></a>Yerel depolama alanı yolu
Örneğin yerel depolama yolunu alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/LocalResources/LocalResource [@name'LocalStore1' =]/@path" |
| Kod |var localResourcePath RoleEnvironment.GetLocalResource("LocalStore1") =. RootPath; |

## <a name="local-storage-size"></a>Yerel depolama boyutu
Örneğin yerel depolama boyutunu alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/LocalResources/LocalResource [@name'LocalStore1' =]/@sizeInMB" |
| Kod |var localResourceSizeInMB RoleEnvironment.GetLocalResource("LocalStore1") =. MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Uç nokta Protokolü
Örneği için uç nokta Protokolü alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/uç noktalar/uç noktanın [@name'Bitiş noktası 1' =]/@protocol" |
| Kod |var KOR RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. Protokol; |

## <a name="endpoint-ip"></a>Uç nokta IP
Belirtilen uç noktanın IP adresini alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/uç noktalar/uç noktanın [@name'Bitiş noktası 1' =]/@address" |
| Kod |var adresi RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Address |

## <a name="endpoint-port"></a>uç nokta bağlantı noktası
Örneği için uç nokta bağlantı noktası alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/uç noktalar/uç noktanın [@name'Bitiş noktası 1' =]/@port" |
| Kod |bağlantı noktası var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Port; |

## <a name="example"></a>Örnek
Bir başlangıç görevi adında bir ortam değişkeni oluşturan çalışan rolü bir örneği burada verilmiştir `TestIsEmulated` kümesine [ @emulated xpath değeri](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosya.

Oluşturma bir [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paket.

Etkinleştirme [Uzak Masaüstü](cloud-services-role-enable-remote-desktop.md) bir rol için.

