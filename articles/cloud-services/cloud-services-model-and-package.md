---
title: Bir bulut hizmeti modeli ve paket nedir | Microsoft Docs
description: "Bulut hizmeti modeli (.csdef, .cscfg) ve Azure paketine (.cspkg) açıklanır"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 0589f2efeaaafc35bcb9d869c391a0533fe6e502
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Bulut hizmeti modeli nedir ve nasıl paket?
Bir bulut hizmeti üç bileşenlerini hizmet tanımı oluşturulur *(.csdef)*, hizmet yapılandırması *(.cscfg)*ve bir hizmet paketi *(.cspkg)*. Her iki **ServiceDefinition.csdef** ve **ServiceConfig.cscfg** dosyaları XML tabanlı ve topluca modeli olarak adlandırılan bulut hizmeti ve nasıl yapılandırıldığını; yapısını açıklar. **ServicePackage.cspkg** oluşturulduğu bir zip dosyası **ServiceDefinition.csdef** ve bunun yanı sıra, ikili tabanlı tüm gerekli bağımlılıklar içerir. Azure hem de bir bulut hizmeti oluşturur **ServicePackage.cspkg** ve **ServiceConfig.cscfg**.

Azure'da bulut hizmeti çalışır duruma geldiğinde üzerinden yapılandırabilirsiniz **ServiceConfig.cscfg** dosyası, ancak tanımı alter olamaz.

## <a name="what-would-you-like-to-know-more-about"></a>Ne hakkında daha fazla bilgi edinmek istiyorsunuz?
* Daha fazla bilgi almak istiyorum [ServiceDefinition.csdef](#csdef) ve [ServiceConfig.cscfg](#cscfg) dosyaları.
* Zaten bu hakkında bilebilirim ver [bazı örnekler](#next-steps) üzerinde ne ı yapılandırabilirsiniz.
* Oluşturmak istediğiniz [ServicePackage.cspkg](#cspkg).
* Visual Studio kullanarak ve istiyorum...
  * [Bir bulut hizmeti oluştur][vs_create]
  * [Var olan bir bulut hizmetini yeniden yapılandırın][vs_reconfigure]
  * [Bir bulut hizmeti projesini dağıtma][vs_deploy]
  * [Bir bulut hizmeti örneği içine Uzak Masaüstü][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
**ServiceDefinition.csdef** dosyasını bir bulut hizmeti yapılandırmak için Azure tarafından kullanılan ayarları belirtir. [Azure Hizmet tanım Şeması (.csdef dosyası)](https://msdn.microsoft.com/library/azure/ee758711.aspx) Hizmet tanım dosyası için izin verilen biçim sağlar. Aşağıdaki örnek, Web ve çalışan rolleri için tanımlanan ayarları gösterir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Başvurabilirsiniz [Hizmet tanım Şeması](https://msdn.microsoft.com/library/azure/ee758711.aspx) daha iyi burada kullanılan XML Şeması anlamak için Bununla birlikte, İşte bazı öğelerin hızlı bir açıklaması:

**siteleri**  
IIS7'de barındırılan Web siteleri veya web uygulamaları için tanımları içerir.

**InputEndpoints**  
Bulut hizmeti ile iletişim kurmada kullanılan uç noktalar için tanımları içerir.

**InternalEndpoints**  
Rol örnekleri tarafından birbirleri ile iletişim kurmak için kullanılan uç noktalar için tanımları içerir.

**ConfigurationSettings**  
Belirli bir rol özelliklerinin ayarı tanımlarını içerir.

**Sertifikalar**  
Bir rol için gerekli sertifikaları için tanımları içerir. Önceki kod örneğinde Azure Connect yapılandırma için kullanılan bir sertifika gösterilmektedir.

**LocalResources**  
Yerel depolama alanı kaynakları için tanımları içerir. Yerel depolama kaynağı, bir rol örneği çalıştığı sanal makinenin dosya sisteminde ayrılmış bir dizindir.

**İçeri aktarmalar**  
İçeri aktarılan modül tanımlarını içerir. Önceki kod örneğinde, Uzak Masaüstü Bağlantısı ve Azure bağlanmak için modülleri gösterir.

**Startup**  
Rol başladığında çalıştırılan görevleri içerir. Görevler bir .cmd veya yürütülebilir dosya içinde tanımlanır.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Bulut hizmetiniz için ayarları yapılandırma değerleri tarafından belirlenen **ServiceConfiguration.cscfg** dosya. Bu dosyadaki her rol için dağıtmak istediğiniz örneklerinin sayısını belirtin. Hizmet tanımı dosyasında tanımlanan yapılandırma ayarları için değerleri service yapılandırma dosyasına eklenir. Bulut hizmeti ile ilişkilendirilmiş herhangi bir yönetim sertifika parmak izleri de dosyaya eklenir. [Azure hizmet yapılandırma şeması (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/ee758710.aspx) için bir hizmet yapılandırma dosyası izin verilen biçim sağlar.

Hizmet yapılandırma dosyası uygulama ile paketlenmiştir değil, ancak ayrı bir dosya olarak Azure karşıya ve bulut hizmeti yapılandırmak için kullanılır. Bulut hizmetinizi gerek olmadan yeni bir hizmet yapılandırma dosyası karşıya yükleyebilirsiniz. Bulut hizmeti çalışırken bulut hizmeti için yapılandırma değerlerini değiştirilebilir. Aşağıdaki örnek, Web ve çalışan rolleri için tanımlanan yapılandırma ayarlarını gösterir:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Başvurabilirsiniz [hizmet yapılandırma şeması](https://msdn.microsoft.com/library/azure/ee758710.aspx) daha iyi burada kullanılan XML Şeması anlamak için ancak öğeleri hızlı bir açıklaması aşağıda verilmiştir:

**Örnekleri**  
Çalışan rolü için örnek sayısını yapılandırır. Bulut hizmetinizi olası yükseltmeleri sırasında kullanılamaz hale gelmesini önlemek için web dönük rollerinizi birden fazla örneğini dağıtmanız önerilir. Birden fazla örneği dağıtarak, yönergelere karşıladığınızdan [Azure işlem hizmet düzeyi sözleşmesi (SLA)](http://azure.microsoft.com/support/legal/sla/), garanti %99,95 harici bağlantı Internet'e yönelik rolleri, iki veya daha fazla rol örnekleri bir hizmet için dağıtılır.

**ConfigurationSettings**  
Bir rolün çalışan örneklerini için ayarları yapılandırır. Adını `<Setting>` öğeleri hizmet tanımı dosyası ayarı tanımlarında eşleşmesi gerekir.

**Sertifikalar**  
Hizmet tarafından kullanılan sertifikalar yapılandırır. Önceki kod örneğinde RemoteAccess modülü için sertifikayı tanımlayan gösterilmektedir. Değeri *parmak izi* özniteliği kullanmak için sertifika parmak izi için ayarlanmalıdır.

<p/>

> [!NOTE]
> Sertifika parmak izini bir metin düzenleyicisi kullanarak yapılandırma dosyasına eklenebilir. Veya, değer üzerinde eklenebilir **sertifikaları** sekmesinde **özellikleri** Visual Studio'da rolünün sayfası.
> 
> 

## <a name="defining-ports-for-role-instances"></a>Rol örnekleri için bağlantı noktalarını tanımlama
Azure web rolü için yalnızca bir giriş noktası sağlar. Tüm trafiği aracılığıyla bir IP adresi oluştuğunu anlamına gelir. İstek doğru konuma yönlendirmek için ana bilgisayar üstbilgisi yapılandırarak bir bağlantı noktasını paylaşmak için Web sitelerinizi yapılandırabilirsiniz. IP adresi iyi bilinen bağlantı noktalarını dinlemek için uygulamalarınızı da yapılandırabilirsiniz.

Aşağıdaki örnek bir Web sitesi ve web uygulaması içeren bir web rolü için yapılandırmayı gösterir. Web sitesi bağlantı noktası 80 üzerinde varsayılan giriş konumu olarak yapılandırılmış ve web uygulamaları "mail.mysite.cloudapp.net" adlı bir alternatif ana bilgisayar üstbilgisi istekleri almak için yapılandırılır.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Bir rolü yapılandırmasını değiştirme
Azure üzerinde hizmet çevrimdışı bırakmadan çalışırken, bulut hizmetinin yapılandırmasını güncelleştirebilirsiniz. Yapılandırma bilgilerini değiştirmek için yeni bir yapılandırma dosyası karşıya yükleme veya yerinde yapılandırma dosyasını düzenlemenize ve çalışan hizmetinize uygulayın. Bir hizmet yapılandırması için aşağıdaki değişiklikler yapılabilir:

* **Yapılandırma ayarlarının değerleri değiştirme**  
  Ne zaman bir yapılandırma değişiklikleri ayarı değişikliği örneği çevrimiçi veya çevrimdışı olduğunda örneğinin düzgün bir şekilde geri dönüşüm ve örnek değişikliği uygulamak için geçerliyken, bir rol örneği seçebilirsiniz.
* **Rol örneklerinin Hizmeti topolojisi değiştirme**  
  Topolojisi değişiklikleri bir örneği burada kaldırılmakta dışında çalışan örneklerini etkilemez. Tüm kalan örnekleri genellikle geri dönüştürülmesi gerekmez; Ancak, bir topoloji değişikliği yanıta rol örneklerinin geri dönüşüm seçebilirsiniz.
* **Sertifika parmak izini değiştirme**  
  Rol örneği çevrimdışı olduğunda, yalnızca bir sertifika güncelleştirebilirsiniz. Bir sertifika eklenmiş, silinmiş veya rol örneği çevrimiçi durumdayken değişti, Azure düzgün biçimde örneği çevrimdışı bir sertifikayı güncelleştirmek ve değişiklik tamamlandıktan sonra yeniden çevrimiçi duruma getirmek için alır.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Yapılandırma değişiklikleriyle hizmeti çalışma zamanı olayları işleme
[Azure çalışma zamanı kitaplığı](https://msdn.microsoft.com/library/azure/mt419365.aspx) içeren [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) ad alanı bir rolden Azure ortamı ile etkileşim kurmaya yönelik sınıflar sağlar. [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) sınıfı önce ve sonra bir yapılandırma değişikliği yükseltilmiş aşağıdaki olaylar tanımlar:

* **[Değiştirme](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) olayı**  
  Yapılandırma değişikliği bir iletmenize gerekirse rol örnekleri almak için bir rolün belirtilen bir örneğini uygulanmadan önce gerçekleşir.
* **[Değiştirilen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) olayı**  
  Belirtilen bir rol örneği için yapılandırma değişikliği uygulandıktan sonra oluşur.

> [!NOTE]
> Sertifika değişiklikleri her zaman bir rolün örnekleri çevrimdışına çünkü RoleEnvironment.Changing veya RoleEnvironment.Changed olayları yükseltmeyin.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Bir uygulamayı Azure bulut hizmeti olarak dağıtmak için önce uygun biçimdeki uygulama paketi gerekir. Kullanabileceğiniz **CSPack** komut satırı aracı (yüklenmiş [Azure SDK'sı](https://azure.microsoft.com/downloads/)) Visual Studio alternatif olarak paket dosyasını oluşturmak için.

**CSPack** Hizmet tanım dosyası ve hizmet yapılandırma dosyasının içeriğini paket içeriğini tanımlamak için kullanır. **CSPack** kullanarak Azure'a yükleyebileceğiniz bir uygulama paketi dosyası (.cspkg) oluşturur [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Varsayılan olarak, adlı paket `[ServiceDefinitionFileName].cspkg`, ancak kullanarak farklı bir ad belirtebilirsiniz `/out` seçeneği **CSPack**.

**CSPack** bulunur  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> CSPack.exe (windows üzerinde) kullanılabilir çalıştırarak **Microsoft Azure komut istemi** SDK ile birlikte yüklenen kısayol.  
> 
> Tüm olası anahtarları ve komutları ile ilgili belgeler görmek için kendi başına CSPack.exe programını çalıştırın.
> 
> 

<p />

> [!TIP]
> Bulut hizmetinizi yerel olarak çalıştırmak **Microsoft Azure işlem öykünücüsü**, kullanın **/copyonly** seçeneği. Bu seçenek, uygulama, bunlar işlem öykünücüsü çalıştırılabilir bir dizin düzeni için ikili dosyaları kopyalar.
> 
> 

### <a name="example-command-to-package-a-cloud-service"></a>Bir bulut hizmeti paketlemek için örnek komut
Aşağıdaki örnek, bir web rolü için bilgileri içeren bir uygulama paketi oluşturur. Komut, ikili dosyaları bulunabileceği, dizini kullanmak için hizmet tanımı dosyası ve paket dosyası adını belirtir.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Uygulama bir web rolü ve çalışan rolü içeriyorsa, aşağıdaki komutu kullanılır:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Burada değişkenleri şunlardır:

| Değişken | Değer |
| --- | --- |
| \[DirectoryName\] |Azure projesi .csdef dosyası içeren kök proje dizini altında alt dizin. |
| \[ServiceDefinition\] |Hizmet tanımı dosyası adı. Varsayılan olarak, bu dosyayı ServiceDefinition.csdef olarak adlandırılır. |
| \[OutputFileName\] |Oluşturulan paket dosyasının adı. Genellikle, bu uygulama adına ayarlanır. Hiçbir dosya adı belirtilirse, uygulama paketi olarak oluşturuldu \[ApplicationName\].cspkg. |
| \[RoleName\] |Hizmet tanımı dosyasında tanımlanan rolün adı. |
| \[RoleBinariesDirectory] |Rolü için ikili dosyalarının konumu. |
| \[VirtualPath\] |Hizmet tanımı siteler bölümünde tanımlanan her sanal yol için fiziksel dizinler. |
| \[PhysicalPath\] |Hizmet tanımı site düğümde tanımlanan her sanal yol için içerik fiziksel dizinleri. |
| \[RoleAssemblyName\] |Rolü için ikili dosya adı. |

## <a name="next-steps"></a>Sonraki adımlar
Bulut hizmeti paket oluşturma ve istiyorum...

* [Bir bulut hizmet örneği için Kurulum Uzak Masaüstü][remotedesktop]
* [Bir bulut hizmeti projesini dağıtma][deploy]

Visual Studio kullanarak ve istiyorum...

* [Yeni bir bulut hizmeti oluştur][vs_create]
* [Var olan bir bulut hizmetini yeniden yapılandırın][vs_reconfigure]
* [Bir bulut hizmeti projesini dağıtma][vs_deploy]
* [Bir bulut hizmet örneği için Kurulum Uzak Masaüstü][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop-new-portal.md
[vs_remote]: cloud-services-role-enable-remote-desktop-visual-studio.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
