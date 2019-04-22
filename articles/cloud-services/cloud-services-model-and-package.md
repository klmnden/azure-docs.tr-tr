---
title: Bir bulut hizmeti modeli ve paketi nedir | Microsoft Docs
description: Bulut hizmeti modeli (.csdef, .cscfg) ve paket (.cspkg) azure'daki açıklar
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: 9c9f7dfd9ecbf085da19fc010e497caef8c18629
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58917320"
---
# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Bulut hizmeti modeli ve nasıl paketi nedir?
Bir bulut hizmeti, üç bileşenlerini hizmet tanımı oluşturulur *(.csdef)*, hizmet yapılandırma *(.cscfg)* ve bir hizmet paketi *(.cspkg)*. Her iki **ServiceDefinition.csdef** ve **ServiceConfig.cscfg** dosyaları XML tabanlı ve topluca model adlı bulut hizmeti ve nasıl yapılandırıldığını; yapısını açıklar. **ServicePackage.cspkg** oluşturulduğu bir zip dosyası **ServiceDefinition.csdef** ve diğerlerinin yanı sıra ikili tabanlı tüm gerekli bağımlılıkları içerir. Azure hem de bulut hizmeti oluşturur **ServicePackage.cspkg** ve **ServiceConfig.cscfg**.

Azure'da bulut hizmeti çalışır duruma geçtikten sonra üzerinden yeniden yapılandırabilirsiniz **ServiceConfig.cscfg** dosyası, ancak tanım alter olamaz.

## <a name="what-would-you-like-to-know-more-about"></a>Ne hakkında daha fazla bilgi edinmek istiyorsunuz?
* Daha fazla bilgi edinmek istiyorum [ServiceDefinition.csdef](#csdef) ve [ServiceConfig.cscfg](#cscfg) dosyaları.
* Zaten bu konuda biliyorum ver [bazı örnekler](#next-steps) ne yapılandırabilirim üzerinde.
* Oluşturmak istediğiniz [ServicePackage.cspkg](#cspkg).
* Visual Studio kullanarak ve istiyorum...
  * [Bulut hizmeti oluşturma][vs_create]
  * [Mevcut bir bulut hizmetinin yeniden yapılandırın][vs_reconfigure]
  * [Bir bulut hizmeti projesini dağıtma][vs_deploy]
  * [Uzak Masaüstü Bağlantısı bir bulut hizmeti örneği][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
**ServiceDefinition.csdef** dosyasını bir bulut hizmeti yapılandırmak için Azure tarafından kullanılan ayarları belirtir. [Azure Hizmet tanım düzenini (.csdef dosyası)](/previous-versions/azure/reference/ee758711(v=azure.100)) Hizmet tanım dosyası için izin verilen biçimini sağlar. Aşağıdaki örnek Web ve çalışan rolleri için tanımlanan ayarları gösterilir:

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

Başvurabilirsiniz [Hizmet tanım düzenini](/previous-versions/azure/reference/ee758711(v=azure.100)) bir daha iyi burada kullanılan XML Şeması anlamak için ancak burada, bazı öğeleri hızlı bir açıklaması:

**Siteleri**  
IIS7'de barındırılan Web siteleri veya web uygulamaları için tanımları içerir.

**InputEndpoints**  
Bulut hizmetiyle bağlantı kurmak için kullanılan uç noktaları için tanımları içerir.

**InternalEndpoints**  
Rol örnekleri tarafından birbirleri ile iletişim kurmak için kullanılan uç noktaları için tanımları içerir.

**ConfigurationSettings**  
Belirli bir rolü özelliklerinin ayarı tanımlarını içerir.

**Sertifikalar**  
Bir rol için gerekli sertifikaları için tanımları içerir. Önceki kod örneği, Azure Connect yapılandırma için kullanılan bir sertifika gösterir.

**LocalResources**  
Yerel depolama kaynakları için tanımları içerir. Yerel depolama kaynağı, dosya sistemindeki bir rol örneği çalıştığı sanal makinenin ayrılmış bir dizindir.

**İçeri aktarmalar**  
İçeri aktarılan modüller için tanımları içerir. Önceki kod örneğinde, Uzak Masaüstü Bağlantısı ve Azure Connect modülleri gösterir.

**Başlangıç**  
Rol başlatıldığında çalıştırılan görevleri içerir. Görevler, .cmd veya yürütülebilir dosya içinde tanımlanır.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Bulut hizmetiniz için ayarları yapılandırma değerleri belirlenir **ServiceConfiguration.cscfg** dosya. Bu dosya her rol için dağıtmak istediğiniz örnek sayısını belirtin. Hizmet tanımı dosyasında tanımlanan yapılandırma ayarları için değerleri hizmet yapılandırma dosyasına eklenir. Bulut hizmeti ile ilişkili olan herhangi bir yönetim sertifika parmak izleri de dosyasına eklenir. [Azure hizmet yapılandırma şemasına (.cscfg dosyası)](/previous-versions/azure/reference/ee758710(v=azure.100)) hizmet yapılandırma dosyası için izin verilen biçimini sağlar.

Hizmet yapılandırma dosyası uygulama ile birlikte paketlenmiştir değil, ancak ayrı bir dosya olarak azure'a yüklenir ve bulut hizmeti yapılandırmak için kullanılır. Bulut hizmetinizi yeniden dağıtmaya gerek kalmadan yeni hizmet yapılandırma dosyasını karşıya yükleyebilirsiniz. Bulut hizmet çalışırken, bulut hizmeti için yapılandırma değerlerini değiştirilebilir. Aşağıdaki örnek, Web ve çalışan rolleri için tanımlanan yapılandırma ayarlarını gösterir:

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

Başvurabilirsiniz [hizmet yapılandırma şeması](/previous-versions/azure/reference/ee758710(v=azure.100)) burada kullanılan XML şemasını anlama daha iyi, ancak öğeleri hızlı bir açıklaması aşağıda verilmiştir:

**Örnekler**  
Çalışan rolü örneklerinin sayısını yapılandırır. Bulut hizmetinizin potansiyel olarak yükseltmeler sırasında kullanılamaz hale gelmesini önlemek için web'e yönelik rollerinizin birden fazla örneğini dağıtmanız önerilir. Birden fazla örneğine dağıtarak, yönergeleri için karşıladığınızdan [Azure işlem hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/), Internet'e yönelik rolleri, iki için % 99,95 harici bağlantı garanti eder veya daha fazla rol örnekleri bir hizmet için dağıtıldı.

**ConfigurationSettings**  
Bir rolün çalışan örneklerini ayarlarını yapılandırır. Adını `<Setting>` öğeleri Hizmet tanım dosyası ayarı tanımlarının eşleşmesi gerekir.

**Sertifikalar**  
Hizmet tarafından kullanılan sertifikaları yapılandırır. Önceki kod örneğinde, RemoteAccess modülü için sertifika tanımlamak gösterilmektedir. Değerini *parmak izi* özniteliği kullanmak için sertifika parmak izi için ayarlanmalıdır.

<p/>

> [!NOTE]
> Sertifikanın parmak izi, bir metin düzenleyicisi kullanarak yapılandırma dosyasına eklenebilir. Veya, değer eklenebilir **sertifikaları** sekmesinde **özellikleri** Visual Studio'da rolünün sayfası.
> 
> 

## <a name="defining-ports-for-role-instances"></a>Rol örnekleri için bağlantı noktalarını tanımlama
Azure web rolü için yalnızca bir giriş noktası sağlar. Tüm trafiği aracılığıyla bir IP adresi oluştuğunu anlamına gelir. İstek doğru konuma yönlendirmek için ana bilgisayar üstbilgisi yapılandırarak bir bağlantı noktasını paylaşmak için Web siteleri yapılandırabilirsiniz. Ayrıca uygulamalarınızın IP adresi bilinen bağlantı noktalarını dinlemek için yapılandırabilirsiniz.

Aşağıdaki örnek, bir Web sitesi ve web uygulaması ile bir web rolü için yapılandırmayı gösterir. Web sitesi bağlantı noktası 80 üzerinde varsayılan giriş konumu olarak yapılandırılmış ve web uygulamaları, "mail.mysite.cloudapp.net" adlı bir alternatif ana bilgisayar üst bilgisinden isteklerini alacak şekilde yapılandırılır.

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


## <a name="changing-the-configuration-of-a-role"></a>Bir rol yapılandırmasını değiştirme
Hizmet çevrimdışı duruma getirmeden Azure üzerinde çalışırken, bulut hizmetinin yapılandırmasını güncelleştirebilirsiniz. Yapılandırma bilgilerini değiştirmek için yeni bir yapılandırma dosyasını karşıya yükleyin veya yerinde yapılandırma dosyasını düzenleyin ve çalışan hizmetiniz için geçerlidir. Aşağıdaki değişiklikler bir hizmetin yapılandırmaya hale getirilebilir:

* **Yapılandırma ayarlarının değerleri değiştirme**  
  Ne zaman yapılandırma değişiklikleri ayarı, bir rol örneği örneği çevrimiçi veya çevrimdışı olduğunda örneğini düzgün biçimde geri ve örneği değişikliği uygulamak için sırada değişikliği uygulamak seçebilirsiniz.
* **Rol örneği service topolojisi değiştiriliyor**  
  Topoloji değişiklikler örneği burada kaldırılıyor dışında çalışan örneklerini etkilemez. Kalan tüm örnekleri genellikle geri dönüştürülmesi gerekmez; Ancak, bir topoloji değişikliği yanıt rol örneğini geri dönüştürmek seçebilirsiniz.
* **Sertifika parmak izini değiştirme**  
  Bir rol örneği çevrimdışı olduğunda, yalnızca bir sertifika güncelleştirebilirsiniz. Bir sertifika eklenir, silinmiş veya rol örneği çevrimiçi durumdayken değiştirildi, Azure düzgün bir şekilde örneği çevrimdışı bir sertifikayı güncelleştirmek ve değişiklik tamamlandıktan sonra yeniden çevrimiçi duruma getirmek için alır.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Yapılandırma değişiklikleri ile hizmet çalışma zamanı olaylarını işleme
[Azure çalışma zamanı kitaplığı](/previous-versions/azure/reference/mt419365(v=azure.100)) içerir [Microsoft.WindowsAzure.ServiceRuntime](/previous-versions/azure/reference/ee741722(v=azure.100)) ad alanı bir rolden Azure ortamı ile etkileşim kurmaya yönelik sınıflar sağlar. [RoleEnvironment](/previous-versions/azure/reference/ee773173(v=azure.100)) sınıfı önce ve sonra bir yapılandırma değişikliği başlatan aşağıdaki olaylar tanımlar:

* **[Değiştirme](/previous-versions/azure/reference/ee758134(v=azure.100)) olay**  
  Bu yapılandırma değişikliğini bir rol rol örnekleri gerekirse almak için bir fırsat vermek belirtilen bir örneğini uygulanmadan önce oluşur.
* **[Değiştirilen](/previous-versions/azure/reference/ee758129(v=azure.100)) olay**  
  Belirtilen bir rol örneği için yapılandırma değişiklik uygulandıktan sonra gerçekleşir.

> [!NOTE]
> Sertifika değişiklikleri her zaman bir rolün örnekleri çevrimdışı olduğundan RoleEnvironment.Changing veya RoleEnvironment.Changed olayları geçirmeyin.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Azure'daki bir bulut hizmeti olarak bir uygulamayı dağıtmak için uygun biçimde uygulama paketi gerekir. Kullanabileceğiniz **CSPack** komut satırı aracı (ile yüklenen [Azure SDK'sı](https://azure.microsoft.com/downloads/)) alternatif olarak, Visual Studio Paket dosyası oluşturmak için.

**CSPack** Hizmet tanım dosyası ve hizmet yapılandırma dosyasının içeriği paket içeriğini tanımlamak için kullanır. **CSPack** kullanarak Azure'a yükleyebilirsiniz. bir uygulama paketi dosyası (.cspkg) oluşturur [Azure portalında](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Varsayılan olarak, adlı paket `[ServiceDefinitionFileName].cspkg`, ancak farklı bir ad kullanarak belirtebilirsiniz `/out` seçeneği **CSPack**.

**CSPack** bulunur  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> (Windows'ta) CSPack.exe kullanılabilir çalıştırarak **Microsoft Azure komut istemi** SDK ile yüklenen bir kısayol.  
> 
> Tüm olası anahtarları ve komutlar hakkında belgeleri görmek için tek başına CSPack.exe programı çalıştırın.
> 
> 

<p />

> [!TIP]
> Bulut hizmetinizi yerel olarak çalıştırmanızı **Microsoft Azure işlem öykünücüsü**, kullanın **/copyonly** seçeneği. Bu seçenek, uygulama bir dizin düzenini içinden işlem öykünücüsünde çalıştırılabilmesi için ikili dosyaları kopyalar.
> 
> 

### <a name="example-command-to-package-a-cloud-service"></a>Örnek komut, bir bulut hizmeti paketlemek için
Aşağıdaki örnek, bir web rolü bilgilerini içeren bir uygulama paketi oluşturur. Komut, ikili dosyaları burada bulunabilir, dizin kullanmak için hizmet tanımı dosyası ve paket dosyasının adını belirtir.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Uygulama hem bir web rolü ve çalışan rolü içeriyorsa, aşağıdaki komutu kullanılır:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Burada değişkenleri şu şekilde tanımlanır:

| Değişken | Değer |
| --- | --- |
| \[DizinAdı\] |Azure projesi .csdef dosyası içeren kök proje dizini altında alt dizini. |
| \[ServiceDefinition\] |Hizmet tanım dosyası adı. Varsayılan olarak, bu dosya, ServiceDefinition.csdef adlandırılır. |
| \[OutputFileName\] |Oluşturulan paket dosyasının adı. Genellikle, bu uygulamanın adına ayarlanır. Dosya adı belirtilirse, uygulama paketi olarak oluşturulur \[ApplicationName\].cspkg. |
| \[Rol adı\] |Hizmet tanımı dosyasında tanımlanan bir rolün adı. |
| \[RoleBinariesDirectory] |Rolü için ikili dosyalarının konumu. |
| \[VirtualPath\] |Fiziksel dizinlerle hizmet tanımı siteler bölümünde tanımlanan her bir sanal yol. |
| \[PhysicalPath\] |Hizmet tanımının site düğümde tanımlanan her bir sanal yol içeriğinin fiziksel dizinler. |
| \[RoleAssemblyName\] |Rolü için ikili dosya adı. |

## <a name="next-steps"></a>Sonraki adımlar
Bir bulut hizmeti paketi oluşturma ve istiyorum...

* [Bir bulut hizmeti örneği için Kurulum Uzak Masaüstü][remotedesktop]
* [Bir bulut hizmeti projesini dağıtma][deploy]

Visual Studio kullanarak ve istiyorum...

* [Yeni bir bulut hizmeti oluşturma][vs_create]
* [Mevcut bir bulut hizmetinin yeniden yapılandırın][vs_reconfigure]
* [Bir bulut hizmeti projesini dağıtma][vs_deploy]
* [Bir bulut hizmeti örneği için Kurulum Uzak Masaüstü][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop-new-portal.md
[vs_remote]: cloud-services-role-enable-remote-desktop-visual-studio.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
