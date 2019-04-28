---
title: Linux üzerinde Apache Tomcat sunucusu için bir Azure Service Fabric kapsayıcısı oluşturma | Microsoft Docs
description: Azure Service Fabric üzerinde Apache Tomcat sunucu üzerinde çalışan bir uygulamayı kullanıma sunmak için Linux kapsayıcısını oluşturun. Uygulama ve Apache Tomcat sunucusunu bir Docker görüntüsü oluşturun, görüntüyü bir kapsayıcı kayıt defterine iletin, oluşturun ve Service Fabric kapsayıcı uygulaması dağıtın.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/08/2018
ms.author: v-jamebr
ms.openlocfilehash: 5ae2ca352c6d3cbe02b659a97fe3147c1a31128f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60947464"
---
# <a name="create-service-fabric-container-running-apache-tomcat-server-on-linux"></a>Apache Tomcat sunucusunu Linux'ta çalışan Service Fabric kapsayıcı oluşturma
Apache Tomcat, Java Servlet ve Java sunucusu teknolojileri popüler, açık kaynaklı bir uygulamasıdır. Bu makalede, Apache Tomcat ve basit bir Web uygulaması ile bir kapsayıcı oluşturmak, Linux çalıştıran bir Service Fabric kümesine dağıtma ve Web uygulamasına bağlanma işlemini göstermektedir.  

Apache Tomcat hakkında daha fazla bilgi için bkz: [Apache Tomcat giriş sayfası](https://tomcat.apache.org/). 

## <a name="prerequisites"></a>Önkoşullar
* Şunları çalıştıran bir geliştirme bilgisayarı:
  * [Service Fabric SDK’sı ve araçları](service-fabric-get-started-linux.md).
  * [Linux için Docker CE](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric CLI](service-fabric-cli.md)

* Azure Container Registry, kapsayıcı kayıt defteri. Kullanarak Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturabilirsiniz [Azure portalında](../container-registry/container-registry-get-started-portal.md) veya [Azure CLI'yı](./service-fabric-tutorial-create-container-images.md#deploy-azure-container-registry). 

## <a name="build-a-tomcat-image-and-run-it-locally"></a>Tomcat görüntüsü derleme ve yerel olarak çalıştırma
Apache Tomcat görüntü ve basit bir Web uygulaması temel alan bir Docker görüntüsü oluşturun ve çalıştırın bir kapsayıcıda yerel sisteminizde için bu bölümdeki adımları izleyin. 
 
1. Kopya [Service Fabric Java ile çalışmaya başlama](https://github.com/Azure-Samples/service-fabric-java-getting-started) geliştirme bilgisayarınızda örnekleri deposu.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   ```

1. Dizinleri Apache Tomcat sunucusu örnek dizinine (*service-fabric-java-getting-started/container-apache-tomcat-web-server-sample*):

   ```bash
   cd service-fabric-java-getting-started/container-apache-tomcat-web-server-sample
   ```

1. Resmi alarak bir Docker dosyası oluştur [Tomcat görüntü](https://hub.docker.com/_/tomcat/) Docker Hub ve Tomcat sunucu örneği bulunur. İçinde *service-fabric-java-getting-started/container-apache-tomcat-web-server-sample* dizin adlı bir dosya oluşturun *Dockerfile* (hiçbir dosya uzantılı). Aşağıdakini *Dockerfile* dosyasına ekleyin ve değişikliklerinizi kaydedin:

   ```
   FROM library/tomcat

   EXPOSE 8080

   COPY ./ApacheTomcat /usr/local/tomcat
   ```

   Bkz: [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) daha fazla bilgi için.


4. Çalıştırma `docker build` web uygulamanızı çalıştıran görüntüyü oluşturmak için komutu:

   ```bash
   docker build . -t tomcattest
   ```

   Bu komut Dockerfile yönergeleri kullanarak yeni görüntüyü oluşturur adlandırma (-t etiketi) görüntü `tomcattest`. Kapsayıcı görüntüsünü oluşturmak için temel görüntü ilk aşağı Docker Hub'ından indirilir ve uygulama için eklenir. 

   Oluşturma komutu tamamlandıktan sonra, yeni görüntü üzerindeki bilgileri görmek için `docker images` komutunu çalıştırın:

   ```bash
   $ docker images
    
   REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
   tomcattest                    latest              86838648aab6        2 minutes ago       194 MB
   ```

5. Bu kapsayıcı kayıt defterine yerel olarak göndermeden önce kapsayıcıya alınmış uygulamanızı çalıştığını doğrulayın:
 
   ```bash
   docker run -itd --name tomcat-site -p 8080:8080 tomcattest.
   ```
   
   * `--name` Böylece kolay adı yerine kimliğini kullanarak başvurabilirsiniz kapsayıcı adları
   * `-p` kapsayıcı ile konak işletim sistemi arasındaki bağlantı noktası eşleme belirtir. 

   > [!Note]
   > Bağlantı noktası olan açık `-p` parametresi, Tomcat uygulama üzerinde istekleri dinlediği bağlantı noktası olmalıdır. Geçerli örnekte yapılandırılmış bir bağlayıcıyı yoktur *ApacheTomcat/conf/server.xml* 8080 bağlantı noktasında HTTP isteklerini dinlemek için dosya. Bu bağlantı noktasını konaktaki bağlantı noktası 8080 eşleştirilir. 

   Diğer parametreler hakkında bilgi edinmek için [Docker çalıştırmasından belgeleri](https://docs.docker.com/engine/reference/commandline/run/).

1. Kapsayıcınızı test etmek için bir tarayıcı açın ve aşağıdaki URL'lerden birini girin. "Hello World!" çeşidini görürsünüz. her URL için Hoş Geldiniz ekranı.

   - `http://localhost:8080/hello` 
   - `http://localhost:8080/hello/sayhello` 
   - `http://localhost:8080/hello/sayhi` 

   ![Hello world /sayhi](./media/service-fabric-get-started-tomcat/hello.png)

2. Kapsayıcıyı durdurmak ve Geliştirme bilgisayarınızdan silebilirsiniz:

   ```bash
   docker stop tomcat-site
   docker rm tomcat-site
   ```

## <a name="push-the-tomcat-image-to-your-container-registry"></a>Tomcat görüntüyü kapsayıcı kayıt defterinize gönderme
Tomcat görüntü geliştirme bilgisayarınızda bir kapsayıcıda çalıştığını doğruladıktan sonra bir kapsayıcı kayıt defteri deponuza gönderin. Bu makalede, görüntünün depolanması için Azure Container Registry kullanılır, ancak adımlar biraz değişiklikle, seçtiğiniz herhangi bir kapsayıcı kayıt defteri kullanabilirsiniz. Bu makalede kayıt adı olarak kabul edilir *myregistry* ve myregistry.azurecr.io tam kayıt defteri adıdır. Bunlar senaryonuz için uygun şekilde değiştirin. 

1. Kapsayıcı kayıt defterinizde [kayıt defteri kimlik bilgileriniz](../container-registry/container-registry-authentication.md) ile oturum açmak için `docker login` komutunu çalıştırın.

   Aşağıdaki örnekte, bir Azure Active Directory [hizmet sorumlusunun](../active-directory/develop/app-objects-and-service-principals.md) kimliği ve parolası geçirilmiştir. Örneğin, bir otomasyon senaryosu için kayıt defterinize bir hizmet sorumlusu atamış olabilirsiniz. İsterseniz, kayıt defteri kullanıcı kimliğiniz ve parolanızı kullanarak oturum açabilirsiniz.

   ```bash
   docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
   ```

2. Aşağıdaki komut, görüntünün kayıt defterinize ait tam yolu içeren bir etiketini veya diğer adını oluşturur. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için `samples` ad alanına görüntüyü yerleştirir.

   ```bash
   docker tag tomcattest myregistry.azurecr.io/samples/tomcattest
   ```

3. Görüntüyü kapsayıcı kayıt defterinize gönderin:

   ```bash
   docker push myregistry.azurecr.io/samples/tomcattest
   ```

## <a name="build-and-deploy-the-service-fabric-container-application"></a>Service Fabric kapsayıcı uygulaması derleme ve dağıtma
Bir kapsayıcı kayıt defterine Tomcat görüntü gönderildi, derleme ve Tomcat görüntüyü kayıt defterinizden çeker ve bir kapsayıcı hizmeti kümenizdeki olarak çalışan bir Service Fabric kapsayıcı uygulaması dağıtma. 

1. Yerel bir kopya dışında yeni bir dizin oluşturma (dışında *service-fabric-java-getting-started* dizin ağacı). Ona geçin ve kapsayıcılı bir uygulama için iskele oluşturma için Yeoman'ı kullanın: 

   ```bash
   yo azuresfcontainer 
   ```
   İstendiğinde aşağıdaki değerleri girin:

   * Uygulamanızı adlandırın: ServiceFabricTomcat
   * Uygulama hizmeti adı: TomcatService
   * Görüntü adı girin: Kapsayıcı kayıt defterindeki kapsayıcı görüntüsünün URL'sini sağlayın; Örneğin, myregistry.azurecr.io/samples/tomcattest.
   * Komutlar: Bunu boş bırakın. Bu görüntüde iş yükü giriş noktası tanımlanmış olduğundan, giriş komutlarının açıkça belirtilmesi gerekmez (komutlar kapsayıcının içinde çalıştırılır ve bu da başlatma sonrasında kapsayıcıyı çalışır durumda tutar).
   * Konuk kapsayıcı uygulamasının örnek sayısı: 1

   ![Kapsayıcılar için Service Fabric Yeoman oluşturucusu](./media/service-fabric-get-started-tomcat/yo-generator.png)

10. Hizmet bildirimindeki (*ServiceFabricTomcat/ServiceFabricTomcat/TomcatServicePkg/ServiceManifest.xml*), kök altında aşağıdaki XML'i ekleyin **ServiceManfest** etiketi bağlantı noktasını açmak için Uygulama istekleri dinlediği. **Uç nokta** uç noktası için bağlantı noktası ve protokol etiketi bildirir. Bu makalede kapsayıcı hizmeti 8080 bağlantı noktasında dinler: 

   ```xml
   <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
       listen. Please note that if your service is partitioned, this port is shared with 
       replicas of different partitions that are placed in your code. -->
      <Endpoint Name="endpointTest" Port="8080" Protocol="tcp"/>
    </Endpoints>
   </Resources>
   ```

11. Uygulama bildiriminde (*ServiceFabricTomcat/ServiceFabricTomcat/ApplicationManifest.xml*) altında **Servicemanifestımport** etiketi, aşağıdaki XML'i ekleyin. Değiştirin **AccountName** ve **parola** içinde **RepositoryCredentials** kapsayıcı kayıt defterinizde oturum açmak için gerekli parolayı ve ada sahip etiket.

   ```xml
   <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="8080" EndpointRef="endpointTest"/>
      <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
    </ContainerHostPolicies>
   </Policies>
   ```

   **Healthcheck** kapsayıcı konağında etkinleştirdiğiniz için ilkeleri etiketini belirtir.
    
   * **PortBinding** etiketi kapsayıcı bağlantı noktasından konak bağlantı noktası eşleme ilkesi yapılandırır. **ContainerPort** özniteliği, 8080 için ayarlanmışsa, çünkü bağlantı noktası 8080 Dockerfile içinde belirtildiği gibi kapsayıcı gösterir. **EndpointRef** özniteliği "endpointTest", önceki adımda hizmet bildiriminde tanımlanan uç nokta ayarlanır. Bu nedenle, 8080 bağlantı noktasında hizmete gelen istekler kapsayıcı üzerindeki 8080 numaralı bağlantı noktasına eşlenir. 
   * **RepositoryCredentials** etiketi kapsayıcı görüntüden bu çeker nerede (özel) depo ile kimlik doğrulaması yapması kimlik bilgilerini belirtir. Görüntü genel bir depodan çekilmesi ise bu ilkeyi gerekmez.
    

12. İçinde *ServiceFabricTomcat* klasöründe, service fabric kümesine bağlanın. 

   * Yerel Service Fabric kümesine bağlanmak için şunu çalıştırın:

     ```bash
     sfctl cluster select --endpoint http://localhost:19080
     ```
    
   * Azure güvenli bir kümeye bağlanmak için istemci sertifikası .pem dosyası olarak mevcut olduğundan emin olun *ServiceFabricTomcat* dizin ve çalıştırın: 

     ```bash
     sfctl cluster select --endpoint https://PublicIPorFQDN:19080 -pem your-certificate.pem -no-verify
     ```
     Önceki komutta `your-certificate.pem` ile istemci sertifika dosyasının adı. Geliştirme ve test ortamlarında küme sertifikası genellikle istemci sertifikası olarak kullanılır. Sertifikanızı kendinden imzalı değilse atla `-no-verify` parametresi. 
       
     Küme sertifikası genellikle .pfx dosyaları yerel olarak indirilir. Sertifikanızı PEM biçiminde yoksa, bir .pfx dosyasından bir .pem dosyasını oluşturmak için aşağıdaki komutu çalıştırabilirsiniz:

     ```bash
     openssl pkcs12 -in your-certificate.pfx -out your-certificate.pem -nodes -passin pass:your-pfx-password
     ```

     .Pfx dosyanızı parola korumalı değilse, `-passin pass:` son parametresi için.


13. Uygulamayı kümenize dağıtmak için şablonda verilen yükleme betiğini çalıştırın. Betik uygulama paketini kümenin görüntü deposuna kopyalayan, uygulama türünü kaydeder ve uygulamanın bir örneğini oluşturur.

     ```bash
     ./install.sh
     ```

   Yükleme betiği çalıştırdıktan sonra bir tarayıcı açın ve Service Fabric Explorer'a gidin:
    
   * Yerel bir kümede kullanmak `http://localhost:19080/Explorer` (Değiştir *localhost* Mac OS X üzerinde Vagrant'ı kullanıyorsanız, sanal makinenin özel IP'si ile).
   * Güvenli bir Azure kümesi üzerindeki `https://PublicIPorFQDN:19080/Explorer`. 
    
   Genişletin **uygulamaları** düğüm ve var. Şimdi, uygulama türü için bir giriş olduğuna dikkat edin **ServiceFabricTomcatType**, bu türün ilk örneği için başka bir. Bu işlem birkaç dakika sürebilir uygulamanın tam olarak dağıtmak bu nedenle sabırlı olun.

   ![Service Fabric Explorer](./media/service-fabric-get-started-tomcat/service-fabric-explorer.png)


1. Tomcat sunucu üzerinde uygulamaya erişmek için bir tarayıcı penceresi açın ve aşağıdaki URL'lerden herhangi birini girin. Yerel kümeye dağıtılır, kullanın *localhost* için *PublicIPorFQDN*. "Hello World!" çeşidini görürsünüz. her URL için Hoş Geldiniz ekranı.

   * http://PublicIPorFQDN:8080/hello  
   * http://PublicIPorFQDN:8080/hello/sayhello
   * http://PublicIPorFQDN:8080/hello/sayhi

## <a name="clean-up"></a>Temizleme
Uygulamanızı kümeden uygulama örneğini silmek ve uygulama türünün kaydını silmek için şablonda sağlanan kaldırma betiğini kullanın.

```bash
./uninstall.sh
```

Görüntüyü kapsayıcı kayıt defterine gönderdikten sonra yerel görüntüyü geliştirme bilgisayarınızdan silebilirsiniz:

```
docker rmi tomcattest
docker rmi myregistry.azurecr.io/samples/tomcattest
```

## <a name="next-steps"></a>Sonraki adımlar
* Ek Linux kapsayıcı özellikleri hızlı adımları için okuma [Linux'ta ilk Service Fabric kapsayıcı uygulamanızı oluşturma](service-fabric-get-started-containers-linux.md).
* Daha ayrıntılı adımlar Linux kapsayıcıları için okuma [bir Linux kapsayıcı uygulaması Öğreticisi oluşturma](service-fabric-tutorial-create-container-images.md) öğretici.
* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.


