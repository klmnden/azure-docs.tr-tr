---
title: Azure'da Service Fabric üzerinde Linux kapsayıcı uygulaması oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta uygulamanızla bir Docker görüntüsü oluşturacak, görüntüyü bir kapsayıcı kayıt defterine iletecek ve kapsayıcınızı bir Service Fabric kümesine dağıtacaksınız.
services: service-fabric
documentationcenter: linux
author: suhuruli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/11/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: b0ded0fb274f6b64935ddaba75abf23a94063120
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452660"
---
# <a name="quickstart-deploy-linux-containers-to-service-fabric"></a>Hızlı başlangıç: Linux kapsayıcıları Service Fabric'e dağıtma

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur.

Bu hızlı başlangıçta Linux kapsayıcıları bir Service Fabric kümesine nasıl dağıtacağınız gösterilmektedir. Tamamladığınızda Service Fabric kümesinde çalışan Python web ön ucu ve Redis arka ucundan oluşan bir oy verme uygulamasına sahip olacaksınız. Ayrıca, bir uygulamanın yükünü devretme ve kümenizde bir uygulamayı ölçeklendirme hakkında da bilgi edineceksiniz.

![Oylama uygulaması web sayfası][quickstartpic]

Bu hızlı başlangıçta, Service Fabric CLI komutlarını çalıştırmak için Azure Cloud Shell’de Bash ortamını kullanacaksınız. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Cloud Shell’i ilk kez çalıştırıyorsanız `clouddrive` dosya paylaşımınızı ayarlamanız istenir. Varsayılan değerleri kabul edebilir veya var olan bir dosya paylaşımını ekleyebilirsiniz. Daha fazla bilgi için bkz. [Bir `clouddrive` dosya paylaşımı ayarlama](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share).

## <a name="get-the-application-package"></a>Uygulama paketini alma

Kapsayıcıları Service Fabric üzerinde dağıtmak için ayrı kapsayıcıları ve uygulamayı açıklayan bildirim dosyası (uygulama tanımı) kümesine ihtiyacınız vardır.

Cloud Shell’de uygulama tanımının bir kopyasını çoğaltmak için git kullanın ve sonra dizinleri kopyanızdaki `Voting` dizini ile değiştirin.

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/Voting
```

## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluşturma

Uygulamayı Azure'a dağıtmak için, uygulamayı çalıştıracak bir Service Fabric kümesine ihtiyacınız vardır. Grup kümeleri, bir Service Fabric kümesini hızlıca oluşturmanın kolay bir yolunu sunar. Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleri olup Service Fabric ekibi tarafından çalıştırılır. Uygulamaları dağıtmak ve platform hakkında bilgi edinmek için grup kümelerini kullanabilirsiniz. Küme, düğümden düğüme ve istemciden düğüme güvenlik için tek bir otomatik olarak imzalanan sertifika kullanır.

Oturum açın ve bir [Linux kümesine](http://aka.ms/tryservicefabric) katılın. **PFX** bağlantısına tıklayarak PFX sertifikasını bilgisayarınıza indirin. **BeniOku** bağlantısına tıklayarak, sertifikayı kullanmak için çeşitli ortamların nasıl yapılandırılacağına ilişkin sertifika parolasını ve yönergelerini bulun. Hem **Hoş Geldiniz** sayfasını hem de **BeniOku** sayfasını açık tutun. Aşağıdaki adımlarda yer alan yönergelerden bazılarını kullanacaksınız.

> [!Note]
> Saat başına sınırlı sayıda grup kümesi vardır. Bir grup kümesine kaydolmaya çalıştığınızda hata alırsanız bir süre bekleyebilir ve tekrar deneyebilirsiniz veya [Azure’da Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md) bölümündeki adımları izleyerek aboneliğinizde bir küme oluşturabilirsiniz.
>
>Kendi kümenizi oluşturursanız, web ön uç hizmetinin 80 numaralı bağlantı noktasında gelen trafiği dinleyecek şekilde yapılandırıldığını unutmayın. Kümenizde bu bağlantı noktasının açık olduğundan emin olun. (Grup kümesi kullanıyorsanız bu bağlantı noktası açık durumdadır.)
>

## <a name="configure-your-environment"></a>Ortamınızı yapılandırma

Service Fabric, bir kümeyi ve uygulamalarını yönetmek için kullanabileceğiniz birçok araç sağlar:

- Tarayıcı tabanlı bir araç: Service Fabric Explorer.
- Azure CLI 2.0 üzerinde çalışan Service Fabric Komut Satırı Arabirimi (CLI).
- PowerShell komutları.

Bu hızlı başlangıçta, Cloud Shell içinde Service Fabric CLI ve Service Fabric Explorer’ı kullanacaksınız. Aşağıdaki bölümlerde bu araçlarla güvenli kümenize bağlanmak için gereken sertifikanın nasıl yükleneceği gösterilmiştir.

### <a name="configure-certificate-for-the-service-fabric-cli"></a>Sertifikayı Service Fabric CLI için yapılandırma

CLI’yi Cloud Shell’de kullanmak için sertifika PFX dosyasını Cloud Shell’e yüklemeniz ve sonra onu kullanarak bir PEM dosyası oluşturmanız gerekir.

1. Sertifikayı Cloud Shell’deki geçerli çalışma dizininize yüklemek için sertifika PFX dosyasını bilgisayarınızda indirildiği klasörden sürükleyip Cloud Shell penceresine bırakın.

2. PFX dosyasını PEM dosyasına dönüştürmek için aşağıdaki komutu kullanın. (Grup kümeleri için, **BeniOku** sayfasındaki yönergelerden PFX dosyanıza özgü bir komutu ve parolayı kopyalayabilirsiniz.)

    ```bash
    openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
    ```

### <a name="configure-certificate-for-service-fabric-explorer"></a>Sertifikayı Service Fabric Explorer için yapılandırma

Service Fabric Explorer’ı kullanmak için, Grup Kümesi web sitesinden indirdiğiniz sertifika PFX dosyasını sertifika deponuza (Windows veya Mac) ya da tarayıcıya (Ubuntu) aktarmanız gerekir. **BeniOku** sayfasından alabileceğiniz PFX özel anahtar parolası gerekir.

Sisteminizde sertifikayı içeri aktarmak için size en rahat gelen yöntemi kullanın. Örnek:

- Windows üzerinde: PFX dosyasına çift tıklayın ve `Certificates - Current User\Personal\Certificates` dizinindeki kişisel deponuza sertifikayı yüklemek için istemleri izleyin. Alternatif olarak, **BeniOku** yönergelerinde PowerShell komutunu kullanabilirsiniz.
- Mac üzerinde: PFX dosyasına çift tıklayın ve Anahtarlığınıza sertifikayı yüklemek için istemleri izleyin.
- Ubuntu üzerinde: Mozilla Firefox, Ubuntu 16.04 sistemindeki varsayılan tarayıcıdır. Sertifikayı Firefox’a aktarmak için, tarayıcınızın sağ üst köşesindeki menü düğmesine ve ardından **Seçenekler**’e tıklayın. **Tercihler** sayfasında arama kutusunu kullanarak "sertifikalar" terimini arayın. **Sertifikaları Görüntüle**’ye tıklayın, **Sertifikalarınız** sekmesini seçin, **İçeri Aktar**’a tıklayın ve sertifikayı içeri aktarma istemlerini izleyin.

   ![Firefox’ta sertifika yükleme](./media/service-fabric-quickstart-containers-linux/install-cert-firefox.png)

## <a name="deploy-the-service-fabric-application"></a>Service Fabric uygulamasını dağıtma

1. Cloud Shell’de CLI kullanarak Azure'daki Service Fabric kümesine bağlanın. Uç nokta, kümenizin yönetim uç noktasıdır. Önceki bölümde PEM dosyasını oluşturdunuz. (Grup kümeleri için, **BeniOku** sayfasındaki yönergelerden PEM dosyanıza özgü bir komutu ve yönetim uç noktasını kopyalayabilirsiniz.)

    ```bash
    sfctl cluster select --endpoint https://linh1x87d1d.westus.cloudapp.azure.com:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
    ```

2. Yükleme betiğini kullanarak Oylama uygulaması tanımını kümeye kopyalayın, uygulama türünü kaydedin ve uygulamanın bir örneğini oluşturun.

    ```bash
    ./install.sh
    ```

3. Bir web tarayıcısı açın ve kümenizin Service Fabric Explorer uç noktasına gidin. Uç nokta biçimi şu şekildedir: **https://\<my-azure-service-fabric-cluster-url>:19080/Explorer**; örneğin, `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`. </br>(Grup kümelerinde kümenizin Service Fabric Explorer uç noktasını **Giriş** sayfasında bulabilirsiniz.)

4. **Uygulamalar** düğümünü genişlettiğinizde oluşturduğunuz Oylama uygulama türü ve örneği için bir giriş oluşturulduğunu göreceksiniz.

    ![Service Fabric Explorer][sfx]

5. Çalışan kapsayıcıya bağlanmak için bir web tarayıcısı açın ve kümenizin URL'sine gidin; örneğin, `http://linh1x87d1d.westus.cloudapp.azure.com:80`. Oy verme uygulamasını tarayıcıda görmeniz gerekir.

    ![Oylama uygulaması web sayfası][quickstartpic]

> [!NOTE]
> Docker compose ile Service Fabric uygulamaları da dağıtabilirsiniz. Örneğin, Docker Compose kullanarak uygulamayı kümeye dağıtıp yüklemek için aşağıdaki komut kullanılabilir.
>  ```bash
> sfctl compose create --deployment-name TestApp --file-path ../docker-compose.yml
> ```

## <a name="fail-over-a-container-in-a-cluster"></a>Kümedeki bir kapsayıcıya yük devretme

Service Fabric, bir hata oluşması durumunda kapsayıcı örneklerinizin kümedeki diğer düğümlere otomatik olarak taşınmasını sağlar. Bir düğümü kapsayıcılar için el ile boşaltabilir ve kümedeki diğer düğümlere taşıyabilirsiniz. Service Fabric, hizmetlerinizi ölçeklendirmek için çeşitli yöntemler sağlar. Aşağıdaki adımlarda Service Fabric Explorer kullanacaksınız.

Ön uç kapsayıcısında yük devretmek için aşağıdaki adımları uygulayın:

1. Kümenizde Service Fabric Explorer'ı açın; örneğin, `https://linh1x87d1d.westus.cloudapp.azure.com:19080/Explorer`.
2. Ağaç görünümünde **fabric:/Voting/azurevotefront** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin. Ağaç görünümünde kapsayıcının üzerinde çalıştığı düğümleri gösteren düğüm adına dikkat edin; örneğin, `_nodetype_4`.
3. Ağaç görünümünde **Düğümler** düğümünü genişletin. Kapsayıcıyı çalıştıran düğümün yanındaki üç noktaya (...) tıklayın.
4. İlgili düğümü yeniden başlatmak için **Yeniden Başlat**'ı seçin ve yeniden başlatma eylemini onaylayın. Yeniden başlatma durumunda kapsayıcıdan kümedeki başka bir düğüme yük devretme gerçekleştirilir.

    ![Service Fabric Explorer'da düğüm görünümü][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme

Hizmet yükünü karşılamak için bir kümedeki Service Fabric hizmetleri kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz.

Web ön uç hizmetini ölçeklendirmek için aşağıdaki adımları gerçekleştirin:

1. Kümenizde Service Fabric Explorer'ı açın; örneğin, `https://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Ağaç görünümünde **fabric:/Voting/azurevotefront** düğümünün yanındaki üç noktaya tıklayın ve **Hizmeti Ölçeklendir**'i seçin.

    ![Service Fabric Explorer hizmet ölçeklendirmeyi başlatma][containersquickstartscale]

    Şimdi web ön uç hizmetindeki örnek sayısını ölçeklendirebilirsiniz.

3. Rakamı **2** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.
4. Ağaç görünümünde **fabric:/Voting/azurevotefront** düğümüne tıklayın ve bölüm düğümünü (GUID ile gösterilir) genişletin.

    ![Service Fabric Explorer hizmet ölçeklendirme tamamlandı][containersquickstartscaledone]

    Hizmetin artık iki örneği olduğunu görebilirsiniz. Ağaç görünümünde örneklerin üzerinde çalıştığı düğümleri görebilirsiniz.

Bu basit yönetim görevi sayesinde ön uç hizmetinin kullanıcı yükünü işlemek için kullanabileceği kaynakları iki katına çıkarmış oldunuz. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığınızı anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric, kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Kümeden uygulama örneğini silmek ve uygulama türünün kaydını silmek için şablonda sağlanan kaldırma betiğini (uninstall.sh) kullanın. Bu betiğin örneği temizlemesi zaman alacağından betiği bu betikten hemen sonra çalıştırmamanız gerekir. Service Fabric Explorer'ı kullanarak örneğin ne zaman kaldırıldığını ve kaydı silinen uygulama türünü belirleyebilirsiniz.

    ```bash
    ./uninstall.sh
    ```

2. Kümenizle çalışmayı tamamladıysanız, sertifikayı sertifika deposundan kaldırabilirsiniz. Örnek:
   - Windows: [Sertifikalar MMC ek bileşenini](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) kullanın. Ek bileşeni eklerken **Kullanıcı hesabım**’ı seçtiğinizden emin olun. `Certificates - Current User\Personal\Certificates` sayfasına gidip sertifikayı kaldırın.
   - Mac: Anahtarlık uygulamasını kullanın.
   - Ubuntu: Sertifikaları görüntülemek ve sertifikayı kaldırmak için kullandığınız adımları izleyin.

3. Cloud Shell kullanmaya devam etmek istemiyorsanız, ücret yansımasını engellemek için onunla ilişkili depolama hesabını silebilirsiniz. Cloud Shell oturumunuzu kapatın. Azure portalında Cloud Shell ile ilişkili depolama hesabına ve sonra sayfanın üst kısmındaki **Sil** öğesine tıklayın ve istemlere yanıt verin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure’daki bir Service Fabric kümesine Linux kapsayıcı uygulaması dağıttınız, uygulama üzerinde bir yük devretme işlemi gerçekleştirdiniz ve uygulamayı kümede ölçeklendirdiniz. Service Fabric’te Linux kapsayıcılarıyla çalışma hakkında daha fazla bilgi için Linux kapsayıcı uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Linux kapsayıcı uygulaması oluşturma](./service-fabric-tutorial-create-container-images.md)

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png
