---
title: Azure bulut Hizmetleri def WebRole şema | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 85368e4e-a0db-4c02-8dbc-8e2928fa6091
caps.latest.revision: 60
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: 6db3edef937dc0b5cdd805d0045897e02b985e7b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34360351"
---
# <a name="azure-cloud-services-definition-webrole-schema"></a>Tanım WebRole şeması Azure bulut Hizmetleri
Azure web rolü, web uygulaması programlama için IIS 7, ASP.NET, PHP, Windows Communication Foundation ve Fastcgı gibi tarafından desteklenen gibi özelleştirilmiş bir rolüdür.

.Csdef hizmet tanımı dosyası için varsayılan uzantısıdır.

## <a name="basic-service-definition-schema-for-a-web-role"></a>Web rolü için temel hizmeti Tanım Şeması  
Web rolü içeren bir hizmet tanımı dosyası temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>  
  <WebRole name="<web-role-name>" vmsize="<web-role-size>" enableNativeCodeExecution="[true|false]">  
    <Certificates>  
      <Certificate name="<certificate-name>" storeLocation="<certificate-store>" storeName="<store-name>" />  
    </Certificates>      
    <ConfigurationSettings>  
      <Setting name="<setting-name>" />  
    </ConfigurationSettings>  
    <Imports>  
      <Import moduleName="<import-module>"/>  
    </Imports>  
    <Endpoints>  
      <InputEndpoint certificate="<certificate-name>" ignoreRoleInstanceStatus="[true|false]" name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<port-number>" port="<port-number>" loadBalancerProbe="<load-balancer-probe-name>" />  
      <InternalEndpoint name="<internal-endpoint-name>" protocol="[http|tcp|udp|any]" port="<port-number>">  
         <FixedPort port="<port-number>"/>  
         <FixedPortRange min="<minium-port-number>" max="<maximum-port-number>"/>  
      </InternalEndpoint>  
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">  
         <AllocatePublicPortFrom>  
            <FixedPortRange min="<minium-port-number>" max="<maximum-port-number>"/>  
         </AllocatePublicPortFrom>  
      </InstanceInputEndpoint>  
    </Endpoints>  
    <LocalResources>  
      <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />  
    </LocalResources>  
    <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />  
    <Runtime executionContext="[limited|elevated]">  
      <Environment>  
         <Variable name="<variable-name>" value="<variable-value>">  
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>  
          </Variable>            
      </Environment>  
      <EntryPoint>  
         <NetFxEntryPoint assemblyName="<name-of-assembly-containing-entrypoint>" targetFrameworkVersion="<.net-framework-version>"/>  
      </EntryPoint>  
    </Runtime>  
    <Sites>  
      <Site name="<web-site-name>">  
        <VirtualApplication name="<application-name>" physicalDirectory="<directory-path>"/>  
        <VirtualDirectory name="<directory-path>" physicalDirectory="<directory-path>"/>  
        <Bindings>  
          <Binding name="<binding-name>" endpointName="<endpoint-name-bound-to>" hostHeader="<url-of-the-site>"/>  
        </Bindings>  
      </Site>  
    </Sites>  
    <Startup priority="<for-internal-use-only>">  
      <Task commandLine="<command-to=execute>" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">  
        <Environment>  
         <Variable name="<variable-name>" value="<variable-value>">  
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>  
          </Variable>            
        </Environment>  
      </Task>  
    </Startup>  
    <Contents>  
      <Content destination="<destination-folder-name>" >  
        <SourceDirectory path="<local-source-directory>" />  
      </Content>  
    </Contents>  
  </WebRole>  
</ServiceDefinition>  
```  

## <a name="schema-elements"></a>Şema öğeleri  
Hizmet tanımı dosyası, bu konunun sonraki bölümlerinde ayrıntılı olarak açıklanan bu öğeleri içerir:  

[WebRole](#WebRole)

[ConfigurationSettings](#ConfigurationSettings)

[Ayar](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Uç noktaları](#Endpoints)

[InternalEndpoint](#InternalEndpoint)

[InstanceInputEndpoint](#InstanceInputEndpoint)

[AllocatePublicPortFrom](#AllocatePublicPortFrom)

[FixedPort](#FixedPort)

[FixedPortRange](#FixedPortRange)

[Sertifikalar](#Certificates)

[Sertifika](#Certificate)

[İçeri aktarmalar](#Imports)

[İçeri Aktarma](#Import)

[Çalışma zamanı](#Runtime)

[Ortam](#Environment)

[değişken](#Variable)

[RoleInstanceValue](#RoleInstanceValue)

[NetFxEntryPoint](#NetFxEntryPoint)

[siteleri](#Sites)

[Site](#Site)

[VirtualApplication](#VirtualApplication)

[VirtualApplication](#VirtualApplication)

[Bağlamaları](#Bindings)

[Bağlama](#Binding)

[Başlangıç](#Startup)

[Görev](#Task)

[içeriği](#Contents)

[İçerik](#Content)

[SourceDirectory](#SourceDirectory)

##  <a name="WebRole"></a> WebRole  
`WebRole` Öğesi, IIS 7 ve ASP.NET tarafından desteklenen gibi web uygulama programlama için özelleştirilmiş bir rolü açıklar. Bir hizmet sıfır veya daha fazla web rolleri içerebilir.

Aşağıdaki tabloda özniteliklerini açıklayan `WebRole` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Web rolü adı. Rolün adı benzersiz olmalıdır.|  
|enableNativeCodeExecution|boole|İsteğe bağlı. Varsayılan değer `true`; yerel kod yürütme ve tam güven varsayılan olarak etkinleştirilir. Bu öznitelik ayarlanırsa `false` web rolü için yerel kodu yürütme devre dışı bırakabilir ve Azure kısmi güven kullanın.|  
|vmsize|dize|İsteğe bağlı. Role ayrıldığını sanal makine boyutunu değiştirmek için bu değeri ayarlayın. Varsayılan değer `Small`. Daha fazla bilgi için bkz: [bulut Hizmetleri için sanal makine boyutlarını](cloud-services-sizes-specs.md).|  

##  <a name="ConfigurationSettings"></a> ConfigurationSettings  
`ConfigurationSettings` Öğesi web rolü için yapılandırma ayarlarını koleksiyonunu açıklar. Bu öğe üst öğesi olan `Setting` öğesi.

##  <a name="Setting"></a> Ayarı  
`Setting` Öğesi bir rol örneği için bir yapılandırma ayarı belirten bir ad ve değer çifti açıklar.

Aşağıdaki tabloda özniteliklerini açıklayan `Setting` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Yapılandırma ayarı için benzersiz bir ad.|  

Bir rol için yapılandırma ayarlarını hizmet tanımı dosyasında bildirilen ve hizmet yapılandırma dosyasında ayarlanan ad ve değer çiftleridir.

##  <a name="LocalResources"></a> LocalResources  
`LocalResources` Öğesi web rolü için yerel depolama kaynaklarını koleksiyonunu açıklar. Bu öğe üst öğesi olan `LocalStorage` öğesi.

##  <a name="LocalStorage"></a> LocalStorage  
`LocalStorage` Öğesi zamanında hizmet dosya sistemi alanı sağlayan yerel depolama kaynağı tanımlar. Bir rol, sıfır veya daha fazla yerel depolama kaynaklarını tanımlayabilir.

> [!NOTE]
>  `LocalStorage` Öğesi bir alt öğesi olarak görünebilir `WebRole` öğesinde Azure SDK'sının önceki sürümleriyle uyumluluk desteklemek için.

Aşağıdaki tabloda özniteliklerini açıklayan `LocalStorage` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Yerel depolama alanı için benzersiz bir ad.|  
|cleanOnRoleRecycle|boole|İsteğe bağlı. Rolü yeniden başlatıldığında, Yerel Depodaki temizlendi olup olmadığını gösterir. Varsayılan değer `true`.|  
|sizeınmb parametresinin|Int|İsteğe bağlı. MB cinsinden yerel depolama alanı için ayırmak için depolama alanı istenen miktarı. Belirtilmezse, ayrılmış varsayılan depolama alanını 100 MB'tır. Minimum ayrılabilir depolama alanı miktarı 1 MB'tır.<br /><br /> Yerel kaynaklar en büyük boyutunu sanal makine boyutuna bağlıdır. Daha fazla bilgi için bkz: [bulut Hizmetleri için sanal makine boyutlarını](cloud-services-sizes-specs.md).|  
  
Yerel depolama kaynağı için ayrılan dizinin adını adı özniteliği için sağlanan değer karşılık gelir.

##  <a name="Endpoints"></a> Uç noktaları  
`Endpoints` Öğesi giriş (harici) iç koleksiyonunu açıklar ve örnek giriş uç noktaları bir rol için. Bu öğe üst öğesi olan `InputEndpoint`, `InternalEndpoint`, ve `InstanceInputEndpoint` öğeleri.

Giriş ve iç uç noktalar için ayrı olarak ayrılır. Bir hizmet 25 giriş, iç, toplam sahip olabilir ve bir hizmet olarak izin 25 rollerde tahsis uç noktalar örneği giriş. Örneğin, 5 rolleri varsa 5 giriş uç noktaları rol başına tahsis edebilirsiniz veya tek bir rol için 25 giriş uç noktaları tahsis edebilirsiniz ya da 1 giriş uç noktası her 25 rollere ayırabilirsiniz.

> [!NOTE]
>  Dağıtılan her bir rol, rol başına bir örneğini gerektirir. Bir abonelik için sağlama varsayılan 20 çekirdeğe sınırlıdır ve bu nedenle 20 bir rolün örnekleri için sınırlı olur. Uygulamanızı bakın sağlama varsayılan olarak sağlanan daha fazla örneğe gerektirip gerektirmediğini [faturalama ve abonelik yönetimi kota Destek](https://azure.microsoft.com/support/options/) kota artırma hakkında daha fazla bilgi.

##  <a name="InputEndpoint"></a> Inputendpoint  
`InputEndpoint` Öğesi web rolü dış uç noktası açıklar.

Birden çok HTTP, HTTPS, UDP birleşimi uç noktaları ve TCP uç noktaları tanımlayabilirsiniz. Bir giriş uç noktası için seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak her bir rol hizmeti için belirtilen bağlantı noktası numaralarını benzersiz olması gerekir. Web rolü 80 numaralı bağlantı noktası HTTP ve 443 numaralı bağlantı noktası için HTTPS için kullandığını belirtirseniz, örneğin, ardından ikinci bir web rolü 8080 bağlantı noktası HTTP ve bağlantı noktası 8043 için HTTPS için kullandığını belirtebilirsiniz.

Aşağıdaki tabloda özniteliklerini açıklayan `InputEndpoint` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Dış uç noktası için benzersiz bir ad.|  
|protokol|dize|Gereklidir. Dış uç noktası için Aktarım Protokolü. Web rolü için olası değerler şunlardır: `HTTP`, `HTTPS`, `UDP`, veya `TCP`.|  
|port|Int|Gereklidir. Dış uç noktası için bağlantı noktası. Seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak her bir rol hizmeti için belirtilen bağlantı noktası numaralarını benzersiz olması gerekir.<br /><br /> Olası değerler aralığı 1 ile 65535 (dahil) (Azure SDK sürüm 1,7 veya üstü) arasında.|  
|sertifika|dize|Bir HTTPS uç noktası için gereklidir. Tarafından tanımlanan bir sertifika adını bir `Certificate` öğesi.|  
|yerel bağlantı noktası|Int|İsteğe bağlı. Uç noktası üzerindeki iç bağlantıları için kullanılan bağlantı noktasını belirtir. `localPort` Özniteliği bir rol üzerinde bir iç bağlantı noktasına uç dış bağlantı noktasını eşler. Bu, burada bir rol bir iç bileşenine bağlantı noktası olandan farklı, dışarıdan #include iletişim kurması gereken senaryolarda kullanışlıdır.<br /><br /> Belirtilmezse, değeri `localPort` aynı `port` özniteliği. Değerini `localPort` için "*" çalışma zamanı API kullanarak bulunabilir olduğundan ayrılmamış bir bağlantı noktası otomatik olarak atanacak.<br /><br /> Olası değerler aralığı 1 ile 65535 (dahil) (Azure SDK sürüm 1,7 veya üstü) arasında.<br /><br /> `localPort` Özniteliği yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha yüksek.|  
|ignoreRoleInstanceStatus|boole|İsteğe bağlı. Bu özniteliğin değeri ayarlandığında `true`, bir hizmetin durumunu göz ardı edilir ve uç nokta yük dengeleyici tarafından kaldırılmaz. Bu değeri ayarlamak `true` Hizmet meşgul örneklerini hata ayıklama için kullanışlıdır. Varsayılan değer `false`. **Not:** bile rol hazır durumda olmadığında bir uç nokta hala trafik alabilir.|  
|loadBalancerProbe|dize|İsteğe bağlı. Giriş uç noktasıyla ilişkili yük dengeleyici araştırmasını adı. Daha fazla bilgi için bkz: [LoadBalancerProbe şema](schema-csdef-loadbalancerprobe.md).|  

##  <a name="InternalEndpoint"></a> InternalEndpoint  
`InternalEndpoint` Öğesi web rolü için iç uç nokta açıklar. Dahili uç noktayı yalnızca hizmet içinde çalışan diğer rol örnekleri kullanılabilir; Hizmet dışındaki istemciler tarafından kullanılabilir değil. Web içermeyen rolleri `Sites` öğesi yalnızca bir tek HTTP, UDP veya TCP iç uç noktası olabilir.

Aşağıdaki tabloda özniteliklerini açıklayan `InternalEndpoint` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Dahili uç noktayı için benzersiz bir ad.|  
|protokol|dize|Gereklidir. Dahili uç noktayı Aktarım Protokolü. Olası değerler şunlardır: `HTTP`, `TCP`, `UDP`, veya `ANY`.<br /><br /> Değerini `ANY` herhangi bir protokolünü herhangi bir bağlantı noktası izin verildiğini belirtir.|  
|port|Int|İsteğe bağlı. İç yük dengeli uç nokta bağlantılarında için kullanılan bağlantı noktası. Uç noktası kullanan iki bağlantı noktası bir yük dengeli. Genel IP adresi için kullanılan bağlantı noktasını ve özel IP adresi üzerinde kullanılan bağlantı noktası. Bunlar genellikle bunlar aynı ayarlanır, ancak farklı bağlantı noktalarını kullanmayı seçebilirsiniz.<br /><br /> Olası değerler aralığı 1 ile 65535 (dahil) (Azure SDK sürüm 1,7 veya üstü) arasında.<br /><br /> `Port` Özniteliği yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha yüksek.|  

##  <a name="InstanceInputEndpoint"></a> InstanceInputEndpoint  
`InstanceInputEndpoint` Öğesi web rolü için bir örnek giriş uç noktası açıklar. Bir örnek giriş uç noktası yük dengeleyicide bağlantı noktası iletme kullanarak belirli bir rol örneği ile ilişkilidir. Her örnek giriş uç noktası belirli bir bağlantı noktasına olası bağlantı noktası aralığından eşlenir. Bu öğe üst öğesi olan `AllocatePublicPortFrom` öğesi.

`InstanceInputEndpoint` Öğesi, yalnızca Azure SDK 1.7 sürümünü kullanarak mevcut veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `InstanceInputEndpoint` öğesi.
  
| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Uç nokta için benzersiz bir ad.|  
|yerel bağlantı noktası|Int|Gereklidir. Tüm rol örneklerini yük dengeleyiciden iletilen gelen trafiği almak için dinleme yapar iç bağlantı noktasını belirtir. Olası değerler aralığı 1 ile 65535 (dahil) arasında.|  
|protokol|dize|Gereklidir. Dahili uç noktayı Aktarım Protokolü. Olası değerler: `udp` veya `tcp`. Kullanım `tcp` http/https trafiğini tabanlı için.|  
  
##  <a name="AllocatePublicPortFrom"></a> AllocatePublicPortFrom  
`AllocatePublicPortFrom` Öğesi her örnek giriş uç noktasına erişmek için dış müşterileri tarafından kullanılan genel bağlantı noktası aralığı açıklar. Genel (VIP) bağlantı noktası numarasını bu aralığında ayrılan ve Kiracı dağıtımı ve güncelleştirme sırasında her tek rol örneğinin uç noktasına atanmış. Bu öğe üst öğesi olan `FixedPortRange` öğesi.

`AllocatePublicPortFrom` Öğesi, yalnızca Azure SDK 1.7 sürümünü kullanarak mevcut veya daha yüksek.

##  <a name="FixedPort"></a> FixedPort  
`FixedPort` Öğesi hangi etkinleştirir yük dengeli uç nokta bağlantılarında iç uç noktası için bağlantı noktasını belirtir.

`FixedPort` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `FixedPort` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|port|Int|Gereklidir. İç bitiş noktası için bağlantı noktası. Bu ayar aynı etkiye sahip `FixedPortRange` min ve max aynı bağlantı noktası.<br /><br /> Olası değerler aralığı 1 ile 65535 (dahil) (Azure SDK sürüm 1,7 veya üstü) arasında.|  

##  <a name="FixedPortRange"></a> FixedPortRange  
`FixedPortRange` Öğesi dahili uç noktayı veya örnek giriş uç noktası için atanan bağlantı noktası aralığını belirtir ve bağlantı noktası kullanılan yük dengelenmiş küme uç bağlantıları.

> [!NOTE]
>  `FixedPortRange` Öğesi içinde bulunduğu öğesi bağlı olarak farklı şekilde çalışır. Zaman `FixedPortRange` öğesidir içinde `InternalEndpoint` öğesi, yük dengeleyicisi tüm sanal makineler rolü çalıştığı min ve max özniteliklerini aralıkta tüm bağlantı noktaları açar. Zaman `FixedPortRange` öğesidir içinde `InstanceInputEndpoint` öğesi, yalnızca bir bağlantı noktası rolünü çalıştıran her bir sanal makine üzerinde min ve max özniteliklerini aralıkta açar.

`FixedPortRange` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `FixedPortRange` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|dk|Int|Gereklidir. Aralık içinde en az bağlantı noktası. Olası değerler aralığı 1 ile 65535 (dahil) (Azure SDK sürüm 1,7 veya üstü) arasında.|  
|en çok|dize|Gereklidir. En fazla bağlantı noktası aralığı içinde. Olası değerler aralığı 1 ile 65535 (dahil) (Azure SDK sürüm 1,7 veya üstü) arasında.|  

##  <a name="Certificates"></a> Sertifikaları  
`Certificates` Öğesi olan bir web rolü için sertifikalar koleksiyonu açıklar. Bu öğe üst öğesi olan `Certificate` öğesi. Bir rolü herhangi bir sayıda ilişkili sertifikaları olabilir. Sertifikaları öğesi kullanma hakkında daha fazla bilgi için bkz: [hizmet tanımı dosyasındaki bir sertifikayla değiştirme](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="Certificate"></a> Sertifika  
`Certificate` Öğesi, bir web rolü ile ilişkili olan bir sertifika açıklar.

Aşağıdaki tabloda özniteliklerini açıklayan `Certificate` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Bir HTTPS ile ilişkili olduğunda başvurmak için kullanılan bu sertifika için bir ad `InputEndpoint` öğesi.|  
|storeLocation|dize|Gereklidir. Bu sertifika yerel makinede burada bulunabilir sertifika deposu konumu. Olası değerler şunlardır: `CurrentUser` ve `LocalMachine`.|  
|storeName|dize|Gereklidir. Bu sertifika yerel makinede bulunduğu sertifika deposu adı. Olası değerler şunlardır yerleşik deposu adları `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`, ya da herhangi bir özel depo adı. Deponun bir özel depolama adı belirtilirse, otomatik olarak oluşturulur.|  
|permissionLevel|dize|İsteğe bağlı. Rol işlemleri verilen erişim izinleri belirtir. Özel anahtara erişim sonra belirtmek için yalnızca yükseltilmiş işlemleri istiyorsanız `elevated` izni. `limitedOrElevated` tüm rol işlemlerin özel anahtara erişim izni verir. Olası değerler: `limitedOrElevated` veya `elevated`. Varsayılan değer `limitedOrElevated`.|  

##  <a name="Imports"></a> İçeri aktarmalar  
`Imports` Öğesi web rolü için konuk işletim sistemi bileşenleri ekleme modülleri Al koleksiyonunu açıklar. Bu öğe üst öğesi olan `Import` öğesi. Bu öğe isteğe bağlıdır ve bir rolü yalnızca bir içeri aktarmalar blok olabilir. 

`Imports` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

##  <a name="Import"></a> İçeri aktarma  
`Import` Öğesi konuk işletim sistemine eklemek üzere modül belirtir.

`Import` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `Import` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|Modül adı|dize|Gereklidir. İçeri aktarmak için modülü adı. Geçerli alma modülleri şunlardır:<br /><br /> -RemoteAccess<br />-RemoteForwarder<br />-Tanılama<br /><br /> RemoteAccess ve RemoteForwarder modüllerini, rol örneği Uzak Masaüstü bağlantıları için yapılandırmanıza olanak tanır. Daha fazla bilgi için bkz: [etkinleştirmek Uzak Masaüstü Bağlantısı](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> Tanılama modülü, bir rol örneği için Tanılama verileri toplayacak şekilde sağlar.|  

##  <a name="Runtime"></a> Çalışma zamanı  
`Runtime` Öğesi, Azure ana işlemin çalışma zamanı ortamı denetlemek için ortam değişkeni ayarlarının web rolü koleksiyonunu açıklar. Bu öğe üst öğesi olan `Environment` öğesi. Bu öğe isteğe bağlıdır ve bir rolü yalnızca bir çalışma zamanı blok olabilir.

`Runtime` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `Runtime` öğe:  

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|executionContext|dize|İsteğe bağlı. Rol işlemi başlatıldığı içeriğini belirtir. Varsayılan bağlam `limited`.<br /><br /> -   `limited` – İşlem yönetici ayrıcalıklarına başlatılır.<br />-   `elevated` – Yönetici ayrıcalıklarıyla işlemi başlatılır.|  

##  <a name="Environment"></a> Ortamı  
`Environment` Öğesi web rolü için ortam değişkeni ayarlarının koleksiyonunu açıklar. Bu öğe üst öğesi olan `Variable` öğesi. Bir rolü ayarlanan ortam değişkenlerine herhangi bir sayıda olabilir.

##  <a name="Variable"></a> değişken  
`Variable` Öğesi konuk işletim ayarlamak için bir ortam değişkeni belirtir.

`Variable` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `Variable` öğe:  

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Ayarlamak için ortam değişkeninin adı.|  
|değer|dize|İsteğe bağlı. Ortam değişkeni için ayarlanacak değer. Bir value özniteliği içermelidir veya `RoleInstanceValue` öğesi.|  

##  <a name="RoleInstanceValue"></a> RoleInstanceValue  
`RoleInstanceValue` Öğesi değişkenin değeri olarak alınacağı xPath belirtir.

Aşağıdaki tabloda özniteliklerini açıklayan `RoleInstanceValue` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|XPath|dize|İsteğe bağlı. Örneği için dağıtım ayarlarını konumu yolu. Daha fazla bilgi için bkz: [XPath yapılandırma değişkenlerle](cloud-services-role-config-xpath.md).<br /><br /> Bir value özniteliği içermelidir veya `RoleInstanceValue` öğesi.|  

##  <a name="EntryPoint"></a> EntryPoint  
`EntryPoint` Öğesi bir rol için giriş noktası belirtir. Bu öğe üst öğesi olan `NetFxEntryPoint` öğeleri. Bu öğeler rol giriş noktası olarak davranacak şekilde WaWorkerHost.exe varsayılan dışındaki başka bir uygulama belirtmenizi sağlar.

`EntryPoint` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.5 kullanarak ya da daha.

##  <a name="NetFxEntryPoint"></a> NetFxEntryPoint  
`NetFxEntryPoint` Öğesi bir rol için çalıştırılacak programı belirtir.

> [!NOTE]
>  `NetFxEntryPoint` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.5 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `NetFxEntryPoint` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|AssemblyName|dize|Gereklidir. Giriş noktası içeren derleme yolu ve dosya adı. Göreli yolu dizinidir  **\\%ROLEROOT%\Approot** (belirtmeyin  **\\%ROLEROOT%\Approot** içinde `commandLine`, kabul edilir). **% ROLEROOT %** bir ortam değişkeni Azure tarafından korunur ve rolünüz için kök klasör konumunu temsil eder. **\\%ROLEROOT%\Approot** klasörü rolünüz için uygulama klasörü temsil eder.<br /><br /> HWC rolleri için her zaman göreli yoludur  **\\%ROLEROOT%\Approot\bin** klasör.<br /><br /> Tam IIS ve IIS Express için web rolleri, derleme göreli bulunamazsa  **\\%ROLEROOT%\Approot** klasörünü  **\\%ROLEROOT%\Approot\bin** aranır.<br /><br /> Bu sıfırlamaya davranışı tam IIS için önerilen en iyi yöntem değildir ve belki de gelecekte sürümleri kaldırıldı.|  
|targetFrameworkVersion|dize|Gereklidir. Derlemeyi .NET framework sürümü. Örneğin, `targetFrameworkVersion="v4.0"`.|  

##  <a name="Sites"></a> siteleri  
`Sites` Öğesi web rolünün içinde barındırılan Web siteleri ve web uygulamalarını koleksiyonunu açıklar. Bu öğe üst öğesi olan `Site` öğesi. Belirtmezseniz, bir `Sites` öğesi, web rolü eski web rolü olarak barındırılır ve yalnızca web rolünüz barındırılan bir Web sitesi olabilir. Bu öğe isteğe bağlıdır ve bir rolü yalnızca bir siteler blok olabilir.

`Sites` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

##  <a name="Site"></a> Site  
`Site` Öğesi web rolünün parçası olan bir Web sitesi veya web uygulaması belirtir.

`Site` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `Site` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Web sitesinin veya uygulamanın adı.|  
|physicalDirectory|dize|Site kökü için içerik dizininin konumu. Konumun .csdef konumun göreli veya mutlak bir yol olarak belirtilebilir.|  

##  <a name="VirtualApplication"></a> VirtualApplication  
`VirtualApplication` Öğesi tanımlayan bir uygulama Internet Information Services (IIS) 7, içerik sunan ve HTTP gibi protokoller üzerinden hizmetler sağlayan bir gruplandırma dosyaların'dir. IIS 7'de bir uygulama oluşturduğunuzda uygulamanın yolu site URL'SİNİN bir parçası olur.

`VirtualApplication` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `VirtualApplication` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Sanal uygulama tanımlamak için bir ad belirtir.|  
|physicalDirectory|dize|Gereklidir. Sanal uygulama içeren geliştirme makinenizde yolunu belirtir. İşlem öykünücüsü, IIS bu konumdan içerik almak için yapılandırılır. Azure'a dağıtırken, fiziksel dizinin içeriğini hizmet geri kalanı ile birlikte paketlenmiştir. Hizmet paketi Azure'a dağıtıldığında, IIS paketten içerik konumu ile yapılandırılır.|  

##  <a name="VirtualDirectory"></a> Sanal dizin  
`VirtualDirectory` Öğesi belirtir (yolu olarak da bilinir) bir dizin adı IIS'de belirtin ve yerel veya uzak bir sunucuda fiziksel bir dizin eşleme.

`VirtualDirectory` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `VirtualDirectory` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Sanal dizin tanımlamak için bir ad belirtir.|  
|değer|physicalDirectory|Gereklidir. Web sitesi veya sanal dizin içeriğini içeren geliştirme makinenizde yolunu belirtir. İşlem öykünücüsü, IIS bu konumdan içerik almak için yapılandırılır. Azure'a dağıtırken, fiziksel dizinin içeriğini hizmet geri kalanı ile birlikte paketlenmiştir. Hizmet paketi Azure'a dağıtıldığında, IIS paketten içerik konumu ile yapılandırılır.|  

##  <a name="Bindings"></a> Bağlamaları  
`Bindings` Öğesi bir Web sitesi için bağlamaları koleksiyonunu açıklar. En üst öğesidir `Binding` öğesi. Öğe için gerekli her `Site` öğesi. Uç noktalarını yapılandırma hakkında daha fazla bilgi için bkz: [Etkinleştir iletişim rol örnekleri için](cloud-services-enable-communication-role-instances.md).

`Bindings` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

##  <a name="Binding"></a> Bağlama  
`Binding` Öğesi bir Web sitesi veya web uygulamasıyla iletişim kurmak istekleri için gerekli yapılandırma bilgilerini belirtir.

`Binding` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|dize|Gereklidir. Bağlama tanımlamak için bir ad belirtir.|  
|endpointName|dize|Gereklidir. Bağlamak için uç nokta adı belirtir.|  
|AnaBilgisayarÜstbilgisi|dize|İsteğe bağlı. Farklı ana bilgisayar adları, tek bir IP adresi/bağlantı noktası numarası bileşimi ile birden çok sitesi barındırmak izin veren bir ana bilgisayar adını belirtir.|  

##  <a name="Startup"></a> Başlangıç  
`Startup` Öğesi rolü başlatıldığında, çalışan görevleri koleksiyonu açıklar. Bu öğenin üst öğesinin olabilir `Variable` öğesi. Rol başlangıç görevleri kullanma hakkında daha fazla bilgi için bkz: [başlangıç görevlerin nasıl yapılandırıldığını](cloud-services-startup-tasks.md). Bu öğe isteğe bağlıdır ve bir rolü yalnızca bir başlangıç blok olabilir.

Özniteliği aşağıdaki tabloda açıklanmaktadır `Startup` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|öncelik|Int|Yalnızca dahili kullanım için.|  

##  <a name="Task"></a> Görev  
`Task` Öğesi rolü başlatıldığında gerçekleşir başlangıç görevi belirtir. Başlangıç görevi, yazılım bileşenleri gibi yükleme çalıştırmak veya diğer uygulamaları çalıştırmak için rol hazırlama görevleri gerçekleştirmek için kullanılabilir. Görev Yürütme göründükleri içinde sırayla `Startup` öğe bloğu.

`Task` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.3 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `Task` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|komut satırı|dize|Gereklidir. Çalıştırılacak komutları içeren bir CMD dosyası gibi bir betik. Başlangıç komut ve toplu iş dosyaları ANSI biçiminde kaydedilmesi gerekir. Dosyanın başlangıcında bir bayt sırası işaret ayarlamak dosya biçimleri düzgün işlemez.|  
|executionContext|dize|Kodun çalıştığı bağlam belirtir.<br /><br /> -   `limited` [– Barındırma işlemi rolü aynı ayrıcalıkları ile çalıştırın varsayılan].<br />-   `elevated` – Yönetici ayrıcalıklarıyla çalıştırın.|  
|taskType|dize|Komut yürütme davranışını belirtir.<br /><br /> -   `simple` [Varsayılan] – sistem herhangi bir görevi başlatılan önce çıkmak görev için bekler.<br />-   `background` – Sistem çıkmak görev için beklemez.<br />-   `foreground` – Benzer arka plan, tüm ön plan görevler çıkana kadar rolü yeniden başlatılmamış dışında.|  

##  <a name="Contents"></a> içeriği  
`Contents` Öğesi web rolü için içerik koleksiyonunu açıklar. Bu öğe üst öğesi olan `Content` öğesi.

`Contents` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.5 kullanarak ya da daha.

##  <a name="Content"></a> İçerik  
`Content` Öğesi Azure sanal makine ve onu kopyalanan hedef yolu kopyalanması için içerik kaynak konumu tanımlar.

`Content` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.5 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `Content` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|Hedef|dize|Gereklidir. İçerik yerleştirildiği Azure sanal makine konumu. Bu konum göre klasörüdür **%ROLEROOT%\Approot**.|  

Bu öğe üst öğesidir `SourceDirectory` öğesi.

##  <a name="SourceDirectory"></a> SourceDirectory  
`SourceDirectory` Öğesi, içerik kopyalanır yerel dizin tanımlar. Azure sanal makineye kopyalamasını sağlayan yerel içeriği belirtmek için bu öğeyi kullanın.

`SourceDirectory` Öğesi, yalnızca kullanılabilir Azure SDK'sı sürüm 1.5 kullanarak ya da daha.

Aşağıdaki tabloda özniteliklerini açıklayan `SourceDirectory` öğesi.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|yol|dize|Gereklidir. Azure sanal makinesi içerikleri kopyalanacak yerel bir dizine göreli veya mutlak yolu. Dizin yolu ortam değişkenleri genişlemesi desteklenir.|  
  
## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)
