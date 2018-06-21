---
title: Azure Service Fabric’e Spring Boot uygulaması dağıtma | Microsoft Docs
description: Bu hızlı başlangıçta, bir Spring Boot örnek uygulaması kullanarak Azure Service Fabric için Spring Boot uygulaması dağıtırsınız.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 860d28cb6726a86194460977b822197a37ab7279
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642878"
---
# <a name="quickstart-deploy-a-java-spring-boot-application-to-azure"></a>Hızlı Başlangıç: Azure’a Java Spring Boot Uygulaması dağıtma
Azure Service Fabric; mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

Bu hızlı başlangıçta tanıdık komut satırı araçları ile, Spring web sitesindeki [Kullanmaya Başlama](https://spring.io/guides/gs/spring-boot/) örneği kullanılarak bir Mac veya Linux geliştirici makinesinde işlevsel bir Spring Boot uygulamasının Service Fabric’e nasıl dağıtılacağı açıklanmaktadır.

![Uygulama Ekran Görüntüsü](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

* Service Fabric’e Spring Boot uygulaması dağıtma
* Uygulamayı yerel kümenize dağıtma 
* Uygulamayı Azure'da bir kümeye dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme
* Kullanılabilirliği etkilemeden hizmetinizin yük devretmesini gerçekleştirme

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıcı tamamlamak için:
1. Service Fabric SDK ve Service Fabric Komut Satırı Arabirimi’ni (CLI) yükleme

    a. [Mac](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cli#cli-mac)
    
    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)

2. [Git'i yükleyin](https://git-scm.com/)
3. Yeoman’ı yükleme

    a. [Mac](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)

    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
4. Java Ortamını Ayarlama

    a. [Mac](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)
    
    b.  [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development)

## <a name="download-the-sample"></a>Örneği indirme
Spring Boot Kullanmaya Başlama örnek uygulamasını yerel makinenize kopyalamak için, bir terminal penceresinde aşağıdaki komutu çalıştırın.
```bash
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="build-the-spring-boot-application"></a>Spring Boot uygulamasını derleme 
1. `gs-spring-boot/complete` dizininin içinde, uygulamayı derlemek için aşağıdaki komutu çalıştırın 

    ```bash
    ./gradlew build
    ``` 

## <a name="package-the-spring-boot-application"></a>Spring Boot uygulamasını paketleme 
1. Kopyanızdaki `gs-spring-boot` dizininde `yo azuresfguest` komutunu çalıştırın. 

2. Her istem için aşağıdaki ayrıntıları girin. 

    ![Yeoman Girişleri](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

3. `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` klasöründe `entryPoint.sh` adlı bir dosya oluşturun. Aşağıdakileri `entryPoint.sh` dosyasına ekleyin. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

4. **Uç noktalar** kaynağını `gs-spring-boot/SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/ServiceManifest.xml` dosyasına ekleme

    ```xml 
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
    ```

    **ServiceManifest.xml** şimdi şu şekilde görünür: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceManifest Name="SpringGettingStartedPkg" Version="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

       <ServiceTypes>
          <StatelessServiceType ServiceTypeName="SpringGettingStartedType" UseImplicitHost="true">
       </StatelessServiceType>
       </ServiceTypes>

       <CodePackage Name="code" Version="1.0.0">
          <EntryPoint>
             <ExeHost>
                <Program>entryPoint.sh</Program>
                <Arguments></Arguments>
                <WorkingFolder>CodePackage</WorkingFolder>
             </ExeHost>
          </EntryPoint>
       </CodePackage>
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
     </ServiceManifest>
    ```

Bu aşamada, Spring Boot Kullanmaya Başlama örneği için Service Fabric’e dağıtabileceğiniz bir Service Fabric uygulaması oluşturdunuz.

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma
1. Aşağıdaki komutu çalıştırarak Ubuntu makinelerinde yerel kümenizi başlatın:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    Mac kullanıyorsanız Docker görüntüsünden yerel kümeyi başlatın (burada Mac için yerel kümenizi ayarlama [önkoşullarına](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-mac#create-a-local-container-and-set-up-service-fabric) uyduğunuz varsayılır). 

    ```bash
    docker run --name sftestcluster -d -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 -p 8080:8080 mysfcluster
    ```

    Yerel kümenin başlatılması biraz zaman alabilir. Kümenin sorunsuz çalıştığından emin olmak için, **http://localhost:19080** adresinden Service Fabric Explorer’a erişin. Sorunsuz beş düğüm, yerel kümenin çalıştığını belirtir. 
    
    ![Yerel küme sorunsuz çalışıyor](./media/service-fabric-quickstart-java-spring-boot/sfxlocalhost.png)

2. `gs-spring-boot/SpringServiceFabric` klasörüne gidin.
3. Yerel kümenize bağlanmak için aşağıdaki komutu çalıştırın. 

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```
4. `install.sh` betiğini çalıştırın. 

    ```bash
    ./install.sh
    ```

5. Sık kullandığınız web tarayıcısını açın ve**http://localhost:8080** adresine giderek uygulamaya erişin. 

    ![Uygulama ön ucu Konumu](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)
    
Bir Service Fabric kümesine dağıtılmış Spring Boot uygulamasına artık erişebilirsiniz.  

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma

### <a name="set-up-your-azure-service-fabric-cluster"></a>Azure Service Fabric Kümesi’ni ayarlama
Uygulamayı Azure'daki bir kümeye dağıtmak için kendi kümenizi oluşturun.

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleri olup Service Fabric ekibi tarafından çalıştırılır. Uygulamaları dağıtmak ve platform hakkında bilgi edinmek için grup kümelerini kullanabilirsiniz. Küme, düğümden düğüme ve istemciden düğüme güvenlik için tek bir otomatik olarak imzalanan sertifika kullanır.

Oturum açın ve bir [Linux kümesine](http://aka.ms/tryservicefabric) katılın. **PFX** bağlantısına tıklayarak PFX sertifikasını bilgisayarınıza indirin. **BeniOku** bağlantısına tıklayarak, sertifikayı kullanmak için çeşitli ortamların nasıl yapılandırılacağına ilişkin sertifika parolasını ve yönergelerini bulun. Hem **Hoş Geldiniz** sayfasını hem de **BeniOku** sayfasını açık tutun. Aşağıdaki adımlarda yer alan yönergelerden bazılarını kullanacaksınız. 

> [!Note]
> Saat başına sınırlı sayıda grup kümesi vardır. Bir grup kümesine kaydolmaya çalıştığınızda hata alırsanız bir süre bekleyebilir ve tekrar deneyebilirsiniz veya [Azure’da Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md) bölümündeki adımları izleyerek aboneliğinizde bir küme oluşturabilirsiniz. 
>
> Spring Boot hizmeti 8080 numaralı bağlantı noktasında gelen trafiği dinleyecek şekilde yapılandırılmıştır. Kümenizde bu bağlantı noktasının açık olduğundan emin olun. Grup Kümesi kullanıyorsanız bu bağlantı noktası açık durumdadır.
>

Service Fabric, bir kümeyi ve uygulamalarını yönetmek için kullanabileceğiniz birçok araç sağlar:

- Tarayıcı tabanlı bir araç: Service Fabric Explorer.
- Azure CLI 2.0 üzerinde çalışan Service Fabric Komut Satırı Arabirimi (CLI).
- PowerShell komutları. 

Bu hızlı başlangıçta, Service Fabric CLI ve Service Fabric Explorer’ı kullanacaksınız. 

CLI kullanmak için, indirdiğiniz PFX dosyasını temel alarak bir PEM dosyası oluşturmanız gerekir. Dosyayı dönüştürmek için aşağıdaki komutu kullanın. (Grup kümeleri için, **BeniOku** sayfasındaki yönergelerden PFX dosyanıza özgü bir komutu kopyalayabilirsiniz.)

```bash
openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
``` 

Service Fabric Explorer’ı kullanmak için, Grup Kümesi web sitesinden indirdiğiniz sertifika PFX dosyasını sertifika deponuza (Windows veya Mac) ya da tarayıcıya (Ubuntu) aktarmanız gerekir. **BeniOku** sayfasından alabileceğiniz PFX özel anahtar parolası gerekir.

Sisteminizde sertifikayı içeri aktarmak için size en rahat gelen yöntemi kullanın. Örnek:

- Windows üzerinde: PFX dosyasına çift tıklayın ve `Certificates - Current User\Personal\Certificates` dizinindeki kişisel deponuza sertifikayı yüklemek için istemleri izleyin. Alternatif olarak, **BeniOku** yönergelerinde PowerShell komutunu kullanabilirsiniz.
- Mac üzerinde: PFX dosyasına çift tıklayın ve Anahtarlığınıza sertifikayı yüklemek için istemleri izleyin.
- Ubuntu üzerinde: Mozilla Firefox, Ubuntu 16.04 sistemindeki varsayılan tarayıcıdır. Sertifikayı Firefox’a aktarmak için, tarayıcınızın sağ üst köşesindeki menü düğmesine ve ardından **Seçenekler**’e tıklayın. **Tercihler** sayfasında arama kutusunu kullanarak "sertifikalar" terimini arayın. **Sertifikaları Görüntüle**’ye tıklayın, **Sertifikalarınız** sekmesini seçin, **İçeri Aktar**’a tıklayın ve sertifikayı içeri aktarma istemlerini izleyin.
 
   ![Firefox’ta sertifika yükleme](./media/service-fabric-quickstart-java-spring-boot/install-cert-firefox.png) 


### <a name="deploy-the-application-using-cli"></a>CLI kullanarak uygulamayı dağıtma
Uygulama ve kümeniz qhazır olduğuna göre, doğrudan komut satırındaki bir kümeye dağıtabilirsiniz.

1. `gs-spring-boot/SpringServiceFabric` klasörüne gidin.
2. Azure kümenize bağlanmak için aşağıdaki komutu çalıştırın. 

    ```bash
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify
    ```
3. `install.sh` betiğini çalıştırın. 

    ```bash
    ./install.sh
    ```

4. Web tarayıcınızı açın ve **http://\<ConnectionIPOrUrl>:8080** adresine giderek uygulamaya erişin. 

    ![Uygulama ön ucu Konumu](./media/service-fabric-quickstart-java-spring-boot/springbootsfazure.png)
    
Artık Azure’da bir Service Fabric kümesi çalıştıran Spring Boot uygulamasına erişebilirsiniz.  
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme
Hizmet yükündeki bir değişikliği karşılamak için kümedeki hizmetler kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizi ölçeklendirmenin birçok yolu vardır; örneğin, Service Fabric CLI’den (sfctl) betikler veya komutlar kullanabilirsiniz. Aşağıdaki adımlarda Service Fabric Explorer kullanılmaktadır.

Service Fabric Explorer tüm Service Fabric kümelerinde çalıştırılır ve tarayıcıdan kümelerin HTTP yönetim bağlantı noktasına (19080) göz atılarak (örneğin, `http://localhost:19080`) erişilebilir.

Web ön uç hizmetini ölçeklendirmek için aşağıdakileri yapın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
2. Ağaç görünümünde **fabric:/SpringServiceFabric/SpringGettingStarted** düğümünün yanındaki üç noktaya tıklayın ve **Hizmeti Ölçeklendir**'i seçin.

    ![Service Fabric Explorer Hizmeti Ölçeklendir](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    Şimdi hizmetin örnek sayısını ölçeklendirebilirsiniz.

3. Rakamı **3** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.

    Komut satırını kullanarak hizmeti ölçeklendirmenin alternatif bir yolu aşağıda verilmiştir.

    ```bash 
    # Connect to your local cluster
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted' --instance-count 3 --stateless 
    ``` 

4. Ağaç görünümünde **fabric:/SpringServiceFabric/SpringGettingStarted** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin.

    ![Service Fabric Explorer Hizmeti Ölçeklendirme Tamamlandı](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    Hizmette üç örnek bulunur ve ağaç görünümünde örneklerin çalıştığı düğümler gösterilir.

Bu basit yönetim görevi sayesinde ön uç hizmetinin kullanıcı yükünü işlemek için kullanabileceği kaynakları iki katına çıkarmış oldunuz. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığınızı anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric, kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="fail-over-services-in-a-cluster"></a>Bir kümedeki hizmetlerin yükünü devretme 
Hizmet yük devretmesini göstermek için Service Fabric Explorer'ı kullanarak bir düğümü yeniden başlatma işleminin benzetimi yapılmıştır. Hizmetinizin yalnızca bir örneğinin çalıştığından emin olun. 

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
2. Hizmetinizin örneğini çalıştıran düğümün yanındaki üç nokta işaretine tıklayın ve düğümü yeniden başlatın. 

    ![Service Fabric Explorer Düğümünü Yeniden Başlatma](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
3. Hizmetinizin örneği bu durumda farklı bir düğüme taşınmıştır ve uygulamanızda kesinti yaşanmaz. 

    ![Service Fabric Explorer Düğümünü Yeniden Başlatma Başarılı](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta şunları öğrendiniz:

* Service Fabric’e Spring Boot uygulaması dağıtma
* Uygulamayı yerel kümenize dağıtma 
* Uygulamayı Azure'da bir kümeye dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme
* Kullanılabilirliği etkilemeden hizmetinizin yük devretmesini gerçekleştirme

Service Fabric’te Java uygulamalarıyla çalışma hakkında daha fazla bilgi için Java uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Java uygulaması dağıtma](./service-fabric-tutorial-create-java-app.md)
