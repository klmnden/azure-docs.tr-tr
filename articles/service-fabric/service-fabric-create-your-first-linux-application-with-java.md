---
title: "Linux üzerinde Azure Service Fabric reliable actors Java uygulaması oluşturma | Microsoft Docs"
description: "Beş dakika içinde Java Service Fabric reliable actors uygulaması oluşturmayı ve dağıtmayı öğrenin."
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
ms.date: 06/29/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 254f38a600ea4026120bc411368eeb01310e56b2
ms.contentlocale: tr-tr
ms.lasthandoff: 07/15/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Linux üzerinde ilk Java Service Fabric reliable actors uygulamanızı oluşturma

Bu hızlı başlangıç, bir Linux geliştirme ortamında ilk Azure Service Fabric Java uygulamanızı yalnızca birkaç dakikada oluşturmanıza yardımcı olur.  İşlemi tamamladığınızda, yerel geliştirme kümesinde çalışan basit bir Java tek hizmet uygulamanız olacak.  

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce Service Fabric SDK’sı ile Azure CLI aracını yükleyin ve [Linux geliştirme ortamınızda](service-fabric-get-started-linux.md) bir geliştirme kümesi kurun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux geliştirme ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

Uygulamanızı dağıtmak için [Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (önerilir) veya [XPlat CLI](service-fabric-azure-cli.md) aracını da yapılandırmanız gerekir.

## <a name="create-the-application"></a>Uygulama oluşturma
Service Fabric uygulaması bir veya birden çok hizmet içerir ve bu hizmetlerin her biri, uygulamanın işlevselliğini sunma konusunda belirli bir role sahiptir. Linux için Service Fabric SDK’sı ilk hizmetinizi oluşturmayı ve daha sonra daha fazlasını eklemenizi kolaylaştıran bir [Yeoman](http://yeoman.io/) oluşturucu içerir.  Service Fabric Java uygulamalarını Eclipse’e yönelik bir eklentiyi kullanarak da oluşturabilir, derleyebilir ve dağıtabilirsiniz. Bkz. [Eclipse kullanarak ilk Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md). Bu hızlı başlangıç için bir sayaç değerini alan ve depolayan tek bir hizmete sahip bir uygulama oluşturmak üzere Yeoman’ı kullanın.

1. Bir terminal penceresinde ``yo azuresfjava`` yazın.
2. Uygulamanızı adlandırın. 
3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğretici için bir Reliable Actor Hizmeti seçin. Diğer hizmet türleri hakkında daha fazla bilgi edinmek için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).
   ![Java için Service Fabric Yeoman oluşturucusu][sf-yeoman]

## <a name="build-the-application"></a>Uygulama oluşturma
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak için kullanabileceğiniz bir [Gradle](https://gradle.org/) derleme betiği içerir. Uygulamayı derlemek ve paketlemek için şu komutu çalıştırın:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma
Uygulama oluşturulduktan sonra uygulamayı yerel kümeye dağıtabilirsiniz.

### <a name="using-xplat-cli"></a>XPlat CLI aracını kullanma

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
    azure servicefabric cluster connect
    ```

2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini çalıştırın.

    ```bash
    ./install.sh
    ```

### <a name="using-azure-cli-20"></a>Azure CLI 2.0 aracını kullanma

Oluşturulan uygulamayı dağıtma işlemi, diğer tüm Service Fabric uygulamalarında olduğu gibidir. Ayrıntılı yönergeler için [Service Fabric uygulamasını Azure CLI ile yönetme](service-fabric-application-lifecycle-azure-cli-2-0.md) ile ilgili belgelere bakın.

Bu komutların parametreleri, uygulama paketi içinde oluşturulmuş bildirimlerde bulunabilir.

Uygulama dağıtıldığında bir tarayıcı açın ve [http://localhost:19080/Explorer](http://localhost:19080/Explorer) konumundaki [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)'a gidin.
Ardından, **Uygulamalar** düğümünü genişletin ve geçerli olarak uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

## <a name="start-the-test-client-and-perform-a-failover"></a>Test istemcisini başlatma ve yük devre gerçekleştirme
Aktörler kendi başına hiçbir şey yapmaz; başka bir hizmet veya istemcinin aktörlere ileti göndermesini gerektirir. Actor şablonu, actor hizmetiyle etkileşim kurmak üzere kullanabileceğiniz basit bir test betiği içerir.

1. Actor hizmetinin çıktısını görmek için izleme yardımcı programını kullanarak betiği çalıştırın.  Test betiği bir sayacın değerini yükseltmek için aktörde `setCountAsync()` yöntemine; yeni sayaç değerini edinmek içinse aktörde `getCountAsync()` yöntemine çağrı yapar ve bu değeri konsolda görüntüler.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Service Fabric Explorer’da actor hizmetinin birincil çoğaltmasını barındıran düğümü bulun. Aşağıdaki ekran görüntüsünde düğüm 3’tür. Birincil hizmet çoğaltması okuma ve yazma işlemlerini işler.  Daha sonra, hizmet durumundaki değişiklikler, aşağıdaki ekran görüntüsünde görülen 0 ve 1 düğümlerinde çalışan ikincil çoğaltmalara çoğaltılır.

    ![Service Fabric Explorer’da birincil çoğaltmayı bulma][sfx-primary]

3. **Düğümler**’de, önceki adımda bulduğunuz düğüme tıklayın ve Eylemler menüsünden **Devre dışı bırak (yeniden başlat)** öğesini seçin. Bu eylem, birincil hizmet çoğaltmasını çalıştıran düğümü yeniden başlatır ve başka bir düğümde çalışan ikincil çoğaltmalardan birine yük devretmeyi zorlar.  Bu ikincil çoğaltma birincil konumuna yükseltilir, farklı bir düğümde başka bir ikincil çoğaltma oluşturulur ve birincil çoğaltma okuma/yazma işlemleri almaya başlar. Düğüm yeniden başlatılırken test istemcisinin çıktısına dikkat edin ve yük devretmeye rağmen sayacın artmaya devam ettiğini gözlemleyin.

## <a name="remove-the-application"></a>Uygulamayı kaldırma
Uygulama örneğini silmek, uygulama paketinin kaydını silmek ve uygulama paketini kümenin görüntü deposundan kaldırmak için şablonda sağlanan kaldırma betiğini kullanın.

```bash
./uninstall.sh
```

Service Fabric Explorer’da uygulamanın ve uygulama türünün artık **Uygulamalar** düğümünde olmadığını görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
* [Linux’ta Eclipse kullanarak ilk Service Fabric Java uygulamanızı oluşturma](service-fabric-get-started-eclipse.md)
* [Reliable Actors hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md)
* [Azure CLI kullanarak Service Fabric kümeleriyle etkileşim kurma](service-fabric-azure-cli.md)
* [Dağıtım sorunlarını giderme](service-fabric-azure-cli.md#troubleshooting)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

## <a name="related-articles"></a>İlgili makaleler

* [Service Fabric ve Azure CLI 2.0 ile çalışmaya başlama](service-fabric-azure-cli-2-0.md)
* [Service Fabric XPlat CLI ile çalışmaya başlama](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

