---
title: Konuşma tanıma hizmetini ücretsiz olarak deneyin
description: Konuşma hizmeti ücretsiz deneyebilir miyim nasıl keşfedin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: a9972d19d33c69ea2eb1b0e00512f37a315b92d8
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285025"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Konuşma hizmeti ile çalışmaya başlama, kolay ve hesaplı. 30 günlük ücretsiz deneme, olanak tanır. hangi hizmet yapmak ve uygulamanızın ihtiyaçları için doğru olup olmadığını karar verebilirsiniz keşfedin.

Daha fazla süreye ihtiyacınız varsa, Microsoft Azure hesabı için kaydolun-200 ABD Doları, ücretli bir konuşma hizmeti abonelik 30 güne kadar doğru uygulayabileceğiniz hizmet kredi ile birlikte gelir.

Son olarak, uygun uygulamaları geliştirmek için ücretsiz, düşük hacimli katmanı konuşma hizmeti sunar. Hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği tutabilirsiniz.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

30 günlük ücretsiz deneme için sınırlı bir süre için fiyatlandırma katmanı S0 standart erişmenizi sağlar. 30 günlük ücretsiz deneme için kaydolmak için aşağıdaki adımları izleyin.

1. Git [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/) sayfası.

1. Konuşma sekmesine geçip tıklatın **alma API anahtarı** yanındaki "Konuşma Hizmetleri" düğmesini.

   ![Konuşma Hizmetleri sekmesi](media/index/try-speech-api-free-trial1.png)<br>
   ![API anahtarı](media/index/try-speech-api-free-trial2.png)

3. Koşulları kabul edin ve aşağı açılan menüden bölgeniz seçin.

   ![Koşulları kabul edin](media/index/try-speech-api-free-trial3.png)

4. Microsoft, Facebook, LinkedIn ve GitHub hesabınızı kullanarak oturum açın. Veya ücretsiz bir Microsoft hesabı için kaydolun:

    * Git [Microsoft hesap portalı](https://account.microsoft.com/account).
    * Tıklayın **Microsoft'ta oturum açma**.

    ![Oturum aç](media/index/try-speech-api-free-trial4.png)

    * Oturum açmak için sorulduğunda "bir tane oluşturun." tıklayın

    ![Yeni hesap oluştur](media/index/try-speech-api-free-trial5.png)

    * E-posta adresi veya telefon numarasını girin, takip adımlar, bir parola atayın ve yeni Microsoft hesabınızı doğrulamak için yönergeleri izleyin.

Oturum açtıktan sonra ücretsiz deneme sürümünüzü başlatır. Görüntülenen Web sayfasının şu anda deneme abonelikleri olan Bilişsel hizmetler, tüm listelenir. İki Abonelik anahtarları "Konuşma Hizmetleri." listelenir. Uygulamalarınızda iki anahtarı kullanabilir.

> [!NOTE]
> Tüm ücretsiz deneme Abonelikleri, Batı ABD bölgesinde içindir. İstekleri yaparken, bölgesine karşılık gelen uç noktasını kullandığınızdan emin olun.

## <a name="new-azure-account"></a>Yeni Azure hesabı

Yeni Azure hesapları en çok 30 gün sürer 200 ABD Doları değerinde bir hizmet kredisi alırsınız. Bu kredi, daha fazla konuşma Hizmeti'ni keşfedin veya uygulama geliştirmesini başlatma için kullanılabilir.

Yeni bir Azure hesabı için kaydolmak için aşağıdaki adımları izleyin.

1. Git [Azure'da oturum sayfası](https://azure.microsoft.com/free/ai/). 

1. Tıklayın **ücretsiz olarak Başla**.

    ![Ücretsiz kullanmaya başlayın](media/index/try-speech-api-new-azure1.png)

3. Microsoft hesabınızla oturum açın. Yoksa:

    * Git [Microsoft hesap portalı](https://account.microsoft.com/account).
    * Tıklayın **Microsoft'ta oturum açma**.
    * Oturum açmak için sorulduğunda "bir tane oluşturun." tıklayın
    * E-posta adresi veya telefon numarasını girin, takip adımlar, bir parola atayın ve yeni Microsoft hesabınızı doğrulamak için yönergeleri izleyin.

1. Kalan bir hesap için kaydolmanız için istenen bilgileri girin. Ülkenizi ve adı belirtin ve telefon numarası ve e-posta adresi sağlayın.

    ![Bilgileri girin](media/index/try-speech-api-new-azure2.png)

    Telefon ve kredi kartı numarası sağlayarak kimliğinizi doğrulayın ve ardından Azure kullanıcı anlaşmasını kabul edin. (Kredi kartınızdan faturalandırılmazsınız.)

    ![Sözleşmesini kabul edin](media/index/try-speech-api-new-azure3.png)

Ücretsiz Azure hesabınızı oluşturulur. Konuşma hizmeti için bir abonelik başlatmak için sonraki bölümdeki adımları izleyin.

## <a name="create-a-speech-resource-in-azure"></a>Azure'da bir konuşma kaynak oluşturma

Azure hesabınızda bir konuşma hizmeti kaynak eklemek için aşağıdaki adımları izleyin.

1. Oturum [Azure portalında](https://ms.portal.azure.com/) Microsoft hesabınızı kullanarak.

1. Tıklayın **kaynak Oluştur** (yeşil **+** simgesi), portalın sol üst köşesindeki.

    ![Kaynak Oluştur](media/index/try-speech-api-create-speech1.png)

1. Yeni pencerede, konuşma arayın.

    ![Konuşma tıklayın](media/index/try-speech-api-create-speech2.png)

1. Arama sonuçlarında "Konuşma (Önizleme)."'a tıklayın.

1. Tıklayın **Oluştur** konuşma hizmet Masası alt kısmındaki düğmesi.

    ![Oluştur'a tıklayın](media/index/try-speech-api-create-speech3.png)

1. Oluşturma panelinde girin:

    * Yeni kaynak için bir ad. Adı aynı hizmetin birden fazla aboneliğe arasında ayırt etmenize yardımcı olur.
    * Yeni kaynak ücretleri nasıl faturalandırılır belirlemek için ilişkili olduğu Azure aboneliğini seçin.
    * Kaynağı nerede kullanılacak bir bölge seçin. Şu anda konuşma hizmeti Doğu Asya, Kuzey Avrupa ve Batı ABD bölgelerinde kullanılabilir.
    * Fiyatlandırma katmanı, her iki F0 seçin (sınırlı, ücretsiz aboneliği) veya S0 (Standart abonelik). Tıklayın **fiyatlandırma ayrıntılarının tamamını görüntüle** her katman için fiyatlandırma ve kullanım Kotalar hakkında tam bilgi için.
    * Bu konuşma abonelik için yeni bir kaynak grubu oluşturun veya mevcut bir atayabilirsiniz. Kaynak düzenlenir, çeşitli Azure aboneliklerinizi tutmak Yardım gruplandırır.
    * Aboneliğinize gelecekte kolay erişim için işaretle **panoya Sabitle** onay kutusu.
    * Tıklayın **oluşturun.**

    ![Panelinde Oluştur'a tıklayın](media/index/try-speech-api-create-speech4.png)

    Bu, oluşturmak ve yeni konuşma kaynağınızı dağıtmak için bir dakika sürebilir. Hızlı Başlangıç panelinde yeni kaynağınız hakkındaki bilgileri görüntülenir.

    ![Hızlı Başlangıç paneli](media/index/try-speech-api-create-speech5.png)

1. Tıklayın **anahtarları** abonelik anahtarlarınızı görüntülemek için hızlı başlangıç panelinde'nin altında 1. adım. Her aboneliğin, iki anahtarı vardır; uygulamanızda kullanabilirsiniz. Kodunuzu yapıştırma panoya kopyalamak için her anahtar yanındaki düğmeye tıklayın.

> [!NOTE]
> Bir veya birden çok bölgede herhangi bir sayıda standart katman abonelikleri oluşturabilir. Ancak, yalnızca bir ücretsiz katman abonelik oluşturabilir.

## <a name="next-steps"></a>Sonraki adımlar

SDK ve bazı örnek kodlar konuşma hizmeti deneyimi için indirin.

> [!div class="nextstepaction"]
> [Konuşma SDK'ları](speech-sdk.md)

> [!div class="nextstepaction"]
> [Örnek kod](samples.md)
