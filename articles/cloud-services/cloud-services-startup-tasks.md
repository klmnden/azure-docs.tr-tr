---
title: Azure bulut hizmetlerine başlangıç görevleri çalıştırmak | Microsoft Docs
description: Başlangıç görevi, bulut hizmeti ortamınızı uygulamanız için hazırlanmanıza yardımcı. Bu, başlangıç görevleri nasıl çalıştığını ve bunları nasıl öğretir
services: cloud-services
documentationcenter: ''
author: Thraka
manager: timlt
editor: ''
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 1c1b3aa86dc8211de0c07c9fb68da5685c86f551
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23843682"
---
# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri Çalıştır
Başlangıç görevi, bir rol başlamadan önce işlemlerini gerçekleştirmek için kullanabilirsiniz. Bir bileşeni yüklemek, COM bileşenlerini kaydetme, kayıt defteri anahtarlarını ayarlamak veya uzun süre çalışan bir işlem başlatılıyor gerçekleştirmek isteyebileceğiniz işlemleri içerir.

> [!NOTE]
> Başlangıç görevi sanal makineler için yalnızca bulut hizmeti Web ve çalışan rolleri için geçerli değildir.
> 
> 

## <a name="how-startup-tasks-work"></a>Başlangıç görevi nasıl çalışır
Başlangıç görevlerdir rollerinizi başlar ve içinde tanımlanan önce gerçekleştirilen eylemler [ServiceDefinition.csdef] kullanarak dosya [görev] öğesi içinde [başlangıç] öğesi. Sık başlangıç görevleri toplu dosyalar, ancak bunlar aynı zamanda konsol uygulamaları veya PowerShell komut dosyalarını Başlat toplu iş dosyaları olabilir.

Ortam değişkenleri, bir başlangıç görevi bilgi aktarmak ve yerel depolama bir başlangıç görevi dışında bilgi geçirmek için kullanılabilir. Örneğin, bir ortam değişkeni yüklemek istediğiniz program yolu belirtebilirsiniz ve ardından daha sonra rolleri tarafından okunabilir yerel depolama alanına dosyalar yazılabilir.

Başlangıç görevi bilgileri ve hataları tarafından belirtilen dizin oturum **TEMP** ortam değişkeni. Başlangıç görevi sırasında **TEMP** ortam değişkenini çözümler için *C:\\kaynakları\\temp\\[GUID]. [ Rol adı]\\RoleTemp* bulut üzerinde çalışırken dizin.

Başlangıç görevi yeniden başlatmalar arasında da birkaç kez çalıştırılabilir. Örneğin, başlangıç görevi rol geri dönüştürme her zaman çalıştırılır ve rol dönüştürür her zaman yeniden başlatma içermeyebilir. Başlangıç görevi, bunları birkaç kez sorunsuz çalışmasına izin veren şekilde yazılmalıdır.

Başlangıç görevleri ile bitmelidir bir **errorlevel** (veya çıkış kodu) başlatma işlemini tamamlamak için sıfır. Bir başlangıç görevi bir sıfır ile bitip bitmediğini **errorlevel**, rol başlatılmaz.

## <a name="role-startup-order"></a>Rol başlatma sırası
Azure rol başlangıç yordamda listeler:

1. Örnek olarak işaretlenmiş **başlangıç** ve trafik almaz.
2. Tüm başlangıç görevleri göre yürütülür kendi **taskType** özniteliği.
   
   * **Basit** görevler eşzamanlı olarak, yürütülen bir kerede.
   * **Arka plan** ve **ön plan** görevleri başlangıç görevi başlatılacağı zaman uyumsuz olarak, paralel.  
     
     > [!WARNING]
     > Role özgü verileri kullanılabilir olmaması IIS tam olarak başlatma işleminde, başlangıç görevi aşaması sırasında yapılandırılmamış olabilir. Role özgü veri gerektiren başlangıç görevleri kullanması gereken [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).
     > 
     > 
3. Rol ana bilgisayar işlemi başlatıldı ve sitesi IIS içinde oluşturulur.
4. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) yöntemi çağrılır.
5. Örnek olarak işaretlenmiş **hazır** ve trafik örneğine yönlendirilir.
6. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi çağrılır.

## <a name="example-of-a-startup-task"></a>Bir başlangıç görevi örneği
Başlangıç görevi tanımlanmış [ServiceDefinition.csdef] dosyasında **görev** öğesi. **CommandLine** özniteliği, adını ve başlangıç toplu iş dosyasını veya konsol komutunu, parametrelerini belirtir **executionContext** özniteliği başlangıç görevinin ayrıcalık düzeyi belirtir ve **taskType** özniteliği, görevin nasıl yürütülecek belirtir.

Bu örnekte, bir ortam değişkeni **MyVersionNumber**, başlangıç görevi oluşturulur ve değerine ayarlayın "**1.0.0.0**".

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

Aşağıdaki örnekte, **Startup.cmd** toplu iş dosyası "geçerli sürüm 1.0.0.0 StartupLog.txt dosyasına TEMP ortam değişkeni tarafından belirtilen dizinde." satır yazar. `EXIT /B 0` Satır sağlar başlangıç görevi ile biten bir **errorlevel** sıfır.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> Visual Studio'da **çıktı dizinine Kopyala** özelliği başlangıç toplu işlem dosyanız için ayarlanmalıdır **her zaman Kopyala** başlangıç toplu iş dosyası projenize Azure üzerinde düzgün bir şekilde dağıtıldıktan emin olmak için (**approot\\bin** Web rolleri için ve **approot** çalışan roller için).
> 
> 

## <a name="description-of-task-attributes"></a>Görev öznitelikler açıklaması
Aşağıdaki özniteliklerini açıklayan **görev** öğesinde [ServiceDefinition.csdef] dosyası:

**komut satırı** -başlangıç görevinin komut satırını belirtir:

* Başlangıç görevi başlatan komutuyla, isteğe bağlı komut satırı parametreleri.
* Sık .cmd veya .bat toplu dosyanın budur.
* AppRoot göre görevdir\\Bin klasörü dağıtımı için. Ortam değişkenleri yolu ve dosya görevinin belirlerken genişletilir değil. Ortam genişletme gerekiyorsa, başlangıç görevi çağıran bir küçük .cmd komut dosyası oluşturabilirsiniz.
* Bir konsol uygulaması ya da başlayan bir toplu iş dosyası bir [PowerShell Betiği](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** -başlangıç görevi ayrıcalık düzeyini belirtir. Ayrıcalık düzeyi sınırlı veya yükseltilmiş:

* **sınırlı**  
  Başlangıç görevi rolle aynı ayrıcalıklarıyla çalıştırır. Zaman **executionContext** için öznitelik [çalışma zamanı] öğesidir de **sınırlı**, kullanıcı ayrıcalıkları kullanılır.
* **yükseltilmiş**  
  Başlangıç görevi yönetici ayrıcalıklarıyla çalıştırır. Bu programları yükleyin, IIS yapılandırma değişiklikleri, kayıt defteri değişiklikleri gerçekleştirmek için başlangıç görevleri ve diğer yönetici düzeyi görevleri rol ayrıcalık düzeyi artırmadan sağlar.  

> [!NOTE]
> Bir başlangıç görevi ayrıcalık düzeyi rolü ile aynı olması gerekmez.
> 
> 

**taskType** -başlangıç görevi yürütüldüğünde yolunu belirtir.

* **Basit**  
  Görevler eşzamanlı olarak, yürütülen bir kerede, belirtilen sırada [ServiceDefinition.csdef] dosya. Bir zaman **basit** başlangıç görevi sonlandırır ile bir **errorlevel** sıfır, sonraki **basit** başlangıç görevi gerçekleştirilir. Artık böyle olduğunda **basit** rol kendisi başlatılır sonra yürütülecek başlangıç görevi.   
  
  > [!NOTE]
  > Varsa **basit** görev bir sıfır ile biten **errorlevel**, örnek engellenir. Sonraki **basit** başlangıç görevleri ve rol kendisinin başlatılmayacak.
  > 
  > 
  
    Toplu iş dosyası ile biten emin olmak için bir **errorlevel** hiç biri komutu yürütün `EXIT /B 0` toplu iş dosyası işleminin sonunda.
* **arka plan**  
  Görevler, başlangıç rolünün ile paralel zaman uyumsuz olarak yürütülür.
* **ön plan**  
  Görevler, başlangıç rolünün ile paralel zaman uyumsuz olarak yürütülür. Arasındaki temel farklılık bir **ön plan** ve **arka plan** görev olan bir **ön plan** görev rol geri dönüştürme ya da görev sona erinceye kadar kapatılıyor engeller. **Arka plan** görevleri bu kısıtlama yok.

## <a name="environment-variables"></a>Ortam değişkenleri
Ortam değişkenleri, bir başlangıç görevi bilgi aktarmak için bir yoldur. Örneğin, yüklemek için bir programı içeren blob veya rolünüze kullanacağı bağlantı noktası numaraları ve başlangıç görevinin özelliklerini denetlemek için ayarları yolu koyabilirsiniz.

Başlangıç görevler için ortam değişkenleri iki tür vardır; statik ortam değişkenleri ve ortam değişkenleri göre üyelerinde [RoleEnvironment] sınıfı. Her ikisi de bulunan [ortam] bölümünü [ServiceDefinition.csdef] dosya ve her iki kullanım [değişken] öğesi ve **adı** özniteliği.

Statik ortam değişkenlerini kullanır **değeri** özniteliği [değişken] öğesi. Yukarıdaki örnekte ortam değişkeni oluşturur **MyVersionNumber** statik değeri olan "**1.0.0.0**". Başka bir örnek oluşturmak için olabilir bir **StagingOrProduction** değerleri el ile ayarlayabilirsiniz ortam değişkeni "**hazırlama**"veya"**üretim**" değerine göre farklı başlangıç eylemleri gerçekleştirmek için **StagingOrProduction** ortam değişkeni.

RoleEnvironment sınıfının üyelere bağlı ortam değişkenlerini kullanma **değeri** özniteliği [değişken] öğesi. Bunun yerine, [RoleInstanceValue] uygun olan alt öğesi **XPath** öznitelik değeri, belirli bir üye üzerinde dayalı bir ortam değişkeni oluşturmak için kullanılan [RoleEnvironment] sınıfı. İçin değerler **XPath** çeşitli erişmek için öznitelik [RoleEnvironment] değerler bulunabilir [burada](cloud-services-role-config-xpath.md).

Örneğin, bir ortam değişkeni oluşturmak için "**true**" örneği için işlem öykünücüsünü çalıştırırken ve "**false**" bulutta çalıştırırken aşağıdaki kullanın [değişken] ve [RoleInstanceValue] öğeleri:

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
Bazı gerçekleştirmek öğrenin [ortak başlangıç görevleri](cloud-services-startup-tasks-common.md) bulut hizmetiniz ile.

[Paket](cloud-services-model-and-package.md) bulut hizmetiniz.  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[görev]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[başlangıç]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[çalışma zamanı]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[ortam]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[değişken]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
