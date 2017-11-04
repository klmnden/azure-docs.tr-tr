---
title: "Bir Azure Service Fabric Java uygulaması oluşturma | Microsoft Docs"
description: "Bir Java uygulaması için Azure Service Fabric hızlı başlangıç örnek kullanarak oluşturun."
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
ms.date: 10/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: c8e598159d2139397952a5c11eac54dc38939f47
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-java-application"></a>Bir Java uygulaması oluşturma
Azure Service Fabric mikro ve kapsayıcıları dağıtma ve yönetme için bir dağıtılmış sistemler platformudur. 

Bu hızlı başlangıç ilk Java uygulamanızı Service Fabric dağıtmak Eclipse IDE kullanarak bir Linux Geliştirici makinede gösterilmiştir. İşlemi tamamladığınızda, durum bilgisi olan bir arka uç hizmeti kümedeki Oylama sonuçlarını kaydeder Java web ön ucuna sahip bir oylama uygulaması sahip.

![Uygulama ekran görüntüsü](./media/service-fabric-quickstart-java/votingapp.png)

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

> [!div class="checklist"]
> * Eclipse aracı olarak Service Fabric Java uygulamalarınız için kullanın.
> * Yerel kümenizdeki uygulamayı dağıtmak 
> * Bir kümede Azure uygulamayı dağıtmak
> * Genişleme uygulama birden çok düğüm arasında

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıcı tamamlamak için:
1. [Service Fabric SDK & hizmeti doku komut satırı arabirimi (CLI) yükle](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Git'i yükleyin](https://git-scm.com/)
3. [Eclipse yükleyin](https://www.eclipse.org/downloads/)
4. [Java ortamını ayarlama](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development), yapmayı emin Eclipse eklentisini yüklemek için isteğe bağlı adımları izlemek için 

## <a name="download-the-sample"></a>Örneği indirme
Bir komut penceresinde örnek uygulama depoyu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.
```
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma
1. Yerel kümenizdeki aşağıdaki komutu çalıştırarak başlatın:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Yerel küme başlangıç biraz zaman alır. Kümenin tam olarak açık olduğunu onaylamak için Service Fabric Explorer erişim **http://localhost: 19080**. Yerel küme çalışır durumda beş sağlıklı düğümleri gösterir. 
    
    ![Yerel küme Sağlıklı](./media/service-fabric-quickstart-java/localclusterup.png)

2. Eclipse açın.
3. Dosyasına tıklayın dosya sisteminden Açık Projeler ->... 
4. Dizin'i tıklatın ve seçin `Voting` gelen dizin `service-fabric-java-quickstart` Github'dan kopyaladığınız klasörü. Son'a tıklayın. 

    ![Eclipse içeri aktarma iletişim kutusu](./media/service-fabric-quickstart-java/eclipseimport.png)
    
5. Artık `Voting` Eclipse paketi Gezgini. 
6. Sağ tıklayın proje ve select **uygulamasını Yayımla...**  altında **Service Fabric** açılır. Seçin **PublishProfiles/Local.json** hedef profil olarak Yayımla'yı tıklatın. 

    ![İletişim yerel yayımlama](./media/service-fabric-quickstart-java/localjson.png)
    
7. Sık kullanılan web tarayıcınızı açın ve uygulama erişerek **http://localhost: 8080**. 

    ![Ön uç yerel uygulama](./media/service-fabric-quickstart-java/runninglocally.png)
    
Şimdi, oylama seçenekleri kümesi ekleyin ve oy almaya başlayın. Uygulamayı çalışır ve ayrı bir veritabanı gerek kalmadan, Service Fabric kümesindeki tüm verileri depolar.

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma

### <a name="set-up-your-azure-service-fabric-cluster"></a>Azure Service Fabric kümesi ayarlayın
Azure kümesinde uygulamayı dağıtmak için kendi küme oluşturun veya bir taraf kümesi kullanın.

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleridir. Burada herkes uygulamaları dağıtabilir ve platform hakkında bilgi edinin Service Fabric ekibi tarafından çalıştırılır. Bir Grup Kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric). 

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da ilk Service Fabric kümenizi oluşturma](service-fabric-get-started-azure-cluster.md).

> [!Note]
> Web ön uç hizmeti 8080 bağlantı noktasından gelen trafiği dinleyecek şekilde yapılandırılır. Kümenizde bu bağlantı noktasının açık olduğundan emin olun. Taraf küme kullanıyorsanız, bu bağlantı noktasının açık olduğundan.
>

### <a name="deploy-the-application-using-eclipse"></a>Eclipse kullanarak uygulamayı dağıtın
Uygulama ve kümenizi hazır, doğrudan Eclipse kümeye dağıtabilirsiniz.

1. Açık **Cloud.json** altında dosya **PublishProfiles** dizin ve doldurun `ConnectionIPOrURL` ve `ConnectionPort` uygun şekilde alanları. Bir örnek sağlanmıştır: 

    ```bash
    {
         "ClusterConnectionParameters": 
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "",
            "ClientCert": ""
         }
    }
    ```

2. Sağ tıklayın proje ve select **uygulamasını Yayımla...**  altında **Service Fabric** açılır. Seçin **PublishProfiles/Cloud.json** hedef profil olarak Yayımla'yı tıklatın. 

    ![İletişim bulut yayımlama](./media/service-fabric-quickstart-java/cloudjson.png)

3. Sık kullanılan web tarayıcınızı açın ve uygulama erişerek **http://\<ConnectionIPOrURL >: 8080**. 

    ![Uygulama ön uç bulut](./media/service-fabric-quickstart-java/runningcloud.png)
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme
Hizmetleri, bir değişiklik Hizmetleri üzerindeki yükü için uygun hale getirmek için bir küme içinde genişletilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizin ölçeklendirme birkaç yolu vardır, betikleri veya komutları Service Fabric clı'dan (sfctl) kullanabilirsiniz. Bu örnekte, Service Fabric Explorer kullanıyoruz.

Service Fabric Explorer tüm Service Fabric kümelerinde çalışır ve kümeleri HTTP yönetim bağlantı noktası (19080), örneğin, göz atarak bir tarayıcıdan erişilebilir `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.

Web ön uç hizmetini ölçeklendirmek için aşağıdaki adımları gerçekleştirin:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.
2. Yanındaki üç nokta (üç nokta) tıklayın **fabric: / oylama/VotingWeb** treeview düğümüne ve **ölçek hizmet**.

    ![Service Fabric Explorer ölçek hizmeti](./media/service-fabric-quickstart-java/scaleservicejavaquickstart.png)

    Şimdi web ön uç hizmetindeki örnek sayısını ölçeklendirebilirsiniz.

3. Rakamı **2** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.
4. Tıklayın **fabric: / oylama/VotingWeb** ağaç görünümünde düğümünü ve (bir GUID ile temsil edilen) bölüm düğümünü genişletin.

    ![Service Fabric Explorer ölçek hizmeti tamamlandı](./media/service-fabric-quickstart-java/servicescaled.png)

    Artık, hizmet iki örneklerine sahip ve örnekleri çalıştıracağınız hangi düğümlerin gördüğünüz ağaç görünümünde de görebilirsiniz.

Bu basit yönetim görevi sayesinde ön uç hizmetinizde kullanıcı yükünü işleyecek kaynak sayısını iki katına çıkarmış olduk. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığını anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Eclipse aracı olarak Service Fabric Java uygulamalarınız için kullanın.
> * Yerel kümenizdeki Java uygulamaları Dağıt 
> * Bir kümeye Azure içinde Java uygulamalarını dağıtma
> * Genişleme uygulama birden çok düğüm arasında

* Daha fazla bilgi edinmek [Eclipse kullanarak Java hizmetlerinde hata ayıklama](service-fabric-debugging-your-application-java.md)
* Hakkında bilgi edinin [sürekli integreation & Jenkins kullanarak dağıtım ayarlama](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* Checkout diğer [Java örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started)