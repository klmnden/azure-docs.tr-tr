---
title: Azure Application Insights OpenCensus dağıtılmış izleme yerel ileticisi'ni (Önizleme) | Microsoft docs
description: Dağıtılmış OpenCensus izlemeleri ve yayılma Python ve Go gibi diller için Azure Application Insights ilet
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/18/2018
ms.reviewer: nimolnar
ms.author: mbullwin
ms.openlocfilehash: a7efe663a75fa29a31e7157c5eab24c2973a3758
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699345"
---
# <a name="local-forwarder-preview"></a>Yerel ileticisi'ni (Önizleme)

Yerel ileticisi olan Application Insights tarafından toplanan bir aracı veya [OpenCensus](https://opencensus.io/) çeşitli SDK'lar alınan telemetri ve uygulama anlayışları'na yönlendirir. Bu, Windows ve Linux altında çalıştırma yeteneğine sahiptir. Ayrıca macOS altında çalıştırmak mümkün olabilir, ancak resmi olarak şu anda desteklenmiyor.

## <a name="running-local-forwarder"></a>Yerel ileticisi çalışıyor

Yerel ileticinin olduğu bir [GitHub üzerinde açık kaynaklı proje](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/releases). Birden çok platformda yerel ileticisi'ni çalıştırmak için yol çeşitli vardır.

### <a name="windows"></a>Windows

#### <a name="windows-service"></a>Windows Hizmeti

Yerel ileticisi Windows altında çalışan en kolay yolu, bir Windows hizmeti olarak yükleyerek ' dir. Sürüm bir Windows hizmeti yürütülebilir dosyası ile birlikte gelir (*WindowsServiceHost/Microsoft.LocalForwarder.WindowsServiceHost.exe*), kolayca kaydedilebilir işletim sistemi.

> [!NOTE]
> Yerel ileticisi hizmetinin en az .NET Framework 4.7 gerektirir. Hizmet .NET Framework 4.7 yoksa yükleme, ancak başlatılamıyor. .NET Framework'ün en son sürümüne erişim sağlamak **[.NET Framework Yükleme sayfasını ziyaret edin](
https://www.microsoft.com/net/download/dotnet-framework-runtime/net472?utm_source=getdotnet&utm_medium=referral)**.

1. LF indirin. WindowsServiceHost.zip dosyasından [yerel ileticisi sürüm sayfası](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/releases) GitHub üzerinde.

    ![Yerel ileticisi yayın indirme sayfasının ekran görüntüsü](./media/opencensus-local-forwarder/001-local-forwarder-windows-service-host-zip.png)

2. Tanıtım kolaylığı için bu örnekte, biz yalnızca yolun .zip dosyasına ayıklayacaktır `C:\LF-WindowsServiceHost`.

    Hizmet kaydı ve sistem önyüklemesi yönetici olarak komut satırından aşağıdaki komutu çalıştırın, başlayacak şekilde yapılandırmak için:

    ```
    sc create "Local Forwarder" binpath="C:\LF-WindowsServiceHost\Microsoft.LocalForwarder.WindowsServiceHost.exe" start=auto
    ```
    
    Bir yanıtı almanız gerekir:
    
    `[SC] CreateService SUCCESS`
    
    Yeni hizmetinizi Hizmetleri GUI türü yoluyla incelemek için ``services.msc``
        
     ![Yerel iletici hizmetinin ekran görüntüsü](./media/opencensus-local-forwarder/002-services.png)

3. **Sağ** seçin ve yeni yerel ileticisi **Başlat**. Hizmetiniz artık çalışır duruma girer.

4. Varsayılan olarak tüm kurtarma eylemleri olmadan hizmeti oluşturulur. Yapabilecekleriniz **sağ** seçip **özellikleri** > **kurtarma** otomatik yanıtlar bir hizmet hatası yapılandırmak için.

    Veya hatalar oluştuğunda, program aracılığıyla için otomatik kurtarma seçeneklerini ayarlamak tercih ederseniz kullanabilirsiniz:

    ```
    sc failure "Local Forwarder" reset= 432000 actions= restart/1000/restart/1000/restart/1000
    ```

5. Aynı konumda, ``Microsoft.LocalForwarder.WindowsServiceHost.exe`` olan bu örnekte dosyası ``C:\LF-WindowsServiceHost`` adlı bir dosya var. ``LocalForwarder.config``. Bu, localforwader yapılandırmasını ayarlamak ve iletilen dağıtılmış izleme verilerinizi istediğiniz Application Insights kaynağına ait izleme anahtarını belirtmek izin veren bir xml tabanlı dosyasıdır. 

    Düzenleme sonra ``LocalForwarder.config`` izleme anahtarınızı eklemek için yeniden başlattığınızdan dosya **yerel iletici hizmeti** , değişikliklerin etkili olması için izin vermek için.
    
6. İstenen ayarlarınızı yerinde olduğundan ve yerel ileticisi için izleme verilerini beklenen onay olarak dinlediğini doğrulamak için ``LocalForwarder.log`` dosya. Dosyanın sonuna görüntüye benzer bir sonuç görmeniz gerekir:

    ![Ekran görüntüsü, LocalForwarder.log dosyası](./media/opencensus-local-forwarder/003-log-file.png)

#### <a name="console-application"></a>Konsol uygulaması

Belirli kullanım örnekleri için yerel iletici bir konsol uygulaması olarak çalıştırmak yararlı olabilir. Sürüm, konsol konağı yürütülebilir aşağıdaki sürümleriyle birlikte gelir:
* framework bağımlı .NET Core ikili */ConsoleHost/publish/Microsoft.LocalForwarder.ConsoleHost.dll*. Bu ikili çalışan bir .NET Core çalışma zamanı yüklü olmasını gerektirir; Bu indirme sayfasına başvuruda [sayfa](https://www.microsoft.com/net/download/dotnet-core/2.1) Ayrıntılar için.
  ```batchfile
  E:\uncdrop\ConsoleHost\publish>dotnet Microsoft.LocalForwarder.ConsoleHost.dll
  ```
* ikili dosyaları x86 ve x64 platformları için kendi içinde .NET Core kümesi. Bu işlem, çalıştırmak için .NET Core çalışma zamanı gerektirmez. */ConsoleHost/win-x86/publish/Microsoft.LocalForwarder.ConsoleHost.exe*, */ConsoleHost/win-x64/publish/Microsoft.LocalForwarder.ConsoleHost.exe*.
  ```batchfile
  E:\uncdrop\ConsoleHost\win-x86\publish>Microsoft.LocalForwarder.ConsoleHost.exe
  E:\uncdrop\ConsoleHost\win-x64\publish>Microsoft.LocalForwarder.ConsoleHost.exe
  ```

### <a name="linux"></a>Linux

Windows gibi yayın konsol konağı yürütülebilir aşağıdaki sürümleriyle birlikte:
* framework bağımlı .NET Core ikili */ConsoleHost/publish/Microsoft.LocalForwarder.ConsoleHost.dll*. Bu ikili çalışan bir .NET Core çalışma zamanı yüklü olmasını gerektirir; Bu indirme sayfasına başvuruda [sayfa](https://www.microsoft.com/net/download/dotnet-core/2.1) Ayrıntılar için.

```batchfile
dotnet Microsoft.LocalForwarder.ConsoleHost.dll
```

* linux-64 ikili dosyalarını kendi başına bir .NET Core kümesi. Bunu çalıştırmak için .NET Core çalışma zamanı gerektirmez. */ConsoleHost/linux-x64/publish/Microsoft.LocalForwarder.ConsoleHost*.

```batchfile
user@machine:~/ConsoleHost/linux-x64/publish$ sudo chmod +x Microsoft.LocalForwarder.ConsoleHost
user@machine:~/ConsoleHost/linux-x64/publish$ ./Microsoft.LocalForwarder.ConsoleHost
```

Çok sayıda Linux kullanıcıları yerel iletici bir daemon çalıştırmak isteyebilirsiniz. Linux sistemleri Upstart, sysv veya systemd gibi hizmet yönetimi çözümleri çeşitli gelir. İnovasyonunuz ne olursa olsun, belirli bir sürümüdür, onu yerel ileticisi senaryonuz için en uygun bir şekilde çalıştırmak için kullanabilirsiniz.

Örneğin, bir arka plan programı hizmeti systemd kullanarak oluşturalım. Framework bağımlı sürümü kullanacağız, ancak aynı de kendi içinde bir yapılabilir.

* adlı aşağıdaki hizmet dosyası oluşturma *localforwarder.service* ve içine yerleştirileceği */lib/systemd/system*.
Bu örnek kullanıcı adınızdır SAMPLE_USER ve yerel ileticisi framework bağımlı ikili dosyaları kopyaladıktan varsayılır (gelen */ConsoleHost/yayımlama*) için */home/SAMPLE_USER/LOCALFORWARDER_DIR*.

```
# localforwarder.service
# Place this file into /lib/systemd/system/
# Use 'systemctl enable localforwarder' to start the service automatically on each boot
# Use 'systemctl start localforwarder' to start the service immediately

[Unit]
Description=Local Forwarder service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=SAMPLE_USER
WorkingDirectory=/home/SAMPLE_USER/LOCALFORWARDER_DIR
ExecStart=/usr/bin/env dotnet /home/SAMPLE_USER/LOCALFORWARDER_DIR/Microsoft.LocalForwarder.ConsoleHost.dll noninteractive

[Install]
WantedBy=multi-user.target
```

* Her önyükleme yerel ileticisi'ni başlatmak için systemd istemek için aşağıdaki komutu çalıştırın

```
systemctl enable localforwarder
```

* Yerel ileticisi hemen başlatmak için systemd istemek için aşağıdaki komutu çalıştırın

```
systemctl start localforwarder
```

* Hizmet inceleyerek izleyin **.log* /home/SAMPLE_USER/LOCALFORWARDER_DIR dizindeki dosyaları.

### <a name="mac"></a>Mac
Yerel ileticisi macOS ile çalışabilir, ancak bunu şu anda resmi olarak desteklenmez.

### <a name="self-hosting"></a>Kendi kendine barındırma
Yerel ileticisi de kendi içindeki .NET uygulama barındırmanıza olanak sağlayan bir standart .NET NuGet paketi olarak dağıtılır.

```C#
using Library;
...
Host host = new Host();

// see section below on configuring local forwarder
string configuration = ...;
    
host.Run(config, TimeSpan.FromSeconds(5));
...
host.Stop();
```

## <a name="configuring-local-forwarder"></a>Yerel ileticisi yapılandırma

* (Konsol konağına ya da Windows Hizmet Konağı) yerel ileticinin kendi konaklardan birine çalıştırırken bulursunuz **LocalForwarder.config** yanındaki ikili yerleştirilir.
* Yerel ileticisi NuGet kendi kendine barındırma, aynı biçimde yapılandırmasını kodda sağlanmalıdır (kendi kendine barındırma bölümüne bakın). Yapılandırma sözdizimi için denetleme [LocalForwarder.config](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/blob/master/src/ConsoleHost/LocalForwarder.config) GitHub deposunda. 

> [!NOTE]
> Yapılandırma yayın sürümü farklı, böylece hangi sürümün kullanmakta olduğunuz dikkat edin.

## <a name="monitoring-local-forwarder"></a>Yerel ileticisi izleme

İzlemeleri yazılır yanındaki yerel ileticisi çalışan yürütülebilir dosya sistemi (Ara **.log* dosyaları). Adıyla bir dosya yerleştirebilirsiniz *NLog.config* yanındaki varsayılanın yerine kendi yapılandırmasını sağlamak için çalıştırılabilir. Bkz: [belgeleri](https://github.com/NLog/NLog/wiki/Configuration-file#configuration-file-format) biçimi açıklaması.

Herhangi bir yapılandırma dosyası (varsayılan değer olan) sağlanıyorsa, yerel ileticisi bulunabilir varsayılan yapılandırmayı kullanacağı [burada](https://github.com/Microsoft/ApplicationInsights-LocalForwarder/blob/master/src/Common/NLog.config).

## <a name="next-steps"></a>Sonraki adımlar

* [Açık sayım](https://opencensus.io/)
