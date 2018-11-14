---
title: Microsoft Translator Hub çalışma ve projeleri geçişini? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Hub çalışma ve projeleri için özel Translator geçirin.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 378baad0735238dc0921e5e78e2a27b3ae907e19
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627865"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Hub çalışma ve projeleri için özel Translator geçirme

Geçirebileceğiniz, [Microsoft Translator Hub](https://hub.microsofttranslator.com/) çalışma alanı ve özel Translator projelerinin. Geçiş Hub'ından başlar.


Bu öğeler işlemi sırasında geçirilir:

1.  Projeleri tanımları.

2.  Eğitim tanımı üzerinde özel Translator yeni bir model tanımı oluşturmak için kullanılır.

3.  Eğitimleri içinde kullanılan paralel ve tek dilli Kinsoku'ya dosyaları tüm yeni belgeleri özel Translator olarak geçirilecektir.

4.  Otomatik Sistem testi ve veri ayarlama verilmeli ve yeni belgeleri özel Translator olarak oluşturulur.

Tüm dağıtılan eğitimleri özel Translator, herhangi bir maliyet olmadan modeli eğitmek. El ile dağıtma seçeneğiniz vardır.

>[!Note]
>Bir eğitim işleminin başarılı olması en düşük 10.000 ayıklanan cümleler özel Translator gerektirir. Ayıklanan cümleler daha az sayıda [önerilen minimum](sentence-alignment.md#suggested-minimum-number-of-extracted-and-aligned-sentences), özel Translator eğitim gerçekleştir olamaz.

Hangi dağıtılmaz, tüm başarılı eğitimleri için bunlar özel Translator taslağında olarak geçirilecektir.

## <a name="find-custom-translator-workspace-id"></a>Özel Translator çalışma alanı kimliği bulunamıyor

Geçirilecek [Microsoft Translator Hub](https://hub.microsofttranslator.com/) çalışma hedef özel Translator çalışma alanı kimliği gerekir. Burada tüm Hub çalışma alanları ve projeler için geçirilmesi özel Translator, hedef çalışma alanıdır.

Özel Translator Ayarları sayfasında, hedef çalışma alanı kimliği bulacaksınız: 

1.  Özel Translator Portalı'nda "Ayarını" sayfasına gidin.

2.  Çalışma alanı kimliği temel bilgileri bölümünde bulabilirsiniz.

    ![Hedef çalışma alanı kimliği bulma](media/how-to/how-to-find-destination-ws-id.png)

3. Geçiş işlemi sırasında başvurmak için çalışma alanı kimliği, hedef tutun.

## <a name="migrate-workspace"></a>Çalışma alanını geçirme

Özel translator'a tam Hub çalışma geçirdiğinizde, projelerinizi, belgeleri ve eğitimleri özel translator'a geçişi. Geçişten önce yalnızca dağıtılmış eğitimleri geçirmek istediğiniz veya başarılı eğitimleri, geçirmek istediğiniz seçmeniz gerekir.

Bir çalışma alanı geçirmek için:

1.  Microsoft Translator Hub'ına oturum açın.

2.  "Ayarlar" sayfasına gidin.

3.  "Ayarlar" sayfasında "Özel Translator geçiş çalışma alanında veri" tıklayın.

    ![Hub'ından geçirme](media/how-to/how-to-migrate-workspace-from-hub.png)

4.  Sonraki sayfada, bu iki seçeneklerinden birini seçin:

    a.  Yalnızca eğitimleri dağıtılan: Bu seçeneğin belirlenmesi geçirme yalnızca dağıtılan sistemler ve ilgili belgeler.

    b.  Tüm başarılı eğitimleri: Bu seçeneğin belirlenmesi, başarılı eğitimleri ve ilgili belgelerin geçirir.

    c.  İçinde özel Translator, hedef çalışma alanı kimliği girin.

    ![Hub'ından geçirme](media/how-to/how-to-migrate-from-hub-screen.png)

5.  İstek Gönder'e tıklayın.

## <a name="migrate-project"></a>Geçiş projesi

Microsoft Translator Hub projelerinizi seçmeli olarak geçirmek istiyorsanız, bu özelliği sunar.

Bir projeyi geçirmek için:

1.  Microsoft Translator Hub'ına oturum açın.

2.  "Projeler" sayfasına gidin.

3.  Uygun bir proje için "Geçiş" bağlantısına tıklayın.

    ![Hub'ından geçirme](media/how-to/how-to-migrate-from-hub.png)

4.  Sonraki sayfada, bu iki seçeneklerinden birini seçin:

    a.  Yalnızca eğitimleri dağıtılan: Bu seçeneğin belirlenmesi geçirme yalnızca dağıtılan sistemler ve ilgili belgeler. 

    b.  Tüm başarılı eğitimleri: Bu seçeneğin belirlenmesi, başarılı eğitimleri ve ilgili belgelerin geçirir.

    c.  İçinde özel Translator, hedef çalışma alanı kimliği girin.

    ![Hub'ından geçirme](media/how-to/how-to-migrate-from-hub-screen.png)

5.  "İstek Gönder" e tıklayın.

## <a name="migration-history"></a>Geçiş geçmişi

Çalışma alanı istediniz / Hub'ından geçiş projesi, geçiş geçmişini özel Translator Ayarları sayfasında bulabilirsiniz. 

Geçiş geçmişini görüntülemek için aşağıdaki adımları izleyin:

1.  Özel Translator Portalı'nda "Ayarını" sayfasına gidin.

2.  Geçiş geçmişini Ayarlar sayfasında geçiş geçmişini bölümüne tıklayın.

    ![Geçiş geçmişi](media/how-to/how-to-migration-history.png)

Geçiş geçmişi sayfası aşağıdaki istediğiniz her geçiş için Özet bilgiler görüntüler.

1.  Tarafından geçirilen: Ad ve e-posta kullanıcının bu geçiş isteği gönderildi

2.  Üzerinde geçişi: Tarih ve zaman damgası geçiş

3.  Projeler: İstenen proje v/sn sayısı geçiş başarıyla geçirildi projeleri sayısı.

4.  Eğitimleri: İstenen geçiş v/sn sayısı eğitimleri başarıyla geçirildi eğitimleri sayısı.

5.  Belgeler: Belge v/sn sayısı geçiş başarıyla geçirildi istenen belge sayısı.

    ![Geçiş geçmişi ayrıntıları](media/how-to/how-to-migration-history-details.png)

Geçiş raporu projeleri, eğitimleri ve belgeleri hakkında daha ayrıntılı istiyorsanız ayrıntıları csv dosyası olarak dışarı aktarma seçeneğiniz vardır.

>[!Note]
>Geçiş yalnızca NMT dilleri bulunduğu dil çiftleri için desteklenir. Lütfen listesini şu anda kontrol [desteklenen NMT diller](https://www.microsoft.com/translator/business/languages/). Burada NMT diller mevcut değil, bir dil çiftleri için özel Translator'a Hub'ından veri taşınır, ancak bu dil çiftlerine dayanarak eğitimleri yönetilemez.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir model eğitip](how-to-train-model.md).
- Dağıtılan özel çeviri modeliniz aracılığıyla kullanmaya başlama [Microsoft Translator Text API v3 sürümüne](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).