---
title: "Jenkins kullanılarak Azure Service Fabric Linux Java uygulamanızın sürekli derlenmesi ve tümleştirilmesi | Microsoft Belgeleri"
description: "Jenkins kullanılarak Linux Java uygulamanızın sürekli derlenmesi ve tümleştirilmesi"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 335f41e2e4a40e64e87382ea338673de3ab79c27
ms.lasthandoff: 03/14/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Jenkins kullanılarak Linux java uygulamanızın derlenmesi ve dağıtılması
Jenkins, sürekli tümleştirme ve dağıtım için yaygın olarak kullanılan bir araçtır. Bu belgede, Jenkins kullanarak Service Fabric uygulamanızı derlemenizde ve dağıtmanızda size rehberlik sunacağız.

## <a name="general-prerequisites"></a>Genel Önkoşullar
1. Git’i yerel olarak yükleyin. Uygun git sürümü, işletim sisteminize göre [buradan](https://git-scm.com/downloads) yüklenebilir. Git’i ilk defa kullanacaksanız, git konusunda bilgilenmek için [belgelerde](https://git-scm.com/docs) belirtilen adımları takip edebilirsiniz.
2. Service Fabric Jenkins kullanışlı eklentisini edinin. Service Fabric Jenkins eklentisini [buradan](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) indirebilirsiniz.

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Service Fabric kümesi içinde Jenkins ayarlama

### <a name="prerequisites"></a>Ön koşullar
1. Bir Service Fabric Linux kümesini hazır bulundurun. Azure portalından oluşturulmuş Service Fabric kümesinde Docker zaten yüklüdür. Kümeyi yerel olarak çalıştırıyorsanız, lütfen ``docker info`` komutunu kullanarak Docker’ın yüklü olup olmadığını denetleyin ve yüklü değilse aşağıdaki komutu kullanarak yükleyin:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Aşağıdaki adımları kullanarak, Service Fabric kapsayıcı uygulamasının kümeye dağıtılmasını sağlayın:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker
cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080   # Azure CLI cluster connect command
bash Scripts/install.sh
```
Bu işlem Jenkins kapsayıcısını kümeye yükler ve Service Fabric Explorer kullanılarak izlenebilir.

### <a name="steps"></a>Adımlar
1. Tarayıcınızdan şu URL’ye gidin: ``http://PublicIPorFQDN:8081``. Bu işlem, oturum açmak için gereken ilk yönetici parolasının yolunu sağlar. Jenkins’i yönetici kullanıcı olarak kullanmaya devam edebilirsiniz veya ilk yönetici hesabıyla oturum açtığınızda kullanıcıyı oluşturabilir ve değiştirebilirsiniz.

  > [!NOTE]
  > Kümeyi oluştururken 8081 bağlantı noktasının uygulamanın uç nokta bağlantı noktası olarak belirtildiğinden emin olmalısınız
  >

2. Kapsayıcı örnek kimliğini ``docker ps -a`` kullanarak alın.
3. Jenkins portalında gösterilen yolu kullanıp yapıştırarak kapsayıcıya SSH oturum açın. Örneğin, portalda `PATH_TO_INITIAL_ADMIN_PASSWORD` yolu gösteriliyorsa şunu yapabilirsiniz -

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. GitHub’ı [bağlantıda](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) belirtilen adımları kullanarak Jenkins ile çalışacak şekilde ayarlayın.
    * GitHub’dan sağlanan yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran GitHub hesabına SSH anahtarını ekleyin.
    * Önceki bağlantıda belirtilen komutları Jenkins Docker kabuğunda (ana bilgisayarınızda değil) çalıştırın
    * Ana bilgisayarınızdan Jenkins kabuğunda oturum açmak için aşağıdaki komutu kullanın:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Service Fabric kümesi dışında Jenkins ayarlama

### <a name="prerequisites"></a>Ön koşullar
Docker’ın yüklü olması gerekir. Terminalden Docker yüklemek için aşağıdaki komutlar kullanılabilir:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Şimdi terminalde ``docker info`` çalıştırdığınızda, çıktıda Docker hizmetinin çalıştığını göreceksiniz.

### <a name="steps"></a>Adımlar
  1. Service Fabric Jenkins kapsayıcı görüntüsünü çekin: ``docker pull servicefabric-microsoft.azurecr.io/jenkins:v1``
  2. Kapsayıcı görüntüsünü çalıştırın:``docker run -itd -p 8080:8080 servicefabric-microsoft.azurecr.io/jenkins:v1``
  3. Kapsayıcı görüntü örneğinin kimliği alın. ``docker ps –a`` komutuyla tüm Docker kapsayıcılarını listeleyebilirsiniz
  4. Aşağıdaki adımları kullanarak Jenkins portalında oturum açın:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Kapsayıcı kimliği 2d24a73b5964 ise, 2d24 kullanın.
    * ``http://<HOST-IP>:8080`` olan bu parola, portaldan Jenkins panosunda oturum açmak için gereklidir
    * İlk defa oturum açtığınızda, kendi kullanıcı hesabı oluşturabilir ve bunu gelecekteki amaçlar için kullanabilirsiniz veya yönetici hesabını kullanmaya devam edebilirsiniz. Bir kullanıcı oluşturduktan sonra, bu kullanıcıyla devam etmeniz gerekir.
  5. GitHub’ı [bağlantıda](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) belirtilen adımları kullanarak Jenkins ile çalışacak şekilde ayarlayın.
      * GitHub’dan temin edilen yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran (barındıracak olan) GitHub-hesabına ssh anahtarını ekleyin.
      * Önceki bağlantıda belirtilen komutları Jenkins Docker kabuğunda (ana bilgisayarınızda değil) çalıştırın
      * Ana bilgisayarınızdan Jenkins kabuğunda oturum açmak için aşağıdaki komutları kullanın:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

    > [!NOTE]
    > Jenkins kapsayıcı görüntüsünün barındırıldığı kümenin / makinenin, GitHub’dan bildirimlerin Jenkins örneği tarafından alınmasını sağlayacak şekilde genel kullanıma yönelik IP’ye sahip olduğundan emin olun.
    >

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>Portaldan Service Fabric Jenkins eklentisini yükleme

1. Şuraya gidin: ``http://PublicIPorFQDN:8081``
2. Jenkins panosundan **Jenkins’i Yönet** -> **Eklentileri Yönet** -> **Gelişmiş**’i seçin.
Burada, bir eklenti karşıya yükleyebilirsiniz. **Dosya seç** seçeneğini belirleyin, ardından önkoşullar altında indirmiş olduğunuz serviceFabric.hpi dosyasını seçin. Karşıya yüklemeyi seçtikten sonra Jenkins, eklentiyi otomatik olarak yükler. Yeniden başlatma isteniyorsa izin verin.

## <a name="creating-and-configuring-a-jenkins-job"></a>Jenkins iş oluşturma ve yapılandırma

1. Panodan **yeni öğe** oluşturma
2. **MyJob** gibi bir öğe adı girin, serbest stil projeyi seçin ve Tamam'a tıklayın
3. Ardından iş sayfasına gidip **Yapılandır** - öğesine tıklayın
  * Genel bölümündeki **Github projesi** altında, Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran GitHub proje URL'sini belirtin (örneğin - ``https://github.com/sayantancs/SFJenkins``).
  * **Kaynak Kodu Yönetimi** bölümünde **Git**’i seçin. Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran depo URL'sini belirtin (örneğin - ``https://github.com/sayantancs/SFJenkins.git``). Ayrıca, burada hangi dalın derleneceğini belirtebilirsiniz, örn. ***/master**.
4. Aşağıdaki adımları kullanarak, *GitHub*’ınızı (depoyu barındıran) Jenkins ile konuşabilecek şekilde yapılandırın -
  1. GitHub depo sayfanıza gidin. **Ayarlar** -> **Tümleştirmeler ve Hizmetler** öğesine gidin.
  2. **Hizmet Ekle**’yi seçin, Jenkins yazın ve **Jenkins-Github eklentisi**’ni seçin.
  3. Jenkins web kancası URL'nizi girin (varsayılan olarak, ``http://<PublicIPorFQDN>:8081/github-webhook/`` olmalıdır). Hizmet ekle/güncelleştir öğesine tıklayın.
  4. Jenkins örneğinize bir test olayı gönderilir. GitHub’da web kancasının yanında yeşil renkli bir onay işareti göreceksiniz ve projeniz derlenecek!
  5. **Derleme Tetikleyicileri** bölümünün altında, hangi derleme seçeneğini istediğinizi belirleyin - bu kullanım örneği için, depoya gönderme gerçekleştiğinde bir derleme tetiklemeyi planlıyoruz - yani ilişkili seçenek **GITScm yoklaması için GitHub kanca tetikleyicisi** olacaktır (daha önce 'GitHub’a bir değişiklik gönderildiğinde derle’ şeklindeydi)
  6. **Derleme bölümü** altında, **Derleme adımı ekle** açılır listesinden **Gradle Betiğini Çağır** seçeneğini belirleyin. Açılan pencere öğesinde, uygulamanız için **Kök derleme betiği** yolunu belirtin. Belirtilen yoldan build.gradle’ı alır ve buna göre çalışır. ``MyActor`` adlı bir proje oluşturursanız (Eclipse eklentisini veya Yeoman oluşturucuyu kullanarak), kök derleme betiği - ``${WORKSPACE}/MyActor`` içermelidir. Örnek olarak, bu bölüm çoğunlukla şuna benzer -

    ![Service Fabric Jenkins Derleme eylemi][build-step]
  7. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. Burada, Jenkins tarafından derlenen Service Fabric uygulamasının dağıtılacağı küme ayrıntılarını ve uygulamayı dağıtmak için kullanılan ek uygulama ayrıntılarını sağlamanız gerekir. Aşağıdaki ekran görüntüsü referans olarak kullanılabilir:

    ![Service Fabric Jenkins Derleme eylemi][post-build-step]

 > [!NOTE]
 > Burada küme, Jenkins kapsayıcı görüntüsünü dağıtmak için Service Fabric kullandığınız durumda Jenkins kapsayıcı uygulamasını barındıran kümeyle aynı olabilir.
 >

### <a name="end-to-end-scenario"></a>Uçtan uca senaryosu
Böylelikle GitHub ve Jenkins yapılandırılmış olur ve şimdi örn. https://github.com/sayantancs/SFJenkins gibi depodaki ``MyActor`` projenizde bazı örnek değişiklikleri yapabilirsiniz ve değişikliklerinizi uzak ``master`` dalına gönderebilirsiniz (veya onunla çalışmak üzere yapılandırdığınız herhangi bir dala). Böylelikle, yapılandırmış olduğunuz Jenkins işi ``MyJob`` tetiklenir. Temel olarak bunun yapacağı şey, GitHub’dan değişiklikleri almak, bunları derlemek ve uygulamayı, derleme sonrası eylemlerde belirttiğiniz küme uç noktasına dağıtmaktır.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

