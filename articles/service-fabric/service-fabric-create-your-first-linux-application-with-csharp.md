---
title: "C# kullanarak Linux’ta ilk Service Fabric uygulamanızı oluşturun | Microsoft Belgeleri"
description: "C kullanarak Service Fabric uygulaması oluşturma ve dağıtma#"
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2016
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 6d8f489ac053db4898741671df73b6abfabeb0dd
ms.openlocfilehash: e3632d89501c1c25b76e9160f0ad82f2b76327d7


---
# <a name="create-your-first-azure-service-fabric-application"></a>İlk Azure Service Fabric uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric, Linux üzerinde hem .NET Core hem de Java dillerinde hizmet oluşturmaya yönelik SDK’lar sağlar. Bu öğreticide, C# (.NET Core) kullanarak Linux için bir uygulama ve hizmet oluşturmayı öğreneceğiz.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce [Linux geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started-linux.md) emin olun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux one-box ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Uygulama oluşturma
Service Fabric uygulaması bir veya birden çok hizmet içerebilir. Bu hizmetlerin her biri uygulamanın işlevselliğini aktarma konusunda belirli bir role sahiptir. Linux için Service Fabric SDK’sı ilk hizmetinizi oluşturmayı ve daha sonra daha fazlasını eklemenizi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir. Tek bir hizmetle uygulama oluşturmak için Yeoman’ı kullanalım.

1. Bir terminalde iskele oluşturmaya başlamak için aşağıdaki komutu yazın:`yo azuresfcsharp`
2. Uygulamanızı adlandırın.
3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğreticinin amaçları doğrultusunda, Reliable Actor Hizmetini seçiyoruz.
   
   ![C için Service Fabric Yeoman oluşturucusu#][sf-yeoman]

> [!NOTE]
> Seçenekler hakkında daha fazla bilgi için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).
> 
> 

## <a name="build-the-application"></a>Uygulama oluşturma
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak (uygulama klasörüne gittikten sonra) için kullanabileceğiniz bir yapı betiği içerir.

  ```sh
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma
Uygulama oluşturulduktan sonra Azure CLI kullanarak yerel kümeye dağıtabilirsiniz.

1. Yerel Service Fabric kümesine bağlanın.
   
    ```sh
    azure servicefabric cluster connect
    ```
2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini kullanın.
   
    ```bash
    ./install.sh
    ```
3. Bir tarayıcı açın ve http://localhost:19080/Explorer adresindeki Service Fabric Explorer’a gidin (Vagrant’ı Mac OS X üzerinde kullanıyorsanız localhost ifadesini sanal makinenin özel IP’si ile değiştirin).
4. Uygulamalar düğümünü genişletin ve şu anda uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

## <a name="start-the-test-client-and-perform-a-failover"></a>Test istemcisini başlatma ve yük devre gerçekleştirme
Actor projeleri kendi başına bir işlem yapamaz. Bunlar başka bir hizmet veya istemcinin kendilerine iletiler göndermesini gerektirir. Actor şablonu, actor hizmetiyle etkileşim kurmak üzere kullanabileceğiniz basit bir test betiği içerir.

1. Actor hizmetinin çıktısını görmek için izleme yardımcı programını kullanarak betiği çalıştırın.
   
    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. Service Fabric Explorer’da actor hizmetinin birincil çoğaltmasını barındıran düğümü bulun. Aşağıdaki ekran görüntüsünde düğüm 3’tür.
   
    ![Service Fabric Explorer’da birincil çoğaltmayı bulma][sfx-primary]
3. Önceki adımda bulduğunuz düğüme tıklayın, ardından Eylemler menüsünden **Devre dışı bırak (yeniden başlat)** öğesini seçin. Bu eylem, yerel kümenizdeki bir düğümü yeniden başlatır. Böylece başka bir düğümde çalışan ikincil bir çoğaltmaya yük devretmesi için zorlanır. Bu eylemi gerçekleştirirken, test istemcisinden gelen çıkışa dikkat edin ve sayacın yük devretmeye rağmen artmaya devam ettiğini unutmayın.

## <a name="adding-more-services-to-an-existing-application"></a>Mevcut bir uygulamaya daha fazla hizmet ekleme

`yo` kullanılarak oluşturulmuş bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın: 
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. `yo azuresfcsharp:AddService` öğesini çalıştırın

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Actors hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md)
* [Azure CLI kullanarak Service Fabric kümeleriyle etkileşim kurma](service-fabric-azure-cli.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png



<!--HONumber=Dec16_HO2-->


