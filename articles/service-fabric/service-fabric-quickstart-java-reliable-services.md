---
title: Azure'da Service Fabric üzerinde bir Java uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, bir Service Fabric güvenilir hizmetler örnek uygulaması kullanarak Azure için Java uygulaması oluşturursunuz.
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
ms.date: 10/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 2e3852ffc01312f01843a90de5f5565784b1c0b5
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114260"
---
# <a name="quickstart-deploy-a-java-reliable-services-application-to-service-fabric"></a>Hızlı başlangıç: Service Fabric'e bir Java güvenilir hizmetler uygulaması dağıtma

Azure Service Fabric; mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur.

Bu hızlı başlangıçta, Linux geliştirici makinesinde Eclipse IDE’yi kullanarak Service Fabric’e ilk Java uygulamanızı dağıtmayla ilgili bilgi verilmektedir. Bitirdiğinizde, oylama sonuçlarını kümedeki durum bilgisi içeren arka uç hizmetine kaydeden bir Java web ön ucuna sahip oylama uygulaması sağlanır.

![Uygulama Ekran Görüntüsü](./media/service-fabric-quickstart-java/votingapp.png)

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

* Service Fabric Java uygulamalarınız için Eclipse’ı kullanma
* Uygulamayı yerel kümenize dağıtma
* Uygulamayı Azure'da bir kümeye dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

1. [Service Fabric SDK ve Service Fabric Komut Satırı Arabirimi’ni (CLI) yükleme](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Git'i yükleyin](https://git-scm.com/)
3. [Eclipse’i yükleyin](https://www.eclipse.org/downloads/)
4. [Java Environment’ı ayarlayın](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development), Eclipse eklentisini yüklemek için isteğe bağlı adımları izlediğinizden emin olun

## <a name="download-the-sample"></a>Örneği indirme

Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```git
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

1. Aşağıdaki komutu çalıştırarak yerel kümenizi başlatın:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Yerel kümenin başlatılması biraz zaman alabilir. Kümenin sorunsuz çalıştığından emin olmak için, **http://localhost:19080** adresinden Service Fabric Explorer’a erişin. Sorunsuz beş düğüm, yerel kümenin çalıştığını belirtir.

    ![Yerel küme sorunsuz çalışıyor](./media/service-fabric-quickstart-java/localclusterup.png)

2. Eclipse’i açın.
3. Dosya Sistemi’nde Dosya -> Proje Aç seçeneğine tıklayın...
4. Dizin’e tıklayın ve GitHub’dan kopyaladığınız `service-fabric-java-quickstart` klasöründe yer alan `Voting` dizinini seçin. Son'a tıklayın.

    ![Eclipse İçeri Aktarma İletişim Kutusu](./media/service-fabric-quickstart-java/eclipseimport.png)

5. Artık, Package Explorer for Eclipse’de `Voting` projesine sahipsiniz.
6. Projeye sağ tıklayın ve **Service Fabric** açılan listesinde **Uygulamayı Yayımla...** seçeneğini belirleyin. **PublishProfiles/Local.json** öğesini Hedef Profil olarak seçin ve Yayımla’ya tıklayın.

    ![Yayımla İletişim Kutusu Konumu](./media/service-fabric-quickstart-java/localjson.png)

7. Sık kullandığınız web tarayıcısını açın ve**http://localhost:8080** adresine giderek uygulamaya erişin.

    ![Uygulama ön ucu Konumu](./media/service-fabric-quickstart-java/runninglocally.png)

Şimdi bir dizi oylama seçeneği ekleyebilir ve oyları almaya başlayabilirsiniz. Uygulama çalıştırılır ve ayrı bir veritabanına gerek kalmadan tüm verileri Service Fabric kümenizde depolar.

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

* Tarayıcı tabanlı bir araç: Service Fabric Explorer.
* Azure CLI 2.0 üzerinde çalışan Service Fabric Komut Satırı Arabirimi (CLI).
* PowerShell komutları.

Bu hızlı başlangıçta, Service Fabric CLI ve Service Fabric Explorer’ı kullanacaksınız.

CLI kullanmak için, indirdiğiniz PFX dosyasını temel alarak bir PEM dosyası oluşturmanız gerekir. Dosyayı dönüştürmek için aşağıdaki komutu kullanın. (Grup kümeleri için, **BeniOku** sayfasındaki yönergelerden PFX dosyanıza özgü bir komutu kopyalayabilirsiniz.)

    ```bash
    openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
    ```

Service Fabric Explorer’ı kullanmak için, Grup Kümesi web sitesinden indirdiğiniz sertifika PFX dosyasını sertifika deponuza (Windows veya Mac) ya da tarayıcıya (Ubuntu) aktarmanız gerekir. **BeniOku** sayfasından alabileceğiniz PFX özel anahtar parolası gerekir.

Sisteminizde sertifikayı içeri aktarmak için size en rahat gelen yöntemi kullanın. Örnek:

* Windows üzerinde: PFX dosyasına çift tıklayın ve `Certificates - Current User\Personal\Certificates` dizinindeki kişisel deponuza sertifikayı yüklemek için istemleri izleyin. Alternatif olarak, **BeniOku** yönergelerinde PowerShell komutunu kullanabilirsiniz.
* Mac üzerinde: PFX dosyasına çift tıklayın ve Anahtarlığınıza sertifikayı yüklemek için istemleri izleyin.
* Ubuntu üzerinde: Mozilla Firefox, Ubuntu 16.04 sistemindeki varsayılan tarayıcıdır. Sertifikayı Firefox’a aktarmak için, tarayıcınızın sağ üst köşesindeki menü düğmesine ve ardından **Seçenekler**’e tıklayın. **Tercihler** sayfasında arama kutusunu kullanarak "sertifikalar" terimini arayın. **Sertifikaları Görüntüle**’ye tıklayın, **Sertifikalarınız** sekmesini seçin, **İçeri Aktar**’a tıklayın ve sertifikayı içeri aktarma istemlerini izleyin.

   ![Firefox’ta sertifika yükleme](./media/service-fabric-quickstart-java/install-cert-firefox.png)

### <a name="add-certificate-information-to-your-application"></a>Sertifika bilgilerini uygulamanıza ekleme

Service Fabric programlama modellerini kullandığından, sertifika parmak izinin uygulamanıza eklenmesi gerekiyor.

1. Sertifikanızın parmak izinin, güvenli bir kümede çalışırken `Voting/VotingApplication/ApplicationManifest.xml` dosyasında olması gerekir. Sertifikanın parmak izini ayıklamak için aşağıdaki komutu çalıştırın.

    ```bash
    openssl x509 -in [CERTIFICATE_PEM_FILE] -fingerprint -noout
    ```

2. `Voting/VotingApplication/ApplicationManifest.xml` dosyasında, aşağıdaki kod parçacığını **ApplicationManifest** etiketi altına ekleyin. **X509FindValue** önceki adımdaki parmak izi olmalıdır (noktalı virgül olmamalı). 

    ```xml
    <Certificates>
        <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
    </Certificates>
    ```

### <a name="deploy-the-application-using-eclipse"></a>Eclipse kullanarak uygulamayı dağıtma

Uygulama ve kümeniz hazır olduğuna göre, doğrudan Eclipse’teki bir kümeye dağıtabilirsiniz.

1. **PublishProfiles** dizinindeki **Cloud.json** dosyasını açın ve `ConnectionIPOrURL` ile `ConnectionPort` alanlarını uygun şekilde doldurun. Örnek:

    ```bash
    {
         "ClusterConnectionParameters":
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "[path_to_your_pem_file_on_local_machine]",
            "ClientCert": "[path_to_your_pem_file_on_local_machine]"
         }
    }
    ```

2. Projeye sağ tıklayın ve **Service Fabric** açılan listesinde **Uygulamayı Yayımla...** seçeneğini belirleyin. **PublishProfiles/Cloud.json** öğesini Hedef Profil olarak seçin ve Yayımla’ya tıklayın.

    ![İletişim Kutusunu Yayımlama Bulutu](./media/service-fabric-quickstart-java/cloudjson.png)

3. Web tarayıcınızı açın ve **http://\<ConnectionIPOrURL>:8080** adresine giderek uygulamaya erişin.

    ![Uygulama ön ucu bulutu](./media/service-fabric-quickstart-java/runningcloud.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme

Hizmet yükündeki bir değişikliği karşılamak için kümedeki hizmetler kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizi ölçeklendirmenin birçok yolu vardır; örneğin, Service Fabric CLI’den (sfctl) betikler veya komutlar kullanabilirsiniz. Aşağıdaki adımlarda Service Fabric Explorer kullanılmaktadır.

Service Fabric Explorer tüm Service Fabric kümelerinde çalıştırılır ve tarayıcıdan kümelerin HTTP yönetim bağlantı noktasına (19080) göz atılarak (örneğin, `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`) erişilebilir.

Web ön uç hizmetini ölçeklendirmek için aşağıdakileri yapın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `https://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.
2. Ağaç görünümünde **fabric:/Voting/VotingWeb** düğümünün yanındaki üç noktaya tıklayın ve **Hizmeti Ölçeklendir**'i seçin.

    ![Service Fabric Explorer Hizmeti Ölçeklendir](./media/service-fabric-quickstart-java/scaleservicejavaquickstart.png)

    Şimdi web ön uç hizmetindeki örnek sayısını ölçeklendirebilirsiniz.

3. Rakamı **2** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.
4. Ağaç görünümünde **fabric:/Voting/VotingWeb** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin.

    ![Service Fabric Explorer Hizmeti Ölçeklendirme Tamamlandı](./media/service-fabric-quickstart-java/servicescaled.png)

    Hizmette üç örnek olduğunu ve ağaç görünümünde örneklerin çalıştığı düğümleri görebilirsiniz.

Bu basit yönetim görevi sayesinde ön uç hizmetinin kullanıcı yükünü işlemek için kullanabileceği kaynakları iki katına çıkarmış oldunuz. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığınızı anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric, kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Service Fabric Java uygulamalarınız için Eclipse’ı kullanma
* Java uygulamalarını yerel kümenize dağıtma
* Java uygulamalarını Azure’da yerel kümenize dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme

Service Fabric’te Java uygulamalarıyla çalışma hakkında daha fazla bilgi için Java uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Java uygulaması dağıtma](./service-fabric-tutorial-create-java-app.md)