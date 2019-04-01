---
title: Azure IOT OPC UA sertifika yönetimi modülü sıfırdan dağıtma | Microsoft Docs
description: Sıfırdan OPC kasası dağıtma
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: a3a9d21b70f16482f05d27aa0df8d8865459aeb4
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759628"
---
# <a name="deploy-opc-vault-from-scratch"></a>Sıfırdan OPC kasası dağıtma

Azure IOT OPC UA sertifika yönetimi, ayrıca OPC kasası olarak yapılandırabilirsiniz, bir mikro hizmet kaydı, bildiğiniz ve sertifika yaşam döngüsü için OPC UA sunucu ve istemci uygulamalarını bulutta yönetin. Bu makalede sıfırdan OPC kasası dağıtma gösterilmektedir.

## <a name="configuration-and-environment-variables"></a>Yapılandırma ve ortam değişkenleri

Hizmet yapılandırması appsettings.ini içinde ASP.NET Core yapılandırma bağdaştırıcıları kullanılarak depolanır. Açıklamaları olan okunabilir bir biçimde değerleri depolamak için INI biçimi sağlar.
Uygulama, kimlik bilgileri ve ağ ayrıntıları gibi ekleme ortam değişkenlerini de destekler. (Bu bölümdeki ilk TODO yapılandırma ve ortam değişkenleri olarak adlandırılmıştır).

Depo yapılandırma dosyasında oluşturulan en az bir kez gereken bazı ortam değişkenlerini başvuruyor. İşletim sisteminiz ve IDE bağlı olarak, ortam değişkenleri yönetmek için birkaç yol vardır:

- Windows kullanıcıları için hazırlanması ve yalnızca bir kez yürütülen env değişkenleri setup.cmd betik gerekir. Yürütüldüğünde, ayarları terminal oturumları ve yeniden başlatmalar arasında açık kalır.

- Ortamlar, Linux ve OSX için yeni bir konsolu her açıldığında yürütülecek env değişkenleri Kurulum betiğini gerekir. İşletim sistemi ve terminal bağlı olarak, değerleri küresel olarak daha fazla bilgi için bu sayfa yardımcı kalıcı hale getirmek için yolu vardır:

  - https://stackoverflow.com/questions/13046624/how-to-permanently-export-a-variable-in-linux
  
  - https://stackoverflow.com/questions/135688/setting-environment-variables-in-os-x
  
  - https://help.ubuntu.com/community/EnvironmentVariables
  

- Visual Studio: Ortam değişkenleri, Proje Özellikleri'nin altında Visual Studio'dan ayarlanabilir, sol bölmede "Yapılandırma özellikleri" ve "Ortam", birden fazla değişken ekleyebileceğiniz bir bölüme almak için seçin.

- IntelliJ Rider: Ortam değişkenlerini her çalıştırma yapılandırmasında, Intellij Idea için benzer şekilde ayarlanabilir. https://www.jetbrains.com/help/idea/run-debug-configuration-application.html

## <a name="run-and-debug-with-visual-studio"></a>Çalıştırın ve Visual Studio ile hata ayıklama

Visual Studio IDE dışında herhangi bir şey yapılandırmadan bir komut istemi kullanarak olmadan uygulamayı hızla açmanızı sağlar.

Visual Studio 2017 kullanılarak adımlar:

1. Kullanarak çözümü açın `iot-opc-gds-service.sln` dosya.

1. Çözüm yüklendiğinde sağ `WebService` proje seçin ve Git `Debug` bölümü.

1. Aynı bölümde gerekli ortam değişkenleri tanımlayın.

1. Tuşuna **F5**, veya **çalıştırma** simgesi. Visual Studio JSON biçiminde hizmet durumunu gösteren tarayıcınızı açmanız gerekir.

## <a name="run-and-debug-with-intellij-rider"></a>Çalıştırın ve Intellij Rider ile hata ayıklama

1. Kullanarak çözümü açın `iot-opc-gds-service.sln` dosya.

1. Çözüm yüklendiğinde Git `Run > Edit Configurations` ve yeni bir `.NET Project` yapılandırma.

1. Yapılandırmada WebService projeyi seçin.

1. Ayarları kaydetmek ve IDE araç çubuğundan oluşturulan yapılandırmayı çalıştırın.

1. URL gibi ayrıntılarla servisin yanı sıra web hizmetinin çalıştığı Intellij Çalıştır penceresine iletilerinde önyükleme hizmeti görmelisiniz günlükleri.

## <a name="build-and-run-from-the-command-line"></a>Derleme ve komut satırından çalıştırma

Scripts klasörü, sık kullanılan görevleri için bazı kodlar içerir:

- `build`: Tüm projeleri derlemek ve testleri çalıştırın.

- `compile`: Tüm projeleri derleyin.

- `run`: Projeleri derlemek ve web hizmetini çalıştırmak için yükseltilmiş ayrıcalıklar Windows ister hizmet çalıştırın.

Ortam değişkenleri kurulumu için komut dosyalarını denetleyin. Ortam değişkenlerini genel olarak, işletim sisteminde ayarlamak veya betikler klasörüne "env-var-setup" komut dosyasını kullanın.

### <a name="sandbox"></a>Korumalı Alan

Betikler, .NET Core ve Docker geliştirme ortamınızı yapılandırılmış varsayılır. .NET Core yüklemekten kaçının ve yalnızca Docker'ı yükleyebilir ve komut satırı parametresini kullanın `--in-sandbox` (veya kısa formunu `-s`), örneğin:

- `build --in-sandbox`: Bir Docker kapsayıcısı içinde derleme görevi yürüten (kısa form `build -s`).

- `compile --in-sandbox`: Bir Docker kapsayıcısı içinde derleme görevi yürüten (kısa form `compile -s`).

- `run --in-sandbox`: Bir Docker kapsayıcısı içinde hizmetini başlatır (kısa form `run -s`).

Korumalı alan için kullanılan Docker görüntüleri Docker Hub üzerinde barındırılan [burada](https://hub.docker.com/r/azureiotpcs/code-builder-dotnet).

## <a name="package-the-application-to-a-docker-image"></a>Docker görüntüsüne Uygulama paketleme

`scripts` Klasörü, hizmeti bir Docker görüntüsü halinde paketlemek için gerekli dosyaları içeren bir docker alt klasör içerir:

- `Dockerfile`: Docker görüntüleri belirtimleri.
- `build`: Bir Docker kapsayıcısı oluşturmak ve görüntüyü yerel kayıt defterinde saklayın.
- `run`: Yerel kayıt defterinde depolanan görüntüden Docker kapsayıcısını çalıştırın.
- `content`: Giriş noktası betiğini de dahil olmak üzere, bir görüntüye kopyalanan dosya içeren bir klasör.

## <a name="azure-iot-hub-setup"></a>Azure IOT hub'ı Kurulumu

Mikro hizmet kullanmak için Azure IOT Hub'ınız için geliştirme ve tümleştirme testleri ayarlayın.

Proje, bu kurulum ile yardımcı olmak için bazı Bash betiklerini içerir:

- Yeni IOT hub'ı oluşturun: `./scripts/iothub/create-hub.sh`

- Mevcut hub'ları listele: `./scripts/iothub/list-hubs.sh`

- IOT hub'ı ayrıntılarını (örneğin, anahtarları) görüntüleyin: `./scripts/iothub/show-hub.sh`

Ve birden çok Azure aboneliğiniz olduğu durumda:

- Aboneliklerin listesini göster: `./scripts/iothub/list-subscriptions.sh`

- Geçerli abonelik değiştirin: `./scripts/iothub/select-subscription.sh`

## <a name="development-setup"></a>Geliştirme Kurulumu

### <a name="net-setup"></a>.NET Kurulumu

Proje iş akışı aracılığıyla yönetilen [.NET Core](https://dotnet.github.io) 1.x, böylece tüm betikleri çalıştırmasına ve IDE'nizi beklendiği gibi çalıştığından emin olun, ortamınızda yüklemeniz gerekir.

Ayrıca, bildirimde bir [Java sürümü](https://github.com/Azure/iot-opc-gds-service-dotnet) bu projenin ve diğer Azure IOT bilgisayarları bileşenleri.

### <a name="ide"></a>IDE

Azure IOT Bilgisayarlarında çalışmak için kullanabileceğiniz IDE'ler bazıları şunlardır:

- [Visual Studio](https://www.visualstudio.com)
- [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac)
- [Intellij Rider](https://www.jetbrains.com/rider)
- [Visual Studio Code](https://code.visualstudio.com)

### <a name="git-setup"></a>Git Kurulumu

Proje bir kod değişikliği kabul etmeden önce bazı denetimler otomatikleştirmek için bir Git kancası içerir. El ile testler veya testleri çalıştırmak için CI platformu sağlar. Otomatik olarak kod değişiklikleri Github'a göndermeden önce tüm testleri çalıştırmak ve geliştirme iş akışı hızlandırmak için aşağıdaki Git kancası kullanırız.

Herhangi bir noktada kanca kaldırmak istiyorsanız, yalnızca altında yüklü dosyayı silin `.git/hooks`. Ön işleme kanca kullanarak atlayabilir `--no-verify` seçeneği.

#### <a name="pre-commit-hook-with-sandbox"></a>Ön işleme kanca korumalı alanı ile

Dahil edilen bağlar ayarlamak için bir Windows/Linux/MacOS konsolu açın ve yürütün:

```
cd PROJECT-FOLDER
cd scripts/git
setup --with-sandbox
```

Bu yapılandırmayla, dosyalar denetlenirken Git uygulama derleme ve tüm geliştirme gereksinimleri ile yapılandırılmış bir Docker kapsayıcısı içinde testleri çalıştıran tüm testlerini geçtiğini doğrular.

#### <a name="pre-commit-hook-without-sandbox"></a>Ön işleme kanca korumalı alanı olmadan

> [!NOTE] 
> Korumalı alan olmadan kanca gerektirir [.NET Core](https://dotnet.github.io) sistem yolu.

Dahil edilen bağlar ayarlamak için bir Windows/Linux/MacOS konsolu açın ve yürütün:

```
cd PROJECT-FOLDER
cd scripts/git
setup --no-sandbox
```
Bu yapılandırmayla, dosyalar denetlenirken Git uygulama derleme ve testleri çalıştırmak, işletim sisteminde yüklü araçları kullanarak iş istasyonunuzu tüm testlerini geçtiğini doğrular.

Proje kod Stili Kılavuzu:

- Uygun yerlerde, kod incelemeleri ve komut satırı düzenleyicileri yardımcı olmak için 80 karakter için en fazla satır uzunluğu sınırlıdır.

- Kod girintileme dört boşluklu engeller. Sekme karakteri kaçınılmalıdır.

- Metin dosyaları UNIX sonuna satır biçimi (LF) kullanın.

- Bağımlılık ekleme ile yönetilir [Autofac](https://autofac.org).

- Web hizmeti API'leri alanlar, CamelCased dışında meta verilerdir.

## <a name="next-steps"></a>Sonraki adımlar

Sıfırdan OPC kasa dağıtmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC İkizi sıfırdan dağıtma](howto-opc-twin-deploy-modules.md)