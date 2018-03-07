---
title: "Azure Service Fabric’e Spring Boot uygulaması dağıtma | Microsoft Docs"
description: "Bu hızlı başlangıçta, bir Spring Boot örnek uygulaması kullanarak Azure Service Fabric için Spring Boot uygulaması dağıtırsınız."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: ab860b8525bcb77d3ab35d3f649532713c661b61
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="quickstart-deploy-a-java-spring-boot-application-to-azure"></a>Hızlı Başlangıç: Azure’a Java Spring Boot Uygulaması dağıtma
Azure Service Fabric; mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

Bu hızlı başlangıçta bir Spring Boot uygulamasının Service Fabric'e nasıl dağıtılacağı gösterilir. Bu hızlı başlangıçta Spring web sitesindeki [Kullanmaya Başlama](https://spring.io/guides/gs/spring-boot/) örneği kullanılır. Bu hızlı başlangıç, bilindik komut satırı araçlarını kullanarak bir Service Fabric uygulaması olarak Spring Boot örneğini dağıtma adımlarını gösterir. İşlemi tamamladığınızda, Spring Boot Kullanmaya Başlama örneği, Service Fabric üzerinde çalışmaya başlar. 

![Uygulama Ekran Görüntüsü](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Service Fabric’e Spring Boot uygulaması dağıtma
> * Uygulamayı yerel kümenize dağıtma 
> * Uygulamayı Azure'da bir kümeye dağıtma
> * Birden çok düğüm arasında uygulamanın ölçeğini genişletme
> * Kullanılabilirliği etkilemeden hizmetinizin yük devretmesini gerçekleştirme

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıcı tamamlamak için:
1. [Service Fabric SDK ve Service Fabric Komut Satırı Arabirimi’ni (CLI) yükleme](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Git'i yükleyin](https://git-scm.com/)
3. [Yeoman yükleme](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
4. [Java Ortamını Ayarlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development)

## <a name="download-the-sample"></a>Örneği indirme
Spring Boot Kullanmaya Başlama örnek uygulamasını yerel makinenize kopyalamak için, bir komut penceresinde aşağıdaki komutu çalıştırın.
```
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="package-the-spring-boot-application"></a>Spring Boot uygulamasını paketleme 
1. Kopyalanan `gs-spring-boot` dizininde `yo azuresfguest` komutunu çalıştırın. 

2. Her istem için aşağıdaki ayrıntıları girin. 

    ![Yeoman Girişleri](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

3. `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` klasöründe `entryPoint.sh` adlı bir dosya oluşturun. Aşağıdakileri dosyaya ekleyin. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

Bu aşamada, Spring Boot Kullanmaya Başlama örneği için Service Fabric’e dağıtabileceğiniz bir Service Fabric uygulaması oluşturdunuz.

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma
1. Aşağıdaki komutu çalıştırarak yerel kümenizi başlatın:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Yerel kümenin başlatılması biraz zaman alabilir. Yerel kümenin sorunsuz çalıştığından emin olmak için, **http://localhost:19080** adresinden Service Fabric Explorer’a erişin. Sorunsuz beş düğüm, yerel kümenin çalıştığını belirtir. 
    
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

5. Sık kullandığınız web tarayıcısını açın ve **http://localhost:8080** adresine giderek uygulamaya erişin. 

    ![Uygulama ön ucu Konumu](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)
    
Bir Service Fabric kümesine dağıtılmış Spring Boot uygulamasına artık erişebilirsiniz.  

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma

### <a name="set-up-your-azure-service-fabric-cluster"></a>Azure Service Fabric Kümesi’ni ayarlama
Uygulamayı Azure'daki bir kümeye dağıtmak için kendi kümenizi oluşturun.

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleridir. Service Fabric ekibi tarafından çalıştırılan bu kümelere herkes uygulama dağıtarak platform hakkında bilgi alabilir. Bir Grup Kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric). 

Güvenli toplu kümede yönetimi işlemleri gerçekleştirmek için Service Fabric Explorer, CLI veya Powershell’i kullanabilirsiniz. Service Fabric Explorer’ı kullanmak için, PFX dosyasını Toplu Küme Web sitesinden indirmeniz ve sertifikayı sertifika deponuza (Windows veya Mac) veya tarayıcının kendisine (Ubuntu) aktarmanız gerekir. Toplu kümeden gelen otomatik olarak imzalanan sertifikaların parolası yoktur. 

Powershell veya CLI ile yönetim işlemleri gerçekleştirmek için PFX (Powershell) veya PEM (CLI) gerekir. PFX dosyasını PEM dosyasına dönüştürmek için lütfen aşağıdaki komutu çalıştırın:  

```bash
openssl pkcs12 -in party-cluster-1277863181-client-cert.pfx -out party-cluster-1277863181-client-cert.pem -nodes -passin pass:
```

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> Spring Boot hizmeti 8080 numaralı bağlantı noktasında gelen trafiği dinleyecek şekilde yapılandırılmıştır. Kümenizde bu bağlantı noktasının açık olduğundan emin olun. Grup Kümesi kullanıyorsanız bu bağlantı noktası açık durumdadır.
>

### <a name="deploy-the-application-using-cli"></a>CLI kullanarak uygulamayı dağıtma
Uygulama ve kümeniz qhazır olduğuna göre, doğrudan komut satırındaki bir kümeye dağıtabilirsiniz.

1. `gs-spring-boot/SpringServiceFabric` klasörüne gidin.
2. Azure kümenize bağlanmak için aşağıdaki komutu çalıştırın. 

    ```bash
    sfctl cluster select --endpoint http://<ConnectionIPOrURL>:19080
    ```
    
    Küme otomatik olarak imzalanan bir sertifika ile güvenli hale getirilmişse, çalıştırdığınız komut şöyle olur: 

    ```bash
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify
    ```
3. `install.sh` betiğini çalıştırın. 

    ```bash
    ./install.sh
    ```

4. Sık kullandığınız web tarayıcısını açın ve **http://\<ConnectionIPOrUrl>:8080** adresine giderek uygulamaya erişin. 

    ![Uygulama ön ucu Konumu](./media/service-fabric-quickstart-java-spring-boot/springbootsfazure.png)
    
Bir Service Fabric kümesine dağıtılmış Spring Boot uygulamasına artık erişebilirsiniz.  
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme
Hizmet yükündeki bir değişikliği karşılamak için kümedeki hizmetler kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizi ölçeklendirmenin birçok yolu vardır; Service Fabric CLI'den (sfctl) betikler veya komutlar kullanabilirsiniz. Bu örnekte, Service Fabric Explorer kullanılmıştır.

Service Fabric Explorer tüm Service Fabric kümelerinde çalıştırılır ve tarayıcıdan kümelerin HTTP yönetim bağlantı noktasına (19080) göz atarak (örneğin, `http://localhost:19080`) erişilebilir.

Web ön uç hizmetini ölçeklendirmek için aşağıdaki adımları gerçekleştirin:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
2. Ağaç görünümünde **fabric:/SpringServiceFabric/SpringGettingStarted** düğümünün yanındaki üç noktaya tıklayın ve **Hizmeti Ölçeklendir**'i seçin.

    ![Service Fabric Explorer Hizmeti Ölçeklendir](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    Şimdi hizmetin örnek sayısını ölçeklendirebilirsiniz.

3. Rakamı **3** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.

    Komut satırını kullanarak hizmeti ölçeklendirmenin alternatif bir yolu aşağıda verilmiştir.

    ```bash 
    # Connect to your local cluster
    sfctl cluster select --endpoint http://localhost:19080

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted` --instance-count 3 --stateless 
    ``` 

4. Ağaç görünümünde **fabric:/SpringServiceFabric/SpringGettingStarted** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin.

    ![Service Fabric Explorer Hizmeti Ölçeklendirme Tamamlandı](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    Hizmette üç örnek bulunur ve ağaç görünümünde örneklerin çalıştığı düğümler gösterilir.

Bu basit yönetim görevi sayesinde Spring hizmetinizde kullanıcı yükünü işleyecek kaynak sayısı artırılmış oldu. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığını anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="fail-over-services-in-a-cluster"></a>Bir kümedeki hizmetlerin yükünü devretme 
Hizmet yük devretmesini göstermek için Service Fabric Explorer'ı kullanarak bir düğümü yeniden başlatma işleminin benzetimi yapılmıştır. Hizmetinizin yalnızca bir örneğinin çalıştığından emin olun. 

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
2. Hizmetinizin örneğini çalıştıran düğümün yanındaki üç nokta işaretine tıklayın ve düğümü yeniden başlatın. 

    ![Service Fabric Explorer Düğümünü Yeniden Başlatma](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
3. Hizmetinizin örneği bu durumda farklı bir düğüme taşınmıştır ve uygulamanızda kesinti yaşanmaz. 

    ![Service Fabric Explorer Düğümünü Yeniden Başlatma Başarılı](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Service Fabric’e Spring Boot uygulaması dağıtma
> * Uygulamayı yerel kümenize dağıtma 
> * Uygulamayı Azure'da bir kümeye dağıtma
> * Birden çok düğüm arasında uygulamanın ölçeğini genişletme
> * Kullanılabilirliği etkilemeden hizmetinizin yük devretmesini gerçekleştirme

* [Service Fabric Programlama Modellerini kullanarak Java mikro hizmetleri derleme](service-fabric-quickstart-java-reliable-services.md) hakkında daha fazla bilgi edinin
* [Jenkins kullanarak sürekli tümleştirme ve dağıtma işlemlerinizi ayarlama](service-fabric-cicd-your-linux-applications-with-jenkins.md) hakkında daha fazla bilgi edinin
* Diğer [Java Örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started)’ne göz atın