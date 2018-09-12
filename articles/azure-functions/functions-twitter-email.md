---
title: Azure Logic Apps ile tümleşen bir işlev oluşturma | Microsoft Docs
description: Tweet duyarlılığını kategorilere ayırmak ve duyarlılık düşük olduğunda bildirim göndermek için Azure Logic Apps ve Azure Bilişsel Hizmetler ile tümleşen bir işlev oluşturun.
services: functions, logic-apps, cognitive-services
keywords: iş akışı, bulut uygulamaları, bulut hizmetleri, iş süreçleri, sistem tümleştirme, kuruluş uygulaması tümleştirme, EAI
author: ggailey777
manager: jeconnoc
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: azure-functions
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: glenga
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 23db8d307892b100f291a1f32c9b77c73a60f23e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44090772"
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a>Azure Logic Apps ile tümleşen bir işlev oluşturma

Azure İşlevleri, Logic Apps Tasarımcısı'nda Azure Logic Apps ile tümleşir. Bu tümleştirme, İşlevlerin bilgi işlem gücünü diğer Azure hizmetleri ve üçüncü taraf hizmetler ile yapılan düzenlemelerde kullanmanıza olanak sağlar. 

Bu öğreticide Twitter gönderilerinden elde edilen duyarlılığı analiz etmek için İşlevler’in Azure’da Logic Apps ve Microsoft Bilişsel Hizmetler ile birlikte nasıl kullanılacağı gösterilir. HTTP ile tetiklenen bir işlev, tweetleri duyarlılık puanına göre yeşil, sarı veya kırmızı kategorilere ayırır. Zayıf duyarlılık algılandığında bir e-posta gönderilir. 

![Logic App Tasarımcısı’nda uygulamanın ilk iki adımını gösteren görüntü](media/functions-twitter-email/designer1.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Bilişsel Hizmetler API Kaynağı oluşturun.
> * Tweet duyarlılığını kategorilere ayıran bir işlev oluşturun.
> * Twitter’a bağlanan bir mantıksal uygulama oluşturun.
> * Mantıksal uygulamaya duyarlılık algılama özelliğini ekleyin. 
> * Mantıksal uygulamayı işleve bağlayın.
> * İşlevden alınan yanıta göre bir e-posta gönderin.

## <a name="prerequisites"></a>Ön koşullar

+ Etkin bir [Twitter](https://twitter.com/) hesabı. 
+ Bir [Outlook.com](https://outlook.com/) hesabı (bildirim göndermek için).
+ Bu konu başlığında, başlangıç noktası olarak [Azure portalında ilk işlevinizi oluşturma](functions-create-first-azure-function.md) bölümünde oluşturulan kaynaklar kullanılmaktadır.  
Daha önce yapmadıysanız işlev uygulamanızı oluşturmak için bu adımları uygulayın.

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

Bilişsel Hizmetler API'leri Azure’da tek kaynaklar halinde kullanılabilir. İzlenmekte olan tweetlerin duyarlılığını algılamak için Metin Analizi API’sini kullanın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

3. **AI + Analiz** > **Metin Analizi API’si** seçeneğine tıklayın. Ardından, tabloda belirtilen ayarları kullanın, koşulları kabul edin ve **Panoya sabitle**’yi işaretleyin.

    ![Bilişsel kaynak sayfası oluşturma](media/functions-twitter-email/cog_svcs_resource.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | --- | --- | --- |
    | **Ad** | MyCognitiveServicesAccnt | Benzersiz bir hesap adı seçin. |
    | **Konum** | Batı ABD | Size en yakın konumu kullanın. |
    | **Fiyatlandırma katmanı** | F0 | En düşük katman ile başlayın. Çağrılarınız biterse daha yüksek bir katmana ölçeklendirin.|
    | **Kaynak grubu** | myResourceGroup | Bu öğreticideki tüm hizmetler için aynı kaynak grubunu kullanın.|

4. Kaynağınızı oluşturmak için **Oluştur**'a tıklayın. Oluşturulduktan sonra, panoya sabitlenmiş yeni Bilişsel Hizmetler kaynağınızı seçin. 

5. Sol gezinti sütununda **Anahtarlar**’a tıklayın ve sonra **Anahtar 1** değerini kopyalayıp kaydedin. Bu anahtarı kullanarak mantıksal uygulamayı Bilişsel Hizmetler API'sine bağlayabilirsiniz. 
 
    ![Anahtarlar](media/functions-twitter-email/keys.png)

## <a name="create-the-function-app"></a>İşlev uygulaması oluşturma

İşlevler bir mantıksal uygulama iş akışındaki işleme görevlerini boşaltmanın harika bir yoludur. Bu öğreticide, Bilişsel Hizmetler’den alınan tweet duyarlılığı puanlarını işlemek ve bir kategori değeri döndürmek için bir HTTP ile tetiklenen bir işlev kullanılır.  

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-an-http-triggered-function"></a>HTTP ile tetiklenen işlev oluşturma  

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Özel işlev**'i seçin. Böylece işlev şablonlarının tamamı görüntülenir.

    ![Azure portalındaki İşlevler hızlı başlangıç sayfası](media/functions-twitter-email/add-first-function.png)

2. Arama alanına `http` yazıp HTTP tetikleyici şablonunuz için **C#** dilini seçin. 

    ![HTTP tetikleyicisini seçin](./media/functions-twitter-email/select-http-trigger-portal.png)

3. İşleviniz için bir **Ad** yazın, **[Kimlik doğrulama düzeyi](functions-bindings-http-webhook.md#http-auth)** için `Function` öğesini ve sonra **Oluştur**’u seçin. 

    ![HTTP ile tetiklenen işlevi oluşturun](./media/functions-twitter-email/select-http-trigger-portal-2.png)

    Bu işlem, HTTP Tetikleyicisi şablonunu kullanarak bir C# betik işlevi oluşturur. Kodunuz yeni bir pencerede `run.csx` olarak görünür.

4. `run.csx` dosyasının içeriğini aşağıdaki kodla değiştirip **Kaydet**’e tıklayın:

    ```csharp
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // The sentiment category defaults to 'GREEN'. 
        string category = "GREEN";
    
        // Get the sentiment score from the request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("The sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set the category based on the sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    Bu işlev kodu, istekte alınan duyarlılık puanına göre bir renk kategorisi döndürür. 

4. İşlevi test etmek için en sağdaki **Test** seçeneğine tıklayarak Test sekmesini genişletin. **İstek gövdesi** `0.2` değerini girip **Çalıştır**’a tıklayın. Yanıtın gövdesinde **RED** değeri döndürülür. 

    ![İşlevi Azure portalında test etme](./media/functions-twitter-email/test.png)

Artık duyarlılık puanlarını kategorilere ayıran bir işleviniz vardır. bundan sonra, işlevinizi Twitter ve Bilişsel Hizmetler API’niz ile tümleştiren bir mantıksal uygulama oluşturun. 

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma   

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **Kurumsal Tümleştirme** > **Mantıksal Uygulama** öğesine tıklayın. Ardından, tabloda belirtilen ayarları kullanın, **Panoya sabitle**’yi işaretleyin ve **Oluştur**’a tıklayın.
 
4. Ardından, `TweetSentiment` gibi bir **Ad** girin, tabloda belirtilen ayarları kullanın, koşulları kabul edin ve **Panoya sabitle**’yi işaretleyin.

    ![Azure portalda mantıksal uygulama oluşturma](./media/functions-twitter-email/new_logic_app.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | ----------------- | ------------ | ------------- |
    | **Ad** | TweetSentiment | Uygulamanız için uygun bir ad seçin. |
    | **Kaynak grubu** | myResourceGroup | Daha önceki ile aynı mevcut kaynak grubunu seçin. |
    | **Konum** | Doğu ABD | Size yakın bir konum seçin. |    

4. **Panoya sabitle**’yi seçin ve sonra **Oluştur**’a tıklayarak mantıksal uygulamanızı oluşturun. 

5. Uygulama oluşturulduktan sonra, panoya sabitlenmiş yeni mantıksal uygulamanıza tıklayın. Ardından Logic Apps Tasarımcısı'nda sayfayı aşağı kaydırıp **Boş Mantıksal Uygulama** şablonuna tıklayın. 

    ![Boş Mantıksal Uygulama şablonu](media/functions-twitter-email/blank.png)

Artık Logic Apps Tasarımcısı'nı kullanarak uygulamanıza hizmetler ve tetikleyiciler ekleyebilirsiniz.

## <a name="connect-to-twitter"></a>Twitter’a Bağlanma

İlk olarak Twitter hesabınızla bir bağlantı oluşturun. Mantıksal uygulama tweetleri yoklar ve uygulamanın çalışması tetiklenir.

1. Tasarımcıda **Twitter** hizmetine ve **Yeni bir tweet gönderildiğinde** tetikleyicisine tıklayın. Twitter hesabınızda oturum açın ve Logic Apps’in hesabınızı kullanmasına izin verin.

2. Tabloda belirtilen Twitter tetikleyici ayarlarını kullanın. 

    ![Twitter bağlayıcı ayarları](media/functions-twitter-email/azure_tweet.png)

    | Ayar      |  Önerilen değer   | Açıklama                                        |
    | ----------------- | ------------ | ------------- |
    | **Arama metni** | #Azure | Seçilen aralıkta yeni tweetler oluşturmak için yeterince popüler olan bir diyez etiketi kullanın. Ücretsiz katmanını kullanırken diyez etiketiniz çok popüler olursa, Bilişsel Hizmetler API'nizdeki işlem kotasını hızlı bir şekilde tüketebilirsiniz. |
    | **Sıklık** | Dakika | Twitter’ı yoklamak için kullanılan sıklık birimi.  |
    | **Aralık** | 15 | Sıklık birimleri cinsinden Twitter istekleri arasında geçen süre. |

3.  Twitter hesabınıza bağlanmak için **Kaydet**’e tıklayın. 

Uygulamanız artık Twitter’a bağlıdır. Ardından, toplanan tweetlerin duyarlılığını algılamak için metin analizine bağlanın.

## <a name="add-sentiment-detection"></a>Duyarlılık algılama ekleme

1. **Yeni Adım**’a ve sonra **Eylem ekle**’ye tıklayın.

    ![Yeni Adım ve sonra Eylem ekle](media/functions-twitter-email/new_step.png)

2. **Eylem seçin** menüsünde **Metin Analizi**’ne ve sonra **Duyarlılığı algıla** eylemine tıklayın.

    ![Duyarlılığı Algıla](media/functions-twitter-email/detect_sent.png)

3. `MyCognitiveServicesConnection` gibi bir bağlantı adı yazın, Bilişsel Hizmetleri API’nizin kaydettiğiniz anahtarını yapıştırın ve **Oluştur**’a tıklayın.  

4. **Analiz edilecek metin** > **Tweet metni**’ne ve sonra **Kaydet**’e tıklayın.  

    ![Duyarlılığı Algıla](media/functions-twitter-email/ds_tta.png)

Duyarlılık algılaması yapılandırıldıktan sonra işlevinize duyarlılık puanı çıktısını kullanan bir bağlantı ekleyebilirsiniz.

## <a name="connect-sentiment-output-to-your-function"></a>Duyarlılık çıktısını işlevinize bağlama

1. Logic Apps Tasarımcısı'nda **Yeni adım** > **Eylem ekle** ve ardından **Azure İşlevleri**’ne tıklayın. 

2. **Bir Azure işlevi seçin**’e tıklayın ve daha önce oluşturduğunuz **CategorizeSentiment** işlevini seçin.  

    ![Bir Azure işlevi seçin’i gösteren Azure İşlevi kutusu](media/functions-twitter-email/choose_fun.png)

3. **İstek Gövdesi**’nde **Puan**’a ve ardından **Kaydet**’e tıklayın.

    ![Puan](media/functions-twitter-email/trigger_score.png)

Artık mantıksal uygulamadan bir duyarlılık puanı gönderildiğinde işleviniz tetiklenir. İşlev tarafından mantıksal uygulamaya renk kodlu bir kategori döndürülür. Daha sonra, işlevden **RED** duyarlılık değeri döndürüldüğünde gönderilen bir e-posta bildirimi ekleyin. 

## <a name="add-email-notifications"></a>E-posta bildirimleri ekleme

İş akışının son parçası, duyarlılık puanı _RED_ olduğunda bir e-postanın tetiklenmesidir. Bu konuda bir Outlook.com bağlayıcısı kullanılır. Gmail veya Office 365 Outlook bağlayıcısını kullanmak için benzer adımlar gerçekleştirebilirsiniz.   

1. Logic Apps Tasarımcısı'nda **Yeni adım** > **Koşul ekle**’ye tıklayın. 

2. **Bir değer seçin** ve ardından **Gövde**’ye tıklayın. **Eşittir**’e tıklayın, **Bir değer seçin**’e tıklayın ve `RED` yazıp **Kaydet**’e tıklayın. 

    ![Mantıksal uygulamaya koşul ekleyin.](media/functions-twitter-email/condition.png)

3. **IF TRUE** alanında **Eylem ekle**’ye tıklayın, `outlook.com` ifadesini arayın, **E-posta gönder**’e tıklayın ve Outlook.com hesabınızda oturum açın.
    
    ![Koşul için bir eylem seçin.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > Bir Outlook.com hesabınız yoksa Gmail veya Office 365 Outlook gibi başka bir bağlayıcı seçebilirsiniz

4. **E-posta gönder** eyleminde tabloda belirtilen e-posta ayarlarını kullanın. 

    ![E-posta gönderme eylemi için e-postayı yapılandırın.](media/functions-twitter-email/send_email.png)

    | Ayar      |  Önerilen değer   | Açıklama  |
    | ----------------- | ------------ | ------------- |
    | **Alıcı** | E-posta adresinizi girin | Bildirimi alan e-posta adresi. |
    | **Konu** | Olumsuz tweet duyarlılığı algılandı  | E-posta bildiriminin konu satırı.  |
    | **Gövde** | Tweet metni, Konum | **Tweet metni** ve **Konum** parametrelerine tıklayın. |

5.  **Kaydet**’e tıklayın.

İş akışı tamamlandığına göre mantıksal uygulamayı etkinleştirip işlevin nasıl çalıştığını görebilirsiniz.

## <a name="test-the-workflow"></a>İş akışını test etme

1. Logic Apps Tasarımcısı'nda, uygulamayı başlatmak için **Çalıştır**'a tıklayın.

2. Sol sütunda **Genel Bakış**’a tıklayarak mantıksal uygulamanın durumuna bakın. 
 
    ![Mantıksal uygulama yürütme durumu](media/functions-twitter-email/over1.png)

3. (İsteğe bağlı) Yürütme ayrıntılarını görmek için çalışmalardan birine tıklayın.

4. İşlevinize gidin, günlükleri görüntüleyin ve duyarlılık değerlerinin alınıp işlendiğini doğrulayın.
 
    ![İşlev günlüklerini görüntüleme](media/functions-twitter-email/sent.png)

5. Olumsuz olabilecek bir duyarlılık algılandığında bir e-posta alırsınız. Bir e-posta almadıysanız işlev kodunu her zaman RED döndürecek şekilde değiştirebilirsiniz:

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    E-posta bildirimlerini doğruladıktan sonra özgün koda geri dönün:

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > Bu öğreticiyi tamamladıktan sonra mantıksal uygulamayı devre dışı bırakmanız gerekir. Uygulamayı devre dışı bırakarak yürütmeler için sizden ücret alınmasını ve Bilişsel Hizmetler API'nizdeki işlemlerin tükenmesini önlersiniz.

İşlevleri bir Logic Apps iş akışıyla tümleştirmek ne kadar kolay olduğunu gördünüz.

## <a name="disable-the-logic-app"></a>Mantıksal uygulamayı devre dışı bırakma

Mantıksal uygulamayı devre dışı bırakmak için **Genel Bakış** ve sonra ekranın üstündeki **Devre Dışı Bırak**’a tıklayın. Bu işlem uygulamayı silmeden mantıksal uygulamanın çalışmasını ve ücretlerin yansıtılmasını durdurur. 

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

