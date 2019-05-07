---
title: Metin okuma çıkış - konuşma Hizmetleri ince ayar yapma
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'da günlüğü etkinleştirme.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 6d602491c66669007ae220c3b8143ce3e805246f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148001"
---
# <a name="fine-tune-text-to-speech-output"></a>Metin okuma çıkış ince ayar yapma

Azure konuşma Hizmetleri hızı, Söyleniş, birim, aralık ve kullanarak çıktıyı bir metin okuma dağılımını ayarlamanızı izin [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md). SSML'yi hizmet hangi özelliğinin ayarlanmasını gerektirir hakkında bilgilendirmek için etiketleri kullanan bir XML-tabanlı işaretleme dilidir. SSML'yi ileti, metin okuma hizmetine her isteğin gövdesindeki sonra gönderilir. Teklif artık konuşma Hizmetleri özelleştirme işlemini kolaylaştırmak için bir [ses ayarlama](https://aka.ms/voicetuning) gerçek zamanlı olarak görsel olarak inceleyin ve ayarlamanıza olanak tanıyan metin okuma aracı çıkarır.

Microsoft'un Ses Ayarlama Aracı'nı destekleyen [standart](language-support.md#standard-voices), [sinir](language-support.md#text-to-speech), ve [özel seslerle](how-to-customize-voice-font.md).

## <a name="get-started-with-the-voice-tuning-tool"></a>Sesli ayarlama aracı ile çalışmaya başlama

Metin okuma çıkış ses ayarlama aracıyla ince ayar başlamadan önce bu adımları tamamlamak gerekir:

1. Oluşturma bir [ücretsiz Microsoft hesabı](https://account.microsoft.com/account) zaten yoksa.
2. Oluşturma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/en-us/free/) zaten yoksa. Tıklayın **ücretsiz Başlat**ve Microsoft hesabınızı kullanarak yeni bir Azure hesabı oluşturun.

3. Bir konuşma Hizmetleri aboneliği, Azure portalında oluşturun. Adım adım yönergeler için [konuşma kaynak oluşturma işlemini](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure) kullanılabilir.
   >[!NOTE]
   >Azure portalında bir konuşma kaynağı oluşturduğunuzda, Azure konum bilgileri TTS ses bölgeyle ile eşleşmesi gerekir. Sinir TTS ses Azure konumları bir alt kümesini destekler. Destek tam bir listesi için bkz. [bölgeleri](regions.md#text-to-speech).

   >[!NOTE]
   >Bir F0 veya hizmetini kullanabilmeniz için önce Azure portalında oluşturulan S0 anahtarı olması gerekir. Sesli ayarlama **değil** Destek [30 günlük ücretsiz deneme sürümü anahtarı](https://review.docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started?branch=release-build-cogserv-speech-services#free-trial).

4. Oturum [ses ayarlama](https://aka.ms/voicetuning) portal ve konuşma Hizmetleri aboneliğinize bağlanın. Tek bir konuşma Hizmetleri aboneliği seçin ve ardından bir proje oluşturun.
5. Seçin **yeni ayarlama**. Ardından aşağıdaki adımları izleyin:

   * Bulun ve seçin **tüm abonelikleri**.  
   * Seçin **mevcut aboneliğe bağlanma**.  
     ![Mevcut bir aboneliğe bağlanma](./media/custom-voice/custom-voice-connect-subscription.png).
   * Konuşma Hizmetleri Azure abonelik anahtarınızı girin ve ardından **Ekle**. Abonelik anahtarlarınızın konuşma özelleştirme Portalı'nda kullanılabilir [abonelik sayfasını](https://go.microsoft.com/fwlink/?linkid=2090458). Kaynak Yönetimi bölmesinde anahtarlar alabilir [Azure portalında](https://portal.azure.com/). 
   * Kullanmayı planladığınız konuşma Hizmetleri birden fazla aboneliğiniz varsa, her abonelik için bu adımları yineleyin.

## <a name="customize-the-text-to-speech-output"></a>Metin okuma çıkış özelleştirme

Hesaplarını oluşturdunuz ve aboneliğinize bağlı göre metin okuma çıkış ayarlama başlayabilirsiniz.

1. Sesli seçin.
2. Düzenlemek istediğiniz metni girin.
3. Düzenlemeleri başlamadan önce bir genel görünüm için çıktıyı almak için ses yürütün.
4. İyileştirmek istediğiniz word/cümle seçin ve farklı SSML'yi tabanlı işlevleri ile denemeler başlatın.

>[!TIP]
> SSML'yi ayarlama ve ses çıkış ayarlama hakkında ayrıntılı bilgi için bkz. [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md).

## <a name="limitations"></a>Sınırlamalar

Ses sinir ayarlama, standart ve özel ses için ayarlama değerinden biraz farklıdır.

* Tonlama sinir ses için desteklenmiyor.
* Aralık ve birim özellikler, yalnızca tam cümle ile çalışır. Bu özellikler word düzeyinde kullanılabilir değildir.
* Hızı için bazı sinir sesleri gerektirirken başkalarının, tam cümlelerden seçmek, sözcüklere göre ayarlanabilecek.

> [!TIP]
> Sesli ayarlama aracın özellikler ve ayarlama hakkında bağlamsal bilgiler sağlar.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure'da bir konuşma kaynak oluşturma](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure)
* [Sesli ayarlama Başlat](https://speech.microsoft.com/app.html#/VoiceTuning)
* [Konuşma sentezi biçimlendirme dili (SSML'yi)](speech-synthesis-markup.md)
