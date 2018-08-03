---
title: Azure'da özel konuşma hizmeti için Abonelik anahtarları alma | Microsoft Docs
description: Cognitive Services çağrılar özel konuşma hizmeti için Abonelik anahtarları alma konusunda bilgi edinin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: d3969114323f5675c5e14ab36990b124e84ead37
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427576"
---
# <a name="obtain-subscription-keys"></a>Abonelik anahtarları alma
Azure özel konuşma hizmeti kullanmaya başlamak için ilk kullanıcı hesabınız bir Azure aboneliğine bağlamanız gerekir. Ücretsiz ve Ücretli katmanlar için abonelik yok. Katmanları hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Bir abonelik anahtarı edinirler
1. Bir abonelik anahtarı iki yoldan biriyle Azure portalından alabilirsiniz:

    * Git [Azure portalında](https://ms.portal.azure.com), ve yeni bir Bilişsel hizmetler API'si için arama yaparak ekleyin _Bilişsel Hizmetler_ seçip **Bilişsel hizmetler API'leri**.

      ![Bilişsel hizmetler arama](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * Veya doğrudan [Bilişsel hizmetler API'leri](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![Bilişsel Hizmetler API’leri](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
1. Aşağıdaki gerekli alanları doldurun:

      a. **Hesap adı**. Sizin için uygun bir ad kullanın. Bu ad kaynaklar listesine Bilişsel Hizmetleri aboneliğinize bulabilmesi unutmayın.

      b. **Abonelik**. Bir Azure aboneliklerinizi seçin.

      c. **API türü**. Seçin **özel konuşma hizmeti (Önizleme)**.

      d. **Konum**. Şu anda **Batı ABD**.

      e. **Fiyatlandırma katmanı**. Sizin için uygun katmanını seçin. **F0** ücretsiz katmanı gösterir. İzin verilen kotaları üzerinde açıklanmıştır [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

      ![Bilişsel Hizmetler hesabı oluşturma](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

1. Panonuzda bir görünüm veya sağlanan hesap adına sahip bir hizmet kaynakları listenizde bulmanız gerekir. Bu seçeneği belirlediğinizde, hizmetinizin genel bakış görebilirsiniz. Soldaki listedeki altında **kaynak yönetimi**seçin **anahtarları**. Kopyalama **anahtar 1**.

      Sonraki adımlarda Bu abonelik anahtarı gereklidir.

      ![ANAHTAR 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > Kopyalamayın **abonelik kimliği** genel bakış sayfasında. Sonraki adımda abonelik anahtarı gerekir.
      >

      ![Genel abonelik kimliği](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

1. Sağ üst Şeritteki abonelik anahtarınızı girmek için kullanıcı hesabınızı seçin. Aşağı açılan menüden **abonelikleri**.

      ![Abonelik menü öğesi](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    İlk kez açılmasından boş olduğu aboneliklerinin bir tablo görüntülenir.

    ![Abonelikleri tablo](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

1. Seçin **yeni Ekle**. Abonelik ve abonelik anahtarı için bir ad girin. Ya da olabilir **anahtar 1** (birincil anahtar) veya **anahtar 2** (ikincil anahtarı) aboneliğinizde.

      ![Abonelik anahtarı adı](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Verileri yüklemek için bir model eğitip veya bir dağıtım yapın, özel konuşma hizmeti etkinliklerinizi bir Azure aboneliğine bağlamanız gerekir. Ücretsiz katman veya bir Ücretli katman abonelik olabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-id"></a>Bir abonelik Kimliğini alın
Abonelik Kimliği almak için Azure portalına gidin. Arama *Bilişsel Hizmetler* ve *özel konuşma hizmeti*ve yönergeleri izleyin.

> [!NOTE]
> Bu işlemde daha sonra abonelik anahtarı gereklidir.
>

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturmaya başlamak, [özel akustik model](cognitive-services-custom-speech-create-acoustic-model.md).
* Oluşturmaya başlamak, [özel dil modeli](cognitive-services-custom-speech-create-language-model.md).
