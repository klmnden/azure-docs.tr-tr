---
title: Konuşma tanıma hizmetini ücretsiz olarak deneyin
description: Konuşma hizmeti ücretsiz deneyebilir miyim nasıl keşfedin.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: a0ff6c09eb0a6fffe0ce82fe9ccb3bc43970ad68
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024627"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Konuşma hizmeti ile çalışmaya başlama, kolay ve hesaplı. 30 günlük ücretsiz deneme, olanak tanır. hangi hizmet yapmak ve uygulamanızın ihtiyaçları için doğru olup olmadığını karar verebilirsiniz keşfedin.

Daha fazla süreye ihtiyacınız varsa, Microsoft Azure hesabı için kaydolun-200 ABD Doları, ücretli bir konuşma hizmeti abonelik 30 güne kadar doğru uygulayabileceğiniz hizmet kredi ile birlikte gelir.

Son olarak, konuşma hizmeti uygulamaları geliştirmek için uygun olan ücretsiz, düşük hacimli katmanı sunar. Hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği tutabilirsiniz.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

30 günlük ücretsiz deneme için sınırlı bir süre için fiyatlandırma katmanı S0 standart erişmenizi sağlar. 

30 günlük ücretsiz deneme için kaydolmak için:

1. Git [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/).

1. Seçin **konuşma API'leri** sekmesi.

   ![Konuşma Hizmetleri sekmesi](media/index/try-speech-api-free-trial1.png)
   
1. Altında **konuşma Hizmetleri <sup>Önizleme</sup>** seçin **API anahtarı alma** düğmesi.

   ![API anahtarı](media/index/try-speech-api-free-trial2.png)

1. Koşulları kabul edin ve aşağı açılan menüden bölgeniz seçin.

   ![Koşulları kabul edin](media/index/try-speech-api-free-trial3.png)

1. Microsoft, Facebook, LinkedIn ve GitHub hesabınızı kullanarak oturum açın. Veya ücretsiz bir Microsoft hesabı için kaydolabilirsiniz:

    * Git [Microsoft hesap portalı](https://account.microsoft.com/account).
    * Seçin **Microsoft'ta oturum açma**.

    ![Oturum aç](media/index/try-speech-api-free-trial4.png)

    * Oturum açma istemi bittiğinde **oluşturmak**.

    ![Yeni hesap oluştur](media/index/try-speech-api-free-trial5.png)

    * E-posta adresi veya telefon numarasını girin, takip adımlar, bir parola atayın ve yeni Microsoft hesabınızı doğrulamak için yönergeleri izleyin.

Oturum açtıktan sonra ücretsiz deneme sürümünüzü başlatır. Görüntülenen Web sayfasının şu anda deneme abonelikleri olan tüm Azure Bilişsel hizmetler hizmetler listelenir. Abonelik anahtarları iki yanında listelenen **konuşma Hizmetleri**. İki anahtarı uygulamalarınızda kullanabilirsiniz.

> [!NOTE]
> Tüm ücretsiz deneme Abonelikleri, Batı ABD bölgesinde içindir. İstekte bulunduğunuzda bölgeniz için karşılık gelen uç noktası kullanmayı unutmayın.

## <a name="new-azure-account"></a>Yeni Azure hesabı

Yeni Azure hesapları 30 gün boyunca kullanılabilir 200 ABD Doları değerinde bir hizmet kredisi alırsınız. Bu kredi konuşma hizmeti daha iyi keşfedilebilmesi için veya uygulama geliştirmeye başlamak için kullanabilirsiniz.

Yeni bir Azure hesabı için kaydolmak için:

1. Git [Azure kayıt sayfasına](https://azure.microsoft.com/free/ai/). 

1. Seçin **ücretsiz olarak Başla**.

    ![Ücretsiz kullanmaya başlayın](media/index/try-speech-api-new-azure1.png)

1. Microsoft hesabınızla oturum açın. Yoksa:

    * Git [Microsoft hesap portalı](https://account.microsoft.com/account).
    * Seçin **Microsoft'ta oturum açma**.
    * Oturum açma istemi bittiğinde **oluşturun.**
    * E-posta adresi veya telefon numarasını girin, takip adımlar, bir parola atayın ve yeni Microsoft hesabınızı doğrulamak için yönergeleri izleyin.

1. Bir hesaba kaydolun istedi bilgilerin kalanını girin. Ülkenizi ve adı belirtin ve telefon numarası ve e-posta adresi sağlayın.

    ![Bilgileri girin](media/index/try-speech-api-new-azure2.png)

    Telefon ve kredi kartı numarası sağlayarak kimliğinizi doğrulayın. (Kredi kartınızdan faturalandırılmazsınız.) olduğunda, Azure kullanıcı anlaşmasını kabul edin. 

    ![Sözleşmesini kabul edin](media/index/try-speech-api-new-azure3.png)

Ücretsiz Azure hesabınızı oluşturulur. Konuşma hizmeti için bir abonelik başlatmak için sonraki bölümdeki adımları izleyin.

## <a name="create-a-speech-resource-in-azure"></a>Azure'da bir konuşma kaynak oluşturma

Azure hesabınızda bir konuşma hizmeti kaynak eklemek için:

1. Oturum [Azure portalında](https://ms.portal.azure.com/) Microsoft hesabınızı kullanarak.

1. Seçin **kaynak Oluştur** , portalın sol üst köşesindeki.

    ![Kaynak oluşturma](media/index/try-speech-api-create-speech1.png)

1. İçinde **yeni** penceresinde, arama **konuşma**.

1. Arama sonuçlarında seçin **konuşma tanıma (Önizleme)**.

    ![Konuşma tanıma (Önizleme) seçin](media/index/try-speech-api-create-speech2.png)

1. Altında **konuşma tanıma (Önizleme)** seçin **Oluştur** düğmesi.

    ![Oluştur düğmesine seçin](media/index/try-speech-api-create-speech3.png)

1. Altında **Oluştur**, girin:

    * Yeni kaynak için bir ad. Adı aynı hizmetin birden fazla aboneliğe arasında ayırt etmenize yardımcı olur.
    * Yeni kaynak ücretleri nasıl faturalandırılır belirlemek için ilişkili olduğu Azure aboneliğini seçin.
    * Kaynağı nerede kullanılacak bir bölge seçin. Şu anda konuşma hizmeti Doğu Asya, Kuzey Avrupa ve Batı ABD bölgelerinde kullanılabilir.
    * Fiyatlandırma katmanı, her iki F0 seçin (sınırlı, ücretsiz aboneliği) veya S0 (Standart abonelik). Seçin **fiyatlandırma ayrıntılarının tamamını görüntüle** her katman için fiyatlandırma ve kullanım Kotalar hakkında tam bilgi için.
    * Bu konuşma abonelik için yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu aboneliği atayın. Kaynak düzenlenir, çeşitli Azure aboneliklerinizi tutmak Yardım gruplandırır.
    * Aboneliğinize gelecekte kolay erişim için seçin **panoya Sabitle** onay kutusu.
    * Seçin **oluşturun.**

    ![Oluştur düğmesine seçin](media/index/try-speech-api-create-speech4.png)

    İşlem oluşturma ve dağıtma yeni konuşma kaynağınızı biraz sürebilir. Seçin **hızlı** yeni kaynağınız hakkındaki bilgileri görmek için.

    ![Hızlı Başlangıç paneli](media/index/try-speech-api-create-speech5.png)

1. Altında **hızlı**seçin **anahtarları** abonelik anahtarlarınızı görüntülemek için 1. adım altında bağlantı. Her aboneliğin, iki anahtarı vardır; uygulamanızda iki anahtarı kullanabilirsiniz. Kodunuzu yapıştırma panoya kopyalamak için her anahtar yanındaki düğmeyi seçin.

> [!NOTE]
> Bir veya birden çok bölgede standart katman abonelikleri sınırsız sayıda oluşturabilirsiniz. Ancak, yalnızca bir ücretsiz katman abonelik oluşturabilir.

## <a name="next-steps"></a>Sonraki adımlar

10 dakikalık başlangıçtan biriyle tamamlamak veya SDK örneklerimizi denetleyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: C# ' de Konuşma tanıma](quickstart-csharp-dotnet-windows.md)
> [Speech SDK'sı örnekleri](speech-sdk.md#get-the-samples)
