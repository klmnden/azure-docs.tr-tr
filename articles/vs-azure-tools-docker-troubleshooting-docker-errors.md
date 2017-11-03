---
title: "Visual Studio kullanarak Windows Docker istemci hatalarında sorun giderme | Microsoft Docs"
description: "Visual Studio oluşturmak ve Visual Studio kullanarak web uygulamaları Docker Windows dağıtmak için kullanırken karşılaştığınız sorunları giderin."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 89fa04a1107b6abb49aefd68066443717ac9b731
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a>Visual Studio Docker geliştirme sorun giderme

Docker Önizleme için Visual Studio Araçları ile çalışırken, Önizleme yapısı nedeniyle bazı sorunlarla karşılaşabilirsiniz.
Bazı yaygın sorunlar ve çözümleri aşağıda verilmiştir.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Linux kapsayıcıları**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Derleme hataları oluşur .NET Core web veya konsol uygulama hata ayıklama sırasında  

Bu proje bulunduğu sürücü Docker ile Windows için paylaşmıyor için ilgili olabilir.  Aşağıdaki gibi bir hata iletisini alabilirsiniz:

```
The "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with the default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
Bu sorunu gidermek için:

1. Sağ **Windows için Docker** bildirim alanına ve ardından **ayarları**.  
2. Seçin **paylaşılan sürücüleri** ve projeyi bulunduğu sürücü paylaşın.

### <a name="windows-containers"></a>**Windows kapsayıcıları**

Aşağıdaki sorunlar, .NET Framework web ve konsol uygulamaları Windows kapsayıcılarında hata ayıklama için özeldir.

#### <a name="prerequisites"></a>Ön koşullar

1. Visual Studio 2017 RC (veya üstü) .NET Core ve Docker Önizleme ile iş yükü yüklenmesi gerekir.
2. Windows 10 Anniversary Update en son Windows Update ile düzeltme ekleri. Özellikle [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) yüklü olması gerekir. 
3. [Windows için docker](https://docs.docker.com/docker-for-windows/) (1.13.0 yapı veya sonraki bir sürümü) yüklü olmalıdır.
4. **Geçiş Windows kapsayıcılara** seçilmelidir. Bildirim alanında tıklatın **Windows için Docker**ve ardından **geçiş Windows kapsayıcılara**. Makine yeniden başlatıldıktan sonra bu ayar korunur emin olun.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Konsol çıktısı Visual Studio'nun çıktı penceresinde bir konsol uygulaması hata ayıklama sırasında görünmüyor

Bu, şu anda bu senaryo için tasarlanmamıştır Visual Studio hata ayıklayıcısı ile (msvsmon.exe) bilinen bir sorundur. Bu senaryo için destek gelecekteki bir sürümde dahil. Visual Studio'da konsol uygulamasından çıkışın görmek için **Docker: başlangıç projesi**, eşdeğer olduğu **Başlat hata ayıklama olmadan**.

#### <a name="debugging-web-applications-with-the-release-configuration-fails-with-403-forbidden-error"></a>Web uygulamalarında hata ayıklama (403) Yasak hata ile yayın yapılandırma başarısız oluyor

Bu sorunu çözmek için çözüm ve yorum çıkışı web.release.config açın veya aşağıdaki satırları sil:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Linux kapsayıcıları**

#### <a name="unable-to-validate-volume-mapping"></a>Birimi eşlemenin doğrulanamıyor
Birimi eşlemenin kapsayıcı uygulama klasöründe kaynak kodu ve uygulamanızın ikili dosyaları paylaşmak için gereklidir.  Özel birim eşlemeleri docker compose.dev.debug.yml ve docker compose.dev.release.yml içinde yer alır. Ana makinede değişen dosyaları gibi kapsayıcılara benzer bir klasörü yapısı içinde bu değişiklikleri yansıtır.

Birimi eşlemenin etkinleştirmek için:

1. Tıklatın **Moby** bildirim alanı seçip **ayarları**.
2. Seçin **sürücüler paylaşılan**.
3. Projenizi barındıran sürücü ve % USERPROFILE % bulunduğu sürücü seçin.
4. **Uygula**'ya tıklayın.

Birimi eşlemenin çalışıyorsa test etmek için yeniden oluşturun ve bir veya daha fazla sürücü paylaşılan, veya silinmiş bir komut isteminden aşağıdaki kodu çalıştırın sonra gelen Visual Studio içinde F5 seçin.

> [!NOTE]
> Bu örnekte, kullanıcıların klasörünüze C sürücüsünde bulunur ve paylaşılan sahip varsayılır.
> Farklı bir sürücü paylaştırılmışsa gerektiği şekilde düzeltin.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Aşağıdaki kod Linux kapsayıcısında çalıştırın.

```
/ # ls
```

Bir dizin kullanıcıların/ortak klasöründen listesi görmeniz gerekir. Hiçbir dosya görüntülenir ve /c/Users/Public klasörünüze boş yoksa, birimi eşlemenin düzgün yapılandırılmamış.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Değiştirme içeriğini görmek için deliği dizinine `/c/Users/Public` dizini:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> Linux VM'ler ile çalışırken, kapsayıcı dosya sistemi büyük küçük harfe duyarlı değil.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>Derleme: "PrepareForBuild" görevi beklenmedik şekilde başarısız oldu

Microsoft.DotNet.Docker.CommandLine.ClientException: Bağlanmaya çalışırken bir hata oluştu.

Varsayılan Docker ana çalıştığını doğrulayın. Bir komut istemi açın ve yürütün:

```
docker info
```

Bu hata verirse, daha sonra başlatmayı deneyebilirsiniz **Windows için Docker** masaüstü uygulaması. Masaüstü uygulaması, ardından çalışıyorsa **Moby** bildirim alanında görünür olmalıdır. Sağ **Moby** açarak **ayarları**. Tıklatın **sıfırlama**ve Docker yeniden başlatın.

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Docker desteği eklemek veya (F5) bir kapsayıcı ASP.NET Core uygulamada hata ayıklama çalışılırken bir hata iletişim kutusu oluşur

Kaldırma ve uzantılarını yüklemeden sonra Visual Studio Yönetilen Genişletilebilirlik Çerçevesi (MEF) önbellekte bozulabilir. Bu durumda, Docker desteği ekleme olduğunda ve/veya çalıştırmak veya (F5) ASP.NET Core uygulamanızda hata ayıklama girişimi çeşitli hata iletileri neden olabilir. Geçici bir çözüm olarak, silin ve MEF önbelleği yeniden oluşturmak için aşağıdaki adımları kullanın.

1. Visual Studio tüm örneklerini kapatın.
1. % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\ açın.
1. Aşağıdaki klasörleri silin:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Visual Studio'yu açın.
1. Senaryo yeniden deneyin.
