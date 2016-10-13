<properties
   pageTitle="Linux üzerinde Java kullanarak ilk Service Fabric uygulamanızı oluşturma | Microsoft Azure"
   description="Java kullanarak bir Service Fabric uygulaması oluşturma ve dağıtma"
   services="service-fabric"
   documentationCenter="java"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="seanmck"/>



# İlk Azure Service Fabric uygulamanızı oluşturma

> [AZURE.SELECTOR]
- [C#](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java](service-fabric-create-your-first-linux-application-with-java.md)

Service Fabric, Linux üzerinde hem .NET Core hem de Java dillerinde hizmet oluşturmaya yönelik SDK’lar sağlar. Bu öğreticide Linux için uygulama oluşturma ve Java kullanarak hizmet oluşturma konuları ele alınacaktır.

## Ön koşullar

Başlamadan önce [Linux geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started-linux.md) emin olun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux one-box ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

## Uygulama oluşturma

Service Fabric uygulaması bir veya birden çok hizmet içerebilir. Bu hizmetlerin her biri uygulamanın işlevselliğini aktarma konusunda belirli bir role sahiptir. Linux için Service Fabric SDK’sı ilk hizmetinizi oluşturmayı ve daha sonra daha fazlasını eklemenizi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir. Yeoman kullanarak tek bir hizmetle yeni bir uygulama oluşturalım.

1. Bir terminal içinde **yo azuresfjava** yazın.

2. Uygulamanızı adlandırın.

3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğretici için Reliable Actor Hizmetini seçeceğiz.

  ![Java için Service Fabric Yeoman oluşturucusu][sf-yeoman]

>[AZURE.NOTE] Seçenekler hakkında daha fazla bilgi için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).

## Uygulama oluşturma

Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak için kullanabileceğiniz bir [Gradle](https://gradle.org/) derleme betiği içerir.

  ```bash
  gradle
  ```

## Uygulamayı dağıtma

Uygulama oluşturulduktan sonra Azure CLI kullanarak yerel kümeye dağıtabilirsiniz.

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
    azuresfcli servicefabric cluster connect
    ```

2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini kullanın.

    ```bash
    cd myapp
    ./install.sh
    ```

3. Bir tarayıcı açın ve http://localhost:19080/Explorer adresindeki Service Fabric Explorer’a gidin (Vagrant’ı Mac OS X üzerinde kullanıyorsanız localhost ifadesini sanal makinenin özel IP’si ile değiştirin).

4. Uygulamalar düğümünü genişletin ve şu anda uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

## Test istemcisini başlatma ve yük devre gerçekleştirme

Actor projeleri kendi başına bir işlem yapamaz. Bunlar başka bir hizmet veya istemcinin kendilerine iletiler göndermesini gerektirir. Actor şablonu, actor hizmetiyle etkileşim kurmak üzere kullanabileceğiniz basit bir test betiği içerir.

1. Actor hizmetinin çıktısını görmek için izleme yardımcı programını kullanarak betiği çalıştırın.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Service Fabric Explorer’da actor hizmetinin birincil çoğaltmasını barındıran düğümü bulun. Aşağıdaki ekran görüntüsünde düğüm 3’tür.

    ![Service Fabric Explorer’da birincil çoğaltmayı bulma][sfx-primary]

3. Önceki adımda bulduğunuz düğüme tıklayın, ardından Eylemler menüsünden **Devre dışı bırak (yeniden başlat)** öğesini seçin. Bunun yapılması yerel kümenizdeki beş düğümden birini yeniden başlatır ve başka bir düğümde çalışan ikincil çoğaltmalardan birine yük devretmeye zorlar. Bunu yaparken test istemcisinin çıktısına dikkat edin ve yük devretmeye rağmen sayacın artmaya devam ettiğini gözlemleyin.

## Eclipse Neon eklentisiyle uygulama oluşturma ve dağıtma

Eclipse Neon Hizmet Eklentisini yüklediyseniz Service Fabric uygulamaları oluşturmak, derlemek ve Java ile derlenmiş uygulamaları dağıtmak için bu eklentiyi kullanabilirsiniz.

### Uygulama oluşturma

Service Fabric eklentisi Eclipse genişletilebilirliği ile kullanılabilir.

1. Eclipse'te **Dosya > Diğer > Service Fabric** öğesini seçin. Aktörler ve Kapsayıcılar dahil bir dizi seçenek görürsünüz.

    ![Eclipse'te Service Fabric şablonları][sf-eclipse-templates]

2. Bu durumda Durum Bilgisi Olmayan Hizmet’i seçin.

3. Service Fabric projeleriyle Eclipse kullanımını iyileştiren Service Fabric perspektifinin kullanıldığını onaylamanız istenir. 'Evet' öğesini seçin.

### Uygulamayı dağıtma

Service Fabric şablonları, Eclipse ile tetikleyebileceğiniz uygulama oluşturma ve dağıtmaya yönelik bir dizi Gradle görevi içerir.

1. **Çalıştır > Yapılandırmaları Çalıştır**’ı seçin.

2. **Gradle Projesi**’ni genişletin ve **ServiceFabricDeployer**’ı seçin.

3. **Çalıştır**’a tıklayın.

Uygulamanız birkaç dakika içinde oluşturulup dağıtılır. Durumunu Service Fabric Explorer’dan izleyebilirsiniz.

## Sonraki adımlar

- [Reliable Actors hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png



<!--HONumber=Sep16_HO5-->


