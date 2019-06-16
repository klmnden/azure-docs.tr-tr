---
title: Sürekli derleme ve tümleştirme için Jenkins kullanarak Azure Service Fabric Linux uygulamalarınızı | Microsoft Docs
description: Sürekli derleme ve tümleştirme için Jenkins kullanarak Service Fabric Linux uygulamanızı
services: service-fabric
documentationcenter: java
author: sayantancs
manager: jpconnock
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/31/2018
ms.author: saysa
ms.openlocfilehash: 3b1e6f769d5c65065d95ac96c4ab4ed10702e5cf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61038840"
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Jenkins kullanarak Linux uygulamaları geliştirmek ve dağıtmak için kullanın
Jenkins, uygulamanızın sürekli tümleştirme ve dağıtımı için yaygın olarak kullanılan bir araçtır. Jenkins kullanarak Azure Service Fabric uygulamanızı derleme ve dağıtma işlemi aşağıda açıklanmaktadır.

## <a name="topic-overview"></a>Konusuna genel bakış
Bu makale, Jenkins ortamı ayarlama birkaç olası yolları yanı sıra, oluşturulduktan sonra uygulamanızı bir Service Fabric kümesine dağıtmanın farklı yöntemleri kapsar. Başarıyla Jenkins kurulumu, Github'dan değişiklikleri isteme, uygulamanızı oluşturmak ve kümenize dağıtmak için aşağıdaki genel adımları izleyin:

1. Yüklediğinizden emin olun [önkoşulları](#prerequisites).
1. Ardından Jenkins'i ayarlamak için aşağıdaki bölümlerden birindeki adımları izleyin:
   * [Jenkins'i bir Service Fabric kümesi içinde ayarlama](#set-up-jenkins-inside-a-service-fabric-cluster), 
   * [Jenkins'i bir Service Fabric kümesi dışında ayarlama](#set-up-jenkins-outside-a-service-fabric-cluster), veya
   * [Mevcut bir Jenkins ortamda Service Fabric eklentisini yüklemek](#install-service-fabric-plugin-in-an-existing-jenkins-environment).
1. Jenkins'i ayarladıktan sonra adımları [oluşturun ve bir Jenkins işi yapılandırma](#create-and-configure-a-jenkins-job) uygulamanıza değişiklik yapıldığında, GitHub'ı Jenkins tetikleyiciye ayarlayın ve çekme için derleme adımını Jenkins işi işlem hattınızı yapılandırmak için Github'dan değişiklikleri ve uygulamanızı oluşturun. 
1. Son olarak, uygulamanızı Service Fabric kümenize dağıtmak için Jenkins işi oluşturma sonrası adımı yapılandırın. Uygulamanızı bir kümeye dağıtmak için Jenkins yapılandırmanın iki yolu vardır:    
   * Geliştirme ve test ortamları için kullanma [küme yönetim uç noktasını kullanarak dağıtım yapılandırma](#configure-deployment-using-cluster-management-endpoint). Bunu ayarlamak için en basit dağıtım yöntemidir.
   * Üretim ortamları için kullanma [Azure kimlik bilgilerini kullanarak dağıtım yapılandırma](#configure-deployment-using-azure-credentials). Microsoft üretim ortamları için bu yöntem, çünkü Azure kimlik bilgilerinizle Azure kaynaklarınıza bir Jenkins işi sahip olduğu erişimi sınırlayabilirsiniz önerir. 

## <a name="prerequisites"></a>Önkoşullar

- Git yerel olarak yüklü olduğundan emin olun. Uygun Git sürümünü yükleyebilirsiniz [Git indirme sayfasından](https://git-scm.com/downloads) işletim sisteminize göre. Git'e yeniyseniz buradan hakkında daha fazla bilgi [Git belgeleri](https://git-scm.com/docs).
- Bu makalede *Service Fabric kullanmaya örneği alma* github'da: [ https://github.com/Azure-Samples/service-fabric-java-getting-started ](https://github.com/Azure-Samples/service-fabric-java-getting-started) uygulamanın derleme ve dağıtma. Örneği takip etmek için bu depoyu çatallaştırmanız veya yönergeleri için biraz değişiklikle, kendi GitHub projesini kullanın.


## <a name="install-service-fabric-plugin-in-an-existing-jenkins-environment"></a>Mevcut bir Jenkins ortamda Service Fabric eklentisini yükleme
Bir Jenkins ortamı için Service Fabric eklentisini ekliyorsanız, aşağıdakiler gerekir:

- [Service Fabric CLI](service-fabric-cli.md) (sfctl).

   > [!NOTE]
   > Kullanıcı düzeyinde, bu nedenle Jenkins CLI komutlarını çalıştırabilirsiniz yerine sistem düzeyindeki CLI'yı yüklemek emin olun. 
   >

- Java uygulamalarını dağıtmak için hem de yüklemeniz [Gradle ve Open JDK 8.0](service-fabric-get-started-linux.md#set-up-java-development). 
- .NET Core 2.0 uygulamaları dağıtmak için yükleme [.NET Core 2.0 SDK'sını](service-fabric-get-started-linux.md#set-up-net-core-20-development). 

Ortamınız için gereken önkoşulları yükledikten sonra Azure Service Fabric eklentisi Jenkins Market'te arayın ve yükleyin.

Eklentiyi yükledikten sonra atlayın [oluşturun ve bir Jenkins işi yapılandırma](#create-and-configure-a-jenkins-job).


## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins’i bir Service Fabric kümesi içinde ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümlerde, kümenin içinde kapsayıcı örneğinin durumunu kaydetmek için bir Azure depolama hesabını kullanırken nasıl ayarlandığı gösterilmektedir.

### <a name="prerequisites"></a>Önkoşullar
- Service Fabric Linux kümesi ile Docker'ın yüklü olması. Zaten Azure'da çalışan Service Fabric kümelerinde Docker'ın yüklü olması. Kümeyi yerel olarak (OneBox geliştirme ortamı) çalıştırıyorsanız, Docker ile makinenizde yüklü olup olmadığını denetleyin. `docker info` komutu. Yüklü değilse, aşağıdaki komutları kullanarak yükleyin:

   ```sh
   sudo apt-get install wget
   wget -qO- https://get.docker.io/ | sh
   ``` 

   > [!NOTE]
   > Küme üzerindeki özel bir uç noktası olarak 8081 bağlantı noktasının belirtildiğinden emin olun. Yerel bir küme kullanıyorsanız, bağlantı noktası 8081 konak makinesi üzerinde açık olduğunu ve genel kullanıma yönelik IP adresine sahip olduğundan emin olun.
   >

### <a name="steps"></a>Adımlar
1. Aşağıdaki komutları kullanarak uygulama kopyalayın:
   ```sh
   git clone https://github.com/suhuruli/jenkins-container-application.git
   cd jenkins-container-application
   ```

1. Bir dosya paylaşımındaki Jenkins kapsayıcı durumu Sürdür:
   1. Bir Azure depolama hesabı oluşturun **aynı bölgede** kümeniz gibi bir ad ile `sfjenkinsstorage1`.
   1. Oluşturma bir **dosya paylaşımı** altında depolama hesabınızı bir adla gibi `sfjenkins`.
   1. Tıklayarak **Connect** Not ve dosya paylaşımı için değerleri altında görüntüler **Linux'tan bağlanılıyor**, değeri aşağıdaki gibi görünmelidir:

      ```sh
      sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
      ```

   > [!NOTE]
   > İçin bağlama CIFS shares, CIFS-utils paketi küme düğümlerinde yüklü olması gerekir.      
   >

1. Yer tutucu değerleri güncelleştirmek `setupentrypoint.sh` 2. adımda azure depolama ayrıntılarını içeren bir betik.
   ```sh
   vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
   ```
   * Değiştirin `[REMOTE_FILE_SHARE_LOCATION]` değerle `//sfjenkinsstorage1.file.core.windows.net/sfjenkins` Bağlan çıktısından yukarıdaki 2. adım.
   * Değiştirin `[FILE_SHARE_CONNECT_OPTIONS_STRING]` değerle `vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777` gelen yukarıdaki 2. adım.

1. **Yalnızca güvenli küme:** 
   
   Jenkins güvenli bir kümeden uygulamalarının dağıtımını yapılandırmak için küme sertifikası Jenkins kapsayıcı içinde erişilebilir olması gerekir. İçinde *ApplicationManifest.xml* altında dosya **Healthcheck** etiketi bu sertifika başvurusu ekleyin ve, küme sertifikası parmak izi değerini güncelleştirin.

   ```xml
   <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
   ```

   Ayrıca, altına aşağıdaki satırları ekleyin **ApplicationManifest** (kök) etiketinde *ApplicationManifest.xml* dosya ve, küme sertifikası parmak izi değerini güncelleştirin.

   ```xml
   <Certificates>
     <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="[Thumbprint]" />
   </Certificates> 
   ```

1. Kümeye bağlanın ve kapsayıcı uygulamasını yükleyin.

   **Küme güvenliğini sağlama**
   ```sh
   sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
   bash Scripts/install.sh
   ```
   Yukarıdaki komut, sertifika PEM biçiminde alır. Sertifikanızı PFX biçimindeyse dönüştürmek için aşağıdaki komutu kullanabilirsiniz. PFX dosyanızı parola korumalı değilse belirtin **passin** parametre olarak `-passin pass:`.
   ```sh
   openssl pkcs12 -in cert.pfx -out cert.pem -nodes -passin pass:MyPassword1234!
   ``` 
   
   **Güvenli olmayan küme**
   ```sh
   sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
   bash Scripts/install.sh
   ```

   Bu işlem, kümeye bir Jenkins kapsayıcısı yükler ve küme, Service Fabric Explorer kullanılarak izlenebilir.

   > [!NOTE]
   > Bu, birkaç kümede indirilmesi Jenkins görüntüsü için dakika sürebilir.
   >

1. Tarayıcınızdan `http://PublicIPorFQDN:8081` sayfasına gidin. Bu işlem, oturum açmak için gereken ilk yönetici parolasının yolunu sağlar. 
1. Jenkins kapsayıcı hangi düğüm üzerinde çalıştığını belirlemek için Service Fabric Explorer'a arayın. Bu düğüm için kabuk (SSH) oturum açma güvenliğini sağlayın.
   ```sh
   ssh user@PublicIPorFQDN -p [port]
   ``` 
1. `docker ps -a` kullanarak kapsayıcı örneğinin kimliğini alın.
1. Güvenli Kabuk (SSH) oturum açma kapsayıcıya ve Jenkins portalında gösterilen yolu yapıştırın. Örneğin, portalda bunu yolu gösteriliyorsa `PATH_TO_INITIAL_ADMIN_PASSWORD`, aşağıdaki komutları çalıştırın:

   ```sh
   docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
   ```
   ```sh
   cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the password value
   ```
1. Jenkins Başlarken sayfasında seçeneğini yüklemek, seçmek için Seç eklentileri seçin **hiçbiri** onay kutusu seçeneğine tıklayıp yükleyin.
1. Bir kullanıcı oluşturabilir veya bir yönetici olarak devam etmek için seçin

Jenkins'i ayarladıktan sonra atlayın [oluşturun ve bir Jenkins işi yapılandırma](#create-and-configure-a-jenkins-job).  

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins’i Service Fabric kümesi dışında ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümlerde, kümenin dışında nasıl ayarlandığı gösterilmektedir.

### <a name="prerequisites"></a>Önkoşullar
- Docker, makinenizde yüklü olduğundan emin olun. Terminalden Docker yüklemek için aşağıdaki komutlar kullanılabilir:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

  Çalıştırdığınızda `docker info` terminalde, Docker hizmetinin çalıştığını çıktıyı göstermelidir.

### <a name="steps"></a>Adımlar
1. Service Fabric Jenkins kapsayıcı görüntüsünü çekin: `docker pull rapatchi/jenkins:latest`. Bu görüntü, Service Fabric Jenkins eklentisi önceden yüklenmiş şekilde gelir.
1. Kapsayıcı görüntüsünü çalıştırın:`docker run -itd -p 8080:8080 rapatchi/jenkins:latest`
1. Kapsayıcı görüntüsü örneğinin kimliğini alın. `docker ps –a` komutuyla tüm Docker kapsayıcılarını listeleyebilirsiniz
1. Aşağıdaki adımlar ile Jenkins portalında oturum açın:

   1. Ana bilgisayarınızdan Jenkins kabuğunda oturum açın. Kapsayıcı kimliği. ilk dört hanesi kullanın Örneğin, kapsayıcı kimliği ise `2d24a73b5964`, kullanın `2d24`.

      ```sh
      docker exec -it [first-four-digits-of-container-ID] /bin/bash
      ```
   1. Jenkins kabuğundan, kapsayıcı Örneğinize için yönetici parolasını alın:

      ```sh
      cat /var/jenkins_home/secrets/initialAdminPassword
      ```      
   1. Jenkins panosunda oturum açmak için bir web tarayıcısında aşağıdaki URL'yi açın: `http://<HOST-IP>:8080`. Jenkins kilidini açmak için önceki adımı parolayı kullanın.
   1. (İsteğe bağlı.) İlk kez oturum açtıktan sonra kendi kullanıcı hesabınızı oluşturabilir ve bunu için aşağıdaki adımları kullanın veya yönetici hesabını kullanmaya devam edebilirsiniz. Bir kullanıcı oluşturursanız, bu kullanıcıyla devam etmeniz gerekir.
1. GitHub'ı adımları kullanarak Jenkins ile çalışacak şekilde [yeni bir SSH anahtarı oluşturma ve SSH aracısına ekleme](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
   * GitHub’dan sağlanan yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran GitHub hesabına SSH anahtarını ekleyin.
   * Önceki bağlantıda belirtilen komutları Jenkins Docker kabuğunda (ana bilgisayarınızda değil) çalıştırın.
   * Ana bilgisayarınızdan Jenkins kabuğunda oturum açmak için aşağıdaki komutu kullanın:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Küme veya Jenkins kapsayıcı görüntüsünün barındırıldığı makine genel kullanıma yönelik IP adresi olduğundan emin olun. Bunun olması, Jenkins örneğinin GitHub'dan bildirimler almasını sağlar.

Jenkins'i ayarladıktan sonra bir sonraki bölüm açın devam [oluşturun ve bir Jenkins işi yapılandırma](#create-and-configure-a-jenkins-job).

## <a name="create-and-configure-a-jenkins-job"></a>Bir Jenkins işi oluşturma ve yapılandırma

Bu bölümdeki adımları GitHub deposunda değişikliklerine yanıt verme, değişiklikleri getirmek ve bunları oluşturmak için bir Jenkins işini yapılandırma işlemini göstermektedir. Bu bölümün sonunda, geliştirme/test ortamı veya üretim ortamına dağıtım yapıyorsanız üzerinde göre uygulamanızı dağıtmak için iş yapılandırmak için son adımlara yönlendirilirsiniz. 

1. Jenkins panosunda **yeni öğe**.
1. Bir öğe adını girin (örneğin, **MyJob**). **Serbest stil proje**’yi seçip **Tamam**’a tıklayın.
1. Proje yapılandırma sayfası açılır. (Jenkins panosundan yapılandırmayı almak için işe tıklayın ve ardından **yapılandırma**).

1. Üzerinde **genel** sekmesinde, onay kutusunu için **GitHub projesini**, ve GitHub projesi URL'nizi belirtin. Bu URL, Jenkins sürekli tümleştirme, sürekli dağıtım (CI/CD) akışı ile tümleştirmek istediğiniz Service Fabric Java uygulamasını barındırır (örneğin, `https://github.com/{your-github-account}/service-fabric-java-getting-started`).

1. Üzerinde **kaynak kodu Yönetimi** sekmesinde **Git**. Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran deponun URL'sini belirtin (örneğin, `https://github.com/{your-github-account}/service-fabric-java-getting-started`). Hangi dalın derleneceğini belirtebilirsiniz (örneğin, `/master`).
1. Yapılandırma, *GitHub* Jenkins ile konuşabilecek şekilde depo:

   a. GitHub depo sayfanıza üzerinde Git **ayarları** > **tümleştirmeler ve Hizmetler**.

   b. **Hizmet Ekle**’yi seçin, **Jenkins** yazın ve **Jenkins-GitHub eklentisi**’ni seçin.

   c. Jenkins web kancası URL'nizi girin (varsayılan olarak, `http://<PublicIPorFQDN>:8081/github-webhook/` olmalıdır). **Hizmet ekle/güncelleştir** öğesine tıklayın.

   d. Jenkins örneğinize bir test olayı gönderilir. GitHub’da web kancasının yanında yeşil renkli bir onay işareti görürsünüz ve projeniz derlenir.

1. Üzerinde **derleme Tetikleyicileri** sekmesinde Jenkins, hangi istediğiniz derleme seçeneğini belirleyin. Bu örnekte, istediğiniz bir depoya gönderme gerçekleştiğinde bir derleme tetiklemeyi, bu nedenle seçin **Gıtscm yoklaması için GitHub kanca tetikleyicisi**. (Daha önce bu seçenek **GitHub’a bir değişiklik uygulandığında derle** olarak adlandırılıyordu.)
1. Üzerinde **derleme** sekmesinde, bir Java uygulaması veya bir .NET Core uygulaması oluşturmakta olduğunuz bağlı olarak aşağıdakilerden birini yapın:

   * **Java uygulamaları için:** Gelen **derleme adımı Ekle** açılan listesinde, select **Gradle betiğini Çağır**. **Gelişmiş**'e tıklayın. Gelişmiş menüsünün yolunu belirtin **kök derleme betiği** uygulamanız için. Belirtilen yoldan build.gradle’ı alır ve buna göre çalışır. İçin [ActorCounter uygulama](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/reliable-services-actor-sample/Actors/ActorCounter), budur: `${WORKSPACE}/reliable-services-actor-sample/Actors/ActorCounter`.

     ![Service Fabric Jenkins Derleme eylemi][build-step]

   * **.NET Core uygulamaları için:** Gelen **derleme adımı Ekle** açılan listesinde, select **Kabuğu Yürüt**. Görüntülenen komut kutusunda dizini ilk build.sh dosyasının bulunduğu yolu değiştirilmesi gerekir. Dizin değiştirilmişse build.sh betik çalıştırılabilir ve uygulaması oluşturacaksınız.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

     Aşağıdaki ekran oluşturmak için kullanılan komutlar örneği gösterilmektedir [sayacı hizmetinin](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) CounterServiceApplication bir Jenkins işi adı ile örnek.

      ![Service Fabric Jenkins Derleme eylemi][build-step-dotnet]

1. Derleme sonrası eylemlerde Service Fabric kümesinde uygulamanızı dağıtmak için Jenkins yapılandırmak için küme sertifikasının konumunu Jenkins kapsayıcınızı gerekir. Jenkins kapsayıcı içinde veya dışında kümenizi kullanılıp kullanılmadığını bağlı olarak aşağıdakilerden birini seçin ve küme sertifikasının konumunu not edin:

   * **Kümenizi içinde çalışan Jenkins için:** Sertifika yolu değerini Yankı tarafından bulunabilir *Certificates_JenkinsOnSF_Code_MyCert_PEM* kapsayıcısındaki ortam değişkeninden.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
   * **Kümenizi dışında çalışan Jenkins için:** Küme sertifikası kapsayıcınıza kopyalamak için aşağıdaki adımları izleyin:
     1. Sertifikanızı PEM biçiminde olması gerekir. PEM dosyası yoksa, bir sertifika PFX dosyasını oluşturabilirsiniz. PFX dosyanızı parola korumalı değilse, ana bilgisayarından aşağıdaki komutu çalıştırın:

        ```sh
        openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:
        ``` 

        PFX dosyasını parola korumalıysa parolayı dahil `-passin` parametresi. Örneğin:

        ```sh
        openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:MyPassword1234!
        ``` 

     1. Jenkins kapsayıcı için kapsayıcı Kimliği almak için çalıştırın `docker ps` , ana bilgisayarından.
     1. PEM dosyasını aşağıdaki Docker komutu ile kapsayıcınız kopyalayın:
    
        ```sh
        docker cp clustercert.pem [first-four-digits-of-container-ID]:/var/jenkins_home
        ``` 

Neredeyse bitti! Jenkins işi açık tutun. Yalnızca kalan görev uygulamanızı Service Fabric kümenize dağıtmak için derleme sonrası adımları yapılandırmaktır:

* Bir geliştirme veya test ortamına dağıtmak için adımları izleyin. [küme yönetim uç noktasını kullanarak dağıtım yapılandırma](#configure-deployment-using-cluster-management-endpoint).
* Bir üretim ortamına dağıtmak için adımları izleyin. [Azure kimlik bilgilerini kullanarak dağıtım yapılandırma](#configure-deployment-using-azure-credentials).

## <a name="configure-deployment-using-cluster-management-endpoint"></a>Küme yönetim uç noktasını kullanarak dağıtımı yapılandırma
Geliştirme ve test ortamları için küme yönetim uç noktası, uygulamanızı dağıtmak için kullanabilirsiniz. Oluşturma sonrası eylem uygulamanızı dağıtmak için küme yönetim uç noktası ile yapılandırma ayarı en az miktarda gerektirir. Bir üretim ortamına dağıtıyorsanız atlayın [Azure kimlik bilgilerini kullanarak dağıtım yapılandırma](#configure-deployment-using-azure-credentials) dağıtımı sırasında kullanılacak bir Azure Active Directory Hizmet sorumlusunu yapılandırmak için.    

1. Jenkins işi tıklayın **derleme sonrası eylemlerde** sekmesi. 
1. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. 
1. Altında **Service Fabric küme yapılandırması**seçin **Service Fabric yönetim uç noktası dolgu** radyo düğmesi.
1. İçin **yönetim konak**, bağlantı uç noktasını girin kümeniz için; örneğin `{your-cluster}.eastus.cloudapp.azure.com`.
1. İçin **istemci anahtarı** ve **Client Cert**, PEM dosyasının konumunu girin, Jenkins kapsayıcı içinde; örneğin `/var/jenkins_home/clustercert.pem`. (Son adımı, sertifikanın konumuna kopyalanan [oluşturun ve bir Jenkins işi yapılandırma](#create-and-configure-a-jenkins-job).)
1. Altında **uygulama yapılandırması**, yapılandırma **uygulama adı**, **uygulama türü**ve (göreli) **uygulama bildirimiyolu** alanları.

   ![Service Fabric Jenkins derleme sonrası eylem yönetim uç noktasını yapılandırma](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-endpoint.png)

1. Tıklayın **yapılandırmasını doğrulama**. Başarılı doğrulamayı tıklayın **Kaydet**. Jenkins işlem hattınızı artık tam olarak yapılandırılmıştır. Atlayın [sonraki adımlar](#next-steps) dağıtımı test etmek.

## <a name="configure-deployment-using-azure-credentials"></a>Azure kimlik bilgilerini kullanarak dağıtımı yapılandırma
Üretim ortamları için uygulamanızı dağıtmak için bir Azure kimlik bilgileri yapılandırılırken önemle tavsiye edilir. Bu bölümde, oluşturma sonrası eylem uygulamanızda dağıtmak için bir Azure Active Directory Hizmet sorumlusu yapılandırma işlemini göstermektedir. Hizmet sorumluları, Jenkins işi izinleri sınırlamak için dizininizde rollerine atayabilirsiniz. 

Geliştirme ve test ortamları için Azure kimlik bilgileri veya uygulamanızı dağıtmak için küme yönetim uç noktası yapılandırabilirsiniz. Bir küme yönetim uç noktasını yapılandırma hakkında daha fazla bilgi için bkz. [küme yönetim uç noktasını kullanarak dağıtım yapılandırma](#configure-deployment-using-cluster-management-endpoint).   

1. Azure Active Directory Hizmet sorumlusu oluşturma ve Azure aboneliğinizde izinleri atamak için adımları [Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Aşağıdakilere dikkat edin:

   * Konu başlığındaki adımları uygulayarak sırasında kopyalayın ve aşağıdaki değerleri kaydedin emin olun: *Uygulama Kimliği*, *uygulama anahtarı*, *dizin kimliği (Kiracı kimliği)* , ve *abonelik kimliği*. Jenkins Azure kimlik bilgileri yapılandırmak için ihtiyaç.
   * Öğeniz yoksa [gerekli izinler](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) dizininize izinleri verin ya da hizmet sorumlusu oluşturmak için bir yöneticiden gerekir ya da Yönetim uç noktası için yapılandırmanız gerekir, içindeki küme **derleme sonrası eylemlerde** jenkins'te işiniz için.
   * İçinde [bir Azure Active Directory uygulaması oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#create-an-azure-active-directory-application) bölümünde iyi biçimlendirilmiş bir URL için girebilirsiniz **oturum açma URL'si**.
   * İçinde [uygulamayı bir Role Atama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) bölümünde, uygulamanız atayabilirsiniz *okuyucu* kümeniz için kaynak grubu üzerinde rol.

1. Jenkins işi geri, tıklayın **derleme sonrası eylemlerde** sekmesi.
1. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. 
1. Altında **Service Fabric küme yapılandırması**seçin **Service Fabric kümesi seçin** radyo düğmesi. Tıklayın **Ekle** yanındaki **Azure kimlik bilgileri**. Tıklayın **Jenkins** Jenkins kimlik sağlayıcısı seçin.
1. Jenkins kimlik sağlayıcısı seçin **Microsoft Azure hizmet sorumlusu** gelen **tür** açılır.
1. 1\. adımında aşağıdaki alanları ayarlamak için hizmet sorumlunuzu'kurmak ayarlarken kaydedilmiş değerleri kullanın:

   * **İstemci kimliği**: *Uygulama Kimliği*
   * **İstemci gizli anahtarı**: *Uygulama anahtarı*
   * **Kiracı kimliği**: *Dizin kimliği*
   * **Abonelik kimliği**: *Abonelik kimliği*
1. Açıklayıcı bir girin **kimliği** Jenkins ve kısa bir kimlik bilgisi seçmek için kullandığınız **açıklama**. Ardından **doğrulayın hizmet sorumlusu**. Doğrulama başarılı olursa tıklayın **Ekle**.

   ![Service Fabric Jenkins Azure kimlik bilgilerini girin](./media/service-fabric-cicd-your-linux-application-with-jenkins/enter-azure-credentials.png)
1. Geri altında **Service Fabric küme yapılandırması**, yeni kimlik bilgileriniz için seçili olduğundan emin olun **Azure kimlik bilgileri**. 
1. Gelen **kaynak grubu** açılan listesinde, uygulamayı dağıtmak istediğiniz küme kaynak grubunu seçin.
1. Gelen **Service Fabric** açılan listesinde, uygulamayı dağıtmak istediğiniz kümeyi seçin.
1. İçin **istemci anahtarı** ve **Client Cert**, Jenkins kapsayıcınızı PEM dosyasının konumunu girin. Örneğin, `/var/jenkins_home/clustercert.pem`. 
1. Altında **uygulama yapılandırması**, yapılandırma **uygulama adı**, **uygulama türü**ve (göreli) **uygulama bildirimiyolu** alanları.
    ![Service Fabric Jenkins derleme sonrası eylem Azure kimlik bilgilerini yapılandırma](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-credentials.png)
1. Tıklayın **yapılandırmasını doğrulama**. Başarılı doğrulamayı tıklayın **Kaydet**. Jenkins işlem hattınızı artık tam olarak yapılandırılmıştır. Geçin [sonraki adımlar](#next-steps) dağıtımı test etmek.

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisiyle ilgili sorunları giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
GitHub ve Jenkins yapılandırılmıştır. Bazı örnek yapmayı düşünün `reliable-services-actor-sample/Actors/ActorCounter` depo çatalınızda bir projede https://github.com/Azure-Samples/service-fabric-java-getting-started. Değişikliklerinizi uzak konuma itme `master` dal (veya çalışmak üzere yapılandırdığınız herhangi bir dala). Bunun yapılması, yapılandırmış olduğunuz `MyJob` Jenkins işini tetikler. Github'dan değişiklikleri getirir, derler ve derleme sonrası eylemlerde belirttiğiniz küme uygulamayı dağıtır.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png

