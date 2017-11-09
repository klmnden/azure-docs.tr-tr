---
title: ".NET Service Fabric uygulaması oluşturma | Microsoft Docs"
description: "Bir .NET uygulaması için Azure Service Fabric hızlı başlangıç örnek kullanarak oluşturun."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: mikhegn
ms.custom: mvc, devcenter
ms.openlocfilehash: 40b29ccb454caf5462807d6c24ca3f470865d368
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="create-a-net-service-fabric-application-in-azure"></a>.NET Service Fabric uygulaması oluşturma
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

Bu hızlı başlangıç ilk .NET uygulamanızı Service Fabric dağıtmak gösterilmektedir. İşlemi tamamladığınızda, oylama bir durum bilgisi olan bir arka uç hizmetinde kümedeki Oylama sonuçlarını kaydettiği ön uç bir ASP.NET Core web uygulamasıyla sahip.

![Uygulama ekran görüntüsü](./media/service-fabric-quickstart-dotnet/application-screenshot.png)

Bu uygulama hakkında bilgi edineceksiniz kullanarak nasıl yapılır:
> [!div class="checklist"]
> * .NET ve Service Fabric kullanarak uygulama oluşturma
> * Bir web ön uç ASP.NET core kullanın
> * Durum bilgisi olan hizmet uygulama verilerini depolamak
> * Uygulamanızı yerel olarak hata ayıklama
> * Bir kümede Azure uygulamayı dağıtmak
> * Genişleme uygulama birden çok düğüm arasında
> * Uygulama yükseltme gerçekleştirme

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıcı tamamlamak için:
1. [Visual Studio 2017 yükleme](https://www.visualstudio.com/) ile **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleri.
2. [Git'i yükleyin](https://git-scm.com/)
3. [Microsoft Azure Service Fabric SDK yükleme](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK)
4. Yerel Service Fabric kümesi dağıtmak Visual Studio etkinleştirmek için aşağıdaki komutu çalıştırın:
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
    ```

## <a name="download-the-sample"></a>Örneği indirme
Bir komut penceresinde örnek uygulama depoyu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.
```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma
Başlat menüsü Visual Studio simgesini sağ tıklatın ve seçin **yönetici olarak çalıştır**. Hata ayıklayıcı hizmetlerinize iliştirmek için Visual Studio'yu yönetici olarak çalıştırmanız gerekir.

Açık **Voting.sln** kopyaladığınız deponun Visual Studio çözümü.  

Varsayılan olarak, oylama uygulaması, 8080 bağlantı noktasında dinleyecek şekilde ayarlanır.  Uygulama bağlantı noktası kümesinde */VotingWeb/PackageRoot/ServiceManifest.xml* dosya.  Güncelleştirerek uygulama bağlantı noktasını değiştirebilirsiniz **bağlantı noktası** özniteliği **Endpoint** öğesi.  Dağıtma ve uygulama yerel olarak çalıştırmak için uygulama bağlantı noktası açık ve bilgisayarınızdaki kullanılabilir olması gerekir.  Uygulama bağlantı noktasını değiştirirseniz, bu makale boyunca "8080" için yeni uygulama bağlantı noktası değeri değiştirin.

Uygulamayı dağıtmak için basın **F5**.

> [!NOTE]
> İlk kez çalıştırın ve uygulama dağıtma, Visual Studio hata ayıklama için yerel bir küme oluşturur. Bu işlem biraz zaman alabilir. Küme oluşturma durumu, Visual Studio çıkış penceresinde görüntülenir.  Çıktıda "Uygulama URL'si ayarlanmadı veya tarayıcı uygulamaya açılmaz için bir HTTP/HTTPS URL'si değil." iletisini görürsünüz  Bir tarayıcı olmayan otomatik başlatılan ancak bu bu iletiyi bir hata olduğunu göstermez.

Dağıtım tamamlandığında, bir tarayıcı başlatmak ve bu sayfayı açın: `http://localhost:8080` -uygulamanın ön uç web.

![Uygulama ön uç](./media/service-fabric-quickstart-dotnet/application-screenshot-new.png)

Şimdi, oylama seçenekleri kümesi ekleyin ve oy almaya başlayın. Uygulamayı çalışır ve ayrı bir veritabanı gerek kalmadan, Service Fabric kümesindeki tüm verileri depolar.

## <a name="walk-through-the-voting-sample-application"></a>Yol üzerinden oylama örnek uygulama
Oylama uygulaması iki hizmetinden oluşur:
- Web ön uç hizmeti (VotingWeb) - bir ASP.NET Core web web sayfası hizmet ön uç hizmeti ve düzenlemenizi sağlayan web arka uç hizmeti ile iletişim için API'ler.
- Arka uç hizmetine (VotingData)-oy sonuçları güvenilir sözlükte depolamak için bir API sunar bir ASP.NET Core web hizmeti kalıcı disk üzerinde.

![Uygulama diyagramı](./media/service-fabric-quickstart-dotnet/application-diagram.png)

Aşağıdaki olaylar, uygulamada oy oluşur:
1. JavaScript oy isteği için web ön uç hizmeti web API'si bir HTTP PUT İsteği gönderir.

2. Web ön uç hizmeti bulun ve bir HTTP PUT İsteği arka uç hizmetine iletmek için bir proxy kullanır.

3. Arka uç hizmetine gelen isteği alır ve güncelleştirilmiş sonuç kümesi içinde birden çok düğüm çoğaltılan alır ve diskte kalıcı bir güvenilir bir sözlük depolar. Hiçbir veritabanı gerektiği şekilde uygulamanın tüm veri kümesinde depolanır.

## <a name="debug-in-visual-studio"></a>Visual Studio'da hata ayıklama
Visual Studio uygulamasında hata ayıklama sırasında yerel bir Service Fabric geliştirme küme kullanıyor. Hata ayıklama deneyiminizi senaryonuz için ayarlamak için seçeneğiniz vardır. Bu uygulama, verileri güvenilir sözlüğünü kullanarak arka uç hizmetinde depolanır. Hata ayıklayıcıyı durdurduğunuzda visual Studio uygulama varsayılan başına kaldırır. Uygulama kaldırma verileri de kaldırılacak arka uç hizmetinde neden olur. Hata ayıklama oturumları arasında veri kalıcı hale getirmek için değiştirebileceğiniz **uygulama hata ayıklama modu** bir özellik olarak **oylama** Visual Studio projesi.

Kod içinde neler aramak için aşağıdaki adımları tamamlayın:
1. Açık **/VotingWeb/Controllers/VotesController.cs** dosya ve web API'ın bir kesme noktası belirleyerek **Put** yöntemi (satır 47) - Visual Studio'daki Çözüm Gezgini'nde dosyayı arayabilirsiniz.

2. Açık **/VotingData/ControllersVoteDataController.cs** dosya ve bu web API'nin bir kesme noktası belirleyerek **Put** yöntemi (satır 50).

3. Tarayıcıya geri dönün ve oylama seçeneği tıklatın veya yeni bir oylama seçeneği ekleyin. Web ön uç 's API denetleyicisi içinde ilk kesme noktası isabet.
    - Burada tarayıcısında JavaScript bir isteği ön uç hizmeti olan web API denetleyicisi gönderir budur.
    
    ![Oy ön uç Hizmet Ekle](./media/service-fabric-quickstart-dotnet/addvote-frontend.png)

    - İlk olarak, arka uç hizmetimizi ReverseProxy URL'si oluşturmak **(1)**.
    - Ardından, ReverseProxy PUT HTTP isteği göndermek **(2)**.
    - Son olarak, arka uç hizmetinden yanıt istemciye Döndür **(3)**.

4. Tuşuna **F5** devam etmek için
    - Artık arka uç hizmetinde kesme noktasında bulunur.
    
    ![Oy arka uç hizmeti ekleme](./media/service-fabric-quickstart-dotnet/addvote-backend.png)

    - Yönteminin ilk satırında **(1)** `StateManager` alır veya adlı güvenilir sözlüğü ekler `counts`.
    - Kullanarak bu işlem, güvenilir bir sözlükteki değerlerle tüm etkileşimleri gerektiren deyimi **(2)** bu işlem oluşturur.
    - İşlem, oylama seçeneği için ilgili anahtarının değerini güncelleştirin ve işlemi yürütme **(3)**. Yürütme yöntemi döndürür, veri sözlüğünde güncelleştirilmiş ve kümedeki diğer düğümlere çoğaltılan sonra. Veri şimdi güvenle kümede depolanır ve arka uç hizmeti üzerinden kullanılabilir veri yaşamaya diğer düğümlere başarısız olabilir.
5. Tuşuna **F5** devam etmek için

Hata ayıklama oturumu durdurmak için basın **Shift + F5**.

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma
Uygulamayı Azure'a dağıtmak için uygulama çalışan bir Service Fabric kümesi gerekir. 

### <a name="join-a-party-cluster"></a>Taraf kümeye Katıl
Grup kümeleri, Azure üzerinde barındırılan ve Service Fabric ekibi tarafından sunulan ücretsiz, sınırlı süreli Service Fabric kümeleridir. Bu kümelerde herkes uygulama dağıtabilir ve platform hakkında bilgi edinebilir. 

Oturum açın ve [Windows kümeye](http://aka.ms/tryservicefabric). Unutmayın **bağlantı uç noktasının** adımları izleyerek kullanılan değer.

> [!Note]
> Varsayılan olarak, web ön uç hizmeti 8080 bağlantı noktasından gelen trafiği dinleyecek şekilde yapılandırılır. Bağlantı noktası 8080 taraf kümede açıktır.  Uygulama bağlantı noktasını değiştirmeniz gerekirse, bir taraf kümede açık bağlantı noktalarını değiştirin.
>

### <a name="deploy-the-application-using-visual-studio"></a>Visual Studio kullanarak uygulamayı dağıtın
Uygulama hazır olduğuna göre, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

1. Sağ **oylama** Çözüm Gezgini'nde ve **Yayımla**. Yayımla iletişim kutusu görüntülenir.

    ![Yayımla İletişim Kutusu](./media/service-fabric-quickstart-dotnet/publish-app.png)

2. Kopya **bağlantı uç noktasının** taraf küme sayfasından **bağlantı uç noktasının** alanına gelin ve **Yayımla**. Örneğin, `winh1x87d1d.westus.cloudapp.azure.com:19000`.

    Kümedeki her bir uygulama benzersiz bir ad olmalıdır.  Ancak bir ortak, paylaşılan ortamı parti kümeleri olan ve var olan bir uygulama ile bir çakışma olabilir.  Bir ad çakışması varsa, Visual Studio projesi yeniden adlandırın ve yeniden dağıtın.

3. Tarayıcı ve küme adresini yazın ve ardından Aç ': 8080 kümedeki - Örneğin, uygulama almak için ' `http://winh1x87d1d.westus.cloudapp.azure.com:8080`. Artık Azure kümede çalışan uygulama görmeniz gerekir.

![Uygulama ön uç](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme
Service Fabric Hizmetleri, bir değişiklik Hizmetleri üzerindeki yükü için uygun hale getirmek için bir küme içinde kolayca genişletilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizin ölçeklendirme birkaç yolu vardır, betikleri veya komutları PowerShell veya Service Fabric CLI (sfctl) kullanabilirsiniz. Bu örnekte, Service Fabric Explorer kullanın.

Service Fabric Explorer tüm Service Fabric kümelerinde çalışır ve kümeleri HTTP yönetim bağlantı noktası (19080), örneğin, göz atarak bir tarayıcıdan erişilebilir `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.

Web ön uç hizmetini ölçeklendirmek için aşağıdaki adımları gerçekleştirin:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Yanındaki üç nokta (üç nokta) tıklayın **fabric: / oylama/VotingWeb** treeview düğümüne ve **ölçek hizmet**.

    ![Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scale.png)

    Şimdi web ön uç hizmetindeki örnek sayısını ölçeklendirebilirsiniz.

3. Rakamı **2** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.
4. Tıklayın **fabric: / oylama/VotingWeb** ağaç görünümünde düğümünü ve (bir GUID ile temsil edilen) bölüm düğümünü genişletin.

    ![Service Fabric Explorer ölçek hizmeti](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scaled-service.png)

    Bir gecikmeden sonra hizmet iki örneği olduğunu görebilirsiniz.  Ağaç görünümünde örnekleri çalıştıracağınız hangi düğümlerin bakın.

Bu basit yönetim görevi tarafından kullanıcı yükünü işlemek ön uç hizmeti için kullanılabilir kaynakları iki katına. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığını anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="perform-a-rolling-application-upgrade"></a>Uygulama yükseltme gerçekleştirme
Service Fabric yeni güncelleştirmeleri uygulamanızı dağıtırken güncelleştirmeyi güvenli bir yolla yapar. Çalışırken hataları gerçekleşeceğini yanı sıra otomatik geri alma yükseltirken kapalı kalma süresi sağlar.

Uygulama yükseltmek için aşağıdakileri yapın:

1. Açık **/VotingWeb/Views/Home/Index.cshtml** dosyasını Visual Studio'da.
2. Değiştirme <h2> Başlık ekleyerek veya metnini güncelleştirme sayfada. Örneğin, "Service Fabric oylama örnek v2" başlığı değiştirin.
3. Dosyayı kaydedin.
4. Sağ **oylama** Çözüm Gezgini'nde ve **Yayımla**. Yayımla iletişim kutusu görüntülenir.
5. ' I tıklatın **bildirim sürüm** düğmesi hizmet ve uygulama sürümü değiştirin.
6. Sürümünü değiştirmek **kod** öğesinin altında **VotingWebPkg** "2.0.0" Örneğin ve tıklatın için **kaydetmek**.

    ![Değişiklik sürüm iletişim](./media/service-fabric-quickstart-dotnet/change-version.png)
7. İçinde **Service Fabric uygulaması yayımlama** iletişim kutusu, onay yükseltme uygulama onay kutusunu ve tıklatın **Yayımla**.

    ![Yayımlama iletişim kutusu ayarı yükseltme](./media/service-fabric-quickstart-dotnet/upgrade-app.png)
8. Tarayıcınızı açın ve Gözat küme adresine bağlantı noktası 19080 - Örneğin, `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
9. Tıklayın **uygulamaları** ağaç görünümünde düğümünü ve ardından **yükseltme devam eden** sağ taraftaki bölmeden. Her etki alanı sonraki geçmeden önce sağlıklı olduğundan emin olmanızı nasıl yükseltme kümenizdeki, yükseltme etki alanlarında ilerlerken yapar bakın. Etki alanının sistem belirlediğinizde bir yükseltme etki alanında ilerleme çubuğu yeşil görüntülenir.
    ![Service Fabric Explorer görünümünde yükseltme](./media/service-fabric-quickstart-dotnet/upgrading.png)

    Service Fabric yükseltmelerinin hizmeti kümedeki her düğümde yükselttikten sonra iki dakika bekleyerek güvenli hale getirir. Tüm güncelleştirme yaklaşık sekiz dakika bekler.

10. Yükseltme çalışırken, uygulamayı kullanmaya devam edebilirsiniz. Kümede çalışan hizmetin iki örneğini bulunduğundan, diğerleri hala eski sürümü alabilir ancak bazı isteklerinizi uygulama yükseltilmiş sürümünü alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * .NET ve Service Fabric kullanarak uygulama oluşturma
> * Bir web ön uç ASP.NET core kullanın
> * Durum bilgisi olan hizmet uygulama verilerini depolamak
> * Uygulamanızı yerel olarak hata ayıklama
> * Bir kümede Azure uygulamayı dağıtmak
> * Genişleme uygulama birden çok düğüm arasında
> * Uygulama yükseltme gerçekleştirme

Service Fabric ve .NET hakkında daha fazla bilgi için Bu öğretici göz atın:
> [!div class="nextstepaction"]
> [Service Fabric üzerinde .NET uygulaması](service-fabric-tutorial-create-dotnet-app.md)