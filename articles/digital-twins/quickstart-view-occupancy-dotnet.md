---
title: Azure Digital Twins ile uygun odaları bulma (C#) | Microsoft Docs
description: Bu hızlı başlangıçta iki .NET Core örnek uygulaması çalıştırarak bir Azure Digital Twins alanına sanal hareket ve karbondioksit telemetri verileri göndereceksiniz. Burada hedefiniz, veriler bulutta işlendikten sonra Yönetim API'leriyle temiz havaya sahip olan uygun odaları bulmak olacak.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/7/2018
ms.author: alinast
ms.openlocfilehash: 6e83ca543937948ad8028969cceca0f8787972c9
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281227"
---
# <a name="quickstart-find-available-rooms-using-azure-digital-twins"></a>Hızlı başlangıç: Azure Digital Twins'i kullanarak uygun odaları bulma

Azure Digital Twins hizmeti, fiziksel ortamınızın dijital görüntüsünü oluşturmanızı sağlar. Bu işlemin ardından ortamınızdaki olaylarla ilgili bildirimler alabilir ve verdiğiniz yanıtları özelleştirebilirsiniz. 

Bu hızlı başlangıçta [bir çift .NET örneği](https://github.com/Azure-Samples/digital-twins-samples-csharp) kullanılarak hayali bir ofis binası dijital ortama aktarılmakta ve bu odadaki uygun odaları nasıl bulacağınız gösterilmektedir. Digital Twins sayesinde birden fazla sensörü ortamınızla ilişkilendirebilirsiniz. Odanın uygun olup olmadığının yanı sıra karbondioksit sensörüyle odadaki hava kalitesinin uygun olup olmadığını da anlayabilirsiniz. Örnek uygulamalardan biri ayrıca bu senaryoyu görselleştirmenize yardımcı olmak için rastgele sensör verileri oluşturacaktır.

Aşağıdaki videoda hızlı başlangıç ayarları özetlenir:

> [!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Ön koşullar

1. Azure hesabınız yoksa, başlamadan önce [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Bu hızlı başlangıçta çalıştırdığınız iki konsol uygulaması, C# kullanılarak yazılmıştır. Geliştirme makinenize [.NET Core SDK sürüm 2.1.403 veya üzerini](https://www.microsoft.com/net/download) yüklemeniz gerekir. .NET Core SDK yüklüyse bir komut isteminde `dotnet --version` komutunu çalıştırarak geliştirme makinenizde bulunan C# sürümünü doğrulayabilirsiniz.

1. [Örnek C# projesini](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) indirin ve digital-twins-samples-csharp-master.zip arşivini ayıklayın. 


## <a name="create-a-digital-twins-instance"></a>Digital Twins örneği oluşturma

Bu bölümdeki adımları izleyerek [portalda](https://portal.azure.com) yeni bir Digital Twins örneği oluşturun.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Uygulamanızın izinlerini ayarlama

Bu bölüm örnek uygulamanızı Azure Active Directory (AAD) hizmetine kaydederek Digital Twins örneğinize erişebilmesini sağlar. Uygulamanızın AAD kaydı varsa örnekte onu kullanabilirsiniz. Bunun için bu bölümde anlatılan şekilde yapılandırıldığından emin olmanız gerekir. 

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="build-application"></a>Uygulama oluşturma

Doluluk uygulamasını oluşturmak için aşağıdaki adımları izleyin:

1. Bir komut istemi açın ve digital-twins-samples-csharp-master.zip dosyalarını ayıkladığınız klasöre gidin.
1. `cd occupancy-quickstart/src` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. *appSettings.json* dosyasını düzenleyerek aşağıdaki değişkenleri güncelleştirin:
    - *ClientId*: Önceki bölümde not aldığınız AAD uygulama kaydınızın *Uygulama Kimliği* değerini girin.
    - *Tenant*: Yine önceki bölümde not aldığınız AAD kiracınızın *Kiracı Kimliği* değerini girin.
    - *BaseUrl*: Digital Twins örneğinizin *Yönetim API'si* URL'sidir ve şu biçimde olacaktır: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Bu URL'deki yer tutucuları önceki bölümde not aldığınız örneğinize ait değerlerle değiştirin.

## <a name="provision-graph"></a>Grafı sağlama

Bu adım çok sayıda alan, bir cihaz, iki sensör, özel bir işlev ve bir rol ataması içeren Digital Twins uzamsal grafınızı sağlar. Uzamsal graf [*provisionSample.yaml*](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) dosyası kullanılarak sağlanır.

1. `dotnet run ProvisionSample` öğesini çalıştırın.
    >[!NOTE]
    >Kullanıcının kimliğini Azure AD ile doğrulamak için Cihaz Oturumu Azure CLI aracını kullanacağız. Kullanıcının [Microsoft oturum açma](https://microsoft.com/devicelogin) sayfasını kullanarak kimlik doğrulamasından geçmek için verilen kodu girmesi gerekir. Kodu girdikten sonra kimlik doğrulaması için belirtilen adımları izleyin. Araç çalıştığında kullanıcıdan her seferinde kimliğini doğrulaması istenir.
    
    >[!TIP]
    > Bu adımı çalıştırırken aşağıdaki hatayla karşılaşıyorsanız lütfen değişkenlerinizin düzgün şekilde kopyalandığından emin olun. 
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`


1. Sağlama adımının tamamlanması birkaç dakika sürebilir. Bu adım aynı zamanda Digital Twins örneğinizin içinde bir IoT Hub sağlayacak ve IoTHub, Status=`Running` değerini verene kadar döngüyü devam ettirecektir.

    ![Örneği Sağlama][4]

1. Yürütme işleminin sonunda cihazın `ConnectionString` değerini cihaz simülatörü örneğinde kullanmak üzere kopyalayın. Yalnızca aşağıdaki görüntüde vurgulanan görüntüyü kopyalayın:

    ![Örneği Sağlama][1]

## <a name="send-sensor-data"></a>Sensör verilerini gönderme

Aşağıdaki adımları kullanarak sensör simülatörü uygulamasını oluşturabilir ve çalıştırabilirsiniz:

1. Yeni bir komut istemi açın ve digital-twins-samples-csharp-master klasöründe bulunan projeye gidin.
1. `cd device-connectivity` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. *appsettings.json* dosyasını düzenleyerek *DeviceConnectionString* değerini yukarıdaki `ConnectionString` değeriyle güncelleştirin.
1. Sensör verilerini göndermeye başlamak için `dotnet run` komutunu çalıştırın. Aşağıdaki görüntüde olduğu gibi Digital Twins hizmetine gönderilmeye başladığını görmeniz gerekir:

     ![Cihaz Bağlantısı][2]

1. Bir sonraki adımda sonuçları karşılaştırmalı görüntülemek için simülatörü çalışır durumda bırakın. Bu pencerede Digital Twins'e gönderilen sanal sensör verileri gösterilir. Bir sonraki adımda ise temiz hava bulunan odaları bulmak üzere gerçek zamanlı sorgu gönderilecektir.

    >[!TIP]
    > Bu adımı çalıştırırken aşağıdaki hatayla karşılaşıyorsanız lütfen `DeviceConnectionString` değerinin düzgün şekilde kopyalandığından emin olun.  
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Temiz havaya sahip olan odaları bulma

Sensör örneğinde hareket ve karbondioksit olmak üzere iki sensör için rastgele veri değerlerinin simülasyonu yapılmaktadır. Temiz hava bulunan uygun alanlar, örneğimizde boş ve karbondioksit düzeyi 1000 ppm değerinin altında olan odalar olarak tanımlanmıştır. Koşulun yerine getirilmemesi alanın uygun olmadığını veya hava kalitesini düşük olduğunu gösterir.

1. Yukarıdaki sağlama adımını çalıştırmak için kullandığınız komut istemini açın.
1. `dotnet run GetAvailableAndFreshSpaces` öğesini çalıştırın.
1. Bu komut istemini ve sensör verileri komut istemini aşağıda gösterilen şekilde yan yana görüntüleyin. 
1. Komut istemlerinden biri 5 saniyede bir Digital Twins örneğine sanal hareket ve karbondioksit verileri göndermektedir. Diğer komut ise grafı gerçek zamanlı olarak okuyarak rastgele sanal verilere göre kullanılabilir durumda ve temiz havaya sahip olan odaları bulmaktadır. En son gönderilen sensör verilerine göre neredeyse gerçek zamanlı olarak şu koşullardan birini görüntüleyecektir:
    - Temiz havaya sahip olan uygun odalar.
    - Dolu veya hava kalitesi düzeyi düşük olan odalar.

     ![Temiz havaya sahip olan odaları alma][3]

Bu hızlı başlangıçta gerçekleştirilen işlemleri ve çağrılan API'leri anlamak için digital-twins-samples-csharp içindeki kod çalışma alanı projesini [Visual Studio Code](https://code.visualstudio.com/Download) ile açın (aşağıdaki komuta bakın). Öğreticiler kodu ayrıntılı bir şekilde inceleyecek ve yapılandırma verilerini değiştirme ile çağrılan API'ler hakkında bilgi verecektir. Yönetim API'lerini daha iyi anlamak için Digital Twins Swagger sayfanıza `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net//management/swagger` gidin veya [Digital Twins Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger)'a göz atın. 

```
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiler, tesis yöneticilerinin çalışan üretkenliğini artırmak ve binayı daha verimli bir şekilde çalıştırmak için kullanabileceği bir uygulama oluşturma hakkında ayrıntılı bilgi sunmaktadır.

Öğreticilere devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın:

1. Örnek depoyu indirdiğinizde oluşturulan klasörü silin.
1. [Azure portalda](http://portal.azure.com) sol taraftaki menüden **Tüm kaynaklar**'a tıklayın ve Digital Twins kaynağınızı seçin. **Tüm kaynaklar** bölmesinin en üstündeki **Sil** seçeneğine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta iyi çalışma koşullarına sahip olan odaları bulmak için kullanabileceğiniz basit bir senaryoya genel bakış bilgilerini incelediniz. Bu senaryo hakkında ayrıntılı bilgi için şu öğreticiye geçin:

> [!div class="nextstepaction"]
> [Öğretici: Azure Digital Twins'i dağıtma ve uzamsal graf yapılandırma](tutorial-facilities-setup.md)

<!-- Images -->
[1]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png
[2]: media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png
[3]: media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png
[4]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png
