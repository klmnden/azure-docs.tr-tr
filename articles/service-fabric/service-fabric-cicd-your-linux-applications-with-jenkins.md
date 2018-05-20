---
title: Sürekli yapı ve Jenkins kullanarak, Azure Service Fabric Linux uygulamalarınız için tümleştirme | Microsoft Docs
description: Sürekli yapı ve Jenkins kullanarak Service Fabric Linux uygulamanız için tümleştirme
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: ''
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: saysa
ms.openlocfilehash: 047b3d00da4f192febeeab79c9c87b67a8a0489b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Jenkins Linux uygulamaları geliştirmek ve dağıtmak için kullanın
Jenkins, uygulamanızın sürekli tümleştirme ve dağıtımı için yaygın olarak kullanılan bir araçtır. Jenkins kullanarak Azure Service Fabric uygulamanızı derleme ve dağıtma işlemi aşağıda açıklanmaktadır.

## <a name="topic-overview"></a>Konusuna genel bakış
Bu makalede, birkaç olası yolları Jenkins ortamınızı ayarlamanın yanı sıra, oluşturulduktan sonra bir Service Fabric kümesi uygulamanızı dağıtmak için farklı yollar ele alınmaktadır. Başarılı bir şekilde Jenkins kurulumu, değişiklikleri Github'da çekme, uygulamanızı ve kümenize dağıtmak için aşağıdaki genel adımları izleyin:

1. Yüklediğinizden emin olun [Önkoşullar](#prerequisites).
2. Ardından Jenkins ayarlamak için bu bölümleri birindeki adımları izleyin:
   * [Jenkins içinde bir Service Fabric kümesi](#set-up-jenkins-inside-a-service-fabric-cluster), 
   * [Jenkins dışında bir Service Fabric kümesi](#set-up-jenkins-outside-a-service-fabric-cluster), veya
   * [Varolan bir Jenkins ortamında Service Fabric eklentisini yükleme](#install-service-fabric-plugin-in-an-existing-jenkins-environment).
3. Jenkins ayarladıktan sonra adımları [oluşturma ve bir Jenkins işini yapılandırmadan](#create-and-configure-a-jenkins-job) uygulamanıza değişiklik yapıldığında GitHub Jenkins tetikleyiciye ayarlamak ve Jenkins iş hattınızı çıkarmak için derleme adımı üzerinden yapılandırmak için Github'dan değiştirir ve uygulamanızı oluşturun. 
4. Son olarak, Service Fabric kümesi uygulamanızı dağıtmak için Jenkins iş sonrası derleme adımı yapılandırın. Uygulamanızı bir kümeye dağıtmak için Jenkins yapılandırmanın iki yolu vardır:    
   * Geliştirme ve test ortamları için kullanmak [küme yönetim uç noktası kullanarak dağıtım yapılandırma](#configure-deployment-using-cluster-management-endpoint). Bunu ayarlamak için basit dağıtım yöntemidir.
   * Üretim ortamları için kullanmak [Azure kimlik bilgilerini kullanarak dağıtımı yapılandırma](#configure-deployment-using-azure-credentials). Microsoft Azure kimlik bilgileriyle Azure kaynaklarınızı Jenkins işin erişimi sınırlandırabilirsiniz kullandığından üretim ortamları için bu yöntem önerir. 

## <a name="prerequisites"></a>Önkoşullar

- Git yerel olarak yüklenmiş olduğundan emin olun. Uygun Git sürümünden yükleyebilir [Git yüklemeleri sayfası](https://git-scm.com/downloads) , işletim sistemi temelinde. Git için yeniyseniz, ondan hakkında daha fazla bilgi [Git belgelerine](https://git-scm.com/docs).
- Bu makalede kullanan *Service Fabric başlatıldı örnek alma* github'da: [ https://github.com/Azure-Samples/service-fabric-java-getting-started ](https://github.com/Azure-Samples/service-fabric-java-getting-started) uygulamanın oluşturmak ve dağıtmak. Bunu izlemek için bu depoyu çatallaştırmanız, veya, yönergeleri için bazı değişikliğe sahip kendi GitHub proje kullanabilirsiniz.


## <a name="install-service-fabric-plugin-in-an-existing-jenkins-environment"></a>Varolan bir Jenkins ortamında Service Fabric eklentisini yükleme
Mevcut bir Jenkins ortamına Service Fabric eklentisi ekliyorsanız, aşağıdakiler gerekir:

- [Service Fabric CLI](service-fabric-cli.md) (sfctl).

   > [!NOTE]
   > Kullanıcı düzeyinde, bu nedenle Jenkins CLI komutları çalıştırabilirsiniz yerine sistem düzeyinde CLI yüklediğinizden emin olun. 
   >

- Java uygulamalarını dağıtmak için hem de yüklemeniz [Gradle ve açık JDK 8.0](service-fabric-get-started-linux.md#set-up-java-development). 
- .NET Core 2.0 uygulamaları dağıtmak için yükleme [.NET Core 2.0 SDK](service-fabric-get-started-linux.md#set-up-net-core-20-development). 

Ortamınız için gereken önkoşulları yükledikten sonra Azure Service Fabric eklentisi Jenkins pazarında arayın ve yükleyin.

İçin eklentiyi yükledikten sonra'nın İleri atlayabilirsiniz [oluşturma ve Jenkins iş yapılandırma](#create-and-configure-a-jenkins-job).


## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins’i bir Service Fabric kümesi içinde ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümlerde, bir küme içinde kapsayıcı örneğinin durumunu kaydetmek için Azure storage hesabı kullanırken nasıl ayarlanacağını gösterir.

### <a name="prerequisites"></a>Önkoşullar
- Service Fabric Linux kümesi ile Docker yüklü olması. Zaten Azure'da çalışan Service Fabric kümeleri yüklü Docker sahiptir. Küme yerel olarak (OneBox geliştirme ortamı) çalıştırıyorsanız, Docker ile makinenizin üzerinde yüklü olup olmadığını denetleyin `docker info` komutu. Yüklenmemişse, aşağıdaki komutları kullanarak yükleyin:

   ```sh
   sudo apt-get install wget
   wget -qO- https://get.docker.io/ | sh
   ``` 

   > [!NOTE]
   > 8081 bağlantı noktası küme üzerinde özel bir uç noktası olarak belirtildiğinden emin olun. Yerel bir küme kullanıyorsanız, bağlantı noktası 8081 konak makinesi üzerinde açık olduğunu ve bir genel kullanıma yönelik IP adresine sahip olduğundan emin olun.
   >

### <a name="steps"></a>Adımlar
1. Uygulama, aşağıdaki komutları kullanarak kopyalama:
   ```sh
   git clone https://github.com/suhuruli/jenkins-container-application.git
   cd jenkins-container-application
   ```

3. Bir dosya paylaşımı Jenkins kapsayıcısında durumunu Sürdür:
   1. Bir Azure depolama hesabı oluşturun **aynı bölgede** kümeniz gibi bir adla `sfjenkinsstorage1`.
   2. Oluşturma bir **dosya paylaşımı** altında depolama hesap adı ile gibi `sfjenkins`.
   3. Tıklayın **Bağlan** Not ve dosya paylaşımı için değerleri altında görüntülediği **Linux bağlanma**, değer birine benzer görünmelidir:

      ```sh
      sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
      ```

   > [!NOTE]
   > Bağlama CIFS paylaşımlar için küme düğümlerinde yüklü CIFS yardımcı programları paketi olması gerekir.      
   >

4. Yer tutucu değerlerini güncelleştirmek `setupentrypoint.sh` 2. adımda azure depolama ayrıntılarla komut dosyası.
   ```sh
   vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
   ```
   * Değiştir `[REMOTE_FILE_SHARE_LOCATION]` değerle `//sfjenkinsstorage1.file.core.windows.net/sfjenkins` Bağlan çıktısından yukarıdaki 2. adım.
   * Değiştir `[FILE_SHARE_CONNECT_OPTIONS_STRING]` değerle `vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777` gelen yukarıdaki 2. adım.

5. **Yalnızca güvenli küme:** 
   
   Jenkins güvenli bir kümeden uygulamalarının dağıtımını yapılandırmak için küme sertifika Jenkins kapsayıcı içinde erişilebilir olması gerekir. İçinde *ApplicationManifest.xml* altında dosya **ContainerHostPolicies** etiketi bu sertifika başvurusu ekleyin ve, küme sertifika parmak izi değerini güncelleştirin.

   ```xml
   <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
   ```

   Ayrıca, altına aşağıdaki satırları ekleyin **ApplicationManifest** (kök) etiketinde *ApplicationManifest.xml* dosya ve, küme sertifika parmak izi değerini güncelleştirin.

   ```xml
   <Certificates>
     <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="[Thumbprint]" />
   </Certificates> 
   ```

6. Kümeye bağlanın ve kapsayıcı uygulamayı yükleyin.

   **Güvenli küme**
   ```sh
   sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
   bash Scripts/install.sh
   ```
   Yukarıdaki komut sertifikayı PEM biçiminde alır. Sertifikanızı PFX biçiminde ise dönüştürülebilmesi için aşağıdaki komutu kullanabilirsiniz. PFX dosyanızı parola korumalı değilse, belirtin **passin** parametre olarak `-passin pass:`.
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
   > Birkaç kümede indirilmesi Jenkins görüntüsü için dakika sürebilir.
   >

7. Tarayıcınızdan `http://PublicIPorFQDN:8081` sayfasına gidin. Bu işlem, oturum açmak için gereken ilk yönetici parolasının yolunu sağlar. 
2. Jenkins kapsayıcı hangi düğüm üzerinde çalışan belirlemek için Service Fabric Explorer arayın. Bu düğüm için kabuk (SSH) oturum açma güvenliğini sağlayın.
   ```sh
   ssh user@PublicIPorFQDN -p [port]
   ``` 
3. `docker ps -a` kullanarak kapsayıcı örneğinin kimliğini alın.
4. Güvenli Kabuk (SSH) oturum açma kapsayıcısı ve Jenkins portalında gösterilen yolu yapıştırın. Örneğin, isteğe bağlı olarak Portalı'nda yolunu gösterir `PATH_TO_INITIAL_ADMIN_PASSWORD`, aşağıdaki komutları çalıştırın:

   ```sh
   docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
   ```
   ```sh
   cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the password value
   ```
5. Jenkins Başlarken sayfasında seçeneği yüklemek, seçmek için Seç eklentileri seçin **hiçbiri** onay ve tıklatın yükleyin.
6. Bir kullanıcı oluşturabilir veya bir yönetici olarak devam etmek için seçin

Jenkins ayarladıktan sonra İleri için atlayabilirsiniz [oluşturma ve Jenkins iş yapılandırma](#create-and-configure-a-jenkins-job).  

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins’i Service Fabric kümesi dışında ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümlerde, kümenin dışında nasıl ayarlandığı gösterilmektedir.

### <a name="prerequisites"></a>Önkoşullar
- Docker makinenize yüklü olduğundan emin olun. Terminalden Docker yüklemek için aşağıdaki komutlar kullanılabilir:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

  Çalıştırdığınızda `docker info` terminale Docker hizmetinin çalıştığını çıktısı göstermelidir.

### <a name="steps"></a>Adımlar
1. Service Fabric Jenkins kapsayıcı görüntüsünü çekin: `docker pull rapatchi/jenkins:latest`. Bu görüntü, Service Fabric Jenkins eklentisi önceden yüklenmiş şekilde gelir.
2. Kapsayıcı görüntüsünü çalıştırın:`docker run -itd -p 8080:8080 rapatchi/jenkins:latest`
3. Kapsayıcı görüntüsü örneğinin kimliğini alın. `docker ps –a` komutuyla tüm Docker kapsayıcılarını listeleyebilirsiniz
4. Aşağıdaki adımlarla Jenkins portalında oturum açın:

   1. Ana bilgisayardan Jenkins kabuğu için oturum açın. Kapsayıcı kimliğinin ilk dört basamak kullanın Örneğin, kapsayıcı kimliği ise `2d24a73b5964`, kullanmak `2d24`.

      ```sh
      docker exec -it [first-four-digits-of-container-ID] /bin/bash
      ```
   2. Jenkins Kabuğu'ndan kapsayıcı Örneğiniz için yönetici parolasını alın:

      ```sh
      cat /var/jenkins_home/secrets/initialAdminPassword
      ```      
   3. Jenkins panoya oturum açmak için aşağıdaki URL'yi bir web tarayıcısında açın: `http://<HOST-IP>:8080`. Jenkins kilidini açmak için parola önceki adımdan kullanın.
   4. (İsteğe bağlı.) İlk kez oturum açtıktan sonra kendi kullanıcı hesabı oluşturun ve için aşağıdaki adımları kullanın veya yönetici hesabını kullanmaya devam edebilirsiniz. Kullanıcı oluşturursanız, bu kullanıcı ile devam etmek için gerekir.
5. GitHub adımları kullanarak Jenkins ile çalışacak biçimde ayarlamak [yeni bir SSH anahtarı oluşturma ve SSH aracı ekleyerek](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
   * GitHub’dan sağlanan yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran GitHub hesabına SSH anahtarını ekleyin.
   * Önceki bağlantıda belirtilen komutları Jenkins Docker kabuğunda (ana bilgisayarınızda değil) çalıştırın.
   * Ana bilgisayarınızdan Jenkins kabuğunda oturum açmak için aşağıdaki komutu kullanın:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Küme veya Jenkins kapsayıcı görüntü barındırıldığı makine genel kullanıma yönelik IP adresi olduğundan emin olun. Bunun olması, Jenkins örneğinin GitHub'dan bildirimler almasını sağlar.

Jenkins ayarladıktan sonra bir sonraki bölüm açın devam [oluşturma ve Jenkins iş yapılandırma](#create-and-configure-a-jenkins-job).

## <a name="create-and-configure-a-jenkins-job"></a>Bir Jenkins işi oluşturma ve yapılandırma

Bu bölümdeki adımları nasıl GitHub deposuna değişikliklere yanıt, değişiklikleri getirin ve bunları oluşturmak için bir Jenkins iş yapılandırılacağını gösterir. Bu bölümün sonunda, geliştirme ve test ortamı veya üretim ortamına dağıtma üzerinde temelinde uygulamanızı dağıtmak için iş yapılandırmak için son adımlar yönlendirilirsiniz. 

1. Jenkins panosunda tıklatın **yeni öğe**.
2. Bir öğe adını girin (örneğin, **MyJob**). **Serbest stil proje**’yi seçip **Tamam**’a tıklayın.
3. İş yapılandırma sayfası açılır. (Jenkins panodan yapılandırmasına dönmek için iş'i tıklatın ve ardından **yapılandırma**).

4. Üzerinde **genel** sekmesinde, onay kutusunu için **GitHub proje**, ve GitHub proje URL'sini belirtin. Bu URL, Jenkins sürekli tümleştirme, sürekli dağıtım (CI/CD) akışı ile tümleştirmek istediğiniz Service Fabric Java uygulamasını barındırır (örneğin, `https://github.com/{your-github-account}/service-fabric-java-getting-started`).

5. Üzerinde **kaynak kodu Yönetimi** sekmesine **Git**. Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran deponun URL'sini belirtin (örneğin, `https://github.com/{your-github-account}/service-fabric-java-getting-started`). Ayrıca oluşturmak için hangi şube belirtebilirsiniz (örneğin, `/master`).
6. Yapılandırma, *GitHub* için Jenkins konuşmaya deposu:

   a. GitHub depo sayfanızda Git **ayarları** > **tümleştirmeler ve Hizmetleri**.

   b. **Hizmet Ekle**’yi seçin, **Jenkins** yazın ve **Jenkins-GitHub eklentisi**’ni seçin.

   c. Jenkins web kancası URL'nizi girin (varsayılan olarak, `http://<PublicIPorFQDN>:8081/github-webhook/` olmalıdır). **Hizmet ekle/güncelleştir** öğesine tıklayın.

   d. Jenkins örneğinize bir test olayı gönderilir. GitHub’da web kancasının yanında yeşil renkli bir onay işareti görürsünüz ve projeniz derlenir.

7. Üzerinde **yapı tetikleyicileri** sekmesinde Jenkins içinde hangi oluşturmak istediğiniz seçeneği seçin. Bu örnek için istediğiniz deponuza push olur her bir derlemeyi tetiklemek için bu nedenle seçin **GITScm yoklama için GitHub kanca tetikleyici**. (Daha önce bu seçenek **GitHub’a bir değişiklik uygulandığında derle** olarak adlandırılıyordu.)
8. Üzerinde **yapı** sekmesinde, bir Java uygulaması veya .NET Core uygulama oluşturmakta olduğunuz bağlı olarak aşağıdakilerden birini yapın:

   * **Java uygulamalarını için:** gelen **Ekle derleme adımı** açılan listesinde, select **çağırma Gradle betik**. Tıklatın **Gelişmiş**. Gelişmiş menüde yolunu belirtin **kök yapı betik** uygulamanız için. Belirtilen yoldan build.gradle’ı alır ve buna göre çalışır. İçin [ActorCounter uygulama](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/reliable-services-actor-sample/Actors/ActorCounter), bu: `${WORKSPACE}/reliable-services-actor-sample/Actors/ActorCounter`.

     ![Service Fabric Jenkins Derleme eylemi][build-step]

   * **.NET Core uygulamaları için:** gelen **Ekle derleme adımı** açılan listesinde, select **yürütme Kabuk**. Görüntülenen komutu kutusunda dizinin ilk build.sh dosyasının bulunduğu yolu değiştirilmesi gerekir. Dizin değiştirilmişse build.sh komut dosyası çalıştırılabilir ve uygulamayı oluşturacaksınız.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

     Aşağıdaki ekran görüntüsü oluşturmak için kullanılan komutlar örneği gösterilmektedir [sayacı hizmetini](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) CounterServiceApplication Jenkins iş adı ile örnek.

      ![Service Fabric Jenkins Derleme eylemi][build-step-dotnet]

9. Service Fabric kümesi oluşturma sonrası eylemleri uygulamanızı dağıtmak için Jenkins yapılandırmak için bu kümenin sertifikasının konumunu Jenkins kapsayıcısında gerekir. Jenkins kapsayıcı içinde veya dışında kümenizi kullanılıp kullanılmadığını bağlı olarak aşağıdakilerden birini seçin ve küme sertifikasının konumunu not edin:

   * **Kümenizin içinde çalışan Jenkins için:** değerini Yankı tarafından sertifika yolunu bulunabilir *Certificates_JenkinsOnSF_Code_MyCert_PEM* ortam değişkenini kapsayıcı içinde.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
   * **Küme dışında çalışan Jenkins için:** , kapsayıcıya küme Sertifikayı kopyalamak için aşağıdaki adımları izleyin:
      1. Sertifikanızı PEM biçiminde olması gerekir. PEM dosyası yoksa, bir sertifika PFX dosyasını oluşturabilirsiniz. PFX dosyanızı parola korumalı değilse, ana bilgisayardan aşağıdaki komutu çalıştırın:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:
         ``` 

      PFX dosyası parola korumalı ise, parolayı dahil `-passin` parametresi. Örneğin:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:MyPassword1234!
         ``` 

      2. Jenkins kapsayıcısı için kapsayıcı Kimliği almak için şunu çalıştırın `docker ps` , ana bilgisayardan.
      3. PEM dosyasını aşağıdaki Docker komut ile kapsayıcı kopyalayın:
    
         ```sh
         docker cp clustercert.pem [first-four-digits-of-container-ID]:/var/jenkins_home
         ``` 

Neredeyse tamamlandı! Jenkins iş açık tutun. Yalnızca kalan görev Service Fabric kümesi uygulamanızı dağıtmak için oluşturma sonrası adımları yapılandırmaktır:

* Bir geliştirme veya test ortamını dağıtmak için adımları [küme yönetim uç noktası kullanarak dağıtım yapılandırma](#configure-deployment-using-cluster-management-endpoint).
* Bir üretim ortamında dağıtmak için adımları [Azure kimlik bilgilerini kullanarak dağıtımı yapılandırma](#configure-deployment-using-azure-credentials).

## <a name="configure-deployment-using-cluster-management-endpoint"></a>Küme yönetim uç noktası kullanarak dağıtımı yapılandırma
Geliştirme ve test ortamları için küme yönetim uç noktası uygulamanızı dağıtmak için kullanabilirsiniz. Uygulamanızı dağıtmak için küme yönetimi uç noktası ile oluşturma sonrası eylem yapılandırma ayarı en az miktarda gerektirir. İçin üretim ortamına dağıtıyorsanız'ın İleri atlayabilirsiniz [Azure kimlik bilgilerini kullanarak dağıtımı yapılandırma](#configure-deployment-using-azure-credentials) dağıtımı sırasında kullanmak için bir Azure Active Directory hizmet asıl yapılandırmak için.    

1. Jenkins işinde tıklatın **oluşturma sonrası eylemleri** sekmesi. 
2. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. 
3. Altında **Service Fabric küme yapılandırması**seçin **hizmet doku Yönetimi uç dolgu** radyo düğmesi.
4. İçin **yönetim konak**, kümeniz için; bağlantı uç noktasının girin örneğin `{your-cluster}.eastus.cloudapp.azure.com`.
5. İçin **istemci anahtar** ve **istemci sertifikası**, PEM dosyasını konumunu girin, Jenkins kapsayıcısında; örneğin `/var/jenkins_home/clustercert.pem`. (Son adımı sertifikasının konumunu kopyaladığınız [oluşturma ve Jenkins iş yapılandırma](#create-and-configure-a-jenkins-job).)
6. Altında **uygulama yapılandırması**, yapılandırma **uygulama adı**, **uygulama türü**ve (göreli) **uygulama bildirimiyolu** alanları.

   ![Service Fabric Jenkins oluşturma sonrası eylem yapılandırma yönetim uç noktası](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-endpoint.png)

7. Tıklatın **yapılandırmasını doğrulama**. Başarılı doğrulamayı tıklatın **kaydetmek**. Jenkins iş hattınızı artık tam olarak yapılandırıldı. İçin İleri atlayabilirsiniz [sonraki adımlar](#next-steps) dağıtımınızı test etmek için.

## <a name="configure-deployment-using-azure-credentials"></a>Azure kimlik bilgilerini kullanarak dağıtımı yapılandırma
Üretim ortamları için uygulamanızı dağıtmak için bir Azure kimlik bilgileri yapılandırılırken önemle tavsiye edilir. Bu bölümde nasıl oluşturma sonrası eylem uygulamanızda dağıtmak için bir Azure Active Directory hizmet asıl yapılandırılacağını gösterir. Hizmet asıl adı Jenkins iş izinleri sınırlamak için dizininizde rolüne atayabilirsiniz. 

Geliştirme ve test ortamları için Azure kimlik veya uygulamanızı dağıtmak için küme yönetimi uç noktası yapılandırabilirsiniz. Bir küme yönetim uç noktasını yapılandırma hakkında ayrıntılı bilgi için bkz: [küme yönetim uç noktası kullanarak dağıtım yapılandırma](#configure-deployment-using-cluster-management-endpoint).   

1. Bir Azure Active Directory Hizmet sorumlusu oluşturmak ve Azure aboneliğinizde izinleri atamak için adımları [bir Azure Active Directory Uygulama ve hizmet sorumlusu oluşturmak için portal'ı kullanmanızı](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal). Aşağıdakilere dikkat edin:

   * Konusundaki adımları izleyerek sırasında kopyalayın ve aşağıdaki değerleri kaydedin emin olun: *uygulama kimliği*, *uygulama anahtarı*, *dizin kimliği (Kiracı kimliği)* ve *Abonelik kimliği*. Jenkins içinde Azure kimlik bilgilerini yapılandırmak için gereksinim duyarsınız.
   * Sahip değilseniz [gerekli izinleri](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) dizininize üzerinde izinleri verin veya sizin için hizmet sorumlusu oluşturmak için bir yöneticiden gerekir ya da Yönetim uç noktası için yapılandırmanız gerekir, içindeki küme **oluşturma sonrası eylemleri** Jenkins işinizde için.
   * İçinde [Azure Active Directory Uygulama oluşturma](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#create-an-azure-active-directory-application) bölümünde, iyi biçimlendirilmiş bir URL için girebilirsiniz **oturum açma URL'si**.
   * İçinde [uygulama rol atama](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) bölümünde, uygulamanız atayabilirsiniz *okuyucu* kümeniz için kaynak grubu üzerinde rol.

2. Geri Jenkins işinde tıklatın **oluşturma sonrası eylemleri** sekmesi.
3. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. 
4. Altında **Service Fabric küme yapılandırması**seçin **Service Fabric kümesi seçin** radyo düğmesi. Tıklatın **Ekle** yanına **Azure kimlik bilgilerini**. Tıklatın **Jenkins** Jenkins kimlik sağlayıcısı seçin.
5. Jenkins kimlik sağlayıcısı seçin **Microsoft Azure hizmet sorumlusu** gelen **türü** açılır.
6. 1. adımda aşağıdaki alanları ayarlamak için hizmet sorumlusu yukarı ayarlarken kaydedilmiş değerleri kullanın:

   * **İstemci kimliği**: *uygulama kimliği*
   * **İstemci parolası**: *uygulama anahtarı*
   * **Kiracı kimliği**: *dizin kimliği*
   * **Abonelik kimliği**: *abonelik kimliği*
6. Bir tanımlayıcı girin **kimliği** Jenkins ve kısa bir kimlik bilgisi seçmek için kullandığınız **açıklama**. Ardından **doğrulayın hizmet sorumlusu**. Doğrulama başarılı olursa, tıklatın **Ekle**.

   ![Service Fabric Jenkins Azure kimlik bilgilerini girin](./media/service-fabric-cicd-your-linux-application-with-jenkins/enter-azure-credentials.png)
7. Geri altında **Service Fabric küme yapılandırması**, yeni kimlik bilgileriniz için seçildiğinden emin olun **Azure kimlik bilgilerini**. 
8. Gelen **kaynak grubu** açılan listesinde, uygulamayı dağıtmak istediğiniz küme kaynak grubunu seçin.
9. Gelen **Service Fabric** açılan listesinde, uygulamayı dağıtmak istediğiniz kümeyi seçin.
10. İçin **istemci anahtar** ve **istemci sertifikası**, Jenkins kapsayıcısında PEM dosyasının konumunu girin. Örneğin, `/var/jenkins_home/clustercert.pem`. 
11. Altında **uygulama yapılandırması**, yapılandırma **uygulama adı**, **uygulama türü**ve (göreli) **uygulama bildirimiyolu** alanları.
    ![Service Fabric Jenkins oluşturma sonrası eylem Azure kimlik bilgilerini yapılandırma](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-credentials.png)
12. Tıklatın **yapılandırmasını doğrulama**. Başarılı doğrulamayı tıklatın **kaydetmek**. Jenkins iş hattınızı artık tam olarak yapılandırıldı. Oturum devam [sonraki adımlar](#next-steps) dağıtımınızı test etmek için.

## <a name="next-steps"></a>Sonraki adımlar
GitHub ve Jenkins yapılandırılmıştır. Bazı örnek değişiklik yapmayı düşünün `reliable-services-actor-sample/Actors/ActorCounter` , çatalı depo projesinde https://github.com/Azure-Samples/service-fabric-java-getting-started. Uzaktan değişikliklerinizi gönderme `master` şube (veya çalışmak üzere yapılandırdığınız herhangi bir dal). Bunun yapılması, yapılandırmış olduğunuz `MyJob` Jenkins işini tetikler. Github'dan değişiklikleri getirir, bunları oluşturur ve uygulama oluşturma sonrası eylemleri belirtilen kümeye dağıtır.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png

