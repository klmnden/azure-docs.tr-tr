---
title: Cloud Services rolü yapılandırması XPath hızlı başvuru sayfası | Microsoft Docs
description: Çeşitli XPath ayarları, ayarları bir ortam değişkeni kullanıma sunmak için bulut hizmeti rolü yapılandırması kullanabilirsiniz.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: 53a262af421dd986e6b70af173a6e8b3f7c06f64
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60527285"
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>XPath ile bir ortam değişkeni olarak rol yapılandırma ayarlarını kullanıma sunma
Bulut hizmeti çalışan veya web rolü Hizmet tanım dosyası, ortam değişkenleri olarak çalışma zamanı yapılandırma değerlerini getirebilir. Aşağıdaki XPath değerleri (Bu API değerlere karşılık gelir) desteklenir.

Bu XPath değerleri de aracılığıyla [Microsoft.WindowsAzure.ServiceRuntime](/previous-versions/azure/reference/ee773173(v=azure.100)) kitaplığı. 

## <a name="app-running-in-emulator"></a>Öykünücüsünde çalışan uygulama
Uygulamayı öykünücüde çalıştığını gösterir.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/Deployment/@emulated" |
| Kod |var x RoleEnvironment.IsEmulated; = |

## <a name="deployment-id"></a>Dağıtım Kimliği
Örneği için dağıtım Kimliğini alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/Deployment/@id" |
| Kod |Varyasyon Deploymentıd RoleEnvironment.DeploymentId; = |

## <a name="role-id"></a>Rol Kimliği
Örneğinin geçerli rol Kimliğini alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@id" |
| Kod |değişken kimliği RoleEnvironment.CurrentRoleInstance.Id; = |

## <a name="update-domain"></a>Etki alanını güncelleştirme
Güncelleme etki alanı örneği alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kod |var ud RoleEnvironment.CurrentRoleInstance.UpdateDomain; = |

## <a name="fault-domain"></a>Hata etki alanı
Hata etki alanı örneği alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kod |var fd RoleEnvironment.CurrentRoleInstance.FaultDomain; = |

## <a name="role-name"></a>Rol adı
Örnek rol adını alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Kod |Varyasyon rname RoleEnvironment.CurrentRoleInstance.Role.Name; = |

## <a name="config-setting"></a>Yapılandırma ayarı
Belirtilen bir yapılandırma ayarı değerini alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/ConfigurationSettings/ConfigurationSetting [@name'Setting1' =]/@value" |
| Kod |değişken ayarı RoleEnvironment.GetConfigurationSettingValue("Setting1"); = |

## <a name="local-storage-path"></a>Yerel depolama yolu
Örneğin yerel depolama yolunu alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/LocalResources/LocalResource [@name'LocalStore1' =]/@path" |
| Kod |Varyasyon localResourcePath RoleEnvironment.GetLocalResource("LocalStore1") =. RootPath; |

## <a name="local-storage-size"></a>Yerel depolama boyutu
Örneğin yerel depolama boyutunu alır.

| Tür | Örnek |
| --- | --- |
| XPath |XPath = "/ RoleEnvironment/currentınstance değerinin/LocalResources/LocalResource [@name'LocalStore1' =]/@sizeInMB" |
| Kod |Varyasyon localResourceSizeInMB RoleEnvironment.GetLocalResource("LocalStore1") =. MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Protokol uç noktası
Uç nokta Protokolü örneği alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Kod |değişken değerler bağlantı noktası RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. Protokol; |

## <a name="endpoint-ip"></a>Uç noktası IP
Belirtilen uç noktasının IP adresini alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Kod |var adresi RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Uç nokta bağlantı noktası
Uç nokta bağlantı noktası örneği alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Kod |var bağlantı noktası RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Port; |

## <a name="example"></a>Örnek
İşte bir örnek adlı bir ortam değişkeni başlangıç görevi oluşturan bir çalışan rolünün `TestIsEmulated` kümesine [ @emulated xpath değeri](#app-running-in-emulator). 

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
Daha fazla bilgi edinin [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosya.

Oluşturma bir [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paket.

Etkinleştirme [Uzak Masaüstü](cloud-services-role-enable-remote-desktop-new-portal.md) bir rol için.

