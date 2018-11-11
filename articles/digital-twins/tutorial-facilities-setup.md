---
title: Azure Digital Twins'i dağıtma | Microsoft Docs
description: Azure Digital Twins örneğinizi dağıtmayı ve uzamsal kaynaklarınızı yapılandırmayı öğrenmek için bu makaledeki adımları uygulayın.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 7e51513e5cc17b18b6822925051b207f61e20ea1
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283861"
---
# <a name="tutorial-deploy-azure-digital-twins-and-configure-a-spatial-graph"></a>Öğretici: Azure Digital Twins'i dağıtma ve uzamsal graf yapılandırma

Azure Digital Twins hizmeti insanları, yerleri ve cihazları tutarlı bir uzamsal sistemde bir araya getirmenizi sağlar. Bu öğretici serisinde Azure Digital Twins'i kullanarak oda doluluk durumunu, uygun sıcaklık ve hava kalitesi koşullarını tespit etme adımları gösterilmektedir. Bu öğreticiler, her birinde birden fazla oda bulunan çok sayıda kattan oluşan bir ofis binası senaryosu oluşturmak için .NET konsol uygulaması tasarlama konusunda size kılavuzluk edecek. Odalarda hareket, ortam sıcaklığı ve hava kalitesi tespiti yapan sensörlere sahip olan cihazlar bulunmaktadır. Digital Twins hizmetini kullanarak binadaki fiziksel alanları ve varlıkları dijital nesneler olarak çoğaltmayı öğreneceksiniz. Başka bir konsol uygulaması kullanarak cihaz olaylarının simülasyonunu yapacaksınız. Ardından bu fiziksel alanlardan ve varlıklardan gelen olayları neredeyse gerçek zamanlı olarak izlemeyi öğreneceksiniz. Ofis yöneticileri bu tür bilgileri kullanarak bu binada görevli bir çalışanın en uygun koşullara sahip toplantı odalarını ayırması konusunda yardımcı olabilir. Ofis tesis yöneticileri oluşturduğunuz sistemi kullanarak odaların kullanım eğilimini belirleyebilir ve bakım amacıyla çalışma koşullarını izleyebilir.

Bu serinin ilk öğreticisinde aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Digital Twins'i dağıtma
> * Uygulamanıza izin verme
> * Digital Twins örnek uygulamasını değiştirme
> * Binanızda sağlama yapma


Bu öğreticilerde [uygun odaları bulma hızlı başlangıcındaki](quickstart-view-occupancy-dotnet.md) örnekler kullanılmakta, kavramlar daha ayrıntılı bir şekilde ele alınmaktadır.


## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Bu öğreticilerde kullanılan Digital Twins örnekleri C# ile yazılmıştır. Örneği derlemek ve çalıştırmak için geliştirme makinenize [.NET Core SDK 2.1.403 veya üzeri sürümünü](https://www.microsoft.com/net/download) yüklediğinizden emin olun. Komut penceresinde `dotnet --version` komutunu çalıştırarak makinenizde doğru sürümün yüklü olup olmadığını kontrol edin.

- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

<a id="deploy" />

## <a name="deploy-digital-twins"></a>Digital Twins'i dağıtma

Digital Twins hizmetinin yeni bir örneğini oluşturmak için bu bölümdeki adımları gerçekleştirin. Abonelik başına yalnızca bir örnek oluşturulabilir. Çalışan bir örneğiniz varsa bir sonraki bölüme geçin. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]


<a id="permissions" />

## <a name="grant-permissions-to-your-app"></a>Uygulamanıza izin verme

Digital Twins, hizmetin [okuma/yazma erişimini](../active-directory/develop/v1-permissions-and-consent.md) denetlemek için [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) hizmetinden faydalanır. Digital Twins örneğinize bağlanacak tüm uygulamaların Azure Active Directory'ye kayıtlı olması gerekir. Bu bölümde örnek uygulamanızı kaydetme adımları gösterilmektedir.

*Uygulama kaydınız* varsa örnek uygulama için bu kaydı kullanabilirsiniz. Ancak bu bölümdeki adımları izleyerek uygulama kaydınızın doğru yapılandırmaya sahip olduğundan emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="configure-digital-twins-sample"></a>Digital Twins örneği yapılandırma

Bu bölümde [Digital Twins REST API'leri](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index) ile iletişim kuran bir Digital Twins uygulaması oluşturma adımlarına yer verilmiştir. 

### <a name="download-the-sample"></a>Örneği indirme
[Uygun odaları bulma hızlı başlangıcı](quickstart-view-occupancy-dotnet.md) için bu örnekleri daha önce indirdiyseniz bu adımları atlayabilirsiniz.

1. [Digital Twins .Net örneklerini](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) indirin. 
2. ZIP klasörünün içindeki dosyaları makinenize ayıklayın. 

### <a name="explore-the-sample"></a>Örneği keşfetme
Ayıklanan örnek klasöründeki **_digital-twins-samples-csharp\digital-twins-samples.code-workspace_** dosyasını Visual Studio Code ile açın. Bu dosyada iki proje bulunur: 

1. Sağlama örneği olan **_occupancy-quickstart_**, [uzamsal zeka grafı](concepts-objectmodel-spatialgraph.md#graph) yapılandırıp sağlamanızı mümkün kılar. Bu graf, fiziksel alanlarınızın ve içindeki kaynakların dijital görüntüsüdür. Akıllı bina nesnelerini tanımlayan bir [nesne modeli](concepts-objectmodel-spatialgraph.md#model) kullanılır. Digital Twins nesnelerinin ve REST API'lerinin tam listesi için [bu REST API belgesini](https://docs.westcentralus.azuresmartspaces.net/management/swagger) veya [örneğiniz](#deploy) için oluşturulmuş olan **Yönetim API'si** URL'sini ziyaret edin.

   Digital Twins örneğinizle nasıl iletişim kurduğunu görmek için örnek uygulamayı incelemeye **_src\actions_** klasöründen başlayabilirsiniz. Bu klasördeki dosyalar, bu öğreticilerde kullanacağınız komutları yerine getirir:
    - *provisionSample.cs* dosyası uzamsal grafı sağlama adımlarını gösterir
    - *getSpaces.cs* dosyası sağlanan alanlarla ilgili bilgileri alır
    - *getAvailableAndFreshSpaces.cs* dosyası user-defined function adlı özel işlevin sonuçlarını alır
    - *createEndpoints.cs* dosyası diğer hizmetlerle etkileşim kurmak için uç noktalar oluşturur

1. **_device-connectivity_** adlı simülasyon örneği, sensör verilerinin simülasyonunu yapar ve Digital Twin örneğiniz için sağlanmış olan IoT hub'ına gönderir. Bu örneği [uzamsal grafınızı sağladıktan sonra geçeceğiniz öğreticide](tutorial-facilities-udf.md#simulate) kullanacaksınız. Bu örneği yapılandırmak için kullanılan sensör ve cihaz tanımlayıcıları, grafınızı sağlamak için kullanacağınız tanımlayıcılarla aynı olmalıdır.

### <a name="configure-the-provisioning-sample"></a>Sağlama örneğini yapılandırma
1. Bir komut penceresi açın ve indirdiğiniz örneğe gidin. Şu komutu çalıştırın:

    ```cmd/sh
    cd occupancy-quickstart/src
    ```

1. Şu komutu çalıştırarak bağımlılıkları örnek projeye geri yükleyin:

    ```cmd/sh
    dotnet restore
    ```

1. Visual Studio Code'da **occupancy-quickstart** projesine ait olan **_appSettings.json_** dosyasını açın ve şu değerleri güncelleştirin:
    1. *ClientId*: [Uygulama izinlerini ayarlama](#permissions) bölümünde not aldığınız AAD uygulama kaydınızın **Uygulama Kimliği** değerini girin.
    1. *Tenant*: Yine [uygulama izinlerini ayarlama](#permissions) bölümünde not aldığınız [AAD kiracınızın](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) **Kiracı Kimliği** değerini girin.
    1. *BaseUrl*: Digital Twins örneğinizin URL'sini girin. Bu URL'yi almak için şu URL'deki yer tutucuları örneğinize ait değerlerle değiştirin: **https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/**. Bu URL'yi [dağıtım bölümündeki](#deploy) **Yönetim API'si** URL'sinde **swagger/** yerine **api/v1.0/** değerini yazarak da alabilirsiniz.

1. Aşağıdaki komutu çalıştırdığınızda bu örneği kullanarak keşfedebileceğiniz Digital Twins özelliklerinin tam listesine ulaşabilirsiniz.

    ```cmd/sh
    dotnet run
    ```

<a id="provision-spaces" />

## <a name="understand-provisioning-process"></a>Sağlama işlemini anlama
Bu bölümde örnekte binanın uzamsal grafının nasıl sağlandığı gösterilmektedir. 

Visual Studio Code'da **_occupancy-quickstart\src\actions_** klasörüne gidin ve *provisionSample.cs* dosyasını açın. Aşağıdaki işleve dikkat edin:

```csharp
public static async Task<IEnumerable<ProvisionResults.Space>> ProvisionSample(HttpClient httpClient, ILogger logger)
{
    IEnumerable<SpaceDescription> spaceCreateDescriptions;
    using (var r = new StreamReader("actions/provisionSample.yaml"))
    {
        spaceCreateDescriptions = await GetProvisionSampleTopology(r);
    }

    var results = await CreateSpaces(httpClient, logger, spaceCreateDescriptions, Guid.Empty);

    Console.WriteLine($"Completed Provisioning: {JsonConvert.SerializeObject(results, Formatting.Indented)}");

    return results;
}

```

Bu işlev aynı klasördeki *provisionSample.yaml* dosyasını kullanmaktadır. Bu dosyayı açtığınızda ofis binasının hiyerarşisini görebilirsiniz: *Venue* (Mekan), *Floor* (Kat), *Area* (Alan) ve *Rooms* (Odalar). Bu fiziksel alanların herhangi birinde *cihazlar* ve *sensörler* bulunabilir. Her girişin `type` değeri önceden tanımlanmıştır. Örneğin, *Floor* (Kat), *Room* (Oda). 

Örnek *yaml* dosyasında `Default` Digital Twins nesne modelini kullanan bir uzamsal graf gösterilmektedir. Bu modelde çoğu tür (örneğin SensorDataType için Temperature, SpaceBlobType için Map) ve alan türü için (örneğin FocusRoom, ConferenceRoom gibi alt türlere sahip Room türü) genel adlar kullanılmıştır ve bu kullanım bir bina için yeterlidir. Fabrika gibi farklı bir mekan için uzamsal graf oluşturmanız gerekmesi durumunda başka bir nesne modeline ihtiyaç duyabilirsiniz. Sağlama örneğinin komut satırında `dotnet run GetOntologies` komutunu çalıştırarak kullanabileceğiniz durumları görebilirsiniz. Uzamsal graflar ve nesne modelleri hakkında ayrıntılı bilgi için bkz. [Digital Twins nesne modelini ve uzamsal grafını anlama](concepts-objectmodel-spatialgraph.md). 

### <a name="modify-sample-spatial-graph"></a>Örnek uzamsal grafı değiştirme
*provisionSample.yaml* dosyasında aşağıdaki düğümler bulunur:

- **resources**: `resources` düğümü, mevcut düzeninizdeki cihazlarla iletişim kurmak için bir IoT Hub kaynağı oluşturur. Grafınızın kök düğümündeki IoT hub kaynağı, grafınızdaki tüm cihaz ve sensörlerle iletişim kurabilir.  

- **spaces**: Digital Twins nesne modelinde `spaces`, fiziksel konumları temsil eder. Her alan *Region* (Bölge), *Venue* (Mekan) veya *Customer* (Müşteri) gibi bir `Type` değerine ve kolay `Name` bilgisine sahiptir. Bir alan, başka bir alana ait olabilir ve bu şekilde hiyerarşik bir yapı oluşturulabilir. *provisionSample.yaml* dosyasında hayali bir binanın uzamsal grafı bulunmaktadır. `Floor` türündeki alanların `Venue` içinde, `Area` türündekilerin katta ve `Room` türündekilerin bir alanda olduğu mantıksal iç içe yerleştirme düzenine dikkat edin. 

- **devices**: Alanlarda, bir dizi sensörü yöneten fiziksel veya sanal varlıklar olan `devices` öğeleri bulunabilir. Örneğin cihaz bir kullanıcının telefonu, bir Raspberry Pi sensör pod'u, ağ geçidi gibi bir nesne olabilir. Örnekteki hayali binanın *Focus Room* adlı odasında bir *Raspberry Pi 3 A1* cihazı bulunmaktadır. Her cihaz düğümü, örneğe sabit kodlanmış benzersiz bir `hardwareId` değerine sahiptir. Bu örneği üretim amaçlı kullanım için yapılandırmak isterseniz bu değerleri kendi sisteminizdeki değerlerle değiştirmeniz gerekir.  

- **sensors**: Bir cihazda sıcaklık, hareket, pil düzeyi gibi fiziksel değişiklikleri algılayıp kaydedebilen birden fazla `sensors` bulunabilir. Her sensör düğümü, burada sabit kodlanmış `hardwareId` değeriyle benzersiz olarak tanımlanmıştır. Gerçek bir uygulamada bu benzersiz tanıtıcıları kendi sisteminizdeki sensörlerin değerleriyle değiştirmeniz gerekir. *provisionSample.yaml* dosyasında biri *Motion* (Hareket) biri de *CarbonDioxide* (Karbondioksit) sensörü olmak üzere iki sensör vardır. CarbonDioxide sensörünün tanımlandığı satırların altına aşağıdaki satırları ekleyerek *Temperature* (Sıcaklık) kaydı yapacak yeni bir sensör ekleyin. Bu değerler *provisionSample.yaml* dosyasında açıklama satırlarıyla sağlanmıştır. Satırların önündeki '#' karakterini silerek açıklama olmaktan çıkarabilirsiniz. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > `dataType` ve `hardwareId` anahtarlarının hizalamasının bu kod parçacığının üzerindeki deyimlerle aynı olduğundan emin olun. Ayrıca düzenleyicinizin boşlukları sekmelerle değiştirmediğinden de emin olun. 

*provisionSample.yaml* dosyasını kaydedin ve kapatın. Bir sonraki öğreticide bu dosyaya daha fazla bilgi ekleyecek ve ardından Azure Digital Twins örnek binanızda sağlama yapacaksınız.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kadar Azure Digital Twins incelemesi sizin için yeterliyse bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure portalda](http://portal.azure.com) sol taraftaki menüden **Tüm kaynaklar**'a tıklayın, Digital Twins kaynak grubunuzu ve ardından **Sil**'i seçin.
2. Gerekirse çalışma makinenizdeki örnek uygulamayı da silebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Örnek binanızdaki koşulları izlemek için özel mantık uygulama hakkında bilgi edinmek istiyorsanız serideki bir sonraki öğreticiye geçin. 
> [!div class="nextstepaction"]
> [Öğretici: Binanızı sağlama ve çalışma koşullarını izleme](tutorial-facilities-udf.md)

