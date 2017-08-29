---
title: "Linux üzerinde geliştirme ortamınızı ayarlama | Microsoft Docs"
description: "Linux üzerinde çalışma zamanını ve SDK'yı yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra uygulama derlemek için hazır hale gelirsiniz."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/23/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 4f51030446d2d2a5a11018b1fce7d7e9193f3dfc
ms.contentlocale: tr-tr
ms.lasthandoff: 08/24/2017

---
# <a name="prepare-your-development-environment-on-linux"></a>Linux üzerinde geliştirme ortamınızı hazırlama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Linux geliştirme makinenizde [Azure Service Fabric uygulamaları](service-fabric-application-model.md) dağıtıp çalıştırmak için çalışma zamanını ve ortak SDK'yı yükleyin. Ayrıca isteğe bağlı Java ve .NET Core SDK’larını yükleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Geliştirme için şu işletim sistemi sürümleri desteklenir:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="update-your-apt-sources"></a>APT kaynaklarınızı güncelleştirme
SDK ve ilişkili çalışma zamanı paketini apt-get komut satırı aracıyla yüklemek için, öncelikle Advanced Packaging Tool (APT) kaynaklarınızı güncelleştirmelisiniz.

1. Bir terminal açın.
2. Service Fabric deponuzu kaynaklar listenize ekleyin.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. `dotnet` deposunu kaynak listenize ekleyin.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Yeni Gnu Privacy Guard (GnuPG, veya GPG) anahtarını APT anahtarlığınıza ekleyin.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Resmi Docker GPG anahtarını APT anahtarlığınıza ekleyin.

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Docker deposunu ayarlayın.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Paket listelerinizi yeni eklenen depolara göre yenileyin.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk-for-local-cluster-setup"></a>Yerel küme kurulumu için SDK'yı yükleme ve ayarlama

Kaynaklarınızı güncelleştirdikten sonra SDK’yı yükleyebilirsiniz. Service Fabric SDK paketini yükleyin, yüklemeyi onaylayın ve lisans sözleşmesini kabul edin.

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   Aşağıdaki komutlar, Service Fabric paketlerine yönelik lisansı kabul etme işlemini otomatik hale getirir:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a>Yerel küme oluşturma
  Yükleme başarılı olduysa, yerel bir kümeyi başlatabilmeniz gerekir.

  1. Küme kurulum betiğini çalıştırın.

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. Bir web tarayıcısı açın ve [Service Fabric Explorer](http://localhost:19080/Explorer) adresine gidin. Küme başlatıldıysa Service Fabric Explorer panosunu görmeniz gerekir.

      ![Linux üzerinde Service Fabric Explorer][sfx-linux]

  Bu noktada, önceden oluşturulmuş Service Fabric uygulama paketlerini ve yeni paketleri konuk kapsayıcılar veya konuk yürütülebilir öğelere göre dağıtabilirsiniz. Java veya .NET Core SDK’larını kullanarak yeni hizmetler oluşturmak için aşağıdaki bölümlerde yer alan kurulum adımlarını izleyin.


  > [!NOTE]
  > Tek başına kümeler Linux’da desteklenmez. Önizleme yalnızca tek kutu ve Azure Linux çok makineli kümeleri destekler.
  >

## <a name="set-up-the-service-fabric-cli"></a>Service Fabric CLI’sını ayarlama

[Service Fabric CLI](service-fabric-cli.md) kümeler ve uygulamalar da dahil olmak üzere Service Fabric varlıklarıyla etkileşime yönelik komutlar içerir. Python tabanlı olduğundan aşağıdaki komutu kullanmadan önce python ve pip’in yüklü olduğundan emin olun:

```bash
pip install sfctl
```

## <a name="install-and-set-up-the-generators-for-containers-and-guest-executables"></a>Kapsayıcılar ve konuk yürütülebilir dosyalar için oluşturucuları yükleme ve ayarlama
Service Fabric, Yeoman şablon oluşturucu kullanarak terminalden Service Fabric uygulamaları oluşturmanıza yardımcı olacak yapı iskelesi araçları sağlar. Lütfen makinenizde çalışan bir Service Fabric yeoman şablon oluşturucu olduğundan emin olmak için aşağıdaki adımları izleyin.

1. Makinenize nodejs ve NPM yükleme

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. NPM’den makinenize [Yeoman](http://yeoman.io/) şablon oluşturucuyu yükleme

  ```bash
  sudo npm install -g yo
  ```
3. NPM’den Service Fabric Yeo kapsayıcı oluşturucusunu ve konuk yürütülebilir dosya oluşturucusunu yükleme

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

Yukarıdaki oluşturucuları yükledikten sonra, sırasıyla `yo azuresfguest` ve `yo azuresfcontainer` komutlarını çalıştırarak konuk yürütülebilir dosyası ya da kapsayıcı hizmetleriyle uygulama oluşturabilmeniz gerekir.

## <a name="install-the-necessary-java-artifacts-optional-if-you-want-to-use-the-java-programming-models"></a>Gereken Java yapıtlarını yükleme (Java programlama modellerini kullanmak istiyorsanız, isteğe bağlı)

Java kullanarak Service Fabric hizmetleri oluşturmak için, derleme görevlerini çalıştırırken kullanılan Gradle ile birlikte JDK 1.8’in de yüklü olduğundan emin olun. Aşağıdaki kod parçacığı Open JDK 1.8’i ve Gradle’ı birlikte yükler. Service Fabric Java kitaplıkları Maven’dan alınır.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a>Eclipse Neon eklentisini yükleme (isteğe bağlı)

Service Fabric için Eclipse eklentisini **Java Geliştiricileri için Eclipse IDE** içinden yükleyebilirsiniz. Eclipse kullanarak, Service Fabric Java uygulamalarına ek olarak Service Fabric konuk yürütülebilir uygulamaları ve kapsayıcı uygulamaları oluşturabilirsiniz.

1. Eclipse’te, en son Eclipse Neon ve en son Buildship sürümünün (1.0.17 veya üstü) yüklü olduğundan emin olun. **Yardım** > **Yükleme Ayrıntıları**’nı seçerek yüklü bileşenlerin sürümlerini denetleyebilirsiniz. [Eclipse Buildship: Gradle için Eclipse eklentileri][buildship-update] bölümünde sağlanan yönergelerden yararlanarak Buildship’i güncelleştirebilirsiniz.

2. Service Fabric eklentisini yüklemek için **Yardım** > **Yeni Yazılım Yükle**’yi seçin.

3. **Birlikte çalışın** kutusuna şunu yazın: **http://dl.microsoft.com/eclipse**.

4. **Ekle**'ye tıklayın.

    ![Kullanılabilir Yazılım sayfası][sf-eclipse-plugin]

5. **ServiceFabric** eklentisini seçip **İleri**’ye tıklayın.

6. Yükleme adımlarını tamamlayın ve ardından son kullanıcı lisans sözleşmesini kabul edin.

Service Fabric Eclipse eklentisi zaten yüklüyse, en yeni sürümü kullandığınızdan emin olun. **Yardım** > **Yükleme Ayrıntıları**’nı seçip ardından yüklü eklentiler listesinde Service Fabric araması yaparak kontrol edebilirsiniz. Daha yeni bir sürüm varsa, **Güncelleştir**’i seçin.

Daha fazla bilgi için bkz. [Eclipse Java uygulama geliştirmesi için Service Fabric eklentisi](service-fabric-get-started-eclipse.md).


## <a name="install-the-net-core-sdk-optional-if-you-want-to-use-the-net-core-programming-models"></a>.NET Core SDK’sını yükleme (.NET Core programlama modellerini kullanmak istiyorsanız, isteğe bağlıdır)
.NET Core SDK’sı, .NET Core ile Service Fabric hizmetleri oluşturmak için gereken kitaplıkları ve şablonları sağlar. .NET Core SDK paketini aşağıdakini çalıştırarak yükleyebilirsiniz:

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-the-sdk-and-runtime"></a>SDK ve çalışma zamanını güncelleştirme

SDK ve çalışma zamanının son sürümüne güncelleştirmek için aşağıdaki komutları çalıştırın. (Tercih etmediğiniz SDK'ların seçimini kaldırın.):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
Maven’dan alınan Java SDK'sı ikili dosyalarını güncelleştirmek için ``build.gradle`` dosyasında karşılık gelen ikili sürüm ayrıntılarını en son sürüme işaret edecek şekilde güncelleştirmeniz gerekir. Sürümü tam olarak nerede güncelleştirmeniz gerektiğini öğrenmek için [buradaki](https://github.com/Azure-Samples/service-fabric-java-getting-started) Service Fabric başlangıç örneklerindeki herhangi bir ``build.gradle`` dosyasına bakabilirsiniz.

> [!NOTE]
> Paketlerin güncelleştirilmesi, yerel geliştirme kümenizin çalışmayı durdurmasına neden olabilir. Yükseltme sonrasında bu sayfadaki yönergeleri uygulayarak yerel kümenizi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux üzerinde Yeoman kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-create-your-first-linux-application-with-java.md)
* [Linux üzerinde Eclipse için Service Fabric Eklentisi kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md)
* [Linux üzerinde ilk CSharp uygulamanızı oluşturma](service-fabric-create-your-first-linux-application-with-csharp.md)
* [OSX üzerinde geliştirme ortamınızı hazırlama](service-fabric-get-started-mac.md)
* [Uygulamalarınızı yönetmek için Service Fabric CLI'yı kullanma](service-fabric-application-lifecycle-sfctl.md)
* [Service Fabric Windows/Linux farkları](service-fabric-linux-windows-differences.md)
* [Service Fabric CLI kullanmaya başlama](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

