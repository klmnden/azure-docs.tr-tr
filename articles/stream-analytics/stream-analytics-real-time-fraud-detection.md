---
title: Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama
description: Stream Analytics ile gerçek zamanlı sahtekarlık algılama çözümü oluşturmayı öğrenin. Gerçek zamanlı Olay işleme için bir olay hub'ı kullanın.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: a13d3b24cd7845de144183d9f2ea825e0e24219f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58883726"
---
# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Azure Stream Analytics'i kullanmaya başlama: Gerçek zamanlı sahtekarlık algılama

Bu öğreticide, Azure Stream Analytics'i kullanmak nasıl bir uçtan uca gösterim sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

* Getir olayları Azure Event Hubs'ın bir örneğine akış. Bu öğreticide, cep telefonu meta veri kayıtlarını akışı taklit eden bir uygulama kullanmanız gerekir.

* Bilgi toplama veya desenleri için bakarak verileri dönüştürmek için SQL benzeri Stream Analytics sorguları yazma. Gelen akışı incelemek ve olabilecek dolandırıcılık amaçlı çağrıları bulmak için bir sorgu kullanmayı görürsünüz.

* Sonuçları için ek Öngörüler analiz edebileceğiniz çıkış havuzu (Depolama) gönderin. Bu durumda, şüpheli araması verileri Azure Blob depolama alanına göndereceğiz.

Bu öğreticide, telefon araması verileri temel alan gerçek zamanlı sahtekarlık algılama örneği kullanılır. Tekniği sahtekarlık algılama, kredi kartı sahtekarlığı veya kimlik hırsızlığı gibi diğer türleri için de uygundur. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Senaryo: Telekomünikasyon ve SIM gerçek zamanlı sahtekarlık algılama

Telekomünikasyon şirketi, büyük miktarlarda veri gelen çağrıları için vardır. Şirket, müşterilere bildirmek veya belirli bir sayıya için hizmet kapatın, gerçek zamanlı olarak sahte çağrıları algılamak istiyor. SIM sahtekarlık bir tür aynı kimlik yaklaşık aynı zamanda ancak farklı coğrafi konumlarda bulunan birden fazla çağrı içerir. Bu tür bir sahtekarlık algılamaya gelen telefon kayıtlarını inceleyebilir ve belirli kalıpları aramak şirketin gerekir; bu durumda, farklı ülkelerde aynı zamanda yapılan çağrılar için. Bu kategoriye giren herhangi bir telefon kayıt sonraki analiz için depolama yazılır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, örnek telefon araması meta verileri üreten bir istemci uygulaması kullanarak telefon araması verileri benzetimini yapmak. Bazı uygulamayı hazırlayan kayıtlar dolandırıcılık amaçlı çağrıları gibi görünüyor. 

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Bir Azure hesabı.
* Araması olay Oluşturucu uygulamasının [TelcoGenerator.zip](https://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip), hangi indirilebilir Microsoft Download Center'dan gelen. Bu paket, bilgisayarınızdaki bir klasöre ayıklayın. Kaynak kodu ve uygulamayı bir hata ayıklayıcıda çalışmasını görmek istiyorsanız, uygulama kaynak kodu alabilirsiniz [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Windows, indirilen .zip dosyasını engelleyebilir. Sıkıştırılmış dosyayı olamaz, dosyaya sağ tıklayın ve seçin **özellikleri**. "Bu dosya başka bir bilgisayardan geldi ve bu bilgisayarın korunmasına yardımcı olmak için engellenmiş olabilir" iletisini görürseniz, seçin **Engellemeyi Kaldır** seçeneğini ve ardından **Uygula**.

Akış analizi işinin sonuçları incelemek isterseniz, bir Azure Blob Depolama kapsayıcısında içeriğini görüntülemek için ayrıca bir aracı gerekir. Visual Studio kullanıyorsanız, kullanabileceğiniz [Visual Studio için Azure Araçları](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) veya [Visual Studio Cloud Explorer](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer). Alternatif olarak, tek başına Araçlar gibi yükleyebilirsiniz [Azure Depolama Gezgini](https://storageexplorer.com/) veya [Cerulean](https://www.cerebrata.com/products/cerulean/features/azure-storage). 

## <a name="create-an-azure-event-hubs-to-ingest-events"></a>Bir Azure Event Hubs için olayları içe alma oluşturma

Bir veri akışını analiz etmek için *alma* Azure içine. Veri alımı için normal bir şekilde kullanmaktır [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), olanak tanıyan saniye başına milyonlarca olayı içe alacak ve ardından ve olay bilgileri depolar. Bu öğreticide, bir olay hub'ı oluşturma ve ardından arama verilerini bu olay hub'ına gönderme araması olay Oluşturucu uygulamasının sağlayabilirsiniz. Event hubs hakkında daha fazla bilgi için bkz. [Azure Service Bus belgeleri](https://docs.microsoft.com/azure/service-bus/).

>[!NOTE]
>Bu yordam daha ayrıntılı bir sürümü için bkz: [bir Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Ad alanı ve olay hub'ı oluşturma
Bu yordam, önce bir olay hub'ı ad alanı oluşturun ve ardından bir olay hub'ı, ad alanına ekleyin. Olay hub'ı ad alanları, mantıksal olarak ilgili olay veri yolu örnekleri gruplandırmak için kullanılır. 

1. Azure portalında oturum açın ve tıklayın **kaynak Oluştur** > **nesnelerin interneti** > **olay hub'ı**. 

2. İçinde **ad alanı oluşturma** bölmesi gibi bir ad alanı adı girin `<yourname>-eh-ns-demo`. Ad alanı için herhangi bir adı kullanabilirsiniz, ancak ad geçerli bir URL olmalıdır ve Azure genelinde benzersiz olmalıdır. 
    
3. Bir abonelik seçin, oluşturmak veya bir kaynak grubu seçin ve sonra tıklayın **Oluştur**.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png" alt="Create event hub namespace in Azure portal" width="300px"/>

4. Ad alanının dağıtımı tamamlandığında, olay hub'ı ad alanı, Azure kaynak listesinde bulun. 

5. Yeni ad alanına tıklayın ve ad alanı bölmesinden **olay hub'ı**.

   ![Yeni bir olay hub'ı oluşturmak için olay Hub'ı Ekle düğmesi](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Yeni olay hub'ı ad `asa-eh-frauddetection-demo`. Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, adı daha sonra ihtiyacınız olduğundan, not edin. Olay hub'ı için diğer seçenekleri şu anda ayarlamanız gerekmez.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png" alt="Name event hub in Azure portal" width="400px"/>
    
 
7. **Oluştur**’a tıklayın.

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Olay hub’ına erişim verme ve bir bağlantı dizesi alma

Olay hub'ı, bir işlem, bir olay hub'ına veri göndermeden önce uygun erişim veren bir ilke olması gerekir. Erişim ilkesi, yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1.  Olay ad alanı bölmesinde **Event Hubs** ve yeni olay hub'ınızın adını tıklatın.

2.  Olay hub'ı bölmesinden **paylaşılan erişim ilkeleri** ve ardından  **+ &nbsp;Ekle**.

    >[!NOTE]
    >Event hub, olay hub'ı ad çalıştığınızdan emin olun.

3.  Adlı bir ilke eklemeyi `sa-policy-manage-demo` ve **talep**seçin **Yönet**.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png" alt="Create shared access policy for Stream Analytics" width="300px"/>
 
4.  **Oluştur**’a tıklayın.

5.  İlke dağıtıldıktan sonra paylaşılan erişim ilkeleri listesinde tıklayın.

6.  Etiketli kutunun seçili olduğunu bulmak **bağlantı DİZESİ-birincil anahtar** ve bağlantı dizesini yanındaki Kopyala düğmesine tıklayın. 

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png" alt="Stream Analytics shared access policy" width="300px"/>
 
7.  Bağlantı dizesini bir metin düzenleyicisine yapıştırın. Bazı küçük düzenlemeler yaptıktan sonra bir sonraki bölüm için bu bağlantı dizesine ihtiyacınız vardır.

    Bağlantı dizesi şöyle görünür:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=asa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=asa-eh-frauddetection-demo

    Bağlantı dizesinin noktalı virgülle ayırarak, birden çok anahtar-değer çifti içerdiğine dikkat edin: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, ve `EntityPath`.  

## <a name="configure-and-start-the-event-generator-application"></a>Yapılandırma ve olay Oluşturucu uygulamasını başlatma

TelcoGenerator uygulamasını başlatmadan önce böylece araması kayıtları oluşturduğunuz olay hub'ına gönderir yapılandırmalısınız.

### <a name="configure-the-telcogenerator-app"></a>TelcoGenerator uygulamasını yapılandırma

1. Düzenleyiciye kopyaladığınız bağlantı dizesini Not `EntityPath` değeri ve Kaldır'ı `EntityPath` çifti (unutmayın kendisinden noktalı virgülü kaldırma). 

2. Burada TelcoGenerator.zip dosyasının sıkıştırması açılan klasöründe telcodatagen.exe.config dosyayı bir düzenleyicide açın. (Birden fazla .config dosyası varsa, bu nedenle doğru olanı açtığınızdan emin olun.)

3. İçinde `<appSettings>` öğesi:

   * Değerini `EventHubName` olay hub'ı adı anahtarını (diğer bir deyişle, varlık yolu değerine).
   * Değerini `Microsoft.ServiceBus.ConnectionString` anahtar bağlantı dizesi. 

   `<appSettings>` Bölümü, aşağıdaki örnekteki gibi görünür. (Netlik, satırları sarmalanır ve yetkilendirme belirteçten bazı karakterler kaldırıldı.)

   ![Olay hub'ı adı ve bağlantı dizesini TelcoGenerator yapılandırma dosyasını gösterir](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4. Dosyayı kaydedin. 

### <a name="start-the-app"></a>Uygulamayı başlatın
1.  Bir komut penceresi açın ve TelcoGenerator uygulamasının sıkıştırması olduğu klasöre gidin.
2.  Aşağıdaki komutu girin:

        ```cmd
        telcodatagen.exe 1000 0.2 2
        ```

    Parametreler şunlardır: 

    * Saat başına CDR sayısı. 
    * SIM kart sahtekarlık olasılığını: Ne sıklıkta yüzdesi uygulama sahte arama benzetimi gerçekleştirmesi gerektiği tüm çağrıları olarak. 0.2 değeri arama kayıtlarının %20'sinin sahte görüneceğini anlamına gelir.
    * Saat cinsinden süre. Uygulamanın çalışması gereken saat sayısı. Ayrıca, komut satırında Ctrl + C tuşlarına basarak uygulamayı dilediğiniz zaman durdurabilirsiniz.

    Birkaç saniye sonra uygulama, telefon araması kayıtlarını olay hub'ına gönderirken ekranda bu kayıtları görüntülemeye başlar.

Bu gerçek zamanlı sahtekarlık algılama uygulamada kullanarak anahtar alanları bazıları şunlardır:

|**Kayıt**|**Tanım**|
|----------|--------------|
|`CallrecTime`|Arama başlangıç zamanı için zaman damgası. |
|`SwitchNum`|Aramayı bağlamak için kullanılan telefon anahtarı. Bu örnekte, anahtarlar kaynak ülkeyi (ABD, Çin, İngiltere, Almanya veya Avustralya) temsil eden dizelerdir. |
|`CallingNum`|Arayanın telefon numarası. |
|`CallingIMSI`|Uluslararası Mobil Abone Kimliği (IMSI). Bu, arayanın benzersiz tanımlayıcısıdır. |
|`CalledNum`|Arama alıcısının telefon numarası. |
|`CalledIMSI`|Uluslararası Mobil Abone Kimliği (IMSI). Bu, arama alıcısının benzersiz tanımlayıcısıdır. |


## <a name="create-a-stream-analytics-job-to-manage-streaming-data"></a>Akış verilerini yönetmek için bir Stream Analytics işi oluşturma

Arama olaylarından oluşan bir akışa sahip olduğunuza göre bir Stream Analytics işi ayarlayabilirsiniz. İşlemini ayarladığınız olay hub'ından veri okur. 

### <a name="create-the-job"></a>İşi oluşturma 

1. Azure portalında **kaynak Oluştur** > **nesnelerin interneti** > **Stream Analytics işi**.

2. İş adı `asa_frauddetection_job_demo`, abonelik, kaynak grubunu ve konumu belirtin.

    İşin ve olay hub'ı en iyi performans için aynı bölgede yerleştirmek için iyi bir uygulamadır ve böylece bölgeler arasında veri aktarmak ödeme yapmayın.

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png" alt="Create Stream Analytics job in portal" width="300px"/>

3. **Oluştur**’a tıklayın.

    Bir iş oluşturulur ve portal iş ayrıntılarını görüntüler. Henüz hiçbir şey ancak çalışıyor; işi yeniden başlatılmadan önce yapılandırmanız gerekir.

### <a name="configure-job-input"></a>İş girişi yapılandırma

1. Panodaki veya **tüm kaynakları** bölmesinde bulun ve seçin `asa_frauddetection_job_demo` Stream Analytics işi. 
2. İçinde **genel bakış** bölümü Stream Analytics iş bölmesinin tıklayın **giriş** kutusu.

   ![Giriş kutusuna Streaming Analytics iş bölmesinin altında topolojisi](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Tıklayın **akış Girişi Ekle** seçip **olay hub'ı**. Ardından yeni giriş sayfasını aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı  |  CallStream   |  İşin girdisini tanımlamak için bir ad girin.   |
   |Abonelik   |  \<Aboneliğiniz\> |  Oluşturduğunuz olay Hub'ına Azure aboneliğini seçin.   |
   |Olay hub'ı ad alanı  |  asa-eh-ns-demo |  Olay hub'ı ad alanı adını girin.   |
   |Olay Hub'ı adı  | asa-eh-frauddetection-demo | Olay Hub'ınızın adını seçin.   |
   |Olay Hub'ı ilke adı  | asa-ilkeyi-yönetme-demo | Daha önce oluşturduğunuz erişim ilkesi seçin.   |

    </br>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png" alt="Create Stream Analytics input in portal" width="300px"/>


4. **Oluştur**’a tıklayın.

## <a name="create-queries-to-transform-real-time-data"></a>Gerçek zamanlı verileri dönüştürmek için sorgular oluşturma

Bu noktada, gelen veri akışını okumak için ayarlama bir Stream Analytics işi var. Sonraki adım, verileri gerçek zamanlı olarak analiz eden bir sorgu oluşturmaktır. Stream Analytics için gerçek zamanlı işleme dönüşümleri açıklayan bir basit, bildirim temelli bir sorgu modelini destekler. Sorgular için Stream Analytics belirli bazı uzantıları olan SQL benzeri bir dil kullanın. 

Basit bir sorgu yalnızca gelen tüm verileri okuyabilir. Ancak, genellikle belirli veri veya verilerdeki ilişkileri arama sorgular oluşturun. Öğreticinin bu bölümünde, oluşturur ve analiz için bir giriş akışına dönüştürme birkaç yollarını öğrenmek için çeşitli sorguları test. 

Burada oluşturduğunuz sorguları yalnızca dönüştürülmüş verileri ekranı görüntüler. Bir sonraki bölümde, bir çıkış havuzu ve dönüştürülen verileri yazar, havuz için bir sorgu yapılandıracaksınız.

Dili hakkında daha fazla bilgi için bkz. [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/dn834998.aspx).

### <a name="get-sample-data-for-testing-queries"></a>Sorgu testi için örnek verileri alma

TelcoGenerator uygulamasının araması kayıtlarını olay hub'ına gönderme ve Stream Analytics işinizi olay hub'ından okumak için yapılandırılır. Test işi doğru okuma emin olmak için bir sorgu kullanabilirsiniz. Azure konsolunda bir sorguyu sınamak için örnek verilere ihtiyacınız vardır. Bu kılavuz için olay hub'ına gelen akışından örnek verileri ayıklayın.

1. TelcoGenerator uygulamasının çalıştığından ve araması kayıtları oluşturduğundan emin olun.
2. Portalda akış analizi işi bölmesine döndürür. (Bölmesinde kapattıysanız, arama `asa_frauddetection_job_demo` içinde **tüm kaynakları** bölmesinde.)
3. Tıklayın **sorgu** kutusu. Azure giriş ve iş için yapılandırılan çıkışları listeler ve çıktıyı gönderilen giriş akışını dönüştürmek olanak sağlayan bir sorgu oluşturmanızı sağlar.
4. İçinde **sorgu** bölmesinde noktaya tıklayın `CallStream` seçin ve sonra **örnek giriş verileri**.

   ![Örnek veri akış analizi işi girişi, "Seçili girişten alınan örnek veriler" ile kullanmak üzere menü seçenekleri](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)


5. Ayarlama **dakika** 3 ve ardından **Tamam**. 
    
   ![Giriş akışı 3 dakika seçili örnekleme seçenekleri](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure 3 dakika değerinde giriş akışından veri örnekleri ve örnek veriler hazır olduğunda size bildirir. (Bu biraz zaman alır.) 

Örnek veriler geçici olarak depolanır ve sorgu penceresi açıkken kullanılabilir. Sorgu penceresini kapatırsanız örnek veriler atılır ve yeni bir örnek veri kümesi oluşturmanız gerekir. 

Alternatif olarak, örnek veriler içerdiğinden bir .json dosyası alabilirsiniz [github'dan](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json)ve ardından için örnek veri olarak kullanmak üzere karşıya yükleyebilirsiniz `CallStream` giriş. 

### <a name="test-using-a-pass-through-query"></a>Doğrudan sorgu kullanarak test edin

Her olay arşivlemek istiyorsanız, tüm alanları olay yükü okumak için doğrudan sorgu kullanabilirsiniz.

1. Sorgu penceresinde bu sorguyu girin:
        
   ```SQL
   SELECT 
       *
   FROM 
       CallStream
   ```

    >[!NOTE]
    >SQL olarak anahtar sözcükler büyük harf duyarlı değildir ve boşluk önemli değildir.

    Bu sorgudaki `CallStream` giriş oluştururken belirttiğiniz bir diğer addır. Farklı bir diğer ad kullandıysanız, bu adı kullanın.

2. Tıklayın **Test**.

    Stream Analytics işi, örnek verileri karşı sorguyu çalıştırır ve çıktıyı pencerenin alt kısmında görüntüler. Sonuçları, olay hub'ı ve akış analizi işi doğru yapılandırıldığını gösterir. (Belirtildiği gibi daha sonra sorgu için veri yazabilen bir çıkış havuzuna oluşturacağınız.)

   ![Stream Analytics iş çıktısı, oluşturulan 73 kayıt gösterme](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    Gördüğünüz kayıtların tam sayı, kaç tane kaydın 3 dakikalık örneğinizi yakalanan bağlıdır.
 
### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Bir sütun yansıtma kullanarak alanların sayısını azaltın

Çoğu durumda, analiz, giriş akışından tüm sütunları gerek yoktur. Döndürülen alanlarına doğrudan sorgu daha küçük bir dizi projeye bir sorguyu kullanabilirsiniz.

1. Kod Düzenleyicisi'nde sorguyu aşağıdaki gibi değiştirin:

   ```SQL
   SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
   FROM 
       CallStream
   ```

2. Tıklayın **Test** yeniden. 

   ![Stream Analytics işi çıktı yansıtma için 25 kayıtları gösterir.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Bölgeye göre gelen çağrıların sayısı: Atlayan pencere toplama

Bölge başına gelen çağrıların sayısını hesaplamak istediğinizi varsayalım. Sayımı gibi toplama işlevleri gerçekleştirmek istediğiniz zaman akış verileri, (veri akışı etkili bir şekilde sonsuz olduğundan) akış zamana bağlı birimler halinde bölmek gerekir. Bir akış analizi kullanarak bunu [pencere işlevi](stream-analytics-window-functions.md). Bir birim olarak bu pencere içindeki verileri ardından çalışabilirsiniz.

Bu dönüştürme için bir dizi çakışmadığından zamana bağlı windows istediğiniz — her pencere gruplandırmak veri toplama ve ayrık bir kümesi gerekir. Bu tür bir pencerede şeklinde adlandırılan bir *atlayan pencere*. Atlayan pencere içinde göre gruplandırılmış gelen çağrıların sayısını alabilirsiniz `SwitchNum`, aramanın nerede ülke temsil eder. 

1. Kod Düzenleyicisi'nde sorguyu aşağıdaki gibi değiştirin:

        ```SQL
        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum
        ```

    Bu sorgu kullanır `Timestamp By` anahtar sözcüğünü `FROM` yan atlayan pencere tanımlamak için kullanmak giriş akışında hangi zaman damgası alanı belirtin. Bu durumda, veri göre parçalara böler penceresi `CallRecTime` her bir kayıttaki alan. (Her olay Olay hub'ı ulaşan zaman Pencereleme işlem alanı yok belirtilirse kullanır. " "Varış zamanı Vs uygulama zamanı" bölümüne bakın [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    Projeksiyon içerir `System.Timestamp`, her pencere sonu için bir zaman damgasını döndürür. 

    Bir atlayan pencere kullanmak istediğinizi belirtmek için kullandığınız [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) işlevi `GROUP BY` yan tümcesi. İşlevinde, bir zaman birimi (herhangi bir yere mikrosaniye ölçeğinde bir gün için) ve pencere boyutunu (kaç birimleri) belirtin. Ülkeye göre bir sayısı çağrıları için her 5 saniyede değerinde erişmenizi sağlayacak şekilde bu örnekte, atlayan pencere 5 saniyelik aralıklarla, oluşur.

2. Tıklayın **Test** yeniden. Sonuçlarda dikkat zaman damgaları altında **WindowEnd** 5 saniyelik artışlarla olan.

   ![Stream Analytics işi çıktı 13 kayıtları gösteren toplama için](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Kendi kendine birleşme kullanarak SIM sahtekarlık algılama

Bu örnekte, birbirinden en fazla 5 saniye içinde farklı konumlarda ancak aynı kullanıcı kaynaklı çağrıları olması için sahte kullanımını göz önünde bulundurun. Örneğin, bir kullanıcı mantıksal olarak aynı anda hem ABD’den hem de Avustralya’dan arama yapamaz. 

Bu durumda kontrol etmek için akış verilerini kendi kendine birleşme kendisine göre stream'e katılmaya kullanabileceğiniz `CallRecTime` değeri. Çağrısının ardından bakabilirsiniz kayıtları `CallingIMSI` değerini (kaynak numara) aynı olduğu ancak `SwitchNum` değeri (kaynak ülke) aynı değil.

Akış verileriyle bir JOIN kullandığınızda, birleştirme eşleşen satırları ne kadar ayrılabildiğine ilişkin bazı sınırlar sürede ayrılabilir sağlamanız gerekir. (Daha önce belirtildiği gibi akış verileri etkili bir şekilde sınırsızdır.) İlişki için zaman sınırları içinde belirtilen `ON` JOIN yan tümcesi kullanarak `DATEDIFF` işlevi. Bu durumda, birleştirme, bir, arama verilerinin 5 saniyelik aralığına bağlıdır.

1. Kod Düzenleyicisi'nde sorguyu aşağıdaki gibi değiştirin: 

        ```SQL
        SELECT  System.Timestamp as Time, 
            CS1.CallingIMSI, 
            CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, 
            CS1.SwitchNum as Switch1, 
            CS2.SwitchNum as Switch2 
        FROM CallStream CS1 TIMESTAMP BY CallRecTime 
            JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
            ON CS1.CallingIMSI = CS2.CallingIMSI 
            AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
        WHERE CS1.SwitchNum != CS2.SwitchNum
        ```

    Bu sorgu dışında herhangi bir SQL birleştirme benzer `DATEDIFF` birleştirme işlevi. Bu sürümü `DATEDIFF` Stream Analytics'e özeldir ve görünmemesi gerekir `ON...BETWEEN` yan tümcesi. Zaman birimi (Bu örnekte saniye) ve diğer adları birleştirme için iki kaynak parametrelerdir. Bu standart SQL'den farklıdır `DATEDIFF` işlevi.

    `WHERE` Yan tümcesi içeren bayrakları sahte arama koşulu: kaynak anahtarları aynı değildir. 

2. Tıklayın **Test** yeniden. 

   ![Stream Analytics işi çıktı oluşturulan görünmeye, kendi kendine birleşme 6 kayıtları için](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Tıklayın **Kaydet** akış analizi işinin bir parçası olarak kendi kendine birleşme sorguyu kaydetmek için. (Örnek veri kaydetmez.)

    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png" alt="Save Stream Analytics query in portal" width="300px"/>

## <a name="create-an-output-sink-to-store-transformed-data"></a>Dönüştürülen verileri depolamak için çıkış havuzu oluşturma

Bir olay hub'ı giriş olayları ve akış üzerinden bir dönüştürme gerçekleştirmek için bir sorgu alma için bir olay akışı tanımladınız. Son adım, işin bir çıkış havuzu tanımlamaktır — dönüştürülmüş akışa yazmak için diğer bir deyişle, bir yer. 

Çok sayıda kaynağı çıktı havuzlarından kullanabilirsiniz — SQL Server veritabanı, tablo depolama, Data Lake depolama, Power BI ve hatta başka bir olay hub'ı. Bu öğreticide, yapılandırılmamış verileri barındırır beri daha sonraki analizler için olay bilgilerini toplama için tipik bir seçenek olan Azure Blob Depolama, stream yazacaksınız.

Blob depolama hesabınız varsa, kullanabilirsiniz. Bu öğretici için yeni bir depolama hesabının nasıl oluşturulacağını öğreneceksiniz.

### <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob Depolama hesabı oluşturma

1. Azure portalının sol üst köşesinden **Kaynak oluştur** > **Depolama** > **Depolama hesabı**’nı seçin. Depolama hesabı işi sayfası ile doldurun **adı** "asaehstorage" ayarlamak **konumu** "Doğu ABD için", ayarlama **kaynak grubu** "asa-eh-ns-rg" (konak depolama hesabı ayarlayın aynı kaynak grubunda akış işi daha yüksek performans için). Diğer ayarlar varsayılan değerlerinde bırakılabilir.  

   ![Azure portalında depolama hesabı oluşturma](./media/stream-analytics-real-time-fraud-detection/stream-analytics-storage-account-create.png)

2. Azure portalında akış analizi işi bölmesine döndürür. (Bölmesinde kapattıysanız, arama `asa_frauddetection_job_demo` içinde **tüm kaynakları** bölmesinde.)

3. İçinde **iş topolojisi** bölümünde **çıkış** kutusu.

4. İçinde **çıkışları** bölmesinde tıklayın **Ekle** seçip **Blob Depolama**. Ardından yeni çıkış sayfasını aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Çıktı diğer adı  |  CallStream FraudulentCalls   |  İşin çıktısını tanımlamak için bir ad girin.   |
   |Abonelik   |  \<Aboneliğiniz\> |  Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır. |
   |Depolama hesabı  |  asaehstorage |  Oluşturduğunuz depolama hesabının adını girin. |
   |Kapsayıcı  | asa-fraudulentcalls-demo | Yeni Oluştur'ı seçin ve bir kapsayıcı adı girin. |

    <br/>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png" alt="Create blob output for Stream Analytics job" width="300px"/>
    
5. **Kaydet**’e tıklayın. 


## <a name="start-the-streaming-analytics-job"></a>Akış analizi işi başlatın

İş artık yapılandırılmıştır. Giriş (event hub), bir dönüştürme (dolandırıcılık amaçlı çağrıları bulmak için sorgu) ve bir çıkış (blob depolama) belirttiniz. İşi şimdi başlayabilirsiniz. 

1. TelcoGenerator uygulamasının çalıştığından emin olun.

2. İş bölmesinden **Başlat**. İçinde **başlangıç işi** bölmesinde iş çıkışı başlangıç zamanı, select **artık**. 

   ![Stream Analytics işini başlatın](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start.png)



## <a name="examine-the-transformed-data"></a>Dönüştürülmüş verileri İnceleme

Artık tam bir akış analizi işi var. İş akışı telefon araması meta verileri inceleme, gerçek zamanlı olarak sahte aramaları mi arıyorsunuz ve bu sahte çağrıları depolama hakkında bilgi yazma. 

Bu öğreticiyi tamamlamak için akış analizi işi tarafından yakalanan veriler bakmak isteyebilirsiniz. Verileri Azure Blog depolama alanına (dosyalar) öbekler halinde yazılmaktadır. Azure Blob Depolama okuyan herhangi bir aracı kullanabilirsiniz. Önkoşullar bölümünde belirtildiği gibi Visual Studio'da Azure uzantıları kullanabilir veya gibi bir araç kullanabilirsiniz [Azure Depolama Gezgini](https://storageexplorer.com/) veya [Cerulean](https://www.cerebrata.com/products/cerulean/features/azure-storage). 

Blob depolama alanındaki bir dosyanın içeriğini incelediğinizde, aşağıdakine benzer bir şey görürsünüz:

   ![Akış analizi çıkışı ile Azure blob depolama](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sahtekarlık algılama senaryoyla devam etmek ve Bu öğreticide oluşturduğunuz kaynakları kullanan başka makaleler vardır. Devam etmek istiyorsanız, altında sunabileceği önerileri görmek **sonraki adımlar**.

Gereksiz Azure ücret oluşmaması ancak oluşturduğunuz kaynakları gerekmez ve işiniz bittiğinde, bunları silebilirsiniz. Bu durumda, aşağıdakileri yapmanızı öneririz:

1. Akış analizi işi durdurun. İçinde **işleri** bölmesinde tıklayın **Durdur** en üstünde.
2. Telekomünikasyon Durdur Oluşturucu uygulama. Uygulama başlatıldığı komut penceresinde Ctrl + C tuşlarına basın.
3. Bu öğretici için yalnızca yeni blob depolama hesabı oluşturduysanız, onu silin. 
4. Akış analizi işi silin.
5. Olay hub'ı silin.
6. Olay hub'ı ad alanını silin.

## <a name="get-support"></a>Destek alın

Daha fazla yardım için deneyin [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalede bu öğreticiyle devam edebilirsiniz:

* [Stream Analytics ve Power BI: Veri akışı için gerçek zamanlı analiz Panosu](stream-analytics-power-bi-dashboard.md). Bu makalede, Stream Analytics işi TelCo çıkışını gerçek zamanlı Görselleştirme ve analiz için Power BI'a göndermeniz işlemini göstermektedir.

Stream Analytics hakkında daha fazla bilgi için genel olarak, şu makalelere bakın:

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
