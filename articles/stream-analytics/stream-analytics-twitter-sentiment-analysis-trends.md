---
title: "Azure Stream Analytics ile gerçek zamanlı Twitter düşünceleri çözümleme | Microsoft Docs"
description: "Akış analizi için gerçek zamanlı Twitter düşünceleri analizi kullanmayı öğrenin. Olay oluşturma hakkında adım adım kılavuzdan verileri canlı bir Pano üzerinde."
keywords: "gerçek zamanlı twitter eğilim analizi, düşünceleri analiz, sosyal medya analizi, eğilim analizi örneği"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: samacha
ms.openlocfilehash: 96a169343481f1cdf43af82a7768cfe08cbd4886
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Azure Stream Analytics gerçek zamanlı Twitter düşünceleri analizi

Azure Event Hubs'a gerçek zamanlı Twitter olayları getirerek sosyal medya analizi için bir düşünceleri analiz çözümü oluşturmayı öğrenin. Veriler ve ya da analiz etmek için bir Azure akış analizi sorgu depolamak için sonuçları daha sonra yazma veya bir Pano kullanın sonra şunları yapabilirsiniz ve [Power BI](https://powerbi.com/) gerçek zamanlı Öngörüler sağlamak için.

Sosyal medya analiz araçları, kuruluşların oluşturan eğilim konularını anlamanıza yardımcı olur. Konular ve sosyal medya içinde postaları hacmi yüksek olan alışkanlıklarınızı bunun oluşturan eğilim konulardır. Olarak da adlandırılır düşünceleri analiz *fikir araştırma*, sosyal medya analiz araçları ürün, fikir ve benzeri doğru alışkanlıklarınızı belirlemek için kullanır. 

Diyez abonelik modeli belirli anahtar sözcükler (diyez etiketlerini) dinleme ve düşünceleri analiz akışın geliştirmek sağladığından gerçek zamanlı Twitter eğilim analizi bir analiz aracı harika bir örnektir.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Senaryo: Sosyal medya düşünceleri gerçek zamanlı analiz

Bir haber medya Web sitesi olan bir şirket rakiplere üzerinde bir avantaj kendi okuyucularına hemen ilgili site içeriğin bulunduğu tarafından sağlamasını ilgileniyor. Şirket Twitter veri analizini gerçek zamanlı düşünceleri yaparak okuyucularına ilgili konularda sosyal medya çözümlemesini kullanır.

Gerçek zamanlı Twitter'da oluşturan eğilim konularını belirlemek için şirket tweet birim ve düşünceleri hakkında gerçek zamanlı analiz için önemli konular gerekir. Diğer bir deyişle, gereken akışı bu sosyal medya üzerinde dayalı bir düşünceleri analiz analytics altyapısıdır.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide, Twitter hesabına bağlanır ve (hangi ayarlayabileceğiniz) belirli diyez etiketlerini sahip tweet'leri benzeyen bir istemci uygulaması kullanın. Uygulamayı çalıştırın ve Azure akış analizi kullanarak tweet'leri çözümlemek için aşağıdakilere sahip olmanız gerekir:

* Bir Azure aboneliği
* Bir Twitter hesabı 
* Bir Twitter uygulama ve [OAuth erişim belirtecinin](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) bu uygulama için. Daha sonra bir Twitter uygulaması oluşturmak için üst düzey yönergeler sağlıyoruz.
* Twitter akışı okur TwitterWPFClient uygulama. Bu uygulamayı almak için indirin [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) dosya Github'dan ve bilgisayarınızdaki bir klasöre paketin sıkıştırmasını. Kaynak kodu ve hata ayıklayıcısı'ndaki uygulamayı çalıştırın görmek istiyorsanız, kaynak kodu alabilirsiniz [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a>Bir event hub'akış analizi girişi için oluşturma

Örnek uygulama olayları oluşturur ve bunları Azure olay hub'ına iter. Azure event hubs'a olay alım akış analizi için tercih edilen yöntemdir. Daha fazla bilgi için bkz: [Azure Event Hubs belgelerine](../event-hubs/event-hubs-what-is-event-hubs.md).


### <a name="create-an-event-hub-namespace-and-event-hub"></a>Bir olay hub'ad alanı ve olay hub'ı Oluştur
Bu yordamda, önce bir olay hub'ad alanı oluşturun ve ardından bir event hub bu ad alanına ekleyin. Olay hub'ı ad alanları, ilgili olay bus örneklerini mantıksal olarak gruplamak için kullanılır. 

1. Azure portalında oturum açın ve tıklatın **kaynak oluşturma** > **nesnelerin interneti** > **olay hub'ı**. 

2. İçinde **ad alanı oluşturma** dikey penceresinde gibi bir ad alanı adı girin `<yourname>-socialtwitter-eh-ns`. Ad alanı için herhangi bir ad kullanabilirsiniz, ancak ad geçerli bir URL olmalıdır ve Azure arasında benzersiz olması gerekir. 
    
3. Bir aboneliği seçin ve oluşturmak veya bir kaynak grubu seçin ve ardından **oluşturma**. 

    ![Bir olay hub'ad alanı oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. Ad alanı dağıtmayı bitirdiğinde, olay hub'ad alanı, Azure kaynak listesinde bulun. 

5. Yeni ad alanına tıklayın ve ad alanı dikey penceresinde tıklayın  **+ &nbsp;olay hub'ı**. 

    ![Yeni bir olay hub'ı oluşturmak için olay Hub'ı Ekle düğmesi ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. Yeni olay hub'ı adı `socialtwitter-eh`. Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, adı daha sonra gerektiğinden, not edin. Olay hub'ı için diğer seçenekleri ayarlamanız gerekmez.

    ![Yeni bir olay hub'ı oluşturmak için dikey penceresi](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. **Oluştur**’a tıklayın.


### <a name="grant-access-to-the-event-hub"></a>Olay hub'ına erişim izni ver

Olay hub'ı, bir işlem olay hub'ına veri göndermeden önce uygun erişim veren bir ilke olması gerekir. Erişim İlkesi yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1.  Olay ad alanı dikey tıklayın **olay hub'ları** ve ardından yeni olay hub'ınızın adını tıklatın.

2.  Olay hub dikey penceresinde tıklayın **paylaşılan erişim ilkeleri** ve ardından  **+ &nbsp;Ekle**.

    >[!NOTE]
    >Olay hub, olay hub'ı ad çalıştığınız emin olun.

3.  Adlı ilke Ekle `socialtwitter-access` ve **talep**seçin **Yönet**.

    ![Yeni bir olay hub'ı erişim ilkesi oluşturmak için dikey penceresi](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  **Oluştur**’a tıklayın.

5.  İlke dağıtıldıktan sonra paylaşılan erişim ilkeleri listesinde tıklayın.

6.  Etiketli kutuyu bulun **bağlantı dize birincil anahtarı** ve bağlantı dizesini yanındaki Kopyala düğmesini tıklatın. 
    
    ![Erişim ilkesinden birincil bağlantı dizesi anahtarını kopyalama](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  Bağlantı dizesi, bir metin düzenleyicisine yapıştırın. Bazı küçük Düzenlemeleri yaptıktan sonra bir sonraki bölüm için bu bağlantı dizesi gerekir.

    Bağlantı dizesi şu şekildedir:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    Bağlantı dizesi noktalı virgülle ayırarak, birden çok anahtar-değer çiftleri içerdiğine dikkat edin: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, ve `EntityPath`.  

    > [!NOTE]
    > Güvenlik için örnekte bağlantı dizesi bölümleri kaldırıldı.

8.  Metin Düzenleyicisi'nde kaldırmak `EntityPath` bağlantı dizesinden çifti (yakalanması noktalı kaldırmayı unutmayın). İşiniz bittiğinde, bağlantı dizesi şu şekildedir:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-the-twitter-client-application"></a>Yapılandırma ve Twitter istemci uygulamasını başlatın
İstemci uygulaması tweet olayları doğrudan Twitter'dan alır. Bunu yapmak için Twitter akış API'leri çağırmak için izniniz gerekiyor. Bu izni yapılandırmak için benzersiz kimlik bilgilerini (örneğin, OAuth belirteci) oluşturur Twitter içinde bir uygulama oluşturun. API çağrıları yaptığında, bu kimlik bilgilerini kullanmak için istemci uygulaması sonra yapılandırabilirsiniz. 

### <a name="create-a-twitter-application"></a>Bir Twitter uygulaması oluşturma
Bu öğretici için kullanabileceğiniz bir Twitter uygulaması zaten yoksa bir tane oluşturabilirsiniz. Zaten bir Twitter hesabı olması gerekir.

> [!NOTE]
> Bir uygulama oluşturma ve anahtarları, gizli ve belirteç alma Twitter tam işleminde değişebilir. Bu yönergeler Twitter sitesinde gördükleri eşleşmiyorsa, Twitter Geliştirici belgelerine başvurun.

1. Git [Twitter Uygulama Yönetimi sayfasında](https://apps.twitter.com/). 

2. Yeni bir uygulama oluşturun. 

    * Web sitesi URL'si için geçerli bir URL belirtin. Canlı bir site olarak yok. (Yalnızca belirtemezsiniz `localhost`.)
    * Geri arama alanı boş bırakın. Bu öğretici için kullandığınız istemci uygulaması geri aramalar gerektirmez.

    ![Twitter içinde bir uygulama oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. İsteğe bağlı olarak, uygulamanın izinleri salt okunur olarak değiştirin.

4. Uygulama oluşturulduğunda, Git **anahtarları ve erişim belirteçleri** sayfası.

5. Bir erişim belirteci ve erişim belirteci gizli anahtarı oluşturmak için düğmeye tıklayın.

Sonraki yordamda gerektiğinden bu bilgiler faydalı, tutun.

>[!NOTE]
>Anahtarlar ve gizli Twitter uygulama için Twitter hesabınıza erişim sağlar. Bu bilgileri gizli olarak, aynı Twitter parolanızı bunu kabul eder. Örneğin, bu bilgileri uygulamada başkalarına size içine eklemeyin. 


### <a name="configure-the-client-application"></a>İstemci uygulaması yapılandırın
Twitter verilerini kullanarak bağlandığı bir istemci uygulaması oluşturduk [Twitter'ın akış API'leri](https://dev.twitter.com/streaming/overview) konuları belirli bir dizi hakkında tweet olaylarını toplamak için. Uygulamanın kullandığı [Sentiment140](http://help.sentiment140.com/) her tweet aşağıdaki düşünceleri değeri atar açık kaynak aracı:

* 0 negatif =
* 2 = neutral =
* 4 pozitif =

Tweet olayları düşünceleri değeri atandıktan sonra daha önce oluşturduğunuz olay hub'ına gönderilen.

Uygulamayı çalıştırmadan önce Twitter anahtarlarının ve olay hub bağlantı dizesine gibi belirli bilgileri gerektirir. Bu yolla yapılandırma bilgileri sağlayabilir:

* Uygulamayı çalıştırın ve anahtarları, gizli ve bağlantı dizesini girmek için uygulamanın kullanıcı arabirimini kullanın. Bunu yaparsanız, yapılandırma bilgilerini geçerli oturumunuz için kullanılır, ancak bunu kaydedilmez.
* Uygulamanın .config dosyasını düzenleyin ve değerleri var. ayarlayın. Bu yaklaşım yapılandırma bilgilerini devam ederse, ancak aynı zamanda bu olası hassas bilgiler düz metin, bilgisayarınızda depolanır gelir.

Aşağıdaki yordam, her iki yaklaşımın belgeler. 

1. Karşıdan yükleyip sıkıştırması açılmış olduğundan emin olun [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) önkoşullarda listelenen gibi uygulama.

2. Çalışma zamanında (ve değerleri yalnızca geçerli oturum için), çalışma ayarlamak için `TwitterWPFClient.exe` uygulama. Uygulama istediğinde, aşağıdaki değerleri girin:

    * Twitter tüketici anahtarı (API anahtarı).
    * Twitter tüketici gizli (API gizli).
    * Twitter erişim belirteci.
    * Twitter erişim belirteci gizli anahtarı.
    * Daha önce kaydettiğiniz bağlantı dizesi bilgilerini. Kaldırdığınız bağlantı dizesini kullandığınızdan emin olun `EntityPath` gelen anahtar-değer çifti.
    * İçin düşünceleri belirlemek istiyorsanız Twitter anahtar sözcükler.

   ![Çalıştıran, örtülü ayarları gösteren TwitterWpfClient uygulama](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. Değerleri kalıcı olarak ayarlamak için TwitterWpfClient.exe.config dosyasını açmak için bir metin düzenleyicisi kullanın. Ardından `<appSettings>` öğesi, bunu yapın:

    * Ayarlama `oauth_consumer_key` Twitter tüketici anahtarı (API anahtarı). 
    * Ayarlama `oauth_consumer_secret` Twitter tüketici gizli anahtarı (API gizli) için.
    * Ayarlama `oauth_token` Twitter erişim belirteci için.
    * Ayarlama `oauth_token_secret` için Twitter erişim belirteci gizli anahtarı.

    Daha sonra `<appSettings>` öğesi, bu değişiklikleri yapın:

    * Ayarlama `EventHubName` olay hub'ı adı için (diğer bir deyişle, varlık yolu değerine).
    * Ayarlama `EventHubNameConnectionString` bağlantı dizesi. Kaldırdığınız bağlantı dizesini kullandığınızdan emin olun `EntityPath` gelen anahtar-değer çifti.

    `<appSettings>` Bölümü aşağıdaki gibi görünür. (Daha anlaşılır olması ve güvenlik için biz bazı satırlar Sarmalanan ve bazı karakterler kaldırılmış.)

    ![Twitter anahtarları ve gizli anahtarları ve olay hub'ı bağlantı dizesi bilgilerini gösteren bir metin düzenleyicisinde TwitterWpfClient uygulama yapılandırma dosyası](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. Uygulama zaten başlatmadıysanız TwitterWpfClient.exe Şimdi Çalıştır. 

5. Sosyal düşünceleri toplamak için Yeşil Başlat düğmesine tıklayın. Tweet olaylara bakın **CreatedAt**, **konu**, ve **SentimentScore** event hub'ına gönderilen değerler.

    ![Tweet'leri listesini gösteren TwitterWpfClient uygulaması çalışıyor](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    >Hataları görmek ve pencerenin alt kısmında görüntülenen tweet'leri akışı görmüyorsanız, anahtarları ve gizli anahtarları denetleyin. Ayrıca bağlantı dizesini kontrol edin (değil eklediğinizden emin olun `EntityPath` anahtar ve değer.)


## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

Twitter gelen gerçek zamanlı akış tweet olaylarını, bu olayları gerçek zamanlı analiz etmek için bir Stream Analytics işi ayarlayabilirsiniz.

1. Azure portalında tıklatın **kaynak oluşturma** > **nesnelerin interneti** > **Stream Analytics işi**.

2. İş adı `socialtwitter-sa-job` ve abonelik, kaynak grubunu ve konumu belirtin.

    En iyi performans için aynı bölgede iş ve olay hub'ı yerleştirmek için iyi bir fikirdir ve böylece bölgeler arasında veri aktarmak ödemeniz gerekmez.

    ![Yeni bir Stream Analytics işi oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. **Oluştur**’a tıklayın.

    İş oluşturulur ve portal iş ayrıntılarını görüntüler.


## <a name="specify-the-job-input"></a>İş Girişi belirtin

1. Stream Analytics işinizde altında **iş topoloji** iş dikey ortasında tıklayın **girişleri**. 

2. İçinde **girişleri** dikey penceresinde tıklatın  **+ &nbsp;Ekle** ve ardından dikey penceresinde şu değerlerle doldurun:

    * **Giriş diğer adı**: adını kullanmak `TwitterStream`. Farklı bir ad kullanırsanız, daha sonra gerektiğinden, not edin.
    * **Kaynak türünü**: seçin **veri akışı**.
    * **Kaynak**: seçin **olay hub'ı**.
    * **Alma seçeneği**: seçin **geçerli aboneliğe ilişkin kullanım olay hub'ı**. 
    * **Hizmet veri yolu ad alanı**: daha önce oluşturduğunuz olay hub'ı ad alanını seçin (`<yourname>-socialtwitter-eh-ns`).
    * **Olay hub'ı**: daha önce oluşturduğunuz olay hub'ı seçin (`socialtwitter-eh`).
    * **Olay hub'ı ilke adı**: daha önce oluşturduğunuz erişim ilkesi seçin (`socialtwitter-access`).

    ![Akış analizi işi için yeni giriş oluşturma](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. **Oluştur**’a tıklayın.


## <a name="specify-the-job-query"></a>İş sorgusu belirtin

Akış analizi dönüşümleri açıklayan bir basit ve bildirim temelli sorgu modelini destekler. Dili hakkında daha fazla bilgi için bkz: [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Bu öğretici, yazar ve birkaç sorgu Twitter verileri üzerinde test yardımcı olur.

Konular arasında belirtilenlerden sayısı karşılaştırmak için kullanabileceğiniz bir [atlayan pencere](https://msdn.microsoft.com/library/azure/dn835055.aspx) beş saniyede konusuna göre belirtilenlerden sayısı alınamadı.

1. Kapat **girişleri** yapmadıysanız dikey.

2. İş dikey penceresinde tıklayın **sorgu** kutusu. Azure girişleri ve iş için yapılandırılmış olan çıkışları listeler ve çıktıyı gönderilmiş gibi giriş akışı dönüştürme olanak sağlayan bir sorgu oluşturmanızı sağlar.

3. TwitterWpfClient uygulama çalıştığından emin olun. 

3. İçinde **sorgu** dikey penceresinde nokta tıklayın `TwitterStream` girin ve ardından **örnek giriş verilerinden**.

    ![Akış analizi işi girişle "Seçili örnek verilerden Giriş" örnek verileri kullanılmak üzere menü seçenekleri](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    Bu giriş akışı okumayı açısından ne kadar süreyle tanımlanan Al ne kadar örnek verileri belirtmenize olanak sağlar. bir dikey pencere açılır.

4. Ayarlama **dakika** 3 ve ardından **Tamam**. 
    
    ![Giriş akışı "3 Seçili dakika ile" örnekleme seçenekleri.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    Azure Giriş akışı verilerden 3 dakika eşitleyeceğini örnekleri ve örnek verileri hazır olduğunda size bildirir. (Bu kısa biraz uzun sürebilir.) 

    Örnek verileri geçici olarak depolanır ve açık sorgu penceresi açıkken kullanılabilir. Sorgu penceresini kapattığınızda, örnek veriler atılır ve yeni bir örnek veri kümesi oluşturmanız gerekir. 

5. Kod düzenleyicisinde sorgu aşağıdaki gibi değiştirin:

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    Varsa kullanmadı `TwitterStream` diğer ad olarak giriş için diğer adınız yerine `TwitterStream` sorgu.  

    Bu sorgu kullanan **TIMESTAMP BY** zamana bağlı hesaplamada kullanılacak yükte bir zaman damgası alanı belirtmek için anahtar sözcüğü. Bu alan belirtilmezse, Pencereleme işlemi her olayın olay hub'ına gelen saati kullanılarak gerçekleştirilir. "Geliş saati vs uygulama zamanı" bölümünde daha fazla bilgi edinin [Stream Analytics sorgu başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Ayrıca bu sorguyu kullanarak bir zaman damgası her penceresinin End erişen **System.Timestamp** özelliği.

5. Tıklatın **Test**. Sorgu, örneklenen verileri karşı çalışır.
    
6. **Kaydet**’e tıklayın. Bu sorguyu akış analizi işi bir parçası olarak kaydeder. (Bu örnek verileri kaydetmez.)


## <a name="experiment-using-different-fields-from-the-stream"></a>Deneme akıştan farklı alanları kullanma 

Aşağıdaki tablo veri akışı JSON parçası olan alanları listeler. Sorgu Düzenleyicisi'nde denemeler çekinmeyin.

|JSON özelliği | Tanım|
|--- | ---|
|CreatedAt | Tweet oluşturulduğu zaman|
|Konu | Belirtilen anahtar sözcük eşleşmeleri konu|
|SentimentScore | Sentiment140 düşünceleri puan|
|Yazar | Tweet gönderilen Twitter tanıtıcısı|
|Metin | Tweet tam gövdesi|


## <a name="create-an-output-sink"></a>Çıkış havuzu oluşturma

Bir olay akışında olayları ve akış üzerinden bir dönüştürme gerçekleştirmek için bir sorgu alma için giriş olay hub'ı şimdi tanımladınız. Son adım, iş için çıkış havuzu tanımlamaktır.  

Bu öğreticide, toplanan tweet olayları Azure Blob depolama alanına sorgu iş yazma.  Sonuçlarınızı Azure SQL veritabanı için Azure Table storage, olay hub'ları gönderebilir veya Power BI bağlı olarak uygulamanız gerekir.

## <a name="specify-the-job-output"></a>İş çıktısı belirtin

1. İçinde **iş topoloji** 'yi tıklatın **çıkış** kutusu. 

2. İçinde **çıkışları** dikey penceresinde tıklatın  **+ &nbsp;Ekle** ve ardından dikey penceresinde şu değerlerle doldurun:

    * **Çıkış diğer adları**: adını kullanmak `TwitterStream-Output`. 
    * **Havuz**: seçin **Blob storage**.
    * **İçe aktarma seçenekleri**: seçin **blob storage'ı geçerli aboneliğe ilişkin kullanma**.
    * **Depolama hesabı**. Seçin **yeni bir depolama hesabı oluşturun.**
    * **Depolama hesabı** (ikinci kutusu). Girin `YOURNAMEsa`, burada `YOURNAME` adınızı veya başka bir benzersiz bir dize. Ad yalnızca küçük harfler ve sayılar kullanabilirsiniz ve Azure arasında benzersiz olması gerekir. 
    * **Kapsayıcı**. Girin `socialtwitter`.
    Depolama hesabı adı ve kapsayıcı adı bir URI şöyle blob depolama sağlamak için birlikte kullanılır: 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    ![Stream Analytics işi için "Yeni çıkış" dikey](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. **Oluştur**’a tıklayın. 

    Azure depolama hesabı oluşturur ve bir anahtarı otomatik olarak oluşturur. 

5. Kapat **çıkışları** dikey. 


## <a name="start-the-job"></a>İşi Başlat

İş Girişi, sorgu ve çıktı belirtilir. Stream Analytics işini başlatmak hazır olursunuz.

1. TwitterWpfClient uygulama çalıştığından emin olun. 

2. İş dikey penceresinde tıklayın **Başlat**.

    ![Stream Analytics işi Başlat](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. İçinde **başlangıç işi** dikey penceresinde için **iş çıktısı başlangıç zamanı**seçin **şimdi** ve ardından **Başlat**. 

    ![Stream Analytics işi dikey penceresinde "işlemini Başlat"](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    Azure bildirir, size, iş başlatıldı ve iş dikey penceresinde, durum olarak görüntülendiğinde **çalıştıran**.

    ![İşi çalıştırma](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Görünüm çıktı düşünceleri analiz için

İşinizi çalışmaya başladıktan ve gerçek zamanlı Twitter akışı işleme sonra düşünceleri analiz için çıktıyı görüntüleyebilirsiniz.

Gibi bir araç kullanabilirsiniz [Azure Storage Gezgini](https://http://storageexplorer.com/) veya [Azure Gezgini](http://www.cerebrata.com/products/azure-explorer/introduction) gerçek zamanlı olarak, iş çıkışı görüntülemek için. Buradan, kullandığınız [Power BI](https://powerbi.com/) aşağıdaki ekran görüntüsünde gösterildiği gibi özelleştirilmiş bir pano eklemek için uygulamanızın genişletmek için:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-to-identify-trending-topics"></a>Oluşturan eğilim konularını belirlemek için başka bir sorgu oluşturun

Twitter düşünceleri anlamak için kullanabileceğiniz başka bir sorguya dayalı bir [kayan pencere](https://msdn.microsoft.com/library/azure/dn835051.aspx). Oluşturan eğilim konularını belirlemek için bir eşik değeri için belirtilen sürede belirtilenlerden arası konuları arayın.

Bu öğreticinin amaçları doğrultusunda, 20 katından fazla son 5 saniye içinde açıklanan konular için denetleyin.

1. İş dikey penceresinde tıklayın **durdurmak** işi durdurulamıyor. 

2. İçinde **iş topoloji** 'yi tıklatın **sorgu** kutusu. 

3. Sorgu aşağıdaki gibi değiştirin:

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. **Kaydet**’e tıklayın.

5. TwitterWpfClient uygulama çalıştığından emin olun. 

6. Tıklatın **Başlat** yeni sorgu kullanarak işi yeniden başlatmak için.


## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
