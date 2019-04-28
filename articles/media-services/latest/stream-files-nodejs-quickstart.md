---
title: Azure Media Services - Node.js ile video dosyaları Stream | Microsoft Docs
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
ms.date: 03/26/2019
ms.author: juliako
ms.openlocfilehash: 22b7f2380b509daa4cb9931d6fc57c1297628e3d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61233186"
---
# <a name="quickstart-stream-video-files---nodejs"></a>Hızlı Başlangıç: Video dosyalarını akışla aktarma - Node.js

Bu hızlı başlangıçta, Azure Media Services kullanarak çok çeşitli tarayıcı ve cihazda videoları kodlamanın akışa almaya başlamanın ne kadar kolay olduğu size gösterilmektedir. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak girdi içeriği belirtilebilir.
Bu konu başlığındaki örnek, bir HTTPS URL’si aracılığıyla erişilebilir hale getirdiğiniz içerikleri kodlar. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın.

Hızlı başlangıcın sonunda bir videoyu akışa alabileceksiniz.  

![Videoyu yürütme](./media/stream-files-nodejs-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/download/)’yi yükleme
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).<br/>Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun.
- Bağlantısındaki [erişim Azure Media Services API'sine Azure CLI ile](access-api-cli-how-to.md) ve kimlik bilgilerini kaydedin. Bunları API'ye erişmek için kullanmanız gerekecektir.

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Aşağıdaki komutu kullanarak makinenize akış Node.js örneği içeren bir GitHub deposunu kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
 ```

Örnek bulunan [StreamFilesSample](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/master/AMSv3Samples/StreamFilesSample) klasör.

Açık [index.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L25) içinde proje indirilir. Değiştirin `endpoint config` adresinden aldığınız kimlik değerleri [API'leri erişme](access-api-cli-how-to.md).

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Oluşturur bir **dönüştürme** (ilk olarak, belirtilen dönüşüm var olup olmadığını denetler). 
2. Bir çıkış oluşturur **varlık** kodlama olarak kullanılan **iş**çıktı.
3. Oluşturur **iş**üzerinde bir HTTPS URL'si tabanlı giriş.
4. Kodlama gönderen **iş** giriş ve çıkış daha önce oluşturulan kullanarak.
5. İşin durumunu denetler.
6. Oluşturur bir **akış Bulucusu**.
7. Akış URL'leri oluşturur.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

1. Uygulama kodlanmış dosyaları indirir. İstediğiniz yere gitmek ve değerini güncelleştirmek çıktı dosyaları için bir klasör oluşturun **outputFolder** değişkeninde [index.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L39) dosya.
1. Açık **komut istemi**örnek'ın dizinine gidin ve aşağıdaki komutları yürütün.

    ```
    npm install 
    node index.js
    ```

İşlem tamamlandıktan sonra çalışan, benzer bir çıktı görmeniz gerekir:

![Çalıştırın](./media/stream-files-nodejs-quickstart/run.png)

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

## <a name="see-also"></a>Ayrıca bkz.

[İş hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Media Services kavramları](concepts-overview.md)
