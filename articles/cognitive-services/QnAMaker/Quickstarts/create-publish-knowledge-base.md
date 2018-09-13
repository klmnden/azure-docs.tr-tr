---
title: 'Hızlı Başlangıç: bir KB - soru-cevap Oluşturucu Oluşturma'
titleSuffix: Azure Cognitive Services
description: Kendi içeriğinden SSS veya ürün kılavuzlarını gibi bir soru-cevap Oluşturucu Bilgi Bankası (KB) oluşturabilirsiniz. Bu örnekte soru-cevap Oluşturucu KB BitLocker anahtar kurtarma ilgili soruları yanıtlamak için basit bir SSS Web oluşturulur.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: f7af86687a8a61fb7aed028d2868752faaa8045a
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715114"
---
# <a name="create-train-and-publish-your-knowledge-base"></a>Oluşturmak, eğitmek ve, Bilgi Bankası yayımlama

Kendi içeriğinden SSS veya ürün kılavuzlarını gibi bir soru-cevap Oluşturucu Bilgi Bankası (KB) oluşturabilirsiniz. Bu örnekte soru-cevap Oluşturucu KB BitLocker anahtar kurtarma ilgili soruları yanıtlamak için basit bir SSS Web oluşturulur.

## <a name="prerequisite"></a>Önkoşul

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası oluşturma

1. QnAMaker.ai için Azure kimlik bilgilerinizle oturum açın.

2. Soru-cevap Oluşturucu Web sitesinde seçin **Bilgi Bankası oluşturma**.

   ![Yeni KB oluşturma](../media/qna-maker-create-kb.png)

3. Üzerinde **Oluştur** sayfasındaki adım 1, select **soru-cevap hizmeti oluşturma**. Yönlendirilirsiniz [Azure portalında](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) bir soru-cevap Oluşturucu hizmeti aboneliğinizi ayarlamak için. Azure portalı zaman aşımına uğrarsa seçin **deneyin** sitesinde. Bağlandıktan sonra Azure panonuza görünür.

4. Azure'da yeni bir soru-cevap Oluşturucu hizmeti başarıyla oluşturduktan sonra qnamaker.ai/create için döndürür. 2. adımda aşağı açılan listelerden soru-cevap hizmetinizi seçin. Yeni bir soru-cevap hizmet oluşturduysanız, sayfayı yenileyin emin olun.

   ![Soru-cevap hizmeti KB seçin](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. 3. adımda, KB ad **My örnek soru-cevap KB**.

6. İçerik, KB olarak eklemek için üç türde veri kaynaklarını seçin. 4, altında adımda **, KB doldurmak**, ekleme [BitLocker kurtarma SSS](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) URL'de **URL** kutusu.

   ![Soru-cevap hizmeti KB seçin](../media/qnamaker-quickstart-kb/add-datasources.png)

7. 5. adımda seçin **, KB oluşturma**.

8. KB oluşturulurken, bir açılır pencere görüntülenir. Ayıklama işlemi HTML sayfasını okuyun ve sorular ve cevaplar tanımlamak için birkaç dakika sürer.

9. KB başarıyla oluşturulduktan sonra **Bilgi Bankası** sayfası açılır. Bu sayfadaki KB içeriğini düzenleyebilirsiniz.

10. Sağ üst köşede seçin **ekleme soru-cevap çifti** yeni bir satır eklemek için **editoryal** KB'lık bölümü. Altında **soru**, girin **Merhaba.** Altında **yanıt**, girin **Merhaba. Bitlocker Soru Sor.**

   ![Soru-cevap çifti Ekle](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. Sağ üst köşede seçin **kaydedin ve eğitme** yaptığınız düzenlemeleri kaydetmek ve soru-cevap Oluşturucu modeli eğitmek için. Kayıtlı sürece düzenlemeleri tutulan değildir.

   ![Kaydet ve eğitme](../media/qnamaker-quickstart-kb/add-qna-pair2.png)

12. Sağ üst köşede seçin **Test** yaptığınız değişiklikleri etkili geçen test etmek için. ENTER **Merhaba var.** kutusunda ve Enter'ı seçin. Oluşturduğunuz yanıt bir yanıt görmeniz gerekir.

13. Seçin **inceleyin** yanıt daha ayrıntılı incelemek için. Test penceresi, bunlar yayımlamadan önce değişikliklerinizi KB test etmek için kullanılır.

   ![Test paneli](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Seçin **Test** kapatmak için tekrar **Test** açılır.

15. Yanındaki menüde **Düzenle**seçin **Yayımla**. Onaylamak için ardından **Yayımla** sayfasında.

16. Soru-cevap Oluşturucu hizmetini şimdi başarıyla yayımlandı. Uç nokta uygulama ya da bot kodu kullanabilirsiniz.

   ![Yayımlama](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](../How-To/create-knowledge-base.md)
