---
title: Oluşturmak, eğitmek ve Bilgi Bankası - soru-cevap Oluşturucu yayımlama
titleSuffix: Azure Cognitive Services
description: SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu örnekte soru-cevap Oluşturucu Bilgi Bankası BitLocker anahtar kurtarma ilgili soruları yanıtlamak için basit bir SSS Web oluşturulur.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 05/10/2019
ms.author: diberry
ms.openlocfilehash: 609d71898d8db027f1cfee9e1b73039367ec94f4
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693365"
---
# <a name="create-train-and-publish-your-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankanızı oluşturma, eğitme ve yayımlama

SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu makale, BitLocker anahtar kurtarma ilgili soruları yanıtlamak için basit bir SSS başlatabilirler soru-cevap Oluşturucu Bilgi Bankası oluşturma örneği içerir.

## <a name="prerequisite"></a>Önkoşul

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankası oluşturma

1. Oturum [QnAMaker.ai](https://QnAMaker.ai) Azure kimlik bilgilerinizle portal.

1. Soru-cevap Oluşturucu portalında seçin **Bilgi Bankası oluşturma**.

   ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qna-maker-create-kb.png)

1. **Create** (Oluştur) sayfasındaki 1. adımda **Create a QnA service** (Soru-Cevap hizmeti oluştur) öğesini seçin. Aboneliğinizde bir Soru-Cevap Oluşturma hizmeti ayarlamak için [Azure portala](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) yönlendirilirsiniz. Azure portal zaman aşımına uğrarsa sitede **Try again** (Yeniden deneyin) öğesini seçin. Bağlantı kurulduktan sonra Azure panonuz görünür.

1. Azure'da yeni bir Soru-Cevap Oluşturma hizmetini başarıyla oluşturduktan sonra qnamaker.ai/create sayfasına geri dönün. 2. adımda aşağı açılan listelerden soru-cevap Oluşturucu hizmetinizi seçin. Yeni bir soru-cevap Oluşturucu hizmeti oluşturduysanız, sayfayı yenileyin emin olun.

   ![Soru-cevap Oluşturucu hizmeti Bankası seçme işleminin ekran görüntüsü](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. 3. adımda bilgi bankanızı ad **My örnek soru-cevap KB**.

1. Bilgi Bankası'na içerik eklemek için üç türde veri kaynaklarını seçin. 4. adımdaki **Populate your KB** (KB'nizi doldurun) bölümünde **URL** kutusuna [BitLocker Kurtarma Hakkında SSS](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) sayfasının URL'sini girin.

   ![Veri kaynaklarını ekleme işleminin ekran görüntüsü](../media/qnamaker-quickstart-kb/add-datasources.png)

1. 5. adımda **Create your KB** (KB'nizi oluşturun) öğesini seçin.

1. Soru-cevap Oluşturucu Bilgi Bankası oluştururken bir açılır pencere görüntülenir. Ayıklama işleminin HTML sayfasını okuyup soruları ve yanıtları tanımlaması birkaç dakika sürer.

1. Soru-cevap Oluşturucu, Bilgi Bankası başarıyla oluşturduktan sonra **Bilgi Bankası** sayfası açılır. Bu sayfadaki Bilgi Bankası içerikleri düzenleyebilirsiniz.

## <a name="edit-the-knowledge-base"></a>Bilgi Bankası Düzenle

1. Soru-cevap Oluşturucu Portalı'nda üzerinde **Düzenle** bölümünden **ekleme soru-cevap çifti** Bilgi Bankası'na yeni bir satır eklemek için. **Question** (Soru) bölümüne **Hi** (Merhaba) yazın. **Answer** (Cevap) bölümüne **Hello. BitLocker Soru Sor.**

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-quickstart-kb/add-qna-pair.png)

1. Sağ üst köşeden **Save and train** (Kaydet ve eğit) öğesine tıklayarak yaptığınız düzenlemeleri kaydedin ve Soru-Cevap Oluşturma modelinizi eğitin. Kaydedilmeyen düzenlemeler silinir.

## <a name="test-the-knowledge-base"></a>Bilgi Bankası test

1. Sağ üst köşedeki soru-cevap Oluşturucu Portalı'nda seçin **Test** yaptığınız değişiklikleri etkili geçen test etmek için. ENTER `hi there` kutusunda ve Enter'ı seçin. Yanıt olarak oluşturduğunuz cevabı almanız gerekir.

1. Yanıtı daha ayrıntılı bir şekilde incelemek için **Inspect** (Denetle) öğesini seçin. Test penceresi, Bilgi Bankası değişikliklerinizi bunlar yayımlamadan önce test etmek için kullanılır.

    ![Test bölmesinin ekran görüntüsü](../media/qnamaker-quickstart-kb/inspect.png)

1. **Test**'i tekrar seçerek **Test** penceresini kapatın.

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama

Bilgi Bankası yayımladığınızda, bilgi bankanızı soru ve yanıt içeriğini taşır test dizini Azure Search'te bir üretim dizini.

![Bilgi bankanızı içeriğini taşıma işleminin ekran görüntüsü](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. Soru-cevap Oluşturucu portalında menüsünün yanındaki **Düzenle**seçin **Yayımla**. Ardından onaylamak için sayfadaki **Publish** (Yayımla) öğesini seçin.

1. Soru-Cevap Oluşturma hizmeti başarıyla yayımlandı. Uç noktayı uygulamanızda veya bot kodunuzda kullanabilirsiniz.

    ![Ekran görüntüsü başarılı yayımlama](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="create-a-bot"></a>Bir bot oluşturun

Yayımladıktan sonra gelen bir bot oluşturma **Yayımla** sayfası: 

* Aynı Bilgi Bankası farklı bölgelerdeki tüm işaret eden veya fiyatlandırma planı için bireysel botlar birkaç botlar hızlı bir şekilde oluşturabilirsiniz. 
* Bilgi Bankası'nda tek bir bot istiyorsanız kullanın **tüm robotlarınızın Azure portalında görüntüleme** geçerli robotlarınızın listesini görüntülemek için bağlantı. 

Bilgi Bankası ve sitemde yeniden yayınlayacağım değişiklikler yaptığınızda, başka bir bot ile işlem yapmanıza gerek yoktur. Bilgi Bankası ile çalışmak için zaten yapılandırıldı ve Bilgi Bankası için yapacağınız tüm değişiklikleri birlikte çalışır. Bilgi Bankası yayımlama her zaman bağlı tüm botlar otomatik olarak güncelleştirilir.

1. Soru-cevap Oluşturucu Portalı'nda üzerinde **Yayımla** sayfasında **bot oluşturma**. Bu düğme, yalnızca bir Bilgi Bankası yayımladıktan sonra görünür.

    ![Bir bot oluşturma işleminin ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Azure portalıyla Azure Bot hizmetin oluşturma sayfası için yeni bir tarayıcı sekmesi açılır. Azure bot hizmeti yapılandırın. Bu yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure Bot hizmeti v4 ile soru-cevap Robotu oluşturun](../tutorials/create-qna-bot.md).
    
    * Azure portalında aşağıdaki ayarlar, robot oluşturulurken değiştirmeyin. Bunlar, var olan bir Bilgi Bankası için önceden doldurulmuştur: 
        * Soru-cevap kimlik doğrulama anahtarı
        * App service planı ve konumu
        * Azure Storage
    * Bot ve soru-cevap Oluşturucu web app service planı paylaşabilir, ancak web uygulaması paylaşamazsınız. Başka bir deyişle **uygulama adı** soru-cevap Oluşturucu hizmetini oluştururken kullandığınız uygulama adından farklı olmalıdır. 


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](../How-To/create-knowledge-base.md)
