---
title: Azure'da Service Fabric üzerinde bir Spring Boot uygulaması oluşturma | Microsoft Docs
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
ms.date: 01/29/2019
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 2c98b069e042f9cbd07ccee643028ac84b3471c6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60728598"
---
# <a name="quickstart-deploy-a-java-spring-boot-application-to-service-fabric"></a>Hızlı Başlangıç: Bir Service Fabric Java Spring Boot uygulaması dağıtma

Azure Service Fabric; mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur.

Bu hızlı başlangıçta bir Spring Boot uygulamasının Service Fabric’e nasıl dağıtılacağı gösterilir. Bu hızlı başlangıçta Spring web sitesindeki [Kullanmaya Başlama](https://spring.io/guides/gs/spring-boot/) örneği kullanılır. Bu hızlı başlangıç, bilindik komut satırı araçlarını kullanarak bir Service Fabric uygulaması olarak Spring Boot örneğini dağıtma adımlarını gösterir. İşlemi tamamladığınızda, Spring Boot Kullanmaya Başlama örneği, Service Fabric üzerinde çalışmaya başlar.

![Uygulama Ekran Görüntüsü](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

* Service Fabric’e Spring Boot uygulaması dağıtma
* Uygulamayı yerel kümenize dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme
* Kullanılabilirliği etkilemeden hizmetinizin yük devretmesini gerçekleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

1. Service Fabric SDK ve Service Fabric Komut Satırı Arabirimi’ni (CLI) yükleme

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli#cli-mac)
    
    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)

1. [Git'i yükleyin](https://git-scm.com/)
1. Yeoman’ı yükleme

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)

    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
1. Java Ortamını Ayarlama

    a. [Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)
    
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

1. Her istem için aşağıdaki ayrıntıları girin.

    ![Yeoman Girişleri](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

1. `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` klasöründe `entryPoint.sh` adlı bir dosya oluşturun. Aşağıdakileri `entryPoint.sh` dosyasına ekleyin. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

1. **Uç noktalar** kaynağını `gs-spring-boot/SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/ServiceManifest.xml` dosyasına ekleme

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
                     xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" >

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

    Mac kullanıyorsanız Docker görüntüsünden yerel kümeyi başlatın (burada Mac için yerel kümenizi ayarlama [önkoşullarına](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-a-local-container-and-set-up-service-fabric) uyduğunuz varsayılır). 

    ```bash
    docker run --name sftestcluster -d -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 -p 8080:8080 mysfcluster
    ```

    Yerel kümenin başlatılması biraz zaman alabilir. Kümenin sorunsuz çalıştığından emin olmak için, **http://localhost:19080** adresinden Service Fabric Explorer’a erişin. Sorunsuz beş düğüm, yerel kümenin çalıştığını belirtir. 
    
    ![Yerel küme sorunsuz çalışıyor](./media/service-fabric-quickstart-java-spring-boot/sfxlocalhost.png)

1. `gs-spring-boot/SpringServiceFabric` klasörüne gidin.
1. Yerel kümenize bağlanmak için aşağıdaki komutu çalıştırın.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```
1. `install.sh` betiğini çalıştırın.

    ```bash
    ./install.sh
    ```

1. Sık kullandığınız web tarayıcınızı açın ve uygulama erişerek `http://localhost:8080`.

    ![Uygulama ön ucu Konumu](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

Bir Service Fabric kümesine dağıtılmış Spring Boot uygulamasına artık erişebilirsiniz.

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme

Hizmet yükündeki bir değişikliği karşılamak için kümedeki hizmetler kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizi ölçeklendirmenin birçok yolu vardır; örneğin, Service Fabric CLI’den (sfctl) betikler veya komutlar kullanabilirsiniz. Aşağıdaki adımlarda Service Fabric Explorer kullanılmaktadır.

Service Fabric Explorer tüm Service Fabric kümelerinde çalıştırılır ve tarayıcıdan kümelerin HTTP yönetim bağlantı noktasına (19080) göz atılarak (örneğin, `http://localhost:19080`) erişilebilir.

Web ön uç hizmetini ölçeklendirmek için aşağıdakileri yapın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
1. Ağaç görünümünde **fabric:/SpringServiceFabric/SpringGettingStarted** düğümünün yanındaki üç noktaya tıklayın ve **Hizmeti Ölçeklendir**'i seçin.

    ![Service Fabric Explorer Hizmeti Ölçeklendir](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    Şimdi hizmetin örnek sayısını ölçeklendirebilirsiniz.

1. Rakamı **3** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.

    Komut satırını kullanarak hizmeti ölçeklendirmenin alternatif bir yolu aşağıda verilmiştir.

    ```bash
    # Connect to your local cluster
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted' --instance-count 3 --stateless 
    ``` 

1. Ağaç görünümünde **fabric:/SpringServiceFabric/SpringGettingStarted** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin.

    ![Service Fabric Explorer Hizmeti Ölçeklendirme Tamamlandı](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    Hizmette üç örnek bulunur ve ağaç görünümünde örneklerin çalıştığı düğümler gösterilir.

Bu basit yönetim görevi sayesinde ön uç hizmetinin kullanıcı yükünü işlemek için kullanabileceği kaynakları iki katına çıkarmış oldunuz. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığınızı anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric, kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="fail-over-services-in-a-cluster"></a>Bir kümedeki hizmetlerin yükünü devretme

Hizmet yük devretmesini göstermek için Service Fabric Explorer'ı kullanarak bir düğümü yeniden başlatma işleminin benzetimi yapılmıştır. Hizmetinizin yalnızca bir örneğinin çalıştığından emin olun.

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
1. Hizmetinizin örneğini çalıştıran düğümün yanındaki üç nokta işaretine tıklayın ve düğümü yeniden başlatın.

    ![Service Fabric Explorer Düğümünü Yeniden Başlatma](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
1. Hizmetinizin örneği bu durumda farklı bir düğüme taşınmıştır ve uygulamanızda kesinti yaşanmaz.

    ![Service Fabric Explorer Düğümünü Yeniden Başlatma Başarılı](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Service Fabric’e Spring Boot uygulaması dağıtma
* Uygulamayı yerel kümenize dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme
* Kullanılabilirliği etkilemeden hizmetinizin yük devretmesini gerçekleştirme

Service Fabric’te Java uygulamalarıyla çalışma hakkında daha fazla bilgi için Java uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Java uygulaması dağıtma](./service-fabric-tutorial-create-java-app.md)
