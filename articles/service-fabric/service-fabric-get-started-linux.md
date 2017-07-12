---
title: "Linux üzerinde geliştirme ortamınızı ayarlama | Microsoft Belgeleri"
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
ms.date: 6/28/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 5840932d79ef9fd99a94eb6ae4e587b0e616065e
ms.contentlocale: tr-tr
ms.lasthandoff: 07/01/2017


---
<a id="prepare-your-development-environment-on-linux" class="xliff"></a>

# Linux üzerinde geliştirme ortamınızı hazırlama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

 Linux geliştirme makinenizde [Azure Service Fabric uygulamaları](service-fabric-application-model.md) dağıtıp çalıştırmak için çalışma zamanını ve ortak SDK'yı yükleyin. Ayrıca isteğe bağlı Java ve .NET Core SDK’larını yükleyebilirsiniz.

<a id="prerequisites" class="xliff"></a>

## Önkoşullar

<a id="supported-operating-system-versions" class="xliff"></a>

### Desteklenen işletim sistemi sürümleri
Geliştirme için şu işletim sistemi sürümleri desteklenir:

* Ubuntu 16.04 (`Xenial Xerus`)

<a id="update-your-apt-sources" class="xliff"></a>

## Apt kaynaklarınızı güncelleştirme
SDK ve ilişkili çalışma zamanı paketini apt-get ile yüklemek için öncelikli apt kaynaklarınızı güncelleştirmeniz gerekir.

1. Bir terminal açın.
2. Service Fabric deponuzu kaynaklar listenize ekleyin.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. `dotnet` deposunu kaynak listenize ekleyin.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Yeni GPG anahtarınızı apt anahtarlığınıza ekleyin.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Docker'ın resmi GPG anahtarını apt kimlik anahtarlığınıza ekleyin.

    ```bash
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

<a id="install-and-set-up-the-sdk-for-containers-and-guest-executables" class="xliff"></a>

## Kapsayıcılar ve konuk yürütülebilir dosyalar için SDK yükleme ve ayarlama

Kaynaklarınız güncelleştirildikten sonra SDK’yı yükleyebilirsiniz.

1. Service Fabric SDK paketini yükleyin. Yüklemeyi onaylamanız ve bir lisans sözleşmesini kabul etmeniz istenir.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

    >   [!TIP]
    >   Aşağıdaki komutlar, Service Fabric paketlerine yönelik lisansı kabul etme işlemini otomatik hale getirir:
    >   ```bash
    >   echo "servicefabric servicefabric/accepted-eula-v1 select true" | debconf-set-selections
    >   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | debconf-set-selections
    >   ```
    
2. SDK kurulum betiğini çalıştırın.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

Ortak SDK paketini yükleme adımlarını gerçekleştirdikten sonra, `yo azuresfguest` veya `yo azuresfcontainer` çalıştırılarak konuk yürütülebilir dosyası ya da kapsayıcı hizmetleriyle uygulama oluşturulabilir. **$NODE_PATH** ortam değişkeninizi, düğüm modüllerinin bulunduğu konuma ayarlamanız gerekebilir. 


```bash
    export NODE_PATH=$NODE_PATH:$HOME/.node/lib/node_modules 
```

Kök olarak ortam kullanıyorsanız, değişkeni aşağıdaki komutla ayarlamanız gerekebilir:

```bash
    export NODE_PATH=$NODE_PATH:/root/.node/lib/node_modules 
```


> [!TIP]
> Her oturum açma işleminde ortam değişkenini ayarlamak zorunda kalmamanız için ~/.bashrc dosyasına bu komutları eklemeniz gerekebilir.
>

<a id="setup-the-xplat-service-fabric-cli" class="xliff"></a>

## XPlat Service Fabric CLI kurulumu
[XPlat CLI][azure-xplat-cli-github] kümeler ve uygulamalar da dahil olmak üzere Service Fabric varlıklarıyla etkileşime yönelik komutlar içerir. Node.js dosyasını temel aldığı için, aşağıdaki yönergelere geçmeden önce [düğümü yüklediğinizden emin olun][install-node]:

1. Github deposunu geliştirme makinenize kopyalayın.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Kopyalanan depoya geçin ve Düğüm Paket Yöneticisi’ni (npm) kullanarak CLI'nın bağımlılıklarını yükleyin.

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Kopyalanan deponun `bin/azure` klasöründen `/usr/bin/azure` hedefine sembolik bağlantı oluşturun.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Son olarak, Service Fabric komutlarının otomatik tamamlanmasını etkinleştirin.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

<a id="setup-azure-cli-20" class="xliff"></a>

### Azure CLI 2.0 kurulumu

XPlat CLI'ye alternatif olarak, Azure CLI'de artık yeni bir Service Fabric komut modülü bulunuyor.

Azure CLI 2.0'ın yüklenmesi ve Service Fabric komutlarının kullanılması hakkında fazla bilgi için [başlangıç belgelerine](service-fabric-azure-cli-2-0.md) bakın.

<a id="set-up-a-local-cluster" class="xliff"></a>

## Yerel küme oluşturma
Her şey başarıyla yüklendiyse yerel bir kümeyi başlatabilmeniz gerekir.

1. Küme kurulum betiğini çalıştırın.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Bir web tarayıcısı açın ve http://localhost:19080/Explorer adresine gidin. Küme başlatıldıysa Service Fabric Explorer panosunu görmeniz gerekir.

    ![Linux üzerinde Service Fabric Explorer][sfx-linux]

Bu noktada, önceden oluşturulmuş Service Fabric uygulama paketlerini ve yeni paketleri konuk kapsayıcılar veya konuk yürütülebilir öğelere göre dağıtabilirsiniz. Java veya .NET Core SDK’ları kullanarak yeni hizmetler oluşturmak için aşağıdaki bölümlerde yer alan kurulum adımlarını izleyin.


> [!NOTE]
> Tek başına kümeler Linux’ta desteklenmez. Yalnızca tek kutu ve Azure Linux çoklu makine kümeleri önizlemede desteklenir.
>

<a id="install-the-java-sdk-optional-if-you-wish-to-use-the-java-programming-models" class="xliff"></a>

## Java SDK’sını yükleme (Java programlama modellerini kullanıyorsanız isteğe bağlı)
Java SDK’sı, Java kullanan Service Fabric hizmetleri oluşturmak için gereken kitaplıkları ve şablonları sağlar.

1. Java SDK paketini yükleyin.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. SDK kurulum betiğini çalıştırın.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

<a id="install-the-eclipse-neon-plugin-optional" class="xliff"></a>

## Eclipse Neon eklentisini yükleme (isteğe bağlı)

Service Fabric için Eclipse eklentisini **Java Geliştiricileri için Eclipse IDE** içinden yükleyebilirsiniz. Eclipse kullanarak, Service Fabric Java uygulamalarına ek olarak Service Fabric konuk yürütülebilir uygulamaları ve kapsayıcı uygulamaları oluşturabilirsiniz.

> [!NOTE]
> Yalnızca konuk yürütülebilir dosyaları ve kapsayıcı uygulamaları için kullanıyor olsanız bile, Java SDK, Eclipse eklentisinin kullanılması için önkoşuldur.
>

1. Eclipse’te, en yeni Eclipse **Neon** ve en yeni Buildship sürümü (1.0.17) veya sonraki bir sürümün yüklü olduğundan emin olun. **Yardım > Yükleme Ayrıntıları**’nı seçerek yüklü bileşenlerin sürümlerini denetleyebilirsiniz. Buildship'i güncelleştirmek için [buradaki][buildship-update] yönergeleri kullanabilirsiniz.
2. Service Fabric eklentisini yüklemek için **Yardım > Yeni Yazılım Yükle...** öğesini seçin
3. "Birlikte çalışın" metin kutusuna şunu girin: http://dl.microsoft.com/eclipse
4. Ekle'ye tıklayın.

    ![Eclipse eklentisi][sf-eclipse-plugin]

5. Service Fabric eklentisini seçin ve **İleri**'ye tıklayın.
6. Yükleme işlemine devam edin ve son kullanıcı lisans sözleşmesini kabul edin.

Service Fabric Eclipse eklentisi zaten yüklüyse, en yeni sürümü kullandığınızdan emin olun. Yüklü eklentiler listesinde ``Help => Installation Details`` öğesini seçip Service Fabric araması yaparak yükleme durumunu kontrol edebilirsiniz. Daha yeni bir sürüm varsa, güncelleştirmeyi seçin.

Daha fazla bilgi için bkz. [Eclipse'i kullanarak Service Fabric ile çalışmaya başlama](service-fabric-get-started-eclipse.md).


<a id="install-the-net-core-sdk-optional-if-you-wish-to-use-the-net-core-programming-models" class="xliff"></a>

## .NET Core SDK’yı yükleme (.NET Core programlama modellerini kullanıyorsanız, isteğe bağlıdır)
.NET Core SDK'sı, .NET Core'u kullanan Service Fabric hizmetleri oluşturmak için gereken kitaplıkları ve şablonları sağlar.

1. .NET Core SDK paketini yükleyin.

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

2. SDK kurulum betiğini çalıştırın.

   ```bash
   sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
   ```

<a id="updating-the-sdk-and-runtime" class="xliff"></a>

## SDK ve çalışma zamanını güncelleştirme

SDK ve çalışma zamanının son sürümüne güncelleştirmek için aşağıdaki komutları çalıştırın. (Tercih etmediğiniz SDK'ların seçimini kaldırın.):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp servicefabricsdkjava
```


> [!NOTE]
> Paketlerin güncelleştirilmesi, yerel geliştirme kümenizin durdurulmasına neden olabilir. Yükseltme sonrasında bu sayfadaki yönergeleri uygulayarak yerel kümenizi yeniden başlatın.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
* [Linux üzerinde Yeoman kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-create-your-first-linux-application-with-java.md)
* [Linux üzerinde Eclipse için Service Fabric Eklentisi kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md)
* [Linux üzerinde ilk CSharp uygulamanızı oluşturma](service-fabric-create-your-first-linux-application-with-csharp.md)
* [OSX üzerinde geliştirme ortamınızı hazırlama](service-fabric-get-started-mac.md)
* [Service Fabric uygulamalarınızı yönetmek için XPlat CLI'yi kullanma](service-fabric-azure-cli.md)
* [Service Fabric Windows/Linux farkları](service-fabric-linux-windows-differences.md)

<a id="related-articles" class="xliff"></a>

## İlgili makaleler

* [Service Fabric ve Azure CLI 2.0 ile çalışmaya başlama](service-fabric-azure-cli-2-0.md)
* [Service Fabric XPlat CLI ile çalışmaya başlama](service-fabric-azure-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

