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
ms.date: 09/20/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: a29f1e7b39b7f35073aa5aa6c6bd964ffaa6ffd0
ms.openlocfilehash: 68f9492231d367b1ede6ab032ec1c66c75150957
ms.contentlocale: tr-tr
ms.lasthandoff: 09/21/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Linux üzerinde ilk Java Service Fabric Reliable Actors uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Bu hızlı başlangıç, bir Linux geliştirme ortamında ilk Azure Service Fabric Java uygulamanızı yalnızca birkaç dakikada oluşturmanıza yardımcı olur.  İşlemi tamamladığınızda, yerel geliştirme kümesinde çalışan basit bir Java tek hizmet uygulamanız olacak.  

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce Service Fabric SDK’sı ile Service Fabric CLI aracını yükleyin ve [Linux geliştirme ortamınızda](service-fabric-get-started-linux.md) bir geliştirme kümesi kurun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux geliştirme ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

[Service Fabric CLI](service-fabric-cli.md)’yı de yüklemeniz gerekir.

### <a name="install-and-set-up-the-generators-for-java"></a>Java için oluşturucuları yükleme ve ayarlama
Service Fabric, Yeoman şablon oluşturucu kullanarak terminalden Service Fabric Java uygulaması oluşturmanıza yardımcı olacak yapı iskelesi araçları sağlar. Lütfen makinenizde çalışan bir Java için Service Fabric yeoman şablon oluşturucu olduğundan emin olmak için aşağıdaki adımları izleyin.
1. Makinenize nodejs ve NPM yükleme

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. NPM’den makinenize [Yeoman](http://yeoman.io/) şablon oluşturucuyu yükleme

  ```bash
  sudo npm install -g yo
  ```
3. NPM’den Service Fabric Yeo Java uygulama oluşturucuyu yükleme

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-the-application"></a>Uygulama oluşturma
Service Fabric uygulaması bir veya birden çok hizmet içerir ve bu hizmetlerin her biri, uygulamanın işlevselliğini sunma konusunda belirli bir role sahiptir. Son bölümde yüklediğiniz oluşturucu, ilk hizmetinizi oluşturmayı ve daha sonra hizmet eklemeyi kolaylaştırır.  Service Fabric Java uygulamalarını Eclipse’e yönelik bir eklentiyi kullanarak da oluşturabilir, derleyebilir ve dağıtabilirsiniz. Bkz. [Eclipse kullanarak ilk Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md). Bu hızlı başlangıç için bir sayaç değerini alan ve depolayan tek bir hizmete sahip bir uygulama oluşturmak üzere Yeoman’ı kullanın.

1. Bir terminal penceresinde ``yo azuresfjava`` yazın.
2. Uygulamanızı adlandırın.
3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğretici için bir Reliable Actor Hizmeti seçin. Diğer hizmet türleri hakkında daha fazla bilgi edinmek için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).
   ![Java için Service Fabric Yeoman oluşturucusu][sf-yeoman]

## <a name="build-the-application"></a>Uygulama oluşturma
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak için kullanabileceğiniz bir [Gradle](https://gradle.org/) derleme betiği içerir.
Service Fabric Java bağımlılıkları Maven’dan alınır. Service Fabric Java uygulamalarını oluşturmak ve çalışmak için JDK ve Gradle’ın yüklü olduğundan emin olmanız gerekir. Henüz yüklü değilse, JDK (openjdk-8-jdk) ve Gradle’ı yüklemek için aşağıdakini çalıştırabilirsiniz.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

Uygulamayı derlemek ve paketlemek için şu komutu çalıştırın:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma
Uygulama oluşturulduktan sonra uygulamayı yerel kümeye dağıtabilirsiniz.

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini çalıştırın.

    ```bash
    ./install.sh
    ```

Oluşturulan uygulamayı dağıtma işlemi, diğer tüm Service Fabric uygulamalarında olduğu gibidir. Ayrıntılı yönergeler için [Service Fabric uygulamasını Service Fabric CLI ile yönetme](service-fabric-application-lifecycle-sfctl.md) ile ilgili belgelere bakın.

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

## <a name="service-fabric-java-libraries-on-maven"></a>Maven'da Service Fabric Java kitaplıkları
Maven’da barındırılan Service Fabric Java kitaplıkları. Projelerinizin **mavenCentral**’daki Service Fabric Java kitaplıklarını kullanması için ``pom.xml`` veya ``build.gradle`` altına bağımlılıklar ekleyebilirsiniz. 

### <a name="actors"></a>Aktörler

Uygulamanız için Service Fabric Güvenilir Aktör desteği.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.12.0'
  }
  ```

### <a name="services"></a>Hizmetler

Uygulamanız için Service Fabric Reliable Services desteği.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.12.0'
  }
  ```

### <a name="others"></a>Diğer
#### <a name="transport"></a>Aktarım

Service Fabric Java uygulaması için Aktarım katmanı desteği. Aktarım katmanında özellikle programlamadığınız sürece bu bağımlılığı Güvenilir Aktör veya Hizmet uygulamalarınız için özellikle eklemeniz gerekmez.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.12.0'
  }
  ```

#### <a name="fabric-support"></a>Fabric desteği

Yerel Service Fabric çalışma zamanıyla iletişim kuran Service Fabric için sistem düzeyinde destek. Bu bağımlılığı Güvenilir Aktör veya Hizmet uygulamalarınız için özellikle eklemeniz gerekmez. Yukarıdaki diğer bağımlılıkları eklediğinizde bu otomatik olarak Maven’dan alınır.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.12.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.12.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a>Eski Service Fabric Java uygulamalarını Maven ile kullanılmak üzere geçirme
Service Fabric Java kitaplıklarını yakın zamanda Service Fabric Java SDK’sından Maven deposuna taşıdık. Yeoman veya Eclipse kullanarak oluşturduğunuz yeni uygulamalar en son güncelleştirilen projeleri oluşturur (Maven ile çalışırlar), ancak daha önce Service Fabric Java SDK’sı kullanan mevcut Service Fabric durum bilgisi olmayan ya da aktör Java uygulamalarını Maven’ın Service Fabric Java bağımlılıklarını kullanacak şekilde güncelleştirebilirsiniz. Eski uygulamanızın Maven ile çalıştığından emin olmak için lütfen [burada](service-fabric-migrate-old-javaapp-to-use-maven.md) belirtilen adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux’ta Eclipse kullanarak ilk Service Fabric Java uygulamanızı oluşturma](service-fabric-get-started-eclipse.md)
* [Reliable Actors hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md)
* [Service Fabric CLI’sını kullanarak Service Fabric kümeleriyle etkileşim kurma](service-fabric-cli.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin
* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

