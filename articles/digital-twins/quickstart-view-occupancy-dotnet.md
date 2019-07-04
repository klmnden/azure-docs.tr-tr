---
title: Azure dijital İkizlerini kullanılabilir odaları - bulma | Microsoft Docs
description: Bu hızlı başlangıçta iki .NET Core örnek uygulaması çalıştırarak bir Azure Digital Twins alanına sanal hareket ve karbondioksit telemetri verileri göndereceksiniz. Burada hedefiniz, veriler bulutta işlendikten sonra Yönetim API'leriyle temiz havaya sahip olan uygun odaları bulmak olacak.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc seodec18
ms.date: 06/26/2019
ms.author: alinast
ms.openlocfilehash: 9f88eccf1e488d52fbbbd064cc5d7f54c2eec32b
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459133"
---
# <a name="quickstart-find-available-rooms-by-using-azure-digital-twins"></a>Hızlı Başlangıç: Azure dijital İkizlerini kullanarak kullanılabilir odaları Bul

Azure dijital İkizlerini service, fiziksel ortamınızın dijital bir görüntüsünü yeniden oluşturmak sağlar. Bu işlemin ardından ortamınızdaki olaylarla ilgili bildirimler alabilir ve verdiğiniz yanıtları özelleştirebilirsiniz.

Bu hızlı başlangıçta kullanılmaktadır [.NET örnekleri çifti](https://github.com/Azure-Samples/digital-twins-samples-csharp) sanal ofis binasını dijitalleştirerek. Bu yapı içinde kullanılabilir odaları nasıl gösterir. Dijital İkizlerini kullanmaya birçok sensörlerden ortamınız ile ilişkilendirebilirsiniz. Bunu ayrıca kullanılabilir odanıza hava kalitesini sanal algılayıcı tasarruf edilen karbon dioksit için Yardım en uygun olup olmadığını öğrenebilirsiniz. Örnek uygulamalarından biridir, bu senaryo görselleştirmenize yardımcı olmak için rastgele sensör verilerini oluşturur.

Aşağıdaki videoda hızlı başlangıç ayarları özetlenir:

>[!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Önkoşullar

1. Azure hesabınız yoksa, başlamadan önce [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Bu hızlı başlangıçta çalıştırdığınız iki konsol uygulamaları kullanılarak yazılan C#. Yükleme [.NET Core SDK'sı sürüm 2.1.403 veya yukarıdaki](https://www.microsoft.com/net/download) geliştirme makinenizde. .NET Core SDK varsa, geçerli sürümünü doğrulama C# geliştirme makinenizde. Çalıştırma `dotnet --version` bir komut isteminde.

1. İndirme [örnek C# proje](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip). Dijital-twins-samples-csharp-master.zip arşivini ayıklayın.

## <a name="create-a-digital-twins-instance"></a>Digital Twins örneği oluşturma

Dijital İkizlerini yeni bir örneğini oluşturma [portalı](https://portal.azure.com) bu bölümdeki adımları izleyerek.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Uygulamanızın izinlerini ayarlama

Dijital İkizlerini örneğinizin erişebilmesi için bu bölümde, örnek uygulamanızı Azure Active Directory (Azure AD) kaydeder. Bir Azure AD uygulama kaydı zaten varsa, Örneğiniz için yeniden kullanın. Bu bölümde açıklanan şekilde yapılandırıldığından emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="build-application"></a>Uygulama oluşturma

Sahiplik uygulamayı aşağıdaki adımları izleyerek oluşturun.

1. Bir komut istemi açın. Klasörüne gidin Burada, `digital-twins-samples-csharp-master.zip` dosyaları ayıklanır.
1. `cd occupancy-quickstart/src` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) dosyasını düzenleyerek aşağıdaki değişkenleri güncelleştirin:
    - **ClientID**: Önceki bölümde belirtilen Azure AD uygulama kaydı uygulama Kimliğini girin.
    - **Kiracı**: Ayrıca önceki bölümde belirtilen Azure AD kiracınızda dizin Kimliğini girin.
    - **BaseUrl**: Dijital İkizlerini örneğinizin yönetim API URL'si şu biçimdedir `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Bu URL içindeki yer tutucuları önceki bölümde Örneğiniz için değerlerle değiştirin.

## <a name="provision-graph"></a>Grafı sağlama

Bu adım, dijital İkizlerini uzamsal grafik ile sağlar:

- Çeşitli alanları.
- Bir cihaz.
- İki algılayıcılar.
- Özel bir işlev.
- Bir rol ataması.

Uzamsal graph kullanılarak sağlanan [provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) dosya.

1. `dotnet run ProvisionSample` öğesini çalıştırın.
    >[!NOTE]
    >Cihaz oturum açma Azure CLI aracını, Azure ad kullanıcının kimliğini doğrulamak için kullanılır. Kullanıcı kimlik doğrulaması için belirli bir kod girmeden [Microsoft oturum açma](https://microsoft.com/devicelogin) sayfası. İlgili kod girildikten sonra kimlik doğrulaması için adımları izleyin. Aracı çalıştırırken, kullanıcının kimliğini doğrulaması gerekir.

    >[!TIP]
    > Bu adımı çalıştırdığınızda aşağıdaki hata iletisi görüntülenirse, değişkenlerinizi düzgün bir şekilde kopyalanan emin olun: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

1. Sağlama adım birkaç dakika sürebilir. Ayrıca, IOT hub'ı dijital İkizlerini örneğinizin içinde sağlar. IOT hub'ı durum gösterilene kadar aracılığıyla döngü =`Running`.

    ![Sağlama örneği][4]

1. Yürütme sonunda kopyalama `ConnectionString` cihazın kullanılmak üzere cihaz simülatörü örnek. Yalnızca bu görüntüde ana hatlarıyla belirtilen dizeyi kopyalayın.

    ![Sağlama örneği][1]

    >[!TIP]
    > Görüntüleyebilir ve uzamsal graph aracılığıyla değiştirmek [Azure dijital İkizlerini graf Görüntüleyicisi](https://github.com/Azure/azure-digital-twins-graph-viewer).

## <a name="send-sensor-data"></a>Sensör verilerini gönderme

Oluşturun ve aşağıdaki adımları izleyerek algılayıcısı simülatör uygulamayı çalıştırın.

1. Yeni bir komut istemi açın. Dijital-twins-samples-csharp-master klasöründe indirdiğiniz projeye gidin.
1. `cd device-connectivity` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. Düzen [appsettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) güncelleştirilecek **DeviceConnectionString** önceki ile `ConnectionString`.
1. Çalıştırma `dotnet run` sensör verilerini göndermeyi başlatamadı.%n%nolası. Aşağıdaki görüntüde gösterildiği gibi sayısal çiftleri için gönderilen görürsünüz.

     ![Cihaz Bağlantısı][2]

1. Bu simülatörünü çalıştırın, böylece sonraki adım eylemi ile yan yana sonuçlarını görüntüleyebilirsiniz olanak tanır. Bu pencere dijital çiftleri için gönderilen sanal sensör verilerini gösterir. Sonraki adım sorgular gerçek zamanlı olarak güncel hava ile kullanılabilir odaları bulunacak.

    >[!TIP]
    > Bu adımı çalıştırdığınızda emin `DeviceConnectionString` aşağıdaki hata iletisi görüntülenirse düzgün bir şekilde kopyalandı: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Temiz havaya sahip olan odaları bulma

Sensör örnek iki algılayıcılar için rastgele veri değerleri benzetimini yapar. Bunlar, hareket ve tasarruf edilen karbon dioksit hedeflenmiştir. Kullanılabilir alanları ile yeni hava örnekte hiçbir durum odadaki tanımlanır. Ayrıca bir tasarruf edilen karbon dioksit düzeyi altında 1.000 ppm tanımlı. Koşul yerine değil, alanı mevcut değil veya uzaktan kalite düşük.

1. Önceki sağlama adım çalıştırmak için kullanılan komut istemi açın.
1. `dotnet run GetAvailableAndFreshSpaces` öğesini çalıştırın.
1. Bu komut istemi ve sensör verilerini komut istemi yan yana bakın.

    Bir komut istemi, beş saniyede dijital çiftleri için sanal hareket ve tasarruf edilen karbon dioksit verileri gönderir. Diğer komutu, grafiğin gerçek zamanlı olarak kullanıma kullanılabilir odaları rastgele simülasyon verileri temel alan yeni hava bulmak için okur. Bu koşullardan biri son gönderilen algılayıcı verileri temel alan neredeyse gerçek zamanlı gösterir:
   - Temiz havaya sahip olan uygun odalar.
   - Dolu veya hava kalitesi düzeyi düşük olan odalar.

     ![Temiz havaya sahip olan odaları alma][3]

Bu hızlı başlangıçta ne olduğunu ve hangi API'ler çağrıldığında anlamak için açık [Visual Studio Code](https://code.visualstudio.com/Download) kod çalışma alanı projeyle dijital-twins-samples-csharp içinde bulunamadı. Aşağıdaki komutu kullanın:

```plaintext
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

Öğreticiler, derin koda gidin. Bunlar, yapılandırma verisini değiştirmesine nasıl ve hangi API'ler çağrıldığında öğretin. Yönetim API'leri hakkında daha fazla bilgi için dijital İkizlerini Swagger sayfanıza gidin:

```plaintext
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Ad | Şununla değiştir |
| --- | --- |
| YOUR_INSTANCE_NAME | Dijital İkizlerini örneğinizin adı |
| YOUR_LOCATION | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

Veya kolaylık sağlamak için göz atın [dijital İkizlerini Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiler kullanma hakkında daha fazla ayrıntıya gidin:

- Tesis yöneticilerinin occupant üretkenliği artırmak için bir uygulama oluşturun.
- Oluşturmaya çalışması daha verimli bir şekilde.

Öğreticilerine devam etmek için kaynakları oluşturulan temizlemeyin Bu hızlı başlangıçta. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silin.

1. Örnek depoyu indirildiğinde oluşturduğunuz klasörü silin.
1. İçinde soldaki menüden [Azure portalında](https://portal.azure.com)seçin **tüm kaynakları**. Ardından dijital İkizlerini kaynağınızı seçin. Üst kısmındaki **tüm kaynakları** bölmesinde **Sil**.

    > [!TIP]
    > Dijital İkizlerini örneğinizin silme sorun olduysa, bir hizmet güncelleştirmesi düzeltme alındı. Örneğiniz silme yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir senaryo odaları ile iyi çalışma koşullarına nasıl göstermek için kullanılır. Bu senaryonun ayrıntılı analiz için bu öğreticiye bakın:

>[!div class="nextstepaction"]
>[Öğretici: Azure dijital İkizlerini dağıtma ve uzamsal graph'ı yapılandırma](tutorial-facilities-setup.md)

<!-- Images -->
[1]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png
[2]: media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png
[3]: media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png
[4]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png
