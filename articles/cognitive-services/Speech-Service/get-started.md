---
title: Konuşma tanıma hizmetini ücretsiz olarak deneyin
description: Konuşma hizmeti ücretsiz deneyebilir miyim nasıl keşfedin.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/24/2018
ms.author: erhopf
ms.openlocfilehash: 4530ecca973054ee73a02ca4e047dfe52ea90d04
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166194"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Konuşma hizmeti ile çalışmaya başlama, kolay ve hesaplı. 30 günlük ücretsiz deneme, olanak tanır. hangi hizmet yapmak ve uygulamanızın ihtiyaçları için doğru olup olmadığını karar verebilirsiniz keşfedin.

Daha fazla süreye ihtiyacınız varsa, Microsoft Azure hesabı için kaydolun-200 ABD Doları, ücretli bir konuşma hizmeti abonelik 30 güne kadar doğru uygulayabileceğiniz hizmet kredi ile birlikte gelir.

Son olarak, konuşma hizmeti uygulamaları geliştirmek için uygun olan ücretsiz, düşük hacimli katmanı sunar. Hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği tutabilirsiniz.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

30 günlük ücretsiz deneme standart fiyatlandırma katmanı sınırlı bir süre için erişmenizi sağlar.

30 günlük ücretsiz deneme için kaydolmak için:

1. Git [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/).

1. Seçin **konuşma API'leri** sekmesi.

   ![Konuşma Hizmetleri sekmesi](media/index/try-speech-api-free-trial1.png)
   
1. Altında **konuşma Hizmetleri**seçin **API anahtarı alma** düğmesi.

   ![API anahtarı](media/index/try-speech-api-free-trial2.png)

1. Koşulları kabul edin ve aşağı açılan menüden bölgeniz seçin.

   ![Koşulları kabul edin](media/index/try-speech-api-free-trial3.png)

1. Microsoft, Facebook, LinkedIn ve GitHub hesabınızı kullanarak oturum açın.

    Ücretsiz bir Microsoft hesabı için kaydolabilirsiniz [Microsoft hesap portalı](https://account.microsoft.com/account). Başlamak için tıklayın **Microsoft'ta oturum açma** oturum açması istenir,'a tıklayarak **oluşturun.** Oluşturun ve yeni Microsoft hesabınızı doğrulamak için adımları izleyin.

Bilişsel hizmetler için deneyin'da oturum açtıktan sonra ücretsiz deneme sürümünüzü başlar. Görüntülenen Web sayfasının şu anda deneme abonelikleri olan tüm Azure Bilişsel hizmetler hizmetler listelenir. Abonelik anahtarları iki yanında listelenen **konuşma Hizmetleri**. İki anahtarı uygulamalarınızda kullanabilirsiniz.

> [!NOTE]
> Tüm ücretsiz deneme Abonelikleri, Batı ABD bölgesinde içindir. İstekte bulunduğunuzda kullandığınızdan emin olun `westus` uç noktası.

## <a name="new-azure-account"></a>Yeni Azure hesabı

Yeni Azure hesapları 30 gün boyunca kullanılabilir 200 ABD Doları değerinde bir hizmet kredisi alırsınız. Bu kredi konuşma hizmeti daha iyi keşfedilebilmesi için veya uygulama geliştirmeye başlamak için kullanabilirsiniz.

Yeni bir Azure hesabı için kaydolmak için Git [Azure kayıt sayfasına](https://azure.microsoft.com/free/ai/), tıklayın **ücretsiz Başlat** ve Microsoft hesabınızı kullanarak yeni bir Azure hesabı oluşturun.

Ücretsiz bir Microsoft hesabı için kaydolabilirsiniz [Microsoft hesap portalı](https://account.microsoft.com/account). Başlamak için tıklayın **Microsoft'ta oturum açma** oturum açması istenir,'a tıklayarak **oluşturun.** Oluşturun ve yeni Microsoft hesabınızı doğrulamak için adımları izleyin.

Azure hesabınızı oluşturduktan sonra konuşma hizmeti için bir abonelik başlatmak için sonraki bölümdeki adımları izleyin.

## <a name="create-a-speech-resource-in-azure"></a>Azure'da bir konuşma kaynak oluşturma

Azure hesabınızda bir konuşma hizmeti kaynak (ücretsiz veya Ücretli katman) eklemek için:

1. Oturum [Azure portalında](https://ms.portal.azure.com/) Microsoft hesabınızı kullanarak.

1. Seçin **kaynak Oluştur** , portalın sol üst köşesindeki.

    ![Kaynak oluşturma](media/index/try-speech-api-create-speech1.png)

1. İçinde **yeni** penceresinde, arama **konuşma**.

1. Arama sonuçlarında seçin **konuşma**.

    ![Konuşma seçin](media/index/try-speech-api-create-speech2.png)

1. Altında **konuşma**seçin **Oluştur** düğmesi.

    ![Oluştur düğmesine seçin](media/index/try-speech-api-create-speech3.png)

1. Altında **Oluştur**, girin:

    * Yeni kaynak için bir ad. Adı aynı hizmetin birden fazla aboneliğe arasında ayırt etmenize yardımcı olur.
    * Yeni kaynak ücretleri nasıl faturalandırılır belirlemek için ilişkili olduğu Azure aboneliğini seçin.
    * Kaynağı nerede kullanılacak bir bölge seçin. Şu anda konuşma hizmeti Doğu Asya, Kuzey Avrupa ve Batı ABD bölgelerinde kullanılabilir.
    * Ya da ücretsiz veya Ücretli fiyatlandırma katmanı seçin. Tıklayın **fiyatlandırma ayrıntılarının tamamını görüntüle** her katman için fiyatlandırma ve kullanım Kotalar hakkında tam bilgi için.
    * Bu konuşma abonelik için yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu aboneliği atayın. Kaynak düzenlenir, çeşitli Azure aboneliklerinizi tutmak Yardım gruplandırır.
    * Aboneliğinize gelecekte kolay erişim için seçin **panoya Sabitle** onay kutusu.
    * Seçin **oluşturun.**

    ![Oluştur düğmesine seçin](media/index/try-speech-api-create-speech4.png)

    Oluşturmak ve yeni konuşma kaynağınızı dağıtmak için bir dakika sürer. Seçin **hızlı** yeni kaynağınız hakkındaki bilgileri görmek için.

    ![Hızlı Başlangıç paneli](media/index/try-speech-api-create-speech5.png)

1. Altında **hızlı**, tıklayın **anahtarları** abonelik anahtarlarınızı görüntülemek için 1. adım altında bağlantı. Her aboneliğin, iki anahtarı vardır; uygulamanızda iki anahtarı kullanabilirsiniz. Kodunuzu yapıştırma panoya kopyalamak için her anahtar yanındaki düğmeyi seçin.

> [!NOTE]
> Bir veya birden çok bölgede standart katman abonelikleri sınırsız sayıda oluşturabilirsiniz. Ancak, yalnızca bir ücretsiz katman abonelik oluşturabilir. Ücretsiz katmanındaki 7 gün için kullanılmaması modeli dağıtımlarını otomatik olarak kullanımdan olacaktır.

## <a name="switch-to-a-new-subscription"></a>Yeni bir aboneliğe geçme

Bir abonelikten diğerine geçmek için örneğin, ücretsiz deneme süresinin sona erdiği veya uygulamanızı yayımladığınızda değiştirin kodunuzu bölge ve abonelik anahtarı ile yeni bir Azure kaynak bölge ve abonelik anahtarı.

> [!NOTE]
> Ücretsiz deneme anahtarlarının yer olan Batı ABD'de oluşturulur (`westus`) bölge. Dilerseniz Azure Panosu oluşturulan bir aboneliği bazı başka bir bölgede olabilir.

* Uygulamanız kullanıyorsa bir [Speech SDK'sı](speech-sdk.md), bölge kodu gibi sağladığınız `westus`, konuşma yapılandırma oluştururken.
* Uygulamanızı konuşma hizmetin birini kullanıp kullanmadığını [REST API'leri](rest-apis.md), bölge uç noktası URI'si istekleri yaparken kullandığınız bir parçasıdır.

Bir bölgede oluşturulan anahtarları yalnızca bu bölgede geçerlidir. Diğer bölgeler ile kullanılmaya çalışılırsa, kimlik doğrulama hataları neden olur.

## <a name="next-steps"></a>Sonraki adımlar

10 dakikalık başlangıçtan biriyle tamamlamak veya SDK örneklerimizi denetleyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: C# ' de Konuşma tanıma](quickstart-csharp-dotnet-windows.md)
> [Speech SDK'sı örnekleri](speech-sdk.md#get-the-samples)
