---
title: Azure Media Services ile video dosyalarını akışa alma - .NET | Microsoft Docs
description: Yeni bir Azure Media Services hesabı oluşturmak, bir dosyayı kodlamak ve Azure Media Player’da akışa almak için bu hızlı başlangıcın adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/20/2019
ms.author: juliako
ms.openlocfilehash: 3a50d78645630e499b11f012da122b12b026ae6b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466876"
---
# <a name="quickstart-stream-video-files---net"></a>Hızlı Başlangıç: Stream video dosyaları - .NET

Bu hızlı başlangıçta, Azure Media Services kullanarak çok çeşitli tarayıcı ve cihazda videoları kodlamanın akışa almaya başlamanın ne kadar kolay olduğu size gösterilmektedir. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak girdi içeriği belirtilebilir.
Bu konu başlığındaki örnek, bir HTTPS URL’si aracılığıyla erişilebilir hale getirdiğiniz içerikleri kodlar. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın.

Hızlı başlangıcın sonunda bir videoyu akışa alabileceksiniz.  

![Videoyu yürütme](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Visual Studio yüklü değilse, [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)’yi edinebilirsiniz.
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).<br/>Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun.
- Bağlantısındaki [erişim Azure Media Services API'sine Azure CLI ile](access-api-cli-how-to.md) ve kimlik bilgilerini kaydedin. Bunları API'ye erişmek için kullanmanız gerekecektir.

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts.git
 ```

Örnek [EncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/tree/master/AMSV3Quickstarts/EncodeAndStreamFiles) klasöründe bulunur.

Açık [appsettings.json](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/appsettings.json) içinde proje indirilir. Aldığınız kimlik değerleri Değiştir [API'leri erişme](access-api-cli-how-to.md).

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Oluşturur bir **dönüştürme** (ilk olarak, belirtilen dönüşüm var olup olmadığını denetler). 
2. Bir çıkış oluşturur **varlık** kodlama olarak kullanılan **iş**çıktı.
3. Oluşturur **iş**üzerinde bir HTTPS URL'si tabanlı giriş.
4. Kodlama gönderen **iş** giriş ve çıkış daha önce oluşturulan kullanarak.
5. İşin durumunu denetler.
6. Oluşturur bir **akış Bulucusu**.
7. Akış URL'leri oluşturur.

Örnekteki her bir işlevin ne yaptığına dair açıklamalar için kodu inceleyin ve [bu kaynak dosyadaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) açıklamalara bakın.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Uygulamayı çalıştırdığınızda, farklı protokolleri kullanarak videoyu kayıttan yürütmek için kullanılabilen URL’ler görüntülenir. 

1. *EncodeAndStreamFiles* uygulamasını çalıştırmak için Ctrl+F5 tuşlarına basın.
2. Apple’ın **HLS** protokolünü (*manifest(format=m3u8-aapl)* ile biter) seçin ve konsoldan akış URL’sini kopyalayın.

![Çıktı](./media/stream-files-tutorial-with-api/output.png)

Örneğin [kaynak kodunda](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs), URL’nin nasıl oluşturulduğunu görebilirsiniz. Bunu derlemek için, akış uç noktasının ana bilgisayar adını ve akış bulucu yolunu birleştirmeniz gerekir.  

## <a name="test-with-azure-media-player"></a>Azure Media Player ile test etme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir web tarayıcısı açın ve [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/) sayfasına gidin.
2. **URL:** kutusuna, uygulamayı çalıştırdığınızda aldığınız akış URL değerlerinden birini yapıştırın. 
 
     HLS, Dash, URL'yi yapıştırabilirsiniz ya da kesintisiz biçimi ve Azure Media Player Cihazınızda kayıttan yürütme için uygun bir akış protokolü için otomatik olarak geçiş yapar.
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıçta oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa kaynak grubunu silin.

Aşağıdaki CLI komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="examine-the-code"></a>Kodu inceleme

Örnekteki her bir işlevin ne yaptığına dair açıklamalar için kodu inceleyin ve [bu kaynak dosyadaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) açıklamalara bakın.

[Dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md) öğreticisi size ayrıntılı açıklamaları içeren daha gelişmiş bir akışa alma örneği sunar. 

### <a name="job-error-codes"></a>İş hata kodları

Bkz: [hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Media Services v3 SDK’ları, iş parçacığı güvenli değildir. Çok iş parçacıklı uygulama ile çalışırken, iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md)
