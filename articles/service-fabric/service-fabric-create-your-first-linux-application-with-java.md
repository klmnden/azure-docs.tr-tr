---
title: "Linux üzerinde Java kullanarak ilk Azure mikro hizmetlerinizi oluşturma | Microsoft Docs"
description: "Java kullanarak bir Service Fabric uygulaması oluşturma ve dağıtma"
services: service-fabric
documentationcenter: java
author: seanmck
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/05/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: dc9234760b0dfb5d109fc86ac47a89c8fcf7d991


---
# <a name="create-your-first-azure-service-fabric-application"></a>İlk Azure Service Fabric uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric, Linux üzerinde hem .NET Core hem de Java dillerinde hizmet oluşturmaya yönelik SDK’lar sağlar. Bu öğreticide Linux için bir uygulama ve Java kullanarak bir hizmet oluşturacağız.  

> [!NOTE]
> Birinci sınıf yerleşik programlama dili olarak Java desteği yalnızca Linux önizlemesinde sunulmaktadır (Windows desteği planlanmaktadır). Ancak Java uygulamaları dahil olmak üzere tüm uygulamalar, Windows veya Linux üzerinde kapsayıcıların içinde yürütülebilir konuk bileşenler olarak çalıştırılabilir. Daha fazla bilgi için bkz. [Var olan yürütülebilir dosyaları Azure Service Fabric’e dağıtma](service-fabric-deploy-existing-app.md) ve [Kapsayıcıları Service Fabric’e dağıtma](service-fabric-deploy-container.md).
>

## <a name="video-tutorial"></a>Video öğretici

Aşağıdaki Microsoft Virtual Academy videosunda Linux üzerinde Java uygulaması oluşturma işlemleri anlatılmaktadır:  
<center><a target="\_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-create-your-first-linux-application-with-java/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>


## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce [Linux geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started-linux.md) emin olun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux one-box ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Uygulama oluşturma
Service Fabric uygulaması bir veya birden çok hizmet içerebilir. Bu hizmetlerin her biri uygulamanın işlevselliğini aktarma konusunda belirli bir role sahiptir. Linux için Service Fabric SDK’sı ilk hizmetinizi oluşturmayı ve daha sonra daha fazlasını eklemenizi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir. Tek bir hizmetle uygulama oluşturmak için Yeoman’ı kullanalım.

1. Bir terminal penceresinde ``yo azuresfjava`` yazın.
2. Uygulamanızı adlandırın.
3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğreticinin amaçları doğrultusunda, Reliable Actor Hizmetini seçiyoruz.

   ![Java için Service Fabric Yeoman oluşturucusu][sf-yeoman]

> [!NOTE]
> Seçenekler hakkında daha fazla bilgi için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).
>

## <a name="build-the-application"></a>Uygulama oluşturma
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak için kullanabileceğiniz bir [Gradle](https://gradle.org/) derleme betiği içerir.

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma
Uygulama oluşturulduktan sonra Azure CLI kullanarak yerel kümeye dağıtabilirsiniz.

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
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

3. Önceki adımda bulduğunuz düğüme tıklayın, ardından Eylemler menüsünden **Devre dışı bırak (yeniden başlat)** öğesini seçin. Bu eylem yerel kümenizdeki beş düğümden birini yeniden başlatır ve başka bir düğümde çalışan ikincil çoğaltmalardan birine yük devretmeye zorlar. Bu eylemi gerçekleştirirken, test istemcisinden gelen çıkışa dikkat edin ve sayacın yük devretmeye rağmen artmaya devam ettiğini unutmayın.

## <a name="build-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Eclipse Neon eklentisiyle uygulama oluşturma ve dağıtma

Eclipse Neon için [Service Fabric Eklentisini](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#install-the-java-sdk-and-eclipse-neon-plugin-optional) yüklediyseniz Service Fabric uygulamaları oluşturmak, derlemek ve Java ile derlenmiş uygulamaları dağıtmak için bu eklentiyi kullanabilirsiniz.  Eclipse’i yüklerken, **Java geliştiricileri için Eclipse IDE**’yi seçin.

### <a name="create-the-application"></a>Uygulama oluşturma

Service Fabric eklentisi Eclipse genişletilebilirliği ile kullanılabilir.

1. Eclipse'te **Dosya > Diğer > Service Fabric** öğesini seçin. Aktörler ve Kapsayıcılar dahil bir dizi seçenek görürsünüz.

    ![Eclipse'te Service Fabric şablonları][sf-eclipse-templates]

2. Bu durumda Durum Bilgisi Olmayan Hizmet’i seçin.

3. Service Fabric projeleriyle Eclipse kullanımını iyileştiren Service Fabric perspektifinin kullanıldığını onaylamanız istenir. 'Evet' öğesini seçin.

### <a name="deploy-the-application"></a>Uygulamayı dağıtma
Service Fabric şablonları, Eclipse ile tetikleyebileceğiniz uygulama oluşturma ve dağıtmaya yönelik bir dizi Gradle görevi içerir.

1. **Çalıştır > Yapılandırmaları Çalıştır**’ı seçin.
2. **yerel** veya **bulut** seçeneğini belirtin. Varsayılan olarak **yerel** seçilidir. Uzak bir kümeye dağıtmak için **bulut** seçeneğini belirleyin.
3. Duruma göre `local.json` veya `cloud.json` öğesine düzenleyerek yayımlama profillerine doğru bilgilerin eklendiğinden emin olun.
4. **Çalıştır**’a tıklayın.

Uygulamanız birkaç dakika içinde oluşturulur ve dağıtılır. Durumunu Service Fabric Explorer’dan izleyebilirsiniz.

## <a name="adding-more-services-to-an-existing-application"></a>Mevcut bir uygulamaya daha fazla hizmet ekleme

`yo` kullanılarak oluşturulmuş bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın:
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. `yo azuresfjava:AddService` öğesini çalıştırın


## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Actors hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md)
* [Azure CLI kullanarak Service Fabric kümeleriyle etkileşim kurma](service-fabric-azure-cli.md)
* [Dağıtım sorunlarını giderme](service-fabric-azure-cli.md#troubleshooting)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Jan17_HO4-->


