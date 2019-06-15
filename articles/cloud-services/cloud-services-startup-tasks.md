---
title: Azure bulut hizmetlerindeki başlangıç görevleri çalıştırma | Microsoft Docs
description: Başlangıç görevleri, uygulamanız için bulut hizmeti ortamınızı Hazırlama yardımcı olur. Bu, başlangıç görevleri nasıl çalıştığını ve bunları nasıl öğretir
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: 59bfa83ab3432adb7a4df5112367f87014a0b292
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60405996"
---
# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Yapılandırma ve bulut hizmeti için başlangıç görevleri çalıştırma
Başlangıç görevleri rol başlamadan önce işlemleri gerçekleştirmek için kullanabilirsiniz. Gerçekleştirmek isteyebileceğiniz işlemler, bir bileşeni yükleniyor, COM bileşenleri kaydediliyor, kayıt defteri anahtarlarını ayarlamak veya uzun süre çalışan bir işlem başlatılıyor içerir.

> [!NOTE]
> Başlangıç görevleri, sanal makineler, bulut hizmeti Web ve çalışan rolleri için geçerli değildir.
> 
> 

## <a name="how-startup-tasks-work"></a>Başlangıç görevleri nasıl çalışır
Başlangıç görevleri, rolleriniz başlamadan tanımlanan önce gerçekleştirilen eylemler olan [ServiceDefinition.csdef] kullanarak dosya [görev] öğesiyle [başlangıç] öğe. Sık sık başlangıç görevleri batch dosyalarıdır, ancak bunlar konsol uygulamaları veya PowerShell betikleri başlatmak toplu iş dosyaları da olabilir.

Ortam değişkenlerini bir başlangıç görevi bilgilerini geçirmek ve yerel depolama, bilgi başlangıç görevi dışına geçirmek için kullanılabilir. Örneğin, bir ortam değişkeni, bir program yüklemek istediğiniz yolu belirtebilirsiniz ve ardından daha sonra rolleri tarafından okunabilir yerel depolama alanında dosyalar yazılabilir.

Başlangıç göreviniz bilgileri ve hataları tarafından belirtilen dizinde oturum **TEMP** ortam değişkeni. Başlangıç görevi sırasında **TEMP** ortam değişkenini çözümler için *C:\\kaynakları\\temp\\[GUID]. [ rolename]\\RoleTemp* bulutta çalışırken dizin.

Başlangıç görevleri, yeniden başlatmalar arasında birçok defa da yürütülebilir. Örneğin, her rol döngüsünde başlangıç görevi çalıştırılır ve rol döngüleri her zaman yeniden başlatma içermeyebilir. Başlangıç görevleri, bunları birkaç kez sorunsuz çalışmasına izin veren bir şekilde yazılması gerekir.

Başlangıç görevleri ile bitmelidir bir **errorlevel** (veya çıkış kodu) başlangıç işleminin tamamlanması sıfır. Başlangıç görevi bir sıfır ile bitip bitmediğini **errorlevel**, rol başlatılmaz.

## <a name="role-startup-order"></a>Rol başlatma sırası
Azure rol başlatma yordamda listeler:

1. Örnek olarak işaretlenmiş **başlangıç** ve trafiği almaz.
2. Şunlara göre yürütüldüğü tüm başlangıç görevleri, **taskType** özniteliği.
   
   * **Basit** görevlerin zaman uyumlu olarak, yürütülme teker teker.
   * **Arka plan** ve **ön plan** görevler için başlangıç görevi başlatılacağı zaman uyumsuz olarak, paralel.  
     
     > [!WARNING]
     > Role özgü verileri kullanılabilir olmaması IIS tam olarak başlangıç görevi aşamasında başlatma işlemi sırasında yapılandırılmamış olabilir. Role özgü veri gerektiren başlangıç görevleri kullanması gereken [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)).
     > 
     > 
3. Rolü ana bilgisayar işlemi başlatılır ve sitesi IIS içinde oluşturulur.
4. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) yöntemi çağrılır.
5. Örnek olarak işaretlenmiş **hazır** ve trafiği örneğine yönlendirilir.
6. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](/previous-versions/azure/reference/ee772746(v=azure.100)) yöntemi çağrılır.

## <a name="example-of-a-startup-task"></a>Başlangıç görevi örneği
Başlangıç görevleri tanımlanmış [ServiceDefinition.csdef] dosyasındaki **görev** öğesi. **CommandLine** özniteliği belirtir başlangıç toplu dosya ya da konsol komutunun parametrelerinin ve adının **executionContext** özniteliği belirtir başlangıç görevinin ayrıcalık düzeyi ve **taskType** özniteliği, görevin nasıl yürütülecek belirtir.

Bu örnekte, bir ortam değişkeni **MyVersionNumber**, için başlangıç görevi oluşturulur ve değerine ayarlanırsa "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

Aşağıdaki örnekte, **Startup.cmd** toplu iş dosyası satırın "geçerli sürüm 1.0.0.0 StartupLog.txt dosyasına TEMP ortam değişkeni tarafından belirtilen dizindeki." yazar. `EXIT /B 0` Satır sağlar başlangıç görevinin ile biten bir **errorlevel** sıfır.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> Visual Studio'da **çıkış dizinine Kopyala** başlangıç toplu iş dosyasında özelliği ayarlanmalıdır **her zaman Kopyala** başlangıç toplu iş dosyasında projenize (azure'dadüzgünşekildedağıtıldığındaneminolmakiçin**approot\\bin** Web rolleri için ve **approot** çalışan rolleri için).
> 
> 

## <a name="description-of-task-attributes"></a>Görev özniteliklerinin açıklaması
Aşağıdaki özniteliklerini açıklayan **görev** öğesinde [ServiceDefinition.csdef] dosyası:

**komut satırı** -başlangıç görevinin komut satırı belirtir:

* Başlangıç görevi başlatan komutla, isteğe bağlı komut satırı parametreleri.
* Sık .cmd veya .bat toplu iş dosyası adını budur.
* AppRoot göre görevdir\\dağıtım Bin klasörü. Ortam değişkenlerini yolu ve dosyayı görevin saptanırken genişletilmiş değil. Ortam genişletme gerekiyorsa, başlangıç göreviniz çağıran bir küçük .cmd komut dosyası oluşturabilirsiniz.
* Bir konsol uygulaması ya da başlayan bir toplu iş dosyası bir [PowerShell Betiği](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** -başlangıç görevinin ayrıcalık düzeyini belirtir. Ayrıcalık düzeyi sınırlı veya yükseltilmiş:

* **Sınırlı**  
  Başlangıç görevi rolle aynı ayrıcalıklarıyla çalıştırır. Zaman **executionContext** özniteliğini [çalışma zamanı] öğedir ayrıca **sınırlı**, kullanıcı ayrıcalıkları kullanılır.
* **yükseltilmiş**  
  Başlangıç görevi yönetici ayrıcalıklarıyla çalıştırır. Bu programları yükleyin, IIS yapılandırma değişiklikleri, kayıt defteri değişiklikleri gerçekleştirmek için başlangıç görevleri ve diğer yönetici düzeyi görevleri rol ayrıcalık düzeyini artırmadan sağlar.  

> [!NOTE]
> Başlangıç görevi ayrıcalık düzeyini rolle aynı olması gerekmez.
> 
> 

**taskType** -başlangıç görevi yürütülür yolunu belirtir.

* **Basit**  
  Görevlerin zaman uyumlu olarak, yürütülme belirtilen sırada, teker teker [ServiceDefinition.csdef] dosya. Bir **basit** başlangıç görevi ile sona erer bir **errorlevel** sıfır, sonraki **basit** başlangıç görevi yürütülür. Artık varsa **basit** başlangıç görevleri rol başlatılacak sonra yürütülecek.   
  
  > [!NOTE]
  > Varsa **basit** görev bir sıfır ile biten **errorlevel**, örnek engellenir. Sonraki **basit** başlangıç görevleri ve rol kendisini başlatılmayacak.
  > 
  > 
  
    Toplu işlem dosyanız ile biten emin olmak için bir **errorlevel** hiç biri bağlamını `EXIT /B 0` , toplu iş dosyası işleminin sonunda.
* **Arka plan**  
  Görevler paralel rol başlangıcı ile zaman uyumsuz olarak yürütülür.
* **Ön plan**  
  Görevler paralel rol başlangıcı ile zaman uyumsuz olarak yürütülür. Arasındaki temel fark bir **ön plan** ve **arka plan** görev olan bir **ön plan** görev alanından döngüdeyken veya görev bitene kadar rol engeller. **Arka plan** görevleri bu kısıtlama yoktur.

## <a name="environment-variables"></a>Ortam değişkenleri
Ortam değişkenleri, başlangıç görevine eklenmesi bilgi geçirmek için bir yoludur. Örneğin, içeren bir program yüklemek için blob veya rolünüz kullanacağı bağlantı noktası numaralarını ve başlangıç göreviniz özellikleri denetlemek için ayarları yol yerleştirebilirsiniz.

Başlangıç görevleri için ortam değişkenlerini iki çeşit vardır; statik değişkenler ve ortam değişkenlerini göre üyelerinde [RoleEnvironment] sınıfı. Hem bulunan [ortam] bölümünü [ServiceDefinition.csdef] dosya ve her iki kullanımı [Değişkeni] öğesi ve **adı** özniteliği.

Statik ortam değişkenlerini kullanan **değer** özniteliği [Değişkeni] öğesi. Yukarıdaki örnekte ortam değişkenini oluşturur **MyVersionNumber** statik değeri olan "**1.0.0.0**". Başka bir örnek oluşturmak olacaktır bir **StagingOrProduction** değerleri el ile ayarlayabileceğiniz ortam değişkeni "**hazırlama**"veya"**üretim**" gerçekleştirmek için farklı başlatma eylemleri değerini temel alarak **StagingOrProduction** ortam değişkeni.

Ortam değişkenlerini RoleEnvironment sınıfın üyelerinde tabanlı kullanmayın **değer** özniteliği [Değişkeni] öğesi. Bunun yerine, [RoleInstanceValue] uygun alt öğesi **XPath** öznitelik değeri, belirli bir üye üzerinde temel bir ortam değişkenini oluşturmak için kullanılan [RoleEnvironment] sınıfı. Değerleri **XPath** çeşitli erişmek için öznitelik [RoleEnvironment] değerleri bulunabilir [burada](cloud-services-role-config-xpath.md).

Örneğin, bir ortam değişkenini oluşturmak için "**true**" işlem öykünücüsünde örnek çalışırken ve "**false**" bulutta çalışırken, aşağıdaki kullanması [Değişkeni] ve [RoleInstanceValue] öğeleri:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Sonraki adımlar
Bazı gerçekleştirmeyi öğreneceksiniz [genel başlangıç görevleri](cloud-services-startup-tasks-common.md) , bulut hizmeti.

[Paket](cloud-services-model-and-package.md) bulut hizmetinizde.  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Görev]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Başlangıç]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Çalışma zamanı]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Ortam]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Değişkeni]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
