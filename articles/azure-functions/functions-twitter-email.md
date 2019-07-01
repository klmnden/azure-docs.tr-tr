---
title: Azure Logic Apps ile tümleşen bir işlev oluşturma
description: Tweet duyarlılığını kategorilere ayırmak ve duyarlılık düşük olduğunda bildirim göndermek için Azure Logic Apps ve Azure Bilişsel Hizmetler ile tümleşen bir işlev oluşturun.
services: functions, logic-apps, cognitive-services
keywords: iş akışı, bulut uygulamaları, bulut hizmetleri, iş süreçleri, sistem tümleştirme, kuruluş uygulaması tümleştirme, EAI
author: craigshoemaker
manager: gwallace
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: azure-functions
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: cshoe
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 11e0170464d8ed901253925eb62cce12d42c0008
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480024"
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Azure Logic Apps ile tümleşen bir işlev oluşturma

Azure İşlevleri, Logic Apps Tasarımcısı'nda Azure Logic Apps ile tümleşir. Bu tümleştirme, İşlevlerin bilgi işlem gücünü diğer Azure hizmetleri ve üçüncü taraf hizmetler ile yapılan düzenlemelerde kullanmanıza olanak sağlar. 

Bu öğreticide Twitter gönderilerinden elde edilen duyarlılığın analizini çalıştırmak için İşlevler’in Azure’da Logic Apps ve Bilişsel Hizmetler ile birlikte nasıl kullanılacağı gösterilir. HTTP ile tetiklenen bir işlev, tweetleri duyarlılık puanına göre yeşil, sarı veya kırmızı kategorilere ayırır. Zayıf duyarlılık algılandığında bir e-posta gönderilir. 

![Logic App Tasarımcısı’nda uygulamanın ilk iki adımını gösteren görüntü](media/functions-twitter-email/00-logic-app-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Bilişsel Hizmetler API Kaynağı oluşturun.
> * Tweet duyarlılığını kategorilere ayıran bir işlev oluşturun.
> * Twitter’a bağlanan bir mantıksal uygulama oluşturun.
> * Mantıksal uygulamaya duyarlılık algılama özelliğini ekleyin. 
> * Mantıksal uygulamayı işleve bağlayın.
> * İşlevden alınan yanıta göre bir e-posta gönderin.

## <a name="prerequisites"></a>Önkoşullar

+ Etkin bir [Twitter](https://twitter.com/) hesabı. 
+ Bir [Outlook.com](https://outlook.com/) hesabı (bildirim göndermek için).
+ Bu makalede, başlangıç noktası olarak [Azure portalında ilk işlevinizi oluşturma](functions-create-first-azure-function.md) bölümünde oluşturulan kaynaklar kullanılmaktadır.  
Daha önce yapmadıysanız işlev uygulamanızı oluşturmak için bu adımları uygulayın.

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

Bilişsel Hizmetler API'leri Azure’da tek kaynaklar halinde kullanılabilir. İzlenmekte olan tweetlerin duyarlılığını algılamak için Metin Analizi API’sini kullanın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

3. **Yapay Zeka ve Makine Öğrenmesi** > **Metin Analizi**’ne tıklayın. Sonra, tabloda belirtilen ayarları kullanarak kaynağı oluşturun.

    ![Bilişsel kaynak sayfası oluşturma](media/functions-twitter-email/01-create-text-analytics.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | --- |
    | **Ad** | MyCognitiveServicesAccnt | Benzersiz bir hesap adı seçin. |
    | **Location** | Batı ABD | Size en yakın konumu kullanın. |
    | **Fiyatlandırma katmanı** | F0 | En düşük katman ile başlayın. Çağrılarınız biterse daha yüksek bir katmana ölçeklendirin.|
    | **Kaynak grubu** | myResourceGroup | Bu öğreticideki tüm hizmetler için aynı kaynak grubunu kullanın.|

4. Kaynağınızı oluşturmak için **Oluştur**'a tıklayın. 

5. **Genel bakış**’a tıklayın ve **Uç nokta** değerini bir metin düzenleyiciye kopyalayın. Bu değer, Bilişsel Hizmetler API’sine bağlantı oluşturulurken kullanılır.

    ![Bilişsel Hizmetler Ayarları](media/functions-twitter-email/02-cognitive-services.png)

6. Sol gezinti sütununda **Anahtarlar**’a tıklayın ve sonra **Anahtar 1** değerini metin düzenleyicisinde bir yere yazın. Anahtarı kullanarak mantıksal uygulamayı Bilişsel Hizmetler API'sine bağlayabilirsiniz. 
 
    ![Bilişsel Hizmetler Anahtarları](media/functions-twitter-email/03-cognitive-serviecs-keys.png)

## <a name="create-the-function-app"></a>İşlev uygulaması oluşturma

İşlevler bir mantıksal uygulama iş akışındaki işleme görevlerini boşaltmanın harika bir yoludur. Bu öğreticide, Bilişsel Hizmetler’den alınan tweet duyarlılığı puanlarını işlemek ve bir kategori değeri döndürmek için bir HTTP ile tetiklenen bir işlev kullanılır.  

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-an-http-triggered-function"></a>HTTP ile tetiklenen işlev oluşturma  

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Portalda**'yı seçin.

    ![Azure portalındaki İşlevler hızlı başlangıç sayfası](media/functions-twitter-email/05-function-app-create-portal.png)

2. Ardından, **Web Kancası + API**’yi seçin ve **Oluştur**’a tıklayın. 

    ![HTTP tetikleyicisini seçin](./media/functions-twitter-email/06-function-webhook.png)

3. `run.csx` dosyasının içeriğini aşağıdaki kodla değiştirip **Kaydet**’e tıklayın:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        string category = "GREEN";
    
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        log.LogInformation(string.Format("The sentiment score received is '{0}'.", requestBody));
    
        double score = Convert.ToDouble(requestBody);
    
        if(score < .3)
        {
            category = "RED";
        }
        else if (score < .6) 
        {
            category = "YELLOW";
        }
    
        return requestBody != null
            ? (ActionResult)new OkObjectResult(category)
            : new BadRequestObjectResult("Please pass a value on the query string or in the request body");
    }
    ```
    Bu işlev kodu, istekte alınan duyarlılık puanına göre bir renk kategorisi döndürür. 

4. İşlevi test etmek için en sağdaki **Test** seçeneğine tıklayarak Test sekmesini genişletin. **İstek gövdesi** `0.2` değerini girip **Çalıştır**’a tıklayın. Yanıtın gövdesinde **RED** değeri döndürülür. 

    ![İşlevi Azure portalında test etme](./media/functions-twitter-email/07-function-test.png)

Artık duyarlılık puanlarını kategorilere ayıran bir işleviniz vardır. bundan sonra, işlevinizi Twitter ve Bilişsel Hizmetler API’niz ile tümleştiren bir mantıksal uygulama oluşturun. 

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma   

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **Web** > **Mantıksal Uygulama**’ya tıklayın.
 
3. Ardından **Ad** alanına `TweetSentiment` gibi bir değer yazın ve tabloda belirtilen ayarları kullanın.

    ![Azure portalda mantıksal uygulama oluşturma](./media/functions-twitter-email/08-logic-app-create.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | ----------------- | ------------ | ------------- |
    | **Ad** | TweetSentiment | Uygulamanız için uygun bir ad seçin. |
    | **Kaynak grubu** | myResourceGroup | Daha önceki ile aynı mevcut kaynak grubunu seçin. |
    | **Location** | Doğu ABD | Size yakın bir konum seçin. |    

4. Uygun ayar değerlerini girdikten sonra mantıksal uygulamanızı oluşturmak için **Oluştur**’a tıklayın. 

5. Uygulama oluşturulduktan sonra, panoya sabitlenmiş yeni mantıksal uygulamanıza tıklayın. Ardından Logic Apps Tasarımcısı'nda sayfayı aşağı kaydırıp **Boş Mantıksal Uygulama** şablonuna tıklayın. 

    ![Boş Mantıksal Uygulama şablonu](media/functions-twitter-email/09-logic-app-create-blank.png)

Artık Logic Apps Tasarımcısı'nı kullanarak uygulamanıza hizmetler ve tetikleyiciler ekleyebilirsiniz.

## <a name="connect-to-twitter"></a>Twitter’a Bağlanma

İlk olarak Twitter hesabınızla bir bağlantı oluşturun. Mantıksal uygulama tweetleri yoklar ve uygulamanın çalışması tetiklenir.

1. Tasarımcıda **Twitter** hizmetine ve **Yeni bir tweet gönderildiğinde** tetikleyicisine tıklayın. Twitter hesabınızda oturum açın ve Logic Apps’in hesabınızı kullanmasına izin verin.

2. Tabloda belirtilen Twitter tetikleyici ayarlarını kullanın. 

    ![Twitter bağlayıcı ayarları](media/functions-twitter-email/10-tweet-settings.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | ----------------- | ------------ | ------------- |
    | **Arama metni** | #Azure | Seçilen aralıkta yeni tweetler oluşturmak için yeterince popüler olan bir diyez etiketi kullanın. Ücretsiz katmanını kullanırken diyez etiketiniz çok popüler olursa, Bilişsel Hizmetler API'nizdeki işlem kotasını hızlı bir şekilde tüketebilirsiniz. |
    | **Aralık** | 15 | Sıklık birimleri cinsinden Twitter istekleri arasında geçen süre. |
    | **Sıklık** | Dakika | Twitter’ı yoklamak için kullanılan sıklık birimi.  |

3.  Twitter hesabınıza bağlanmak için **Kaydet**’e tıklayın. 

Uygulamanız artık Twitter’a bağlıdır. Ardından, toplanan tweetlerin duyarlılığını algılamak için metin analizine bağlanın.

## <a name="add-sentiment-detection"></a>Duyarlılık algılama ekleme

1. **Yeni Adım**’a ve sonra **Eylem ekle**’ye tıklayın.

2. **Eylem seçin** alanına **Metin Analizi** yazın ve sonra **Duyarlılığı algıla** eylemine tıklayın.
    
    ![Yeni Adım ve sonra Eylem ekle](media/functions-twitter-email/11-detect-sentiment.png)

3. `MyCognitiveServicesConnection` gibi bir bağlantı adı yazın, metin düzenleyicide kaydettiğiniz Bilişsel Hizmetler API’nizin ve Bilişsel Hizmetler uç noktasının anahtarını yapıştırın, sonra **Oluştur**’a tıklayın.

    ![Yeni Adım ve sonra Eylem ekle](media/functions-twitter-email/12-connection-settings.png)

4. Ardından, metin kutusuna **Tweet metni** girin ve **Yeni Adım**’a tıklayın.

    ![Analiz edilecek metni tanımlama](media/functions-twitter-email/13-analyze-tweet-text.png)

Duyarlılık algılaması yapılandırıldıktan sonra işlevinize duyarlılık puanı çıktısını kullanan bir bağlantı ekleyebilirsiniz.

## <a name="connect-sentiment-output-to-your-function"></a>Duyarlılık çıktısını işlevinize bağlama

1. Logic Apps Tasarımcısı’nda **Yeni adım** > **Eylem ekle**’ye tıklayın, **Azure İşlevleri**’yle filtreleyin ve **Bir Azure işlevi seçin** öğesine tıklayın.

    ![Duyarlılığı Algıla](media/functions-twitter-email/14-azure-functions.png)
  
4. Daha önce oluşturduğunuz işlev uygulamasını seçin.

    ![İşlev seçme](media/functions-twitter-email/15-select-function.png)

5. Bu öğretici için oluşturduğunuz işlevi seçin.

    ![İşlev seçme](media/functions-twitter-email/16-select-function.png)

4. **İstek Gövdesi**’nde **Puan**’a ve ardından **Kaydet**’e tıklayın.

    ![Puan](media/functions-twitter-email/17-function-input-score.png)

Artık mantıksal uygulamadan bir duyarlılık puanı gönderildiğinde işleviniz tetiklenir. İşlev tarafından mantıksal uygulamaya renk kodlu bir kategori döndürülür. Daha sonra, işlevden **RED** duyarlılık değeri döndürüldüğünde gönderilen bir e-posta bildirimi ekleyin. 

## <a name="add-email-notifications"></a>E-posta bildirimleri ekleme

İş akışının son parçası, duyarlılık puanı _RED_ olduğunda bir e-postanın tetiklenmesidir. Bu konuda bir Outlook.com bağlayıcısı kullanılır. Gmail veya Office 365 Outlook bağlayıcısını kullanmak için benzer adımlar gerçekleştirebilirsiniz.   

1. Logic Apps Tasarımcısı'nda **Yeni adım** > **Koşul ekle**’ye tıklayın. 

    ![Mantıksal uygulamaya koşul ekleyin.](media/functions-twitter-email/18-add-condition.png)

2. **Bir değer seçin** ve ardından **Gövde**’ye tıklayın. **Eşittir**’e tıklayın, **Bir değer seçin**’e tıklayın ve `RED` yazıp **Kaydet**’e tıklayın. 

    ![Koşul için bir eylem seçin.](media/functions-twitter-email/19-condition-settings.png)    

3. **IF TRUE** alanında **Eylem ekle**’ye tıklayın, `outlook.com` ifadesini arayın, **E-posta gönder**’e tıklayın ve Outlook.com hesabınızda oturum açın.

    ![E-posta gönderme eylemi için e-postayı yapılandırın.](media/functions-twitter-email/20-add-outlook.png)

    > [!NOTE]
    > Bir Outlook.com hesabınız yoksa Gmail veya Office 365 Outlook gibi başka bir bağlayıcı seçebilirsiniz

4. **E-posta gönder** eyleminde tabloda belirtilen e-posta ayarlarını kullanın. 

    ![E-posta gönderme eylemi için e-postayı yapılandırın.](media/functions-twitter-email/21-configure-email.png)
    
| Ayar      |  Önerilen değer   | Açıklama  |
| ----------------- | ------------ | ------------- |
| **Alıcı** | E-posta adresinizi girin | Bildirimi alan e-posta adresi. |
| **Konu** | Olumsuz tweet duyarlılığı algılandı  | E-posta bildiriminin konu satırı.  |
| **Gövde** | Tweet metni, Konum | **Tweet metni** ve **Konum** parametrelerine tıklayın. |

1. **Kaydet**’e tıklayın.

İş akışı tamamlandığına göre mantıksal uygulamayı etkinleştirip işlevin nasıl çalıştığını görebilirsiniz.

## <a name="test-the-workflow"></a>İş akışını test etme

1. Logic Apps Tasarımcısı'nda, uygulamayı başlatmak için **Çalıştır**'a tıklayın.

2. Sol sütunda **Genel Bakış**’a tıklayarak mantıksal uygulamanın durumuna bakın. 
 
    ![Mantıksal uygulama yürütme durumu](media/functions-twitter-email/22-execution-history.png)

3. (İsteğe bağlı) Yürütme ayrıntılarını görmek için çalışmalardan birine tıklayın.

4. İşlevinize gidin, günlükleri görüntüleyin ve duyarlılık değerlerinin alınıp işlendiğini doğrulayın.
 
    ![İşlev günlüklerini görüntüleme](media/functions-twitter-email/sent.png)

5. Olumsuz olabilecek bir duyarlılık algılandığında bir e-posta alırsınız. Bir e-posta almadıysanız işlev kodunu her zaman RED döndürecek şekilde değiştirebilirsiniz:

    ```csharp
    return (ActionResult)new OkObjectResult("RED");
    ```

    E-posta bildirimlerini doğruladıktan sonra özgün koda geri dönün:

    ```csharp
    return requestBody != null
        ? (ActionResult)new OkObjectResult(category)
        : new BadRequestObjectResult("Please pass a value on the query string or in the request body");
    ```

    > [!IMPORTANT]
    > Bu öğreticiyi tamamladıktan sonra mantıksal uygulamayı devre dışı bırakmanız gerekir. Uygulamayı devre dışı bırakarak yürütmeler için sizden ücret alınmasını ve Bilişsel Hizmetler API'nizdeki işlemlerin tükenmesini önlersiniz.

İşlevleri bir Logic Apps iş akışıyla tümleştirmek ne kadar kolay olduğunu gördünüz.

## <a name="disable-the-logic-app"></a>Mantıksal uygulamayı devre dışı bırakma

Mantıksal uygulamayı devre dışı bırakmak için **Genel Bakış** ve sonra ekranın üstündeki **Devre Dışı Bırak**’a tıklayın. Uygulamayı devre dışı bıraktığınızda, uygulama silinmeden mantıksal uygulamanın çalışması ve ücretlerin yansıtılması durdurulur.

![İşlev günlükleri](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir Bilişsel Hizmetler API Kaynağı oluşturun.
> * Tweet duyarlılığını kategorilere ayıran bir işlev oluşturun.
> * Twitter’a bağlanan bir mantıksal uygulama oluşturun.
> * Mantıksal uygulamaya duyarlılık algılama özelliğini ekleyin. 
> * Mantıksal uygulamayı işleve bağlayın.
> * İşlevden alınan yanıta göre bir e-posta gönderin.

İşlevinize yönelik sunucusuz bir API oluşturma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"] 
> [Azure İşlevlerini kullanarak sunucusuz bir API oluşturma](functions-create-serverless-api.md)

Logic Apps hakkında daha fazla bilgi için bkz. [Azure Logic Apps](../logic-apps/logic-apps-overview.md).

