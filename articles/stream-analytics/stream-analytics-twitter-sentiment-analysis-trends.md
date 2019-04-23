---
title: Azure Stream Analytics ile gerçek zamanlı Twitter yaklaşım analizi
description: Bu makalede, Stream Analytics gerçek zamanlı Twitter yaklaşım analizi için kullanmayı açıklar. Olay oluşturma adım adım kılavuzdan verileri canlı bir Pano üzerinde.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/29/2017
ms.openlocfilehash: abb2a89f41340e8e2e26fa36cc20b790341618d0
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003709"
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Azure Stream analytics'te gerçek zamanlı Twitter yaklaşım analizi

> [!IMPORTANT] 
> Twitter uygulaması oluşturma aracılığıyla, artık [apps.twitter.com](https://apps.twitter.com/). Bu öğretici, yeni Twitter API'si içerecek şekilde güncelleştirilme sürecindedir bölümüdür.

Azure Event Hubs'a giren gerçek zamanlı Twitter olayları taşıyarak sosyal medya analizi için bir yaklaşım analizi çözümü oluşturmayı öğrenin. Sonra veriler ve ya da analiz etmek için bir Azure Stream Analytics sorgu depolamak için sonuçları daha sonra yazma veya panoyu kullanan kullanabilirsiniz ve [Power BI](https://powerbi.com/) gerçek zamanlı bilgiler sağlamak için.

Sosyal medya analizi araçları, kuruluşların popüler konularını anlamanıza yardımcı olur. Popüler Konular şunlardır: konuları ve sosyal medya yüksek hacimli gönderilerin sahip alışkanlıklarından. Olarak da bilinen yaklaşım analizi *fikrim araştırma*, sosyal medya analizi araçları, bir ürün, fikir ve benzeri alışkanlıklarından belirlemek için kullanır. 

Diyez etiketi abonelik modeli (diyez etiketlerini) belirli anahtar sözcükler için dinleme ve akışın yaklaşım analizi geliştirme sağladığından gerçek zamanlı Twitter eğilim analizi bir analiz aracı harika bir örnektir.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Senaryo: Gerçek zamanlı sosyal medya yaklaşım analizi

Haber medya Web sitesi olan bir şirket, kendi okuyucularına hemen ilgili site içeriğin bulunduğu tarafından rakiplere üzerinde bir avantaj elde içinde ilgilenmektedir. Şirket, sosyal medya analizi Twitter verilerini gerçek zamanlı duygu çözümlemesi yaparak okuyucularına ilgilendiren konular kullanır.

Gerçek zamanlı twitter popüler konularını belirlemek için şirketin tweet toplu ve yaklaşımı hakkında gerçek zamanlı analizler için önemli konular gerekir. Diğer bir deyişle, bu, sosyal medya akışı üzerinde tabanlı bir yaklaşım analizi analiz altyapısı gerekli değildir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide, Twitter'a bağlanır ve (hangi ayarlayabilirsiniz) belirli bir diyez etiketlerini içeren tweetler için görünür bir istemci uygulaması kullanın. Uygulamayı çalıştırın ve Azure akış analizi kullanarak tweet'leri çözümleme için aşağıdakilere sahip olmanız gerekir:

* Bir Azure aboneliği
* Bir Twitter hesabı 
* Bir Twitter uygulaması ve [OAuth erişim belirteci](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) o uygulama için. Daha sonra bir Twitter uygulaması oluşturma hakkında üst düzey yönergeler sağlarız.
* TwitterWPFClient uygulama Twitter akışı okur. Bu uygulama edinmek için indirme [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) dosyasını Github'dan ve bilgisayarınızdaki bir klasöre, sonra paketin sıkıştırmasını açın. Kaynak kodu ve uygulamanın bir hata ayıklayıcıda çalışmasını görmek istiyorsanız, kaynak kodu alabilirsiniz [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TwitterClient). 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a>Streaming Analytics girişi için bir olay hub'ı oluşturma

Örnek uygulama olayları oluşturur ve bunları Azure olay hub'ına gönderir. Azure event hubs'a olay alımı Stream Analytics için tercih edilen yöntemdir. Daha fazla bilgi için [Azure Event Hubs belgeleri](../event-hubs/event-hubs-what-is-event-hubs.md).


### <a name="create-an-event-hub-namespace-and-event-hub"></a>Bir olay hub'ı ad alanı ve olay hub'ı oluşturma
Bu yordam, önce bir olay hub'ı ad alanı oluşturun ve ardından bir olay hub'ı, ad alanına ekleyin. Olay hub'ı ad alanları, mantıksal olarak ilgili olay veri yolu örnekleri gruplandırmak için kullanılır. 

1. Azure portalında oturum açın ve tıklayın **kaynak Oluştur** > **nesnelerin interneti** > **olay hub'ı**. 

2. İçinde **ad alanı oluşturma** dikey penceresinde gibi bir ad alanı adı girin `<yourname>-socialtwitter-eh-ns`. Ad alanı için herhangi bir adı kullanabilirsiniz, ancak ad geçerli bir URL olmalıdır ve Azure genelinde benzersiz olmalıdır. 
    
3. Bir abonelik seçin, oluşturmak veya bir kaynak grubu seçin ve sonra tıklayın **Oluştur**. 

    ![Olay hub'ı ad alanı oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. Ad alanının dağıtımı tamamlandığında, olay hub'ı ad alanı, Azure kaynak listesinde bulun. 

5. Yeni ad alanına tıklayın ve ad alanı dikey penceresinde  **+ &nbsp;olay hub'ı**. 

    ![Yeni bir olay hub'ı oluşturmak için olay Hub'ı Ekle düğmesi](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. Yeni olay hub'ı ad `socialtwitter-eh`. Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, adı daha sonra ihtiyacınız olduğundan, not edin. Diğer seçenekleri için olay hub'ı ayarlamanız gerekmez.

    ![Yeni bir olay hub'ı oluşturmak için dikey penceresi](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. **Oluştur**’a tıklayın.


### <a name="grant-access-to-the-event-hub"></a>Olay hub'ına erişim verme

Olay hub'ı, bir işlem, bir olay hub'ına veri göndermeden önce uygun erişim veren bir ilke olması gerekir. Erişim ilkesi, yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1.  Olay ad alanı dikey penceresinde **Event Hubs** ve yeni olay hub'ınızın adını tıklatın.

2.  Olay hub'ı dikey penceresinde **paylaşılan erişim ilkeleri** ve ardından  **+ &nbsp;Ekle**.

    >[!NOTE]
    >Event hub, olay hub'ı ad çalıştığınızdan emin olun.

3.  Adlı bir ilke eklemeyi `socialtwitter-access` ve **talep**seçin **Yönet**.

    ![Yeni bir olay hub'ı erişim ilkesini oluşturmak için dikey penceresi](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  **Oluştur**’a tıklayın.

5.  İlke dağıtıldıktan sonra paylaşılan erişim ilkeleri listesinde tıklayın.

6.  Etiketli kutunun seçili olduğunu bulmak **bağlantı DİZESİ-birincil anahtar** ve bağlantı dizesini yanındaki Kopyala düğmesine tıklayın. 
    
    ![Birincil bağlantı dizesi anahtarı erişim ilkesinden kopyalama](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  Bağlantı dizesini bir metin düzenleyicisine yapıştırın. Bazı küçük düzenlemeler yaptıktan sonra bir sonraki bölüm için bu bağlantı dizesine ihtiyacınız vardır.

    Bağlantı dizesi şöyle görünür:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    Bağlantı dizesinin noktalı virgülle ayırarak, birden çok anahtar-değer çifti içerdiğine dikkat edin: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, ve `EntityPath`.  

    > [!NOTE]
    > Güvenlik için örnek bağlantı dizesinde bölümleri kaldırıldı.

8.  Metin düzenleyicide, kaldırma `EntityPath` bağlantı dizesinden çifti (kendisinden noktalı virgülü kaldırmayı unutmayın). İşiniz bittiğinde, bağlantı dizesi şöyle görünür:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-the-twitter-client-application"></a>Yapılandırma ve Twitter istemci uygulamasını başlatın
İstemci uygulaması tweet olayları doğrudan Twitter'dan alır. Bunu yapmak için akış Twitter API'lerini çağırma izni gerekir. Bu izni yapılandırmak için benzersiz kimlik bilgilerini (örneğin, bir OAuth belirteci) oluşturur, twitter bir uygulama oluşturun. API çağrısı yaptığında bu kimlik bilgilerini kullanan istemci uygulaması daha sonra yapılandırabilirsiniz. 

### <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma
Bu öğretici için kullanabileceğiniz bir Twitter uygulaması zaten yoksa, bir oluşturabilirsiniz. Zaten bir Twitter hesabı olması gerekir.

> [!NOTE]
> Bir uygulama oluşturup anahtarlara, parolalara ve belirteci almak için twitter tam işlem değişebilir. Bu yönergeler Twitter sitesinde bkz eşleşmiyorsa, Twitter Geliştirici belgelerine bakın.

1. Git [Twitter Uygulama Yönetimi sayfası](https://apps.twitter.com/). 

2. Yeni bir uygulama oluşturun. 

   * Web sitesi URL'si için geçerli bir URL belirtin. Canlı site yok. (Yalnızca belirtemezsiniz `localhost`.)
   * Geri çağırma alanı boş bırakın. Bu öğretici için kullandığınız istemci uygulamasına geri çağırmaları gerektirmez.

     ![Twitter'da bir uygulama oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. İsteğe bağlı olarak, uygulamanın izinlerini salt okunur olarak değiştirin.

4. Uygulama oluşturulduğunda, Git **anahtarlar ve erişim belirteçleri** sayfası.

5. Bir erişim belirteci ve erişim belirteci gizli anahtarı oluşturmak için düğmeye tıklayın.

Sonraki yordamda ihtiyacınız olacağı için bu bilgileri elinizde tutun.

>[!NOTE]
>Twitter uygulaması için gizli dizileri ve anahtarları Twitter hesabınıza erişim sağlayın. Bu bilgiler, hassas olarak, aynı Twitter parolanızı olarak kabul eder. Örneğin, bu bilgileri başkalarının size bir uygulamada eklemeyin. 


### <a name="configure-the-client-application"></a>İstemci uygulaması yapılandırma
Twitter verilerini kullanarak bağlanan bir istemci uygulaması oluşturduk [Twitter'ın akış API'leri](https://dev.twitter.com/streaming/overview) belirli birtakım konular hakkında tweet olayları toplamak için. Uygulamanın kullandığı [Sentiment140](http://help.sentiment140.com/) aşağıdaki duyarlılık değeri atar her tweet için açık kaynak aracı:

* 0 = negatif
* 2 = nötr
* 4 pozitif =

Tweet olayları duyarlılık değeri atandıktan sonra bunlar daha önce oluşturduğunuz olay hub'ına gönderilir.

Uygulamayı çalıştırmadan önce Twitter anahtarları ve olay hub'ı bağlantı dizesi gibi belirli bilgileri gerektirir. Yapılandırma bilgilerini bu şekilde sağlayabilirsiniz:

* Uygulamayı çalıştırın ve ardından uygulamanın kullanıcı Arabiriminde anahtarlara, parolalara ve bağlantı dizesini girin. Bunu yaparsanız geçerli oturumunuz için yapılandırma bilgileri kullanılır, ancak bunu kaydedilmez.
* Uygulamanın .config dosyasını düzenleyin ve yok değerlerini ayarlayın. Bu yaklaşım, yapılandırma bilgilerini devam ediyorsa, ancak bu olası duyarlı bilgileri düz metin bilgisayarınızda depolanır ayrıca anlamına gelir.

Aşağıdaki yordam, her iki yaklaşım belgeler. 

1. İndirilen ve sıkıştırması açılan emin [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) önkoşullarda listelenen uygulama.

2. Çalışma zamanında (ve değerleri yalnızca geçerli oturum için), çalışma ayarlanacak `TwitterWPFClient.exe` uygulama. Uygulama istediğinde, aşağıdaki değerleri girin:

    * Twitter tüketici anahtarı (API anahtarı).
    * Twitter tüketici gizli (API gizli anahtarı).
    * Twitter erişim belirteci.
    * Twitter erişim belirteci gizli anahtarı.
    * Daha önce kaydettiğiniz bağlantı dizesi bilgilerini. Kaldırdığınız bağlantı dizesini kullandığınızdan emin olun `EntityPath` dan anahtar-değer çifti.
    * Yaklaşım için belirlemek istediğiniz Twitter anahtar sözcükler.

   ![Çalışan, örtülü ayarlarını gösteren TwitterWpfClient uygulama](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. Değerleri kalıcı olarak ayarlamak için TwitterWpfClient.exe.config dosyayı açmak için bir metin düzenleyicisi kullanın. Ardından `<appSettings>` öğesi, bunu:

   * Ayarlama `oauth_consumer_key` Twitter tüketici anahtarı (API anahtarı). 
   * Ayarlama `oauth_consumer_secret` için Twitter tüketici gizli (API gizli anahtarı).
   * Ayarlama `oauth_token` için Twitter erişim belirteci.
   * Ayarlama `oauth_token_secret` için Twitter erişim belirteci gizli anahtarı.

     Daha sonra `<appSettings>` öğe, şu değişiklikleri yapın:

   * Ayarlama `EventHubName` olay hub'ı adı için (diğer bir deyişle, varlık yolu değerine).
   * Ayarlama `EventHubNameConnectionString` bağlantı dizesi. Kaldırdığınız bağlantı dizesini kullandığınızdan emin olun `EntityPath` dan anahtar-değer çifti.

     `<appSettings>` Bölümü aşağıdaki örnekteki gibi görünür. (Netlik ve güvenlik için biz bazı satırları sarmalanmış ve bazı karakterler kaldırıldı.)

     ![Twitter anahtarlar ve gizli dizileri ve olay hub'ı bağlantı dizesi bilgilerini gösteren bir metin düzenleyicisinde TwitterWpfClient uygulama yapılandırma dosyası](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. Uygulama zaten başlatmadıysanız, TwitterWpfClient.exe Şimdi Çalıştır. 

5. Sosyal yaklaşım toplamak için Yeşil Başlat düğmesine tıklayın. Tweet olaylarla gördüğünüz **CreatedAt**, **konu**, ve **Duygupuanı** olay hub'ına gönderilen değerler.

    ![Tweetlerin listesini gösteren TwitterWpfClient uygulaması çalışıyor](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    >Görüyorsunuz ve pencerenin alt kısmında görüntülenen tweet'lerin akışını görmüyorsanız, anahtarları ve gizli anahtarları denetleyin. Ayrıca bağlantı dizesini kontrol edin (değil eklediğinizden emin olun `EntityPath` anahtar ve değer.)


## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

Tweet olayları Twitter gerçek zamanlı akış, bu olaylar gerçek zamanlı olarak analiz etmek için bir Stream Analytics işi ayarlayabilirsiniz.

1. Azure portalında **kaynak Oluştur** > **nesnelerin interneti** > **Stream Analytics işi**.

2. İş adı `socialtwitter-sa-job` ve bir abonelik, kaynak grubunu ve konumu belirtin.

    İşin ve olay hub'ı en iyi performans için aynı bölgede yerleştirmek için iyi bir uygulamadır ve böylece bölgeler arasında veri aktarmak ödeme yapmayın.

    ![Yeni bir Stream Analytics işi oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. **Oluştur**’a tıklayın.

    Bir iş oluşturulur ve portal iş ayrıntılarını görüntüler.


## <a name="specify-the-job-input"></a>İş girdisi belirtin

1. Stream Analytics işinizi içinde altında **iş topolojisi** işi dikey ortasında **girişleri**. 

2. İçinde **girişleri** dikey penceresinde tıklayın  **+ &nbsp;Ekle** ve sonra dikey penceresini aşağıdaki değerlerle doldurun:

   * **Giriş diğer adı**: Adı `TwitterStream`. Farklı bir ad kullanırsanız, daha sonra ihtiyacınız olduğundan bunu not edin.
   * **Kaynak türü**: Seçin **veri akışı**.
   * **Kaynak**: Seçin **olay hub'ı**.
   * **İçeri aktarma seçeneği**: Seçin **kullanımı olay hub'ından geçerli abonelik**. 
   * **Service bus ad alanı**: Daha önce oluşturduğunuz olay hub'ı ad alanını seçin (`<yourname>-socialtwitter-eh-ns`).
   * **Olay hub'ı**: Daha önce oluşturduğunuz olay hub'ı seçin (`socialtwitter-eh`).
   * **Olay hub'ı ilke adı**: Daha önce oluşturduğunuz erişim ilkesi seçin (`socialtwitter-access`).

     ![Yeni giriş için akış analizi işi oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. **Oluştur**’a tıklayın.


## <a name="specify-the-job-query"></a>İş sorgusu belirtin

Stream Analytics, dönüştürmeleri tanımlayan bir basit, bildirim temelli bir sorgu modelini destekler. Dili hakkında daha fazla bilgi için bkz. [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Bu öğreticide, yazmak ve test Twitter veriler üzerinde çeşitli sorguları yardımcı olur.

Konular arasında bahsetmeleri sayısını karşılaştırmak için kullanabileceğiniz bir [atlayan pencere](https://msdn.microsoft.com/library/azure/dn835055.aspx) konuya göre her beş saniyede bahsetmeleri sayısı alınamıyor.

1. Kapat **girişleri** henüz yapmadıysanız bir dikey pencere.

2. İçinde **genel bakış** dikey penceresinde tıklayın **sorguyu Düzenle** üstüne yakın sorgu kutusunun sağında. Azure giriş ve iş için yapılandırılan çıkışları listeler ve çıktıyı gönderilen giriş akışını dönüştürmek olanak sağlayan bir sorgu oluşturmanızı sağlar.

3. TwitterWpfClient uygulamasının çalıştığından emin olun. 

3. İçinde **sorgu** dikey penceresinde noktaya tıklayın `TwitterStream` seçin ve sonra **örnek giriş verileri**.

    ![Örnek veri akış analizi işi girişi, "Seçili girişten alınan örnek veriler" ile kullanmak üzere menü seçenekleri](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    Bu, giriş akışını okumak için ne kadar süre bakımından tanımlanmış Al örnek veri miktarını belirtmenize olanak tanıyan bir dikey pencere açılır.

4. Ayarlama **dakika** 3 ve ardından **Tamam**. 
    
    !["3 dakika seçili" Giriş akışı örnekleme seçenekleri.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    Azure 3 dakika değerinde giriş akışından veri örnekleri ve örnek veriler hazır olduğunda size bildirir. (Bu biraz zaman alır.) 

    Örnek veriler geçici olarak depolanır ve sorgu penceresi açıkken kullanılabilir. Sorgu penceresini kapatırsanız örnek veriler atılır ve yeni bir örnek veri kümesini oluşturmak sahip. 

5. Kod Düzenleyicisi'nde sorguyu aşağıdaki gibi değiştirin:

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    Varsa kullanmadı `TwitterStream` giriş diğer adı, diğer adınız için alternatif `TwitterStream` sorgu.  

    Bu sorgu kullanan **TIMESTAMP BY** zamana bağlı olan hesaplamayı kullanılacak yükünde bir zaman damgası alanı belirtmek için anahtar sözcüğü. Bu alan belirtilmezse, Pencereleme işlemi her olay Olay hub'ı gelen saati kullanılarak gerçekleştirilir. "Geliş saati vs uygulama süresi" bölümünde daha fazla bilgi edinin [Stream Analytics sorgu başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Ayrıca bu sorgu kullanarak her penceresinin bitiş zaman damgası erişir **System.Timestamp** özelliği.

5. Tıklayın **Test**. Sorgu, örneklenen verileri karşı çalışır.
    
6. **Kaydet**’e tıklayın. Bu sorgu akış analizi işinin bir parçası olarak kaydeder. (Örnek veri kaydetmez.)


## <a name="experiment-using-different-fields-from-the-stream"></a>Akıştan farklı alanlara kullanarak denemeler yapın 

Aşağıdaki tabloda, akış verileri JSON parçası olan alanları listeler. Sorgu Düzenleyicisi'nde deneme çekinmeyin.

|JSON özelliği | Tanım|
|--- | ---|
|createdAt | Tweet oluşturulduğu zaman|
|Konu | Belirtilen anahtar sözcükle eşleşen konu|
|Duygupuanı | Yaklaşım puanını Sentiment140 gelen|
|Yazma | Tweet ile gönderilen bir Twitter tanıtıcısı|
|Text | Tweet tam gövdesi|


## <a name="create-an-output-sink"></a>Çıkış havuzu oluşturma

Artık, bir olay hub'ı giriş olayları ve akış üzerinden bir dönüştürme gerçekleştirmek için bir sorgu alma için bir olay akışı tanımladınız. Son adım, işin bir çıkış havuzu tanımlamaktır.  

Bu öğreticide, toplanan tweet olayları Azure Blob depolama alanına işi sorgudan yazın.  Sonuçlarınızı Azure SQL veritabanı, Azure tablo depolama, olay hub'ları gönderebilirsiniz veya uygulamanızı bağlı olarak Power BI gerekiyor.

## <a name="specify-the-job-output"></a>İş çıktısını belirtin

1. İçinde **iş topolojisi** bölümünde **çıkış** kutusu. 

2. İçinde **çıkışları** dikey penceresinde tıklayın  **+ &nbsp;Ekle** ve sonra dikey penceresini aşağıdaki değerlerle doldurun:

   * **Çıkış diğer adı**: Adı `TwitterStream-Output`. 
   * **Havuz**: **Blob depolama**'yı seçin.
   * **İçeri aktarma seçenekleri**: Seçin **geçerli abonelik kullanım blob depolamadan**.
   * **Depolama hesabı**. Seçin **yeni bir depolama hesabı oluşturun.**
   * **Depolama hesabı** (ikinci kutusu). Girin `YOURNAMEsa`burada `YOURNAME` adınızı veya başka bir benzersiz bir dize. Ad yalnızca küçük harflerden ve rakamlardan kullanabilirsiniz ve Azure genelinde benzersiz olmalıdır. 
   * **Kapsayıcı**. `socialtwitter` yazın.
     Depolama hesabı adı ve kapsayıcı adı bir URI böyle bir blob depolama sağlamak için birlikte kullanılır: 

     `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
     ![Stream Analytics işi dikey penceresini "Yeni çıktı."](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. **Oluştur**’a tıklayın. 

    Azure depolama hesabı oluşturur ve bir anahtar otomatik olarak oluşturur. 

5. Kapat **çıkışları** dikey penceresi. 


## <a name="start-the-job"></a>İşi başlatma

İş girdisi, sorgu ve çıktı belirtilir. Stream Analytics işi başlatmak hazır olursunuz.

1. TwitterWpfClient uygulamasının çalıştığından emin olun. 

2. İşi dikey penceresinde **Başlat**.

    ![Stream Analytics işini başlatın](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. İçinde **başlangıç işi** dikey penceresinde için **iş çıkışı başlangıç zamanı**seçin **artık** ve ardından **Başlat**. 

    ![Stream Analytics işi dikey penceresini "işlemini Başlat"](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    Azure bildirir, işi başlatıldı ve iş dikey pencerede, durum olarak görüntülendiğinde **çalıştıran**.

    ![Çalışan iş](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Yaklaşım analizi için çıkış görünümü

İş çalışmaya başladı ve gerçek zamanlı Twitter akışı işleme sonra yaklaşım analizi için çıktıyı görüntüleyebilirsiniz.

Gibi bir araç kullanabilirsiniz [Azure Depolama Gezgini](https://storageexplorer.com/) veya [Azure Gezgini](https://www.cerebrata.com/products/azure-explorer/introduction) gerçek zamanlı olarak, işi çıktısını görüntülemek için. Burada kullandığınız [Power BI](https://powerbi.com/) uygulamanız aşağıdaki ekran görüntüsünde gösterildiği gibi özelleştirilmiş Pano içerecek şekilde genişletmek için:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-to-identify-trending-topics"></a>Popüler konularını belirlemek için başka bir sorgu oluşturma

Twitter yaklaşımı anlamak için kullanabileceğiniz başka bir sorguya dayalı bir [kayan pencere](https://msdn.microsoft.com/library/azure/dn835051.aspx). Popüler konularını belirlemek için bir eşik değeri için belirtilen bir sürede bahsetmelerini çapraz konuları arayın.

Bu öğreticinin amaçları doğrultusunda, 20 kereden fazla son 5 saniye içinde açıklanan konular için denetleyin.

1. İşi dikey penceresinde **Durdur** işi durdurulamadı. 

2. İçinde **iş topolojisi** bölümünde **sorgu** kutusu. 

3. Sorguyu şu şekilde değiştirin:

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. **Kaydet**’e tıklayın.

5. TwitterWpfClient uygulamasının çalıştığından emin olun. 

6. Tıklayın **Başlat** yeni bir sorgu kullanarak işi yeniden başlatmak için.


## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
