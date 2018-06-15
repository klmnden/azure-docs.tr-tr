---
title: Konuşma hizmeti ücretsiz deneyebilir | Microsoft Docs
description: Herhangi bir ücret ödemeden konuşma hizmeti nasıl anlarım bulur.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: 3feef04694336d9173b419285a96fcaef543e18f
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356242"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma hizmet ücretsiz deneyin

Konuşma hizmetini kullanmaya başlama kolay ve uygun maliyetli. 30 günlük ücretsiz deneme sağlayan ne hizmet yapın ve uygulamanızın gereksinimleriniz için doğru olup olmadığını karar verebilirsiniz keşfedin.

Daha fazla zaman ihtiyacınız varsa, bir Microsoft Azure hesabı için kaydolun: 30 gün için ücretli bir konuşma hizmeti abonelik doğru uygulayabilirsiniz hizmet kredisi 200 ABD Doları birlikte gelir.

Son olarak, bir ücretsiz, düşük hacimli katmanı uygulamaları geliştirmek için uygun konuşma hizmeti sunar. Hatta, hizmet iadesi süresi dolduktan sonra bu ücretsiz abonelik kullanmaya devam edebilir.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

30 günlük ücretsiz deneme sürümü için sınırlı bir süre için fiyatlandırma katmanı S0 standart erişmenizi sağlar. 30 günlük ücretsiz deneme için kaydolmak için aşağıdaki adımları izleyin.

1. Git [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/) sayfası.

1. Konuşma sekmesine geçin ve tıklatın **alma API anahtarı** "Konuşma Hizmetleri" yanındaki düğmesi.

   ![Konuşma hizmetler sekmesi](media/index/try-speech-api-free-trial1.png)<br>
   ![API anahtarı](media/index/try-speech-api-free-trial2.png)

3. Kabul ediyor ve açılan menüden bölgeniz seçin.

   ![Koşulları kabul ediyorum](media/index/try-speech-api-free-trial3.png)

4. Microsoft, Facebook, LinkedIn veya GitHub hesabınızı kullanarak oturum açın. Ya da boş bir Microsoft hesabı için kaydolmadan:

    * Git [Microsoft hesap portalı](https://account.microsoft.com/account).
    * Tıklatın **oturum oturum Microsoft**.

    ![Oturum aç](media/index/try-speech-api-free-trial4.png)

    * Oturum açmak için sorulduğunda "bir tane oluşturun."'i tıklatın

    ![Yeni hesap oluştur](media/index/try-speech-api-free-trial5.png)

    * İzleyin, e-posta adresi veya telefon numarası girin adımlar, bir parola atayın ve yeni Microsoft hesabınızı doğrulamak için yönergeleri izleyin.

Oturum açtıktan sonra ücretsiz deneme sürümünüzü başlar. Görüntülenen Web sayfası tüm Bilişsel için şu anda deneme abonelikleri olan hizmetler listelenir. İki abonelik anahtar "Konuşma Hizmetleri." listelenir Uygulamalarınızda iki anahtarı kullanabilir.

> [!NOTE]
> Tüm ücretsiz deneme aboneliği Batı ABD bölgesinde ' dir. Bölgeniz için karşılık gelen istekleri yaparken, uç nokta kullandığınızdan emin olun.

## <a name="new-azure-account"></a>Yeni bir Azure hesabı

Yeni Azure hesaplar 30 güne kadar sürer 200 ABD Doları hizmet iadesi alırsınız. Bu iade daha fazla konuşma Hizmetleri keşfedin veya uygulama geliştirme başlamak için kullanılabilir.

Yeni bir Azure hesabı için kaydolmak için aşağıdaki adımları izleyin.

1. Git [Azure'da oturum PageUp](https://azure.microsoft.com/free/ai/). 

1. Tıklatın **ücretsiz Başlat**.

    ![Ücretsiz kullanmaya başlayın](media/index/try-speech-api-new-azure1.png)

3. Microsoft hesabınızla oturum açın. Yoksa:

    * Git [Microsoft hesap portalı](https://account.microsoft.com/account).
    * Tıklatın **oturum oturum Microsoft**.
    * Oturum açmak için sorulduğunda "bir tane oluşturun."'i tıklatın
    * İzleyin, e-posta adresi veya telefon numarası girin adımlar, bir parola atayın ve yeni Microsoft hesabınızı doğrulamak için yönergeleri izleyin.

1. Kalan bir hesap için kaydolmanız için istenen bilgileri girin. Ülkeniz ve adınızı belirtmek ve bir telefon numarası ve e-posta adresi sağlayın.

    ![Bilgileri girin](media/index/try-speech-api-new-azure2.png)

    Telefonla ve kredi kartı numarası sağlayarak kimliğinizi doğrulayın, sonra Azure kullanıcı Sözleşmesi'ni kabul edin. (Kredi kartınızdan Fatura edilecek değil.)

    ![Anlaşmasını kabul et](media/index/try-speech-api-new-azure3.png)

Ücretsiz Azure hesabınız oluşturulur. Konuşma hizmeti için bir abonelik başlatmak için sonraki bölümdeki adımları izleyin.

## <a name="create-a-speech-resource-in-azure"></a>Konuşma kaynak oluşturma

Azure hesabınızda bir konuşma Hizmet kaynağı eklemek için aşağıdaki adımları izleyin.

1. Oturum [Azure portal](https://ms.portal.azure.com/) Microsoft hesabınızı kullanarak.

1. Tıklatın **kaynak oluşturma** (yeşil **+** simgesi), portalın sol üst.

    ![Kaynak Oluştur](media/index/try-speech-api-create-speech1.png)

1. Yeni pencerede konuşma için arama yapın.

    ![Konuşma'yı tıklatın](media/index/try-speech-api-create-speech2.png)

1. Arama sonuçlarında "Konuşma (Önizleme)."'yı tıklatın.

1. Tıklatın **oluşturma** konuşma hizmet Masası sonundaki düğmesi.

    ![Oluştur'u tıklatın](media/index/try-speech-api-create-speech3.png)

1. Oluşturma panelinde girin:

    * Yeni kaynak için bir ad. Ad, aynı hizmetin birden fazla abonelik ayırt yardımcı olur.
    * Yeni kaynak ücretler nasıl faturalandırılır belirlemek için ilişkili olduğu Azure aboneliğini seçin.
    * Kaynak nerede kullanılacak bir bölge seçin. Şu anda, konuşma hizmeti Doğu Asya, Kuzey Avrupa ve Batı ABD bölgelerde kullanılabilir.
    * Her iki F0 fiyatlandırma katmanını seçin (sınırlı ücretsiz abonelik) veya S0 (Standart abonelik). Tıklatın **görünüm fiyatlandırma ayrıntıları tam** her katman için fiyatlandırma ve kullanım kotaları hakkında tam bilgi için.
    * Bu konuşma abonelik için yeni bir kaynak grubu oluşturmak veya mevcut bir atayabilirsiniz. Kaynak düzenlenmiş, çeşitli Azure aboneliklerinize tutmak Yardım gruplandırır.
    * Gelecekte, aboneliğiniz için kolay erişim için işaretlemek **panoya Sabitle** onay kutusu.
    * Tıklatın **oluşturun.**

    ![Masası'nda Oluştur'u tıklatın](media/index/try-speech-api-create-speech4.png)

    Oluşturun ve yeni konuşma kaynağınız dağıtmak için bir dakika sürebilir. Hızlı Başlangıç paneli yeni kaynağınız hakkındaki bilgileri görüntülenir.

    ![Hızlı Başlangıç paneli](media/index/try-speech-api-create-speech5.png)

1. Tıklatın **anahtarları** abonelik tuşlarınızı görüntülemek için hızlı başlangıç panelinde 1. adım altında bağlantı. Her abonelik iki anahtar vardır; uygulamanızdaki ya da kullanabilir. Her anahtar kodunuzu yapıştırma panoya kopyalamak için İleri düğmesine tıklayın.

> [!NOTE]
> Bir veya birden çok bölgede herhangi bir sayıda standart katmanı abonelikleri oluşturabilir. Ancak, yalnızca bir ücretsiz katmanı abonelik oluşturabilir.

## <a name="next-steps"></a>Sonraki adımlar

Konuşma hizmeti deneyimi için bir SDK ve bazı örnek kodu indirin.

> [!div class="nextstepaction"]
> [Konuşma SDK'ları](speech-sdk.md)

> [!div class="nextstepaction"]
> [Örnek kod](samples.md)
