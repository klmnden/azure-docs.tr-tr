---
title: Azure Media Services ile video dosyalarını akışa alma - .NET | Microsoft Docs
description: Yeni bir Azure Media Services hesabı oluşturmak, bir dosyayı kodlamak ve Azure Media Player’da akışa almak için bu hızlı başlangıcın adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/08/2018
ms.author: juliako
ms.openlocfilehash: 48f85311f38d7e4ab1414dfc22c111b92163740e
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42023636"
---
# <a name="quickstart-stream-video-files---net"></a>Hızlı Başlangıç: Video dosyalarını akışa alma - .NET

> [!NOTE]
> En son Azure Media Services sürümü Önizleme aşamasındadır ve v3 olarak adlandırılabilir. v3 API’lerini kullanmaya başlamak için, bu hızlı başlangıçta açıklandığı gibi yeni bir Media Services hesabı oluşturmanız gerekir. 

Bu hızlı başlangıçta, Azure Media Services kullanarak çok çeşitli tarayıcı ve cihazda videoları akışa almaya başlamanın ne kadar kolay olduğu size gösterilmektedir. Bu konu başlığındaki örnek, bir HTTPS URL’si aracılığıyla erişilebilir hale getirdiğiniz içerikleri kodlar. 

Hızlı başlangıcın sonunda bir videoyu akışa alabileceksiniz.  

![Videoyu yürütme](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Visual Studio yüklü değilse, [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)’yi edinebilirsiniz.

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone http://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts.git
 ```

Örnek [EncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/tree/master/AMSV3Quickstarts/EncodeAndStreamFiles) klasöründe bulunur.

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Bir Dönüşüm oluşturur (ilk olarak, belirtilen Dönüşümün var olup olmadığını denetler). 
2. Kodlama İşinin çıkışı olarak kullanılan bir çıkış Varlığı oluşturur.
3. İşin bir HTTPS URL'sini temel alan girişini oluşturur.
4. Daha önce oluşturulan giriş ve çıkışı kullanarak kodlama İşini gönderir.
5. İşin durumunu denetler.
6. StreamingLocator oluşturur.
7. Akış URL'leri oluşturur.

Örnekteki her bir işlevin ne yaptığına dair açıklamalar için kodu inceleyin ve [bu kaynak dosyadaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) açıklamalara bakın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](http://portal.azure.com)’da oturum açın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

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
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıçta oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki hiçbir kaynağa artık ihtiyacınız yoksa kaynak grubunu silin. **CloudShell** aracını kullanabilirsiniz.

**CloudShell**’de aşağıdaki komutu yürütün:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="examine-the-code"></a>Kodu inceleme

Örnekteki her bir işlevin ne yaptığına dair açıklamalar için kodu inceleyin ve [bu kaynak dosyadaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) açıklamalara bakın.

[Dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md) öğreticisi size ayrıntılı açıklamaları içeren daha gelişmiş bir akışa alma örneği sunar. 

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Media Services v3 SDK’ları, iş parçacığı güvenli değildir. Çok iş parçacıklı uygulama ile çalışırken, iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md)
