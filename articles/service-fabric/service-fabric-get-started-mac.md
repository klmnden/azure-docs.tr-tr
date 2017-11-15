---
title: "Azure Service Fabric ile çalışmak için Mac OS X’te geliştirme ortamınızı ayarlama | Microsoft Docs"
description: "Çalışma zamanını, SDK'yı ve araçları yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra Mac OS X üzerinde uygulama derlemek için hazır hale gelirsiniz."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: saysa
ms.openlocfilehash: cf67c377cd5c17f7b540fb23af48a333a9cddf69
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Mac OS X’te geliştirme ortamınızı ayarlama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Mac OS X kullanarak Linux kümelerinde çalışacak Service Fabric uygulamaları derleyebilirsiniz. Bu makalede Mac’inizi geliştirme için nasıl ayarlayacağınız ele alınmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Service Fabric, OS X üzerinde yerel olarak çalışmaz. Yerel bir Service Fabric kümesini çalıştırmak için önceden yapılandırılmış bir Docker kapsayıcı görüntüsü sağlıyoruz. Başlamadan önce şunlar gereklidir:

* En az 4 GB RAM
* [Docker](https://www.docker.com/)'ın en son sürümü
* Service Fabric One-box Docker kapsayıcı [görüntüsüne](https://hub.docker.com/r/servicefabricoss/service-fabric-onebox/) erişim

>[!TIP]
> * Docker’ı Mac bilgisayarınıza yüklemek için resmi Docker [belgelerinde](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install) anlatılan adımları izleyebilirsiniz. 
> * Yüklemeyi tamamladıktan sonra, [burada](https://docs.docker.com/docker-for-mac/#check-versions-of-docker-engine-compose-and-machine) anlatılan adımları izleyerek yüklemeyi doğru yaptığınızı onaylayın


## <a name="create-a-local-container-and-setup-service-fabric"></a>Yerel bir kapsayıcı oluşturma ve Service Fabric’i ayarlama
Yerel bir Docker kapsayıcısı ayarlamak ve üzerinde bir service fabric kümesi çalıştırmak aşağıdaki adımları uygulayın:

1. Docker merkez deposundan görüntü çekme:

    ```bash
    docker pull servicefabricoss/service-fabric-onebox
    ```

2. Görüntü ile bir Service Fabric One-box kapsayıcı örneği başlatın:

    ```bash
    docker run -itd -p 19080:19080 --name sfonebox servicefabricoss/service-fabric-onebox
    ```
    >[!TIP]
    >Kapsayıcı örneğinize için bir ad belirterek, örneğinizi daha okunaklı bir biçimde işleyebilirsiniz. 

3. Etkileşimli ssh modunda Docker kapsayıcısı oturumunu açın:

    ```bash
    docker exec -it sfonebox bash
    ```

4. Gerekli bağımlılıkları getirecek olan ayar betiğini çalıştırın ve ondan sonra kümeyi kapsayıcı üzerinde başlatın.

    ```bash
    ./setup.sh     # Fetches and installs the dependencies required for Service Fabric to run
    ./run.sh       # Starts the local cluster
    ```

5. Adım 4 başarıyla tamamlandıktan sonra Mac bilgisayarınızdan ``http://localhoist:19080`` bölümüne giderek Service Fabric gezginini görebilirsiniz.


## <a name="set-up-the-service-fabric-cli-sfctl-on-your-mac"></a>Mac'inizde Service Fabric CLI'sını (sfctl) ayarlama

Service Fabric CLI'sını (`sfctl`) Mac'inize yüklemek için [Service Fabric CLI'sı](service-fabric-cli.md#cli-mac) talimatlarını izleyin.
CLI kümeler, uygulamalar ve hizmetler de dahil olmak üzere Service Fabric varlıklarıyla etkileşime yönelik komutlar içerir.

## <a name="create-application-on-your-mac-using-yeoman"></a>Yeoman kullanarak Mac'inizde uygulama oluşturma

Service Fabric, Yeoman şablon oluşturucu kullanarak terminalden Service Fabric uygulaması oluşturmanıza yardımcı olan yapı iskelesi araçları sağlar. Makinenizde çalışan bir Service Fabric yeoman şablon oluşturucu olduğundan emin olmak için aşağıdaki adımları izleyin.

1. Mac’inizde Node.js ve NPM yüklü olması gerekir. Yüklü değilse, Node.js ve NPM’yi Homebrew kullanarak aşağıdaki adımlarla yükleyebilirsiniz. Mac'inizde yüklü Node.js ve NPM sürümlerini denetlemek için ``-v`` seçeneğini kullanabilirsiniz.

    ```bash
    brew install node
    node -v
    npm -v
    ```
2. NPM’den makinenize [Yeoman](http://yeoman.io/) şablon oluşturucuyu yükleyin.

    ```bash
    npm install -g yo
    ```
3. Başlarken [belgelerinde](service-fabric-get-started-linux.md) bulunan adımları izleyerek kullanmak istediğiniz Yeoman oluşturucuyu yükleyin. Yeoman kullanarak Service Fabric uygulamaları oluşturmak için adımları takip edin:

    ```bash
    npm install -g generator-azuresfjava       # for Service Fabric Java Applications
    npm install -g generator-azuresfguest      # for Service Fabric Guest executables
    npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
    ```
4. Mac üzerinde bir Service Fabric Java uygulaması derlemek için ana makinenizde JDK 1.8 ve Gradle yüklü olmalıdır. Henüz yoksa, [HomeBrew](https://brew.sh/) kullanarak yükleyebilirsiniz. 

    ```bash
    brew update
    brew cask install java
    brew install gradle
    ```

## <a name="deploy-application-on-your-mac-from-terminal"></a>Terminalden Mac’inize uygulama dağıtma

Service Fabric uygulamanızı oluşturup derledikten sonra aşağıdaki adımlarla [Service Fabric CLI](service-fabric-cli.md#cli-mac)’yi kullanarak uygulamanızı dağıtabilirsiniz:

1. Mac’inizdeki kapsayıcı örneğinin içinde çalışan Service Fabric kümesine bağlanın.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Proje dizininize gidip yükleme betiğini çalıştırın.

    ```bash
    cd MyProject
    bash install.sh
    ```

## <a name="set-up-net-core-20-development"></a>.NET Core 2.0 ile geliştirmeyi ayarlama

[C# Service Fabric uygulamaları oluşturmaya](service-fabric-create-your-first-linux-application-with-csharp.md) başlamak amacıyla [Mac için .NET Core 2.0 SDK'sını](https://www.microsoft.com/net/core#macos) yükleyin. .NET Core 2.0 Service Fabric uygulamaları paketleri NuGet.org üzerinde barındırılmaktadır ve şu anda önizleme sürümündedir.

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-on-your-mac"></a>Mac’inizde Eclipse Neon için Service Fabric eklentisini yükleme

Service Fabric, **Java IDE için Eclipse Neon** için Java hizmetlerini oluşturma, derleme ve dağıtmayı kolaylaştırabilen bir eklenti sağlar. Service Fabric Eclipse eklentisini yükleme veya en son sürüme güncelleştirme hakkındaki bu genel [belgede](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) bahsedilen yükleme adımlarını izleyebilirsiniz.

Uygulama derleme, uygulamaya hizmet ekleme, uygulama yükleme/kaldırma için [Service Fabric Eclipse belgelerinde](service-fabric-get-started-eclipse.md) anlatılan diğer tüm adımlar burada da geçerlidir.

Service Fabric Eclipse eklentisinin Mac’inizde Docker kapsayıcısı ile birlikte çalışması için, yukarıdaki adımlar dışında konağınızla paylaşılan bir yol ile aşağıdaki gibi kapsayıcının bir örneğini oluşturmanız gerekir:
```bash
docker run -itd -p 19080:19080 -v /Users/sayantan/work/workspaces/mySFWorkspace:/tmp/mySFWorkspace --name sfonebox servicefabricoss/service-fabric-onebox
```
Burada ``/Users/sayantan/work/workspaces/mySFWorkspace``, Mac üzerindeki çalışma alanının tam yolu ve ``/tmp/mySFWorkspace``, kapsayıcı içinde kapsayıcının eşleneceği yoldur.

> [!NOTE]
>1. Çalışma alanınızın adı/yolu farklı ise, yukarıdaki ``docker run`` komutunda bu değerleri güncelleştirin.
>2. Kapsayıcıyı ``sfonebox`` dışında bir adla başlatırsanız, lütfen Service Fabric aktör Java uygulamasındaki ``testclient.sh`` dosyasında bu değerleri güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar
<!-- Links -->
* [Linux üzerinde Yeoman kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-create-your-first-linux-application-with-java.md)
* [Linux üzerinde Eclipse için Service Fabric Eklentisi kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md)
* [Azure portalında bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Azure Resource Manager’ı kullanarak bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-arm.md)
* [Service Fabric uygulama modelini anlama](service-fabric-application-model.md)
* [Uygulamalarınızı yönetmek için Service Fabric CLI'yı kullanma](service-fabric-application-lifecycle-sfctl.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
