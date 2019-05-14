---
title: Azure Media Services - Node.js ile video dosyaları Stream | Microsoft Docs
description: Yeni bir Azure Media Services hesabı oluşturma, bir dosya kodlama ve Azure Media Player ile akış için bu öğreticideki adımları izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/26/2019
ms.author: juliako
ms.openlocfilehash: 3e4172cd149726e28e0c7dff435ec1f7a59ee169
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550164"
---
# <a name="tutorial-stream-video-files---nodejs"></a>Öğretici: Video dosyalarını akışla aktarma - Node.js

Bu öğreticide, kodlamak ve çok çeşitli tarayıcılarda ve cihazlarla Azure Media Services kullanarak video akışını başlatmak için ne kadar kolay olduğunu gösterir. Azure Blob depolamada bulunan dosyaların yolları, SAS URL’leri veya HTTPS URL’leri kullanılarak girdi içeriği belirtilebilir.

Bu makalede örnek bir HTTPS URL'si aracılığıyla erişilebilir duruma içerik kodlar. AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın.

Öğreticinin sonunda bir video akışını yapmak mümkün olacaktır.  

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

![Çalıştır](./media/stream-files-nodejs-quickstart/run.png)

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

Artık herhangi bir kaynağa medya Hizmetleri ve Bu öğreticide, oluşturulan depolama hesapları dahil olmak üzere, kaynak grubundaki ihtiyacınız varsa, kaynak grubunu silin.

Aşağıdaki CLI komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="see-also"></a>Ayrıca bkz.

[İş hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Media Services kavramları](concepts-overview.md)
