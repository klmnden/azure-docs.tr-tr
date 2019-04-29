---
title: Azure bulut Hizmetleri def olarak WorkerRole şeması | Microsoft Docs
services: cloud-services
ms.custom: ''
ms.date: 04/14/2015
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 41cd46bc-c479-43fa-96e5-d6c83e4e6d89
caps.latest.revision: 55
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 90a11c5bb81a0d29f5f8a1c1696732453aa4b1ab
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62095414"
---
# <a name="azure-cloud-services-definition-workerrole-schema"></a>Azure Cloud Services tanım WorkerRole şeması
Azure çalışan rolünü genelleştirilmiş geliştirme için yararlı olan ve bir web rolü için arka plan işlemlerini gerçekleştirebilir rolüdür.

.Csdef Hizmet tanım dosyası için varsayılan uzantısıdır.

## <a name="basic-service-definition-schema-for-a-worker-role"></a>Bir çalışan rolü için temel hizmet tanımı şema.
Bir çalışan rolü içeren Hizmet tanım dosyası temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
  <WorkerRole name="<worker-role-name>" vmsize="<worker-role-size>" enableNativeCodeExecution="[true|false]">
    <Certificates>
      <Certificate name="<certificate-name>" storeLocation="[CurrentUser|LocalMachine]" storeName="[My|Root|CA|Trust|Disallow|TrustedPeople|TrustedPublisher|AuthRoot|AddressBook|<custom-store>" />
    </Certificates>
    <ConfigurationSettings>
      <Setting name="<setting-name>" />
    </ConfigurationSettings>
    <Endpoints>
      <InputEndpoint name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<local-port-number>" port="<port-number>" certificate="<certificate-name>" loadBalancerProbe="<load-balancer-probe-name>" />
      <InternalEndpoint name="<internal-endpoint-name" protocol="[http|tcp|udp|any]" port="<port-number>">
         <FixedPort port="<port-number>"/>
         <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
      </InternalEndpoint>
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">
         <AllocatePublicPortFrom>
            <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
         </AllocatePublicPortFrom>
      </InstanceInputEndpoint>
    </Endpoints>
    <Imports>
      <Import moduleName="[RemoteAccess|RemoteForwarder|Diagnostics]"/>
    </Imports>
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
         <ProgramEntryPoint commandLine="<application>" setReadyOnProcessStart="[true|false]" "/>
      </EntryPoint>
    </Runtime>
    <Startup priority="<for-internal-use-only>”>
      <Task commandLine="" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">
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
  </WorkerRole>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema öğeleri
Hizmet tanım dosyası, bu konunun sonraki bölümlerinde ayrıntılı olarak açıklanan, bu öğeleri içerir:

[WorkerRole](#WorkerRole)

[ConfigurationSettings](#ConfigurationSettings)

[Ayar](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Uç noktaları](#Endpoints)

[Inputendpoint](#InputEndpoint)

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

[EntryPoint](#EntryPoint)

[NetFxEntryPoint](#NetFxEntryPoint)

[ProgramEntryPoint](#ProgramEntryPoint)

[Değişkeni](#Variable)

[RoleInstanceValue](#RoleInstanceValue)

[Başlangıç](#Startup)

[Görev](#Task)

[İçeriği](#Contents)

[İçerik](#Content)

[SourceDirectory](#SourceDirectory)

##  <a name="WorkerRole"></a> WorkerRole
`WorkerRole` Genelleştirilmiş geliştirme için yararlı olan ve bir web rolü için arka plan işlemlerini gerçekleştirebilir bir rolü açıklar. Bir hizmet, sıfır veya daha fazla çalışan rollerini içerebilir.

Aşağıdaki tabloda özniteliklerini açıklayan `WorkerRole` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Çalışan rolü adı. Rolün adı benzersiz olmalıdır.|
|enableNativeCodeExecution|boole|İsteğe bağlı. Varsayılan değer `true`; yerel kod yürütme ve tam güven varsayılan olarak etkinleştirilir. Bu öznitelik ayarlanan `false` çalışan rolü için yerel kod yürütme devre dışı bırakabilir ve bunun yerine Azure kısmi güven kullanın.|
|vmsize|string|İsteğe bağlı. Bu rol için ayrılan sanal makinenin boyutunu değiştirmek için bu değeri ayarlayın. Varsayılan değer `Small` şeklindedir. Olası sanal makine boyutları ve öznitelikleri listesi için bkz: [bulut Hizmetleri için sanal makine boyutları](cloud-services-sizes-specs.md).|

##  <a name="ConfigurationSettings"></a> ConfigurationSettings
`ConfigurationSettings` Öğesi, bir çalışan rolü için yapılandırma ayarlarını koleksiyonunu açıklar. Bu öğenin üst öğesi değil `Setting` öğesi.

##  <a name="Setting"></a> Ayarı
`Setting` Öğesi, bir rol örneği için bir yapılandırma ayarı belirten bir ad ve değer çifti açıklar.

Aşağıdaki tabloda özniteliklerini açıklayan `Setting` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Yapılandırma ayarı için benzersiz bir ad.|

Bir rol için yapılandırma ayarlarını, hizmet tanımı dosyasında bildirilir ve hizmet yapılandırma dosyasında ayarlanan ad ve değer çiftleridir.

##  <a name="LocalResources"></a> LocalResources
`LocalResources` Öğesi, bir çalışan rolü için yerel depolama kaynakları koleksiyonunu açıklar. Bu öğenin üst öğesi değil `LocalStorage` öğesi.

##  <a name="LocalStorage"></a> LocalStorage
`LocalStorage` Öğesi zamanında hizmet dosya sistemi alanı sağlayan bir yerel depolama kaynağı tanımlar. Bir rol, sıfır veya daha fazla yerel depolama kaynaklarını tanımlayabilir.

> [!NOTE]
>  `LocalStorage` Öğesi alt öğesi olarak görünebilir `WorkerRole` Azure SDK'sının önceki sürümleriyle uyumluluk desteği için öğesi.

Aşağıdaki tabloda özniteliklerini açıklayan `LocalStorage` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Yerel depo için benzersiz bir ad.|
|cleanOnRoleRecycle|boole|İsteğe bağlı. Rolü yeniden başlatıldığında, yerel depo temizlendi olup olmadığını gösterir. Varsayılan değer `true`.|
|sizeInMb|int|İsteğe bağlı. İstenen depolama alanı için yerel depoda MB ayrılacak miktarı. Belirtilmezse, varsayılan depolama alanı ayrılan 100 MB'dir. Ayrılabileceği depolama alanı miktarını en az 1 MB'dir.<br /><br /> Yerel kaynak boyutu üst sınırı sanal makine boyutuna bağlıdır. Daha fazla bilgi için [bulut Hizmetleri için sanal makine boyutları](cloud-services-sizes-specs.md).|

Yerel depolama kaynağına ayrılmış dizinin adını ad özniteliği için sağlanan değer karşılık gelir.

##  <a name="Endpoints"></a> Uç noktaları
`Endpoints` Giriş (Dış) iç koleksiyonunu açıklar ve örnek giriş uç noktaları bir rol için. Bu öğenin üst öğesi değil `InputEndpoint`, `InternalEndpoint`, ve `InstanceInputEndpoint` öğeleri.

Giriş ve iç uç noktalar için ayrı olarak ayrılır. Bir hizmetin 25 giriş, iç, toplam olabilir ve bir hizmette izin verilen 25 rollerde ayrılan uç noktalar örneği giriş. Örneğin, 5 rolü varsa rol başına 5 giriş uç noktaları ayırabilirsiniz, 25 giriş uç noktaları tek bir rol için tahsis edebilirsiniz veya 1 giriş uç noktasına her 25 rolleri ayırabilirsiniz.

> [!NOTE]
>  Dağıtılan her bir rol, rol başına bir örneğini gerektirir. Varsayılan bir abonelik için sağlama 20 adede kadar çekirdek sınırlıdır ve bu nedenle bir rolün 20 olayla sınırlıdır. Uygulamanızı bakın sağlama varsayılan olarak sağlanan çok daha fazla örnek gerektirip gerektirmediğini [faturalama, abonelik yönetimi ve kota Destek](https://azure.microsoft.com/support/options/) kota artırma hakkında daha fazla bilgi.

##  <a name="InputEndpoint"></a> Inputendpoint
`InputEndpoint` Öğesi, bir çalışan rolü için dış uç noktası açıklar.

Birden çok HTTP, HTTPS, UDP birleşimi olan uç noktaları ve TCP uç noktaları tanımlayabilirsiniz. Giriş uç noktası için seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak her rol için belirtilen bağlantı noktası numaraları benzersiz olması gerekir. Bir rol 80 numaralı bağlantı noktası için HTTP ve bağlantı noktası 443 HTTPS kullandığını belirtirseniz, örneğin, ardından ikinci bir rol 8080 bağlantı noktası için HTTP ve bağlantı noktası 8043 HTTPS kullandığını belirtebilirsiniz.

Aşağıdaki tabloda özniteliklerini açıklayan `InputEndpoint` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Dış uç nokta için benzersiz bir ad.|
|protokol|string|Gereklidir. Dış uç noktası için Aktarım Protokolü. Bir çalışan rolü için olası değerler `HTTP`, `HTTPS`, `UDP`, veya `TCP`.|
|port|int|Gereklidir. Dış uç noktası için bağlantı noktası. Seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak her rol için belirtilen bağlantı noktası numaraları benzersiz olması gerekir.<br /><br /> Olası değerler aralığı 1 ila 65535, kapsamlı (Azure SDK sürüm 1.7 veya üzerini).|
|sertifika|string|Bir HTTPS uç noktası için gereklidir. Tarafından tanımlanan bir sertifika adını bir `Certificate` öğesi.|
|yerel bağlantı noktası|int|İsteğe bağlı. İç uç nokta bağlantıları için kullanılan bir bağlantı noktasını belirtir. `localPort` Özniteliği bir iç bağlantı noktasına bir rol üzerinde dış bağlantı uç noktasında eşler. Bu, burada bir rol bir bağlantı noktası iç bir bileşen için kullandığınızın dışında harici olarak kullanıma sunulduğunu iletişim kurması gereken senaryolarda yararlıdır.<br /><br /> Belirtilmezse, değerini `localPort` aynı `port` özniteliği. Değerini `localPort` için "*" çalışma zamanı API'si kullanılarak bulunabilir olduğundan ayrılmamış bir bağlantı noktası otomatik olarak atamak için.<br /><br /> Olası değerler aralığı 1 ila 65535, kapsamlı (Azure SDK sürüm 1.7 veya üzerini).<br /><br /> `localPort` Özniteliği, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.|
|ignoreRoleInstanceStatus|boole|İsteğe bağlı. Bu özniteliğin değeri ayarlandığında `true`, bir hizmetin durumunu göz ardı edilir ve uç nokta yük dengeleyici tarafından kaldırılmaz. Bu değeri ayarlamak `true` meşgul bir hizmetin örneklerine hata ayıklama için kullanışlıdır. Varsayılan değer `false` şeklindedir. **Not:** Bir uç nokta trafiği bile rolün hazır durumda değil yine de alabilirsiniz.|
|loadBalancerProbe|string|İsteğe bağlı. Giriş uç noktasıyla ilişkili yük dengeleyici araştırması adı. Daha fazla bilgi için [LoadBalancerProbe şeması](schema-csdef-loadbalancerprobe.md).|

##  <a name="InternalEndpoint"></a> InternalEndpoint
`InternalEndpoint` Öğesi, bir çalışan rolü için bir iç uç nokta açıklar. Bir iç uç nokta yalnızca Hizmeti'nde çalışan diğer rol örnekleri kullanılabilir; Hizmet dışındaki istemciler tarafından kullanılabilir değil. Bir çalışan rolü en fazla beş HTTP, UDP veya TCP iç uç nokta olabilir.

Aşağıdaki tabloda özniteliklerini açıklayan `InternalEndpoint` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. İç uç nokta için benzersiz bir ad.|
|protokol|string|Gereklidir. İç uç nokta için Aktarım Protokolü. Olası değerler `HTTP`, `TCP`, `UDP`, veya `ANY`.<br /><br /> Değerini `ANY` herhangi bir protokolünü herhangi bir bağlantı noktasına izin verildiğini belirtir.|
|port|int|İsteğe bağlı. İç yük dengeli uç nokta bağlantıları için kullanılan bağlantı noktası. Uç nokta kullanan iki bağlantı noktası bir yük dengeli. Genel IP adresi için kullanılan bağlantı noktasını ve özel IP adresinde kullanılan bağlantı noktası. Bunlar genellikle bunlar aynı ayarlanır, ancak farklı bağlantı noktalarını kullanmayı seçebilirsiniz.<br /><br /> Olası değerler aralığı 1 ila 65535, kapsamlı (Azure SDK sürüm 1.7 veya üzerini).<br /><br /> `Port` Özniteliği, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.|

##  <a name="InstanceInputEndpoint"></a> InstanceInputEndpoint
`InstanceInputEndpoint` Öğesi, bir çalışan rolü için bir örnek giriş uç noktası açıklar. Örnek giriş uç noktası yük dengeleyicide bağlantı noktası iletme kullanarak belirli bir rol örneği ile ilişkilidir. Her örnek giriş uç noktası, olası bağlantı noktası aralığından belirli bir bağlantı noktasıyla eşlenir. Bu öğenin üst öğesi değil `AllocatePublicPortFrom` öğesi.

`InstanceInputEndpoint` Öğesi, yalnızca Azure SDK'sı sürüm 1.7 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `InstanceInputEndpoint` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Uç nokta için benzersiz bir ad.|
|yerel bağlantı noktası|int|Gereklidir. Tüm rol örneklerine yük dengeleyiciden ileten gelen trafiği almak için dinleyeceği iç bağlantı noktasını belirtir. Olası değerler aralığı 1 ila 65535 (dahil).|
|protokol|string|Gereklidir. İç uç nokta için Aktarım Protokolü. Olası değerler: `udp` veya `tcp`. Kullanım `tcp` http/https trafiğini tabanlı için.|

##  <a name="AllocatePublicPortFrom"></a> AllocatePublicPortFrom
`AllocatePublicPortFrom` Her örnek giriş uç noktasına erişmek için dış müşteriler tarafından kullanılan ortak bağlantı noktası aralığını açıklar. Genel (VIP) bağlantı noktası numarasını bu aralıktaki ayrılmış ve Kiracı dağıtım ve güncelleştirme sırasında her ayrı rol örneğinin uç noktası atanmış. Bu öğenin üst öğesi değil `FixedPortRange` öğesi.

`AllocatePublicPortFrom` Öğesi, yalnızca Azure SDK'sı sürüm 1.7 kullanılarak kullanılabilirlik veya daha yüksek.

##  <a name="FixedPort"></a> FixedPort
`FixedPort` Öğesi için yük dengeli uç nokta bağlantılarda hangi etkinleştirir iç uç nokta, bağlantı noktasını belirtir.

`FixedPort` Öğesi, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `FixedPort` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|port|int|Gereklidir. İç uç noktası için bağlantı noktası. Bu ayar ile aynı etkiye sahip `FixedPortRange` MIN ve max aynı bağlantı noktası.<br /><br /> Olası değerler aralığı 1 ila 65535, kapsamlı (Azure SDK sürüm 1.7 veya üzerini).|

##  <a name="FixedPortRange"></a> FixedPortRange
`FixedPortRange` Öğesi iç uç nokta veya örnek giriş uç noktası için atanan bağlantı noktası aralığını belirtir ve bağlantı noktasının kullanılması için yük dengelenmiş küme uç noktası bağlantıları.

> [!NOTE]
>  `FixedPortRange` Öğesi içinde bulunduğu öğesi bağlı olarak farklı şekilde çalışır. Zaman `FixedPortRange` öğe konusu `InternalEndpoint` öğesi, tüm bağlantı noktalarında yük dengeleyicisi tüm sanal makineler üzerinde rolün çalıştığı MIN ve max özniteliklerini aralık içinde açılır. Zaman `FixedPortRange` öğe konusu `InstanceInputEndpoint` öğesi, yalnızca bir bağlantı noktası rolünü çalıştıran her sanal makinede MIN ve max özniteliklerini aralık içinde açılır.

`FixedPortRange` Öğesi, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `FixedPortRange` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|dk|int|Gereklidir. En az bağlantı noktası aralığı içinde. Olası değerler aralığı 1 ila 65535, kapsamlı (Azure SDK sürüm 1.7 veya üzerini).|
|en çok|string|Gereklidir. En fazla bağlantı noktası aralığı içinde. Olası değerler aralığı 1 ila 65535, kapsamlı (Azure SDK sürüm 1.7 veya üzerini).|

##  <a name="Certificates"></a> Sertifikaları
`Certificates` Öğesi, bir çalışan rolü için sertifika koleksiyonunu açıklar. Bu öğenin üst öğesi değil `Certificate` öğesi. Bir rol herhangi bir sayıda ilişkili sertifikaları olabilir. Sertifikaları öğesini kullanarak daha fazla bilgi için bkz: [bir sertifika ile Hizmet tanım dosyasını değiştirmektir](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="Certificate"></a> Sertifika
`Certificate` Öğesi, bir çalışan rolü ile ilişkili sertifika açıklar.

Aşağıdaki tabloda özniteliklerini açıklayan `Certificate` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Bir HTTPS ile ilişkili olduğunda başvurmak için kullanılan bu sertifika için bir ad `InputEndpoint` öğesi.|
|storeLocation|string|Gereklidir. Bu sertifika, yerel makinede burada bulunabilir sertifika deposunun konumu. Olası değerler `CurrentUser` ve `LocalMachine`.|
|storeName|string|Gereklidir. Bu sertifika yerel makine üzerinde bulunduğu sertifika deposunun adı. Olası değerler şunlardır: yerleşik deposu adları `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`, ya da herhangi bir özel depo adı. Özel depo adı belirtilirse, bu deponun otomatik olarak oluşturulur.|
|permissionLevel|string|İsteğe bağlı. Rol işlemler için verilen erişim izinleri belirtir. Özel anahtarına erişim ve belirlemek için yalnızca yükseltilmiş işlemleri istiyorsanız `elevated` izni. `limitedOrElevated` özel anahtarına erişim tüm rol işlemler izin verir. Olası değerler: `limitedOrElevated` veya `elevated`. Varsayılan değer `limitedOrElevated` şeklindedir.|

##  <a name="Imports"></a> İçeri aktarmalar
`Imports` Bileşenleri eklemek için konuk işletim sistemini içeri aktarma modülleri bir çalışan rolü için koleksiyonunu açıklar. Bu öğenin üst öğesi değil `Import` öğesi. Bu öğe isteğe bağlıdır ve yalnızca bir çalışma zamanı blok bir role sahip olabilir.

`Imports` Öğesi, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

##  <a name="Import"></a> içeri aktarma
`Import` Öğesi konuk işletim sistemine eklemek için bir modüle belirtir.

`Import` Öğesi, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `Import` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Modül adı|string|Gereklidir. İçeri aktarmak için modülünün adı. Geçerli alma modülleri şunlardır:<br /><br /> -   RemoteAccess<br />-RemoteForwarder<br />-Diagnostics<br /><br /> RemoteAccess ve RemoteForwarder modüllerini rol Örneğiniz için Uzak Masaüstü bağlantılarını yapılandırmanıza olanak sağlar. Daha fazla bilgi için [Uzak Masaüstü Bağlantısı etkinleştirme](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> Tanılama modülü bir rol örneği için Tanılama verileri toplamanızı sağlar|

##  <a name="Runtime"></a> Çalışma zamanı
`Runtime` Denetleyen Azure ana bilgisayarı işlemlerinin çalışma zamanı ortamı için ortam değişkeni ayarlarının bir çalışan rolü koleksiyonunu açıklar. Bu öğenin üst öğesi değil `Environment` öğesi. Bu öğe isteğe bağlıdır ve yalnızca bir çalışma zamanı blok bir role sahip olabilir.

`Runtime` Öğesi, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `Runtime` öğesi:

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|executionContext|string|İsteğe bağlı. Hangi rol işlemi her bağlam belirtir. Varsayılan bağlamı `limited`.<br /><br /> -   `limited` – İşlem yönetici ayrıcalıklarına gerek kalmadan başlatılmadı.<br />-   `elevated` – İşlem yönetici ayrıcalıklarıyla başlatılmadı.|

##  <a name="Environment"></a> Ortam
`Environment` Öğesi, bir çalışan rolü için ortam değişkeni ayarlarının koleksiyonunu açıklar. Bu öğenin üst öğesi değil `Variable` öğesi. Bir rol herhangi bir sayıda ayarlanan ortam değişkenlerine sahip olabilir.

##  <a name="Variable"></a> Değişkeni
`Variable` Öğesi, konuk işletim ayarlamak için bir ortam değişkenini belirtir.

`Variable` Öğesi, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `Variable` öğesi:

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Ayarlanacak ortam değişkeninin adı.|
|value|string|İsteğe bağlı. Ortam değişkeni için ayarlanacak değer. Bir değer özniteliği içermelidir veya `RoleInstanceValue` öğesi.|

##  <a name="RoleInstanceValue"></a> RoleInstanceValue
`RoleInstanceValue` Öğesi, değişkenin değerini almak xPath belirtir.

Aşağıdaki tabloda özniteliklerini açıklayan `RoleInstanceValue` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|XPath|string|İsteğe bağlı. Konumu yolu bir örneği için dağıtım ayarları. Daha fazla bilgi için [XPath yapılandırma değişkenleriyle](cloud-services-role-config-xpath.md).<br /><br /> Bir değer özniteliği içermelidir veya `RoleInstanceValue` öğesi.|

##  <a name="EntryPoint"></a> Giriş noktası
`EntryPoint` Öğesi, bir rol için giriş noktasını belirtir. Bu öğenin üst öğesi değil `NetFxEntryPoint` öğeleri. Bu öğeleri rol giriş noktası olarak görev yapacak bir uygulamayı WaWorkerHost.exe varsayılan dışındaki belirtmenizi sağlar.

`EntryPoint` Öğesi, yalnızca Azure SDK'sı sürüm 1.5 kullanılarak kullanılabilirlik veya daha yüksek.

##  <a name="NetFxEntryPoint"></a> NetFxEntryPoint
`NetFxEntryPoint` Öğesi, bir rol için çalıştırılacak programı belirtir.

> [!NOTE]
>  `NetFxEntryPoint` Öğesi, yalnızca Azure SDK'sı sürüm 1.5 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `NetFxEntryPoint` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|AssemblyName|string|Gereklidir. Giriş noktasını içeren derleme yolu ve dosya adı. Klasörüyle ilgili yol olduğu  **\\%ROLEROOT%\Approot** (belirtmeyin  **\\%ROLEROOT%\Approot** içinde `commandLine`, kabul edilir). **ROLEROOT %** bir ortam değişkeni, Azure tarafından korunur ve rolünüz için kök klasör konumunu gösterir.  **\\%ROLEROOT%\Approot** klasör rolünüz için uygulama klasörü temsil eder.|
|targetFrameworkVersion|string|Gereklidir. Derlemeyi .NET framework sürümü. Örneğin, `targetFrameworkVersion="v4.0"`.|

##  <a name="ProgramEntryPoint"></a> ProgramEntryPoint
`ProgramEntryPoint` Öğesi, bir rol için çalıştırılacak programı belirtir. `ProgramEntryPoint` Öğesi, bir .NET derleme üzerine dayalı olmayan bir programın giriş noktası belirtmenize olanak sağlar.

> [!NOTE]
>  `ProgramEntryPoint` Öğesi, yalnızca Azure SDK'sı sürüm 1.5 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `ProgramEntryPoint` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|komut satırı|string|Gereklidir. Yol, dosya adı ve yürütmek için programın herhangi bir komut satırı bağımsız değişkeni. Klasörüyle ilgili yol olduğu **%ROLEROOT%\Approot** (belirtmeyin **%ROLEROOT%\Approot** komut satırı kabul edilir). **ROLEROOT %** bir ortam değişkeni, Azure tarafından korunur ve rolünüz için kök klasör konumunu gösterir. **%ROLEROOT%\Approot** klasör rolünüz için uygulama klasörü temsil eder.<br /><br /> Rol, bu nedenle genellikle programın, yalnızca başlar ve sınırlı bir görev çalıştırdığında bir program olan yerine çalıştırmaya devam etmek için program sona ererse dönüştürülmeden.|
|setReadyOnProcessStart|boole|Gereklidir. Rol örneği için komut satırı programı başlatıldığını göstermek bekleyip beklemeyeceğini belirtir. Bu değer ayarlanmalıdır `true` şu anda. Değerini `false` gelecekte kullanılmak üzere ayrılmıştır.|

##  <a name="Startup"></a> Başlangıç
`Startup` Rolü başlatıldığında çalıştırılan görev koleksiyonunu açıklar. Bu öğenin üst öğesi olabilir `Variable` öğesi. Rol başlangıç görevleri kullanma hakkında daha fazla bilgi için bkz. [başlangıç görevlerini yapılandırma](cloud-services-startup-tasks.md). Bu öğe isteğe bağlıdır ve yalnızca bir başlangıç bloğu bir role sahip olabilir.

Öznitelik, aşağıdaki tabloda açıklanmıştır `Startup` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|öncelik|int|Yalnızca iç kullanım içindir.|

##  <a name="Task"></a> Görev
`Task` Öğesi rol başladığında gerçekleşir başlangıç görevi belirtir. Başlangıç görevleri, tür yükleme yazılım bileşenlerini çalıştırmak veya diğer uygulamaları çalıştırmak için rol hazırlama görevleri gerçekleştirmek için kullanılabilir. Görevleri yürütme göründükleri içinde sırayla `Startup` öğe bloğu.

`Task` Öğesi, yalnızca Azure SDK'sı sürüm 1.3 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `Task` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|komut satırı|string|Gereklidir. Çalıştırılacak komutları içeren bir CMD dosyası gibi bir betik. Başlangıç komutu ve toplu iş dosyaları ANSI biçimde kaydedilmesi gerekir. Dosyanın başında bayt sırası işaret Ayarla dosya biçimleri düzgün şekilde işlemez.|
|executionContext|string|Betiğin çalıştırıldığı bağlam belirtir.<br /><br /> -   `limited` [– Barındırma işlemi rolle aynı ayrıcalıklarla çalıştır varsayılan].<br />-   `elevated` – Yönetici ayrıcalıklarıyla çalıştırın.|
|taskType|string|Komutun yürütme davranışını belirtir.<br /><br /> -   `simple` [Varsayılan] – sistemi herhangi bir görevi tasarlandıkça önce çıkmak görevin tamamlanmasını bekler.<br />-   `background` – Sistem çıkmak görevin tamamlanmasını beklemez.<br />-   `foreground` – Benzer arka plan, tüm ön plan görevlerini çıkana kadar rolü yeniden başlatılmaz.|

##  <a name="Contents"></a> İçeriği
`Contents` Öğesi, bir çalışan rolü için içerik koleksiyonunu açıklar. Bu öğenin üst öğesi değil `Content` öğesi.

`Contents` Öğesi, yalnızca Azure SDK'sı sürüm 1.5 kullanılarak kullanılabilirlik veya daha yüksek.

##  <a name="Content"></a> İçeriği
`Content` Öğe içeriği, Azure sanal makinesi ve bunun kopyalandığı hedef yol için Kopyalanacak kaynak konumunu tanımlar.

`Content` Öğesi, yalnızca Azure SDK'sı sürüm 1.5 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `Content` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Hedef|string|Gereklidir. Azure sanal makinesine içeriği yerleştirildiği konum. Bu konum klasörüyle ilgili olan **%ROLEROOT%\Approot**.|

Bu öğenin üst öğesi olan `SourceDirectory` öğesi.

##  <a name="SourceDirectory"></a> SourceDirectory
`SourceDirectory` Öğe içeriği kopyalandığı yerel dizin tanımlar. Azure sanal makineye kopyalamak için yerel içeriği belirtmek için bu öğeyi kullanırsınız.

`SourceDirectory` Öğesi, yalnızca Azure SDK'sı sürüm 1.5 kullanılarak kullanılabilirlik veya daha yüksek.

Aşağıdaki tabloda özniteliklerini açıklayan `SourceDirectory` öğesi.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|path|string|Gereklidir. Azure sanal makinesine içerikleri kopyalanacak yerel bir dizine göreli veya mutlak yolu. Ortam değişkenlerini dizin yolunda genişletme desteklenir.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)