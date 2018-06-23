---
title: Azure üzerinde özel konuşma hizmeti için Abonelik anahtarları alma | Microsoft Docs
description: Bilişsel Hizmetleri'nde özel konuşma hizmeti çağrıları için abonelik anahtarlarını alma hakkında bilgi.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: fcef86a19a77581ff82b64173e2ac68b26ae708a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351653"
---
# <a name="obtain-subscription-keys"></a>Abonelik anahtarları alma
Azure özel konuşma hizmeti kullanmaya başlamak için öncelikle bir Azure aboneliğine kullanıcı hesabınızın bağlamanız gerekir. Abonelikleri ücretsiz ve Ücretli katmanlar için kullanılabilir. Katmanları hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Bir abonelik anahtarı edinme
1. İki yoldan biriyle Azure portalından bir abonelik anahtarı alabilirsiniz:

    * Git [Azure portal](https://ms.portal.azure.com), ve yeni bir Bilişsel Hizmetleri API eklemek için arama yaparak _Bilişsel Hizmetler_ seçilerek **Bilişsel Hizmetleri API**.

      ![Bilişsel hizmetler arama](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * Veya doğrudan gitmek [Bilişsel hizmetler API'leri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![Bilişsel Hizmetler API’leri](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
2. Aşağıdaki gerekli alanları doldurun:

      a. **Hesap adı**. Uygun bir ad kullanın. Bilişsel hizmetler aboneliğiniz kaynaklar listesine bulabilmesi için bu ad unutmayın.

      b. **Abonelik**. Bir Azure aboneliğinizin seçin.

      c. **API türü**. Seçin **özel konuşma hizmet (Önizleme)**.

      d. **Konum**. Şu anda olduğu **Batı ABD**.

      e. **Fiyatlandırma katmanı**. Uygun katmanı seçin. **F0** ücretsiz katmanı. İzin verilen kotaları üzerinde açıklanacak [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

      ![Bilişsel Hizmetler hesabı oluşturma](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

3. Panonuzda bir görünüm veya sağlanan hesap adına sahip bir hizmet, kaynaklar listesine bulmanız gerekir. Seçtiğinizde, hizmetiniz özetini görebilirsiniz. Soldaki listedeki altında **kaynak yönetimi**seçin **anahtarları**. Kopya **anahtar 1**.

      Bu abonelik anahtarı sonraki adımlarda gereklidir.

      ![ANAHTAR 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > Kopyalama **abonelik kimliği** genel bakış sayfasında. Sonraki adımda abonelik anahtarı gerekir.
      >

      ![Genel Bakış abonelik kimliği](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

4. Sağ üst şeridinde abonelik anahtarınızı girmek için kullanıcı hesabını seçin. Aşağı açılan menüsünden seçin **abonelikleri**.

      ![Abonelikleri menü öğesi](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    İlk açılışına boş olduğu abonelikleri oluşan bir tablo görünür.

    ![Abonelik tablosu](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

5. Seçin **yeni Ekle**. Abonelik ve abonelik anahtarı için bir ad girin. Ya da olabilir **anahtar 1** (birincil anahtar) veya **anahtar 2** (ikincil anahtar) aboneliğiniz.

      ![Abonelik anahtar adı](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Verileri yüklemek için bir modeli eğitmek veya bir dağıtım yapmak için bir Azure aboneliğine özel konuşma hizmet etkinliklerinizi bağlamanız gerekir. Ücretsiz katmanı veya Ücretli katmanı abonelik olabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-id"></a>Bir abonelik kimliği alma
Bir abonelik kimliği almak için Azure portalına gidin. Arama *Bilişsel Hizmetler* ve *özel konuşma hizmet*ve yönergeleri izleyin.

> [!NOTE]
> Abonelik anahtarı bu işlemi daha sonra gereklidir.
>

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturmaya başlamadan, [özel akustik modeli](cognitive-services-custom-speech-create-acoustic-model.md).
* Oluşturmaya başlamadan, [özel dil modeli](cognitive-services-custom-speech-create-language-model.md).
