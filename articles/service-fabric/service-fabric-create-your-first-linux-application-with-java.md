---
title: "Linux üzerinde Java kullanarak ilk Azure mikro hizmetlerinizi oluşturma | Microsoft Docs"
description: "Java kullanarak bir Service Fabric uygulaması oluşturma ve dağıtma"
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/02/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: e229602b4bfa72977c9b15e854d796ed09fa55d2
ms.contentlocale: tr-tr
ms.lasthandoff: 07/01/2017


---
<a id="create-your-first-service-fabric-java-application-on-linux" class="xliff"></a>

# Linux’ta ilk Service Fabric Java uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Bu hızlı başlangıç, bir Linux geliştirme ortamında ilk Azure Service Fabric Java uygulamanızı yalnızca birkaç dakikada oluşturmanıza yardımcı olur.  İşlemi tamamladığınızda, yerel geliştirme kümesinde çalışan basit bir Java tek hizmet uygulamanız olacak.  

<a id="prerequisites" class="xliff"></a>

## Ön koşullar
Başlamadan önce Service Fabric SDK’sı ile Azure CLI aracını yükleyin ve [Linux geliştirme ortamınızda](service-fabric-get-started-linux.md) bir geliştirme kümesi kurun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux geliştirme ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

Uygulamanızı dağıtmak için [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (önerilir) veya [XPlat CLI](service-fabric-azure-cli.md) aracını da yapılandırmanız gerekir.

<a id="create-the-application" class="xliff"></a>

## Uygulama oluşturma
Service Fabric uygulaması bir veya birden çok hizmet içerir ve bu hizmetlerin her biri, uygulamanın işlevselliğini sunma konusunda belirli bir role sahiptir. Linux için Service Fabric SDK’sı ilk hizmetinizi oluşturmayı ve daha sonra daha fazlasını eklemenizi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir.  Service Fabric Java uygulamalarını Eclipse’e yönelik bir eklentiyi kullanarak da oluşturabilir, derleyebilir ve dağıtabilirsiniz. Bkz. [Eclipse kullanarak ilk Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md). Bu hızlı başlangıç için bir sayaç değerini alan ve depolayan tek bir hizmete sahip bir uygulama oluşturmak üzere Yeoman’ı kullanın.

1. Bir terminal penceresinde ``yo azuresfjava`` yazın.
2. Uygulamanızı adlandırın. 
3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğretici için bir Reliable Actor Hizmeti seçin. Diğer hizmet türleri hakkında daha fazla bilgi edinmek için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).
   ![Java için Service Fabric Yeoman oluşturucusu][sf-yeoman]

<a id="build-the-application" class="xliff"></a>

## Uygulama oluşturma
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak için kullanabileceğiniz bir [Gradle](https://gradle.org/) derleme betiği içerir. Uygulamayı derlemek ve paketlemek için şu komutu çalıştırın:

  ```bash
  cd myapp
  gradle
  ```

<a id="deploy-the-application" class="xliff"></a>

## Uygulamayı dağıtma
Uygulama oluşturulduktan sonra uygulamayı yerel kümeye dağıtabilirsiniz.

<a id="using-xplat-cli" class="xliff"></a>

### XPlat CLI kullanma

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
    azure servicefabric cluster connect
    ```

2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini çalıştırın.

    ```bash
    ./install.sh
    ```

<a id="using-azure-cli-20" class="xliff"></a>

### Azure CLI 2.0 aracını kullanma

Derlenen uygulamayı dağıtma işlemi, diğer tüm Service Fabric uygulamalarında olduğu gibidir. Ayrıntılı yönergeler için bkz. [Service Fabric uygulamasını Azure CLI ile yönetme](service-fabric-application-lifecycle-azure-cli-2-0.md).

Bu komutların parametreleri, uygulama paketi içinde oluşturulmuş bildirimlerde bulunabilir.

Uygulama dağıtıldığında, bir tarayıcı açın ve [http://localhost:19080/Explorer](http://localhost:19080/Explorer) konumundaki [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)’a gidin.
Sonra, **Uygulamalar** düğümünü genişletin ve şu anda uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

<a id="start-the-test-client-and-perform-a-failover" class="xliff"></a>

## Test istemcisini başlatma ve yük devre gerçekleştirme
Aktörler kendi başına hiçbir şey yapmaz; başka bir hizmet veya istemcinin aktörlere ileti göndermesini gerektirir. Actor şablonu, actor hizmetiyle etkileşim kurmak üzere kullanabileceğiniz basit bir test betiği içerir.

1. Actor hizmetinin çıktısını görmek için izleme yardımcı programını kullanarak betiği çalıştırın.  Test betiği bir sayacın değerini yükseltmek için aktörde `setCountAsync()` yöntemine; yeni sayaç değerini edinmek içinse aktörde `getCountAsync()` yöntemine çağrı yapar ve bu değeri konsolda görüntüler.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Service Fabric Explorer’da actor hizmetinin birincil çoğaltmasını barındıran düğümü bulun. Aşağıdaki ekran görüntüsünde düğüm 3’tür. Birincil hizmet çoğaltması okuma ve yazma işlemlerini işler.  Daha sonra, hizmet durumundaki değişiklikler, aşağıdaki ekran görüntüsünde görülen 0 ve 1 düğümlerinde çalışan ikincil çoğaltmalara çoğaltılır.

    ![Service Fabric Explorer’da birincil çoğaltmayı bulma][sfx-primary]

3. **Düğümler**’de, önceki adımda bulduğunuz düğüme tıklayın ve Eylemler menüsünden **Devre dışı bırak (yeniden başlat)** öğesini seçin. Bu eylem, birincil hizmet çoğaltmasını çalıştıran düğümü yeniden başlatır ve başka bir düğümde çalışan ikincil çoğaltmalardan birine yük devretmeyi zorlar.  Bu ikincil çoğaltma birincil konumuna yükseltilir, farklı bir düğümde başka bir ikincil çoğaltma oluşturulur ve birincil çoğaltma okuma/yazma işlemleri almaya başlar. Düğüm yeniden başlatılırken test istemcisinin çıktısına dikkat edin ve yük devretmeye rağmen sayacın artmaya devam ettiğini gözlemleyin.

<a id="add-another-service-to-the-application" class="xliff"></a>

## Uygulamaya başka bir hizmet ekleme
`yo` kullanarak mevcut bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın:
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. `yo azuresfjava:AddService` öğesini çalıştırın
3. Uygulamayı önceki adımlarda olduğu gibi derleyin ve dağıtın.

<a id="remove-the-application" class="xliff"></a>

## Uygulamayı kaldırma
Uygulama örneğini silmek, uygulama paketinin kaydını silmek ve uygulama paketini kümenin görüntü deposundan kaldırmak için şablonda sağlanan kaldırma betiğini kullanın.

```bash
./uninstall.sh
```

Service Fabric Explorer’da uygulamanın ve uygulama türünün artık **Uygulamalar** düğümünde olmadığını görürsünüz.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
* [Linux’ta Eclipse kullanarak ilk Service Fabric Java uygulamanızı oluşturma](service-fabric-get-started-eclipse.md)
* [Reliable Actors hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md)
* [Azure CLI kullanarak Service Fabric kümeleriyle etkileşim kurma](service-fabric-azure-cli.md)
* [Dağıtım sorunlarını giderme](service-fabric-azure-cli.md#troubleshooting)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

<a id="related-articles" class="xliff"></a>

## İlgili makaleler

* [Service Fabric ve Azure CLI 2.0 kullanmaya başlama](service-fabric-azure-cli-2-0.md)
* [Service Fabric xPlat CLI kullanmaya başlama](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

