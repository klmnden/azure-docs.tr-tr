---
title: "Sürekli yapı ve Jenkins kullanarak, Azure Service Fabric Linux uygulamalarınız için tümleştirme | Microsoft Docs"
description: "Sürekli yapı ve Jenkins kullanarak Service Fabric Linux uygulamanız için tümleştirme"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/27/2017
ms.author: saysa
ms.openlocfilehash: e9422745de1f46098f1a1b0605c2560f44c02f3c
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Jenkins Linux uygulamaları geliştirmek ve dağıtmak için kullanın
Jenkins, uygulamanızın sürekli tümleştirme ve dağıtımı için yaygın olarak kullanılan bir araçtır. Jenkins kullanarak Azure Service Fabric uygulamanızı derleme ve dağıtma işlemi aşağıda açıklanmaktadır.

## <a name="general-prerequisites"></a>Genel önkoşullar
- Git’i yerel olarak yükleyin. İşletim sisteminize uygun Git sürümünü, [Git indirme sayfasından](https://git-scm.com/downloads) yükleyebilirsiniz. Yeni bir Git kullanıcısıysanız, daha fazla bilgi almak için [Git belgelerine](https://git-scm.com/docs) bakın.
- Service Fabric için kullanışlı Jenkins eklentisini edinin. Eklentiyi [Service Fabric indirmeleri](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) sayfasından indirebilirsiniz.

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins’i bir Service Fabric kümesi içinde ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümlerde, bir küme içinde kapsayıcı örneğinin durumunu kaydetmek için Azure storage hesabı kullanırken nasıl ayarlanacağını gösterir.

### <a name="prerequisites"></a>Ön koşullar
1. Bir Service Fabric Linux kümesini hazır bulundurun. Azure portalından oluşturulmuş bir Service Fabric kümesinde Docker zaten yüklüdür. Kümeyi yerel olarak çalıştırıyorsanız, ``docker info`` komutunu kullanarak Docker’ın yüklü olup olmadığını denetleyin. Yüklü değilse, aşağıdaki komutları kullanarak yükleyin:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ``` 

   > [!NOTE]
   > 8081 bağlantı noktası küme üzerinde özel bir uç noktası olarak belirtildiğinden emin olun.
   >

2. Uygulama, aşağıdaki adımları kullanarak kopyalama:
  ```sh
  git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
  cd service-fabric-java-getting-started/Services/JenkinsDocker/
  ```

3. Bir dosya paylaşımı Jenkins kapsayıcısında durumunu Sürdür:
  * Bir Azure depolama hesabı oluşturun **aynı bölgede** kümeniz gibi bir adla ``sfjenkinsstorage1``.
  * Oluşturma bir **dosya paylaşımı** altında depolama hesap adı ile gibi ``sfjenkins``.
  * Tıklayın **Bağlan** Not ve dosya paylaşımı için değerleri altında görüntülediği **Linux bağlanma**, değer birine benzer görünmelidir:
  ```sh
  sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
  ```

  > [!NOTE]
  > Bağlama CIFS paylaşımlar için küme düğümlerinde yüklü CIFS yardımcı programları paketi olması gerekir.       
  >

4. Yer tutucu değerlerini güncelleştirmek ```setupentrypoint.sh``` 3. adımındaki azure depolama ayrıntılarla komut dosyası.
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
  * Değiştir ``[REMOTE_FILE_SHARE_LOCATION]`` değerle ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` Bağlan çıktısından Yukarıdaki 3. adım.
  * Değiştir ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` değerle ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` gelen Yukarıdaki 3. adım.

5. **Yalnızca güvenli küme:** Jenkins güvenli bir kümeden üzerinde uygulamalarının dağıtımını yapılandırmak için sertifika Jenkins kapsayıcı içinde erişilebilir olmalıdır. Linux kümelerinde certificates(PEM) yalnızca kopyalanır üzerinden kapsayıcı X509StoreName tarafından belirtilen deposundan. ContainerHostPolicies altında ApplicationManifest içinde bu sertifika başvurusu ekleyin ve parmak izi değerini güncelleştirin. Parmak izi değeri bir sertifika, düğümde bulunduğundan emin olması gerekir.
  ```xml
  <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
  ```
  > [!NOTE]
  > Parmak izi değeri güvenli kümeye bağlanmak için kullanılan sertifika ile aynı olması gerekir. 
  >

6. Kümeye bağlanın ve kapsayıcı uygulamayı yükleyin.

  **Güvenli küme**
  ```sh
  sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
  bash Scripts/install.sh
  ```

  **Güvenli olmayan küme**
  ```sh
  sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
  bash Scripts/install.sh
  ```

  Bu işlem, kümeye bir Jenkins kapsayıcısı yükler ve küme, Service Fabric Explorer kullanılarak izlenebilir.

    > [!NOTE]
    > Birkaç kümede indirilmesi Jenkins görüntüsü için dakika sürebilir.
    >

### <a name="steps"></a>Adımlar
1. Tarayıcınızdan ``http://PublicIPorFQDN:8081`` sayfasına gidin. Bu işlem, oturum açmak için gereken ilk yönetici parolasının yolunu sağlar. 
2. Jenkins kapsayıcı hangi düğüm üzerinde çalışan belirlemek için Service Fabric Explorer arayın. Bu düğüm için kabuk (SSH) oturum açma güvenliğini sağlayın.
```sh
ssh user@PublicIPorFQDN -p [port]
``` 
3. ``docker ps -a`` kullanarak kapsayıcı örneğinin kimliğini alın.
4. Kapsayıcıda Güvenli Kabuk (SSH) oturumunu açın ve Jenkins portalında gösterilen yolu yapıştırın. Örneğin, portalda `PATH_TO_INITIAL_ADMIN_PASSWORD` yolu gösteriliyorsa aşağıdaki komutu çalıştırın:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  ```
  ```sh
  cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the pasword value
  ```
5. Jenkins sistemden başlatılan sayfada seçeneği yüklemek, seçmek için Seç eklentileri seçin **hiçbiri** onay ve tıklatın yükleyin.
6. Bir kullanıcı oluşturabilir veya bir yönetici olarak devam etmek için seçin

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins’i Service Fabric kümesi dışında ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümlerde, kümenin dışında nasıl ayarlandığı gösterilmektedir.

### <a name="prerequisites"></a>Ön koşullar
Docker’ın yüklü olması gerekir. Terminalden Docker yüklemek için aşağıdaki komutlar kullanılabilir:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Şimdi terminalde ``docker info`` çalıştırdığınızda, çıktıda Docker hizmetinin çalıştığını göreceksiniz.

### <a name="steps"></a>Adımlar
  1. Service Fabric Jenkins kapsayıcı görüntüsünü çekin: ``docker pull sayantancs/jenkins:v9``
  2. Kapsayıcı görüntüsünü çalıştırın:``docker run -itd -p 8080:8080 sayantancs/jenkins:v9``
  3. Kapsayıcı görüntüsü örneğinin kimliğini alın. ``docker ps –a`` komutuyla tüm Docker kapsayıcılarını listeleyebilirsiniz
  4. Aşağıdaki adımları kullanarak Jenkins portalında oturum açın:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Kapsayıcı kimliği 2d24a73b5964 ise, 2d24 kullanın.
    * ``http://<HOST-IP>:8080`` olan bu parola, portaldan Jenkins panosunda oturum açmak için gereklidir
    * İlk defa oturum açtıktan sonra, kendi kullanıcı hesabınızı oluşturabilir ve bunu gelecekteki amaçlar için kullanabilirsiniz veya yönetici hesabını kullanmaya devam edebilirsiniz. Bir kullanıcı oluşturduktan sonra, bu kullanıcıyla devam etmeniz gerekir.
  5. [Yeni bir SSH anahtarı oluşturma ve SSH aracısına ekleme](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) bölümünde anlatılan adımları kullanarak, GitHub’ı Jenkins ile çalışacak şekilde ayarlayın.
        * GitHub’dan sağlanan yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran GitHub hesabına SSH anahtarını ekleyin.
        * Önceki bağlantıda belirtilen komutları Jenkins Docker kabuğunda (ana bilgisayarınızda değil) çalıştırın.
      * Ana bilgisayarınızdan Jenkins kabuğunda oturum açmak için aşağıdaki komutları kullanın:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Jenkins kapsayıcı görüntüsünün barındırıldığı küme veya makinede genel kullanıma yönelik bir IP bulunduğundan emin olun. Bunun olması, Jenkins örneğinin GitHub'dan bildirimler almasını sağlar.

## <a name="install-the-service-fabric-jenkins-plug-in-from-the-portal"></a>Portaldan Service Fabric Jenkins eklentisini yükleme

1. Şuraya gidin: ``http://PublicIPorFQDN:8081``
2. Jenkins panosundan **Jenkins’i Yönet** > **Eklentileri Yönet** > **Gelişmiş**’i seçin.
Burada, bir eklentiyi karşıya yükleyebilirsiniz. Seçin **dosya**ve ardından **serviceFabric.hpi** , Önkoşullar altında indirilen dosya veya indirebilirsiniz [burada](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi). **Karşıya Yükle**’yi seçtiğinizde Jenkins, eklentiyi otomatik olarak yükler. Yeniden başlatma isteniyorsa izin verin.

## <a name="create-and-configure-a-jenkins-job"></a>Bir Jenkins işi oluşturma ve yapılandırma

1. Panodan **yeni öğe** oluşturun.
2. Bir öğe adını girin (örneğin, **MyJob**). **Serbest stil proje**’yi seçip **Tamam**’a tıklayın.
3. İş sayfasına gidip **Yapılandır** öğesine tıklayın.

   a. Genel bölümünde onay kutusunu seçin **GitHub proje**, ve GitHub proje URL'sini belirtin. Bu URL, Jenkins sürekli tümleştirme, sürekli dağıtım (CI/CD) akışı ile tümleştirmek istediğiniz Service Fabric Java uygulamasını barındırır (örneğin, ``https://github.com/sayantancs/SFJenkins``).

   b. **Kaynak Kodu Yönetimi** bölümünde **Git**’i seçin. Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran deponun URL'sini belirtin (örneğin, ``https://github.com/sayantancs/SFJenkins.git``). Ayrıca, burada hangi dalın derleneceğini belirtebilirsiniz (örneğin, **/master**).
4. *GitHub*’ınızı (depoyu barındıran) Jenkins ile konuşabilecek şekilde yapılandırın. Aşağıdaki adımları kullanın:

   a. GitHub depo sayfanıza gidin. **Ayarlar** > **Tümleştirmeler ve Hizmetler** öğesine gidin.

   b. **Hizmet Ekle**’yi seçin, **Jenkins** yazın ve **Jenkins-GitHub eklentisi**’ni seçin.

   c. Jenkins web kancası URL'nizi girin (varsayılan olarak, ``http://<PublicIPorFQDN>:8081/github-webhook/`` olmalıdır). **Hizmet ekle/güncelleştir** öğesine tıklayın.

   d. Jenkins örneğinize bir test olayı gönderilir. GitHub’da web kancasının yanında yeşil renkli bir onay işareti görürsünüz ve projeniz derlenir.

   e. **Derleme Tetikleyicileri** bölümünde, istediğiniz derleme seçeneğini belirleyin. Bu örnekte, depoda bazı itmeler gerçekleştiğinde derleme tetiklemeyi istersiniz. Bu nedenle **GITScm yoklaması için GitHub kanca tetikleyicisi**’ni seçin. (Daha önce bu seçenek **GitHub’a bir değişiklik uygulandığında derle** olarak adlandırılıyordu.)

   f. **Bölüm için Java uygulamaları oluşturmak:** altında **yapı bölüm**, aşağı açılan gelen **Ekle derleme adımı**, seçeneğini **çağırma Gradle betik**. Gelişmiş menüsünü açın gelen pencere öğesinde, yolunu belirtin **kök yapı betik** uygulamanız için. Belirtilen yoldan build.gradle seçer ve uygun şekilde çalışır. ``MyActor`` adlı bir proje oluşturursanız (Eclipse eklentisini veya Yeoman oluşturucuyu kullanarak), kök derleme betiği ``${WORKSPACE}/MyActor`` içermelidir. Bunun nasıl göründüğüne ilişkin bir örnek için aşağıdaki ekran görüntüsüne bakın:

    ![Service Fabric Jenkins Derleme eylemi][build-step]

   g. **Bölüm .net Core uygulamaları derleme:** altında **yapı bölüm**, açılır menüde gelen **Ekle derleme adımı**, seçeneğini **yürütme Kabuk**. Görüntülenen komutu kutusunda dizinin ilk build.sh dosyasının bulunduğu yolu değiştirilmesi gerekir. Dizin değiştirilmişse build.sh betik çalıştırılabilir ve uygulamayı oluşturacaksınız.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

    Bir exmaple oluşturmak için kullanılan komut aşağıdadır [sayacı hizmetini](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) CounterServiceApplication Jenkins iş adı ile örnek.

    ![Service Fabric Jenkins Derleme eylemi][build-step-dotnet]
  
   h. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. Burada, Jenkins tarafından derlenen Service Fabric uygulamasının dağıtılacağı kümenin ayrıntılarını sağlamanız gerekir. Sertifika yolunu Yankı kapsayıcı içindeki Certificates_JenkinsOnSF_Code_MyCert_PEM ortam değişkeni değeri Yankı tarafından bulunabilir. Bu yol, istemci anahtarı ve istemci sertifikası alanları için kullanılabilir.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
    Uygulamayı dağıtmak için kullanılan ek uygulama ayrıntıları da sağlayabilirsiniz. Bunun nasıl göründüğüne ilişkin bir örnek için aşağıdaki ekran görüntüsüne bakın:

    ![Service Fabric Jenkins Derleme eylemi][post-build-step]

      > [!NOTE]
      > Burada küme, Jenkins kapsayıcı görüntüsünü dağıtmak için Service Fabric kullandığınız durumda Jenkins kapsayıcı uygulamasını barındıran kümeyle aynı olabilir.
      >

## <a name="next-steps"></a>Sonraki adımlar
GitHub ve Jenkins yapılandırılmıştır. https://github.com/sayantancs/SFJenkins depo örneğinde, ``MyActor`` projenizde bazı örnek değişiklikler yapmayı düşünün. Değişikliklerinizi uzak ``master`` dalına (veya birlikte çalışmak üzere yapılandırdığınız herhangi bir dala) gönderin. Bunun yapılması, yapılandırmış olduğunuz ``MyJob`` Jenkins işini tetikler. Bu işlem GitHub’dan değişiklikleri getirir, derler ve derleme sonrası eylemlerde belirttiğiniz küme uç noktasına uygulamayı dağıtır.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-step.png

