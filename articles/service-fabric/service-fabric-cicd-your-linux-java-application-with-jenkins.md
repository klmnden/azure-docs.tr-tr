---
title: "Jenkins kullanılarak Linux java uygulamanızın sürekli derlenmesi ve dağıtılması| Microsoft Docs"
description: "Jenkins kullanılarak Linux java uygulamanızın sürekli derlenmesi ve dağıtılması"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: raunakp
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: sayantancs
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: d3c64e3ed78a5379592b5e4276c83d26f04d2e1c
ms.lasthandoff: 03/08/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Jenkins kullanılarak Linux java uygulamanızın derlenmesi ve dağıtılması
Jenkins, sürekli tümleştirme ve dağıtım için yaygın olarak kullanılan bir araçtır. Bu belgede, Jenkins kullanarak Service Fabric uygulamanızı derlemenizde ve dağıtmanızda size rehberlik sunacağız.

## <a name="general-prerequisites"></a>Genel Önkoşullar
1. Git’in yerel olarak yüklü olması gerekir. Uygun git sürümü, işletim sisteminize göre [buradan](https://git-scm.com/downloads) yüklenebilir. Git’i ilk defa kullanacaksanız, git konusunda bilgilenmek için [belgelerde](https://git-scm.com/docs) belirtilen adımları takip edebilirsiniz.
2. Service Fabric Jenkins kullanışlı eklentisine sahip olmanız gerekir. Service Fabric Jenkins eklentisini [buradan](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) indirebilirsiniz.

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Service Fabric Kümesi içinde Jenkins ayarlama

### <a name="prerequisites"></a>Ön koşullar
1. Bir Service Fabric Linux kümesini hazır bulundurun. Azure portalından oluşturulmuş Service Fabric kümelerinde Docker zaten yüklüdür. Yerel kümeyi çalıştırıyorsanız, lütfen ``docker info`` komutunu kullanarak Docker’ın yüklü olup olmadığını denetleyin ve yüklü değilse şu komutu kullanarak yükleyin -
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
2. Service Fabric kapsayıcısı uygulamasının ``http://PublicIPorFQDN:19080`` kümesine dağıtılmasını sağlayın. Aşağıdaki adımları izleyebilirsiniz:
```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker && cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080 # Azure CLI cluster connect command
bash Scripts/install.sh
```
  Böylelikle, Jenkins kapsayıcı, bağlandığınız kümeye yüklenir. Service Fabric explorer’ı kullanarak kapsayıcı uygulamanızı izleyebilirsiniz.

### <a name="steps"></a>Adımlar
1. Tarayıcınızdan şu URL’ye gidin: ``http://PublicIPorFQDN:8081``. Bu, oturum açmak için gereken ilk yönetici parolasının yolunu sağlar. Jenkins’i yönetici kullanıcı olarak kullanmaya devam edebilirsiniz veya ilk yönetici hesabıyla oturum açtığınızda kullanıcıyı oluşturabilir ve değiştirebilirsiniz.
> [!NOTE]
> Kümeyi oluştururken 8081 bağlantı noktasının uygulamanın uç nokta bağlantı noktası olarak belirtildiğinden emin olmalısınız
>

2. Kapsayıcı örnek kimliğini ``docker ps -a`` kullanarak alın.
3. Jenkins portalında gösterilen yolu kullanıp yapıştırarak kapsayıcıya SSH oturum açın. Örneğin, portalda `PATH_TO_INITIAL_ADMIN_PASSWORD` yolu gösteriliyorsa şunu yapabilirsiniz -  
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash # This takes you inside docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. [Bunu](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) kullanarak GitHub’ı Jenkins ile çalışacak şekilde ayarlayın.
    * GitHub’dan temin edilen yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran (barındıracak olan) GitHub-hesabına ssh anahtarını ekleyin.
    * Yukarıdaki bağlantıda belirtilen komutları Jenkins-docker kabuğunda (ana bilgisayarınızda değil) çalıştırın
    * Ana bilgisayarınızdan Jenkins kabuğuna oturum açmak için, şunu yapmanız gerekir -
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Service Fabric Kümesi dışında Jenkins ayarlama

### <a name="prerequisites"></a>Ön koşullar
1. Docker’ın yüklü olması gerekir. Yüklü değilse, terminalde aşağıdaki komutları yazarak docker’ı yükleyebilirsiniz.
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
Şimdi terminalde ``docker info`` çalıştırdığınızda, docker hizmetinin çalıştığını çıktıyı göreceksiniz.

### <a name="steps"></a>Adımlar
  1. Service Fabric Jenkins kapsayıcı çekin:``docker pull IMAGE_NAME ``
  2. Kapsayıcı görüntüsünü çalıştırın:``docker run -itd -p 8080:8080 IMAGE_NAME``
  3. (Az önce yüklediğiniz) jenkins görüntüsü olan, docker kapsayıcısının kimliğini alın. ``docker ps –a`` komutuyla tüm docker kapsayıcılarını listeleyebilirsiniz
  4. Jenkins hesabı için yönetici parolasını alın. Bunun için aşağıdakileri yapabilirsiniz -
    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Burada kapsayıcı kimliği 2d24a73b5964 ise, yalnızca 2d24 eklemeniz gerekir
    * Bu parola, ``http://<HOST-IP>:8080`` portalından Jenkins panosuna oturum açmak için gerekecektir
    * İlk defa oturum açtığınızda, kendi kullanıcı hesabı oluşturabilir ve bunu gelecekteki amaçlar için kullanabilirsiniz veya yönetici hesabını kullanmaya devam edebilirsiniz. Yeni bir kullanıcı oluşturduktan sonra, bununla devam etmeniz gerekir.
  5. [Bunu](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) kullanarak GitHub’ı Jenkins ile çalışacak şekilde ayarlayın.
      * GitHub’dan temin edilen yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran (barındıracak olan) GitHub-hesabına ssh anahtarını ekleyin.
      * Yukarıdaki bağlantıda belirtilen komutları Jenkins-docker kabuğunda (ana bilgisayarınızda değil) çalıştırın
      * Ana bilgisayarınızdan Jenkins kabuğuna oturum açmak için, şunu yapmanız gerekir -
    ```sh
    docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
    ```

> [!NOTE]
> Jenkins kapsayıcı görüntüsünün barındırıldığı kümenin / makinenin, GitHub’dan bildirimlerin Jenkins örneği tarafından alınmasını sağlayacak şekilde genel kullanıma yönelik IP’ye sahip olduğundan emin olun.
>

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>Portaldan Service Fabric Jenkins Eklentisini ekleme

1. Şuraya gidin: ``http://PublicIPorFQDN:8081``
2. Jenkins panosundan, ``Manage Jenkins``  ->  ``Manage Plugins``  ->  ``Advanced`` öğelerini seçin.
Burada, bir eklenti karşıya yükleyebilirsiniz. ``Choose file`` seçeneğini belirleyin, ardından Önkoşullar altında indirmiş olduğunuz serviceFabric.hpi dosyasını seçin. Karşıya yüklemeyi seçtikten sonra, Jenkins eklentiyi sizin için otomatik olarak yükler. Ayrıca bir yeniden başlatma isteyebilir, lütfen buna izin verin.

## <a name="creating-and-configuring-a-jenkins-job"></a>Jenkins iş oluşturma ve yapılandırma

1. Panodan ``new item`` oluşturma
2. Yeni bir öğe adı, örneğin ``MyJob`` girin, serbest stil projeyi seçin ve tamam'a tıklayın
3. Ardından iş sayfasına gidin ve ``Configure`` - öğesine tıklayın
  * Genel bölümünde, ``Github project`` altında, Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran GitHub proje URL'sinizi belirtin (örneğin - ``https://github.com/sayantancs/SFJenkins``).
  * ``Source Code Management`` Bölümü altında ``Git`` öğesini seçin. Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran depo URL'sini belirtin (örneğin - ``https://github.com/sayantancs/SFJenkins.git``). Ayrıca, burada hangi dalın derleneceğini belirtebilirsiniz, örn. ``*/master``.
4. *GitHub*’ınızı (depoyu barındıran) aşağıdaki adımları kullanarak, Jenkins ile konuşabilecek şekilde yapılandırın -
  - GitHub depo sayfanıza gidin. ``Settings`` -> ``Integrations and Services`` kısmına gidin.
  - ``Add Service`` öğesini seçin, Jenkins yazın, ``Jenkins-Github plugin`` öğesini seçin.
  - Jenkins web kancası URL'nizi girin (varsayılan olarak, ``http://<PublicIPorFQDN>:8081/github-webhook/`` olmalıdır). Ekleme/güncelleştirme hizmetine tıklayın.
  - Jenkins örneğinize bir test olayı gönderilir. GitHub’da web kancasının yanında yeşil renkli bir onay işareti göreceksiniz ve projeniz derlenecek!
  - ``Build Triggers`` bölümünün altında, hangi derleme seçeneğini istediğinizi belirleyin - bu kullanım vakası için, depoya gönderme gerçekleştiğinde bir derleme tetiklemeyi planlıyoruz - yani ilişkili seçenek ``GitHub hook trigger for GITScm polling`` olacaktır (daha önce 'GitHub’a bir değişiklik gönderildiğinde derle’ şeklindeydi)
  - ``Build section`` altında, ``Add build step`` açılan listesinden, ``Invoke Gradle Script`` seçeneğini belirleyin. Gelen pencere öğesinde, uygulamanız için ``Root build script`` yolunu belirtin. Belirtilen yoldan build.gradle’ı alır ve buna göre çalışır. ``MyActor`` adlı bir proje oluşturursanız (Eclipse eklentisini veya Yeoman oluşturucuyu kullanarak), kök derleme betiğinin ``${WORKSPACE}/MyActor`` içermesi gerektiğini lütfen unutmayın. Örnek olarak, bu bölüm çoğunlukla şuna benzer -

    ![Service Fabric Jenkins Derleme eylemi][build-step]
  - ``Post-Build Actions`` açılan listesinde, ``Deploy Service Fabric Project`` öğesini seçin. Burada, Jenkins tarafından derlenen Service Fabric uygulamasının dağıtılacağı küme ayrıntılarını ve uygulamayı dağıtmak için kullanılan ek uygulama ayrıntılarını sağlamanız gerekir. Aşağıdaki ekran görüntüsü referans olarak kullanılabilir:

    ![Service Fabric Jenkins Derleme eylemi][post-build-step]

 >[!Note]
 > Burada küme, Jenkins kapsayıcı görüntüsünü dağıtmak için Service Fabric kullandığınız durumda Jenkins kapsayıcı uygulamasını barındıran kümeyle aynı olabilir.
 >

### <a name="end-to-end-scenario"></a>Uçtan uca senaryosu
Böylelikle GitHub ve Jenkins yapılandırılmış olur ve şimdi örn. https://github.com/sayantancs/SFJenkins gibi depodaki ``MyActor`` projenizde bazı örnek değişiklikleri yapabilirsiniz ve değişikliklerinizi uzak ``master`` dalına gönderebilirsiniz (veya onunla çalışmak üzere yapılandırdığınız herhangi bir dala). Böylelikle, yapılandırmış olduğunuz Jenkins işi ``MyJob`` tetiklenir. Temel olarak bunun yapacağı şey, GitHub’dan değişiklikleri almak, bunları derlemek ve uygulamayı, derleme sonrası eylemlerde belirttiğiniz küme uç noktasına dağıtmaktır.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

