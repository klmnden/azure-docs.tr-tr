---
title: "Linux üzerinde Java kullanarak ilk Service Fabric uygulamanızı oluşturma | Microsoft Belgeleri"
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
ms.date: 10/04/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 288d504b44fd7588a03a31171da1bfb332e2429f


---
# <a name="create-your-first-azure-service-fabric-application"></a>İlk Azure Service Fabric uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric, Linux üzerinde hem .NET Core hem de Java dillerinde hizmet oluşturmaya yönelik SDK’lar sağlar. Bu öğreticide Linux için uygulama oluşturma ve Java kullanarak hizmet oluşturma konuları ele alınacaktır.

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce [Linux geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started-linux.md) emin olun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux one-box ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Uygulama oluşturma
Service Fabric uygulaması bir veya birden çok hizmet içerebilir. Bu hizmetlerin her biri uygulamanın işlevselliğini aktarma konusunda belirli bir role sahiptir. Linux için Service Fabric SDK’sı ilk hizmetinizi oluşturmayı ve daha sonra daha fazlasını eklemenizi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir. Yeoman kullanarak tek bir hizmetle yeni bir uygulama oluşturalım.

1. Bir terminal içinde **yo azuresfjava** yazın.
2. Uygulamanızı adlandırın.
3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğretici için Reliable Actor Hizmetini seçeceğiz.
   
   ![Java için Service Fabric Yeoman oluşturucusu][sf-yeoman]

> [!NOTE]
> Seçenekler hakkında daha fazla bilgi için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).
> 
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
3. Önceki adımda bulduğunuz düğüme tıklayın, ardından Eylemler menüsünden **Devre dışı bırak (yeniden başlat)** öğesini seçin. Bunun yapılması yerel kümenizdeki beş düğümden birini yeniden başlatır ve başka bir düğümde çalışan ikincil çoğaltmalardan birine yük devretmeye zorlar. Bunu yaparken test istemcisinin çıktısına dikkat edin ve yük devretmeye rağmen sayacın artmaya devam ettiğini gözlemleyin.

## <a name="build-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Eclipse Neon eklentisiyle uygulama oluşturma ve dağıtma
Eclipse Neon Hizmet Eklentisini yüklediyseniz Service Fabric uygulamaları oluşturmak, derlemek ve Java ile derlenmiş uygulamaları dağıtmak için bu eklentiyi kullanabilirsiniz.  Eclipse’i yüklerken, **Java geliştiricileri için Eclipse IDE**’yi seçin.

### <a name="create-the-application"></a>Uygulama oluşturma
Service Fabric eklentisi Eclipse genişletilebilirliği ile kullanılabilir.

1. Eclipse'te **Dosya > Diğer > Service Fabric** öğesini seçin. Aktörler ve Kapsayıcılar dahil bir dizi seçenek görürsünüz.
   
    ![Eclipse'te Service Fabric şablonları][sf-eclipse-templates]
2. Bu durumda Durum Bilgisi Olmayan Hizmet’i seçin.
3. Service Fabric projeleriyle Eclipse kullanımını iyileştiren Service Fabric perspektifinin kullanıldığını onaylamanız istenir. 'Evet' öğesini seçin.

### <a name="deploy-the-application"></a>Uygulamayı dağıtma
Service Fabric şablonları, Eclipse ile tetikleyebileceğiniz uygulama oluşturma ve dağıtmaya yönelik bir dizi Gradle görevi içerir.

1. **Çalıştır > Yapılandırmaları Çalıştır**’ı seçin.
2. **Gradle Projesi**’ni genişletin ve **ServiceFabricDeployer**’ı seçin.
3. **Çalıştır**’a tıklayın.

Uygulamanız birkaç dakika içinde oluşturulup dağıtılır. Durumunu Service Fabric Explorer’dan izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Actors hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md)
* [Azure CLI kullanarak Service Fabric kümeleriyle etkileşim kurma](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Nov16_HO2-->


