---
title: "Akış analizi: Gerçek zamanlı sahtekarlık algılama | Microsoft Docs"
description: "Akış Analizi ile gerçek zamanlı sahtekarlık algılama çözüm oluşturmayı öğrenin. Gerçek zamanlı Olay işleme için bir olay hub'ı kullanın."
keywords: "anomali algılama, sahtekarlık algılama, gerçek zamanlı anomali algılama"
services: stream-analytics
documentationcenter: 
author: SnehaGunda
manager: jhubbard
editor: cgronlun
ms.assetid: c10dd53f-d17a-4268-a561-cb500a8c04eb
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: sngun
ms.openlocfilehash: a3b61b0eeef9ffc97b0cc06a8de44859e4d6db85
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Azure Stream Analytics'i kullanmaya başlama: Gerçek zamanlı sahtekarlık algılama

Bu öğretici bir Azure akış analizi kullanma uçtan uca çizimi sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

* Getir olayları Azure Event Hubs bir örneğine akış. Bu öğreticide, cep telefonu meta veri kayıtlarını akışı benzetim sağladığımız bir uygulaması kullanacaksınız.

* Bilgi toplama veya desenlerini bakarak verileri dönüştürmek için SQL benzeri Stream Analytics sorgular yazarsınız. Gelen akış inceleyin ve sahte olabilir çağrıları için aramak için bir sorgu kullanma görürsünüz.

* Sonuçları ilgili ek bilgileri analiz bir çıkış havuzu (Depolama) gönderin. Bu durumda, Azure Blob depolama alanına şüpheli çağrısı veri göndereceğiz.

Bu öğreticide, telefon araması verilerine dayalı gerçek zamanlı sahtekarlık algılama örneği kullanın. Ancak biz göstermeye teknik sahtekarlık algılama, kredi kartı sahtekarlık veya kimlik hırsızlığı gibi diğer türleri için de uygundur. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Senaryo: Gerçek zamanlı telekomünikasyon ve SIM sahtekarlık algılama

Telekomünikasyon şirket, fazla miktarda verinin gelen çağrıları için sahiptir. Böylece müşteriler bildir veya belirli bir sayıda hizmet kapatmak sahte çağrıları gerçek zamanlı olarak algılamak şirket ister. SIM sahtekarlık bir tür aynı kimliğe yaklaşık aynı zamanda ancak coğrafi olarak farklı konumlarda gelen birden çok çağrıları içerir. Bu tür sahtekarlık algılamak için gelen telefon kayıtları incelemek ve belirli kalıpları aramak şirketin gerekir; bu durumda, aynı anda farklı ülkelerde yapılan aramalar için. Bu kategoriye herhangi bir telefon kayıt sonraki analiz için depolama alanına yazılır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, örnek telefon araması meta verilerini oluşturur bir istemci uygulaması kullanarak, telefon araması veri benzetimi. Bazı uygulama üreten kayıtlar gibi sahte çağrıları arayın. 

Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

* Bir Azure hesabı.
* Çağrı olay Oluşturucu uygulama. Bu indirerek elde [TelcoGenerator.zip dosya](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) Microsoft Download Center gelen. Bu paket, bilgisayarınızdaki bir klasöre ayıklayın. Kaynak kodu ve hata ayıklayıcısı'ndaki uygulama çalıştırma görmek istiyorsanız, uygulama kaynak kodundan alabilirsiniz [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Windows indirilen .zip dosyası engelleyebilir. Unzip olamaz, dosyaya sağ tıklayın ve seçin **özellikleri**. "Bu dosya başka bir bilgisayardan gelen ve bu bilgisayarın korunmasına yardımcı olmak için engellenmiş olabilir" iletisini görürseniz, seçin **Engellemeyi Kaldır** seçeneğini ve ardından **Uygula**.

Akış analizi işi sonuçlarını incelemek isterseniz, Azure Blob Storage kapsayıcısının içeriğini görüntülemek için ayrıca bir aracı gerekir. Visual Studio kullanırsanız, kullanabileceğiniz [Visual Studio için Azure Araçları](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) veya [Visual Studio Cloud Explorer](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer). Alternatif olarak, tek başına araçlarla yükleyebilirsiniz [Azure Storage Gezgini](http://storageexplorer.com/) veya [Azure Gezgini](http://www.cerebrata.com/products/azure-explorer/introduction). 

## <a name="create-an-azure-event-hubs-to-ingest-events"></a>Azure olay hub'ları olayları alma oluşturma

Bir veri akışı çözümlemek için *alma* Azure içine. Veri alma için tipik bir yol kullanmaktır [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), olanak sağlayan, saniye başına milyonlarca olayı alma ve sonra işlem ve olay bilgileri depolar. Bu öğretici için bir olay hub'ı oluşturun ve bu olay hub'ına çağrısı veri gönderme çağırma olayı Oluşturucu uygulamaya sahip. Event hubs hakkında daha fazla bilgi için bkz: [Azure Service Bus belgelerine](https://docs.microsoft.com/azure/service-bus/).

>[!NOTE]
>Bu yordamı daha ayrıntılı bir sürümü için bkz: [bir olay hub'ları ad alanı oluşturup Azure portalını kullanarak bir event hub](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Bir ad alanı ve olay hub'ı Oluştur
Bu yordamda, önce bir olay hub'ad alanı oluşturun ve ardından bir event hub bu ad alanına ekleyin. Olay hub'ı ad alanları, ilgili olay bus örneklerini mantıksal olarak gruplamak için kullanılır. 

1. Azure portalında oturum açın ve tıklatın **kaynak oluşturma** > **nesnelerin interneti** > **olay hub'ı**. 

2. İçinde **ad alanı oluşturma** bölmesinde gibi bir ad alanı adı girin `<yourname>-eh-ns-demo`. Ad alanı için herhangi bir ad kullanabilirsiniz, ancak ad geçerli bir URL olmalıdır ve Azure arasında benzersiz olması gerekir. 
    
3. Bir aboneliği seçin ve oluşturmak veya bir kaynak grubu seçin ve ardından **oluşturma**. 

    ![Bir olay hub'ad alanı oluşturma](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png)
 
4. Ad alanı dağıtmayı bitirdiğinde, olay hub'ad alanı, Azure kaynak listesinde bulun. 

5. Yeni ad alanına tıklayın ve ad alanı bölmesinde **olay hub'ı**.

    ![Yeni bir olay hub'ı oluşturmak için olay Hub'ı Ekle düğmesi ](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Yeni olay hub'ı adı `sa-eh-frauddetection-demo`. Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, adı daha sonra gerektiğinden, not edin. Olay hub'ı için herhangi bir seçenek şu anda ayarlamanız gerekmez.

    ![Yeni bir olay hub'ı oluşturmak için dikey penceresi](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png)
    
 
7. **Oluştur**’a tıklayın.
### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Olay hub'ına erişim vermek ve bir bağlantı dizesi alma

Olay hub'ı, bir işlem olay hub'ına veri göndermeden önce uygun erişim veren bir ilke olması gerekir. Erişim İlkesi yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1.  Olay ad alanı bölmesinde **olay hub'ları** ve ardından yeni olay hub'ınızın adını tıklatın.

2.  Olay hub'ı bölmesinde **paylaşılan erişim ilkeleri** ve ardından  **+ &nbsp;Ekle**.

    >[!NOTE]
    >Olay hub, olay hub'ı ad çalıştığınız emin olun.

3.  Adlı ilke Ekle `sa-policy-manage-demo` ve **talep**seçin **Yönet**.

    ![Yeni bir olay hub'ı erişim ilkesi oluşturmak için dikey penceresi](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png)
 
4.  **Oluştur**’a tıklayın.

5.  İlke dağıtıldıktan sonra paylaşılan erişim ilkeleri listesinde tıklayın.

6.  Etiketli kutuyu bulun **bağlantı dize birincil anahtarı** ve bağlantı dizesini yanındaki Kopyala düğmesini tıklatın. 
    
    ![Erişim ilkesinden birincil bağlantı dizesi anahtarını kopyalama](./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png)
 
7.  Bağlantı dizesi, bir metin düzenleyicisine yapıştırın. Bazı küçük Düzenlemeleri yaptıktan sonra bir sonraki bölüm için bu bağlantı dizesi gerekir.

    Bağlantı dizesi şu şekildedir:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=sa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=sa-eh-frauddetection-demo

    Bağlantı dizesi noktalı virgülle ayırarak, birden çok anahtar-değer çiftleri içerdiğine dikkat edin: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, ve `EntityPath`.  

## <a name="configure-and-start-the-event-generator-application"></a>Yapılandırma ve olay Oluşturucu uygulamasını başlatın

TelcoGenerator uygulama başlamadan önce böylece çağrısı kayıtları oluşturduğunuz olay hub'ına gönderir, yapılandırın.

### <a name="configure-the-telcogeneratorapp"></a>TelcoGeneratorapp yapılandırın

1.  Bağlantı dizesi kopyaladığınız düzenleyicisinde Not `EntityPath` değer ve Kaldır'ı `EntityPath` çifti (unutmayın yakalanması noktalı kaldırmak için). 

2.  Burada TelcoGenerator.zip dosya unzipped klasöründe telcodatagen.exe.config dosyasına bir düzenleyicide açın. (Birden fazla .config dosyası, bu nedenle doğru olanı açık olduğundan emin olun.)

3.  İçinde `<appSettings>` öğe:

    * Değerini `EventHubName` olay hub'ı adı anahtar (diğer bir deyişle, varlık yolu değerine).
    * Değerini `Microsoft.ServiceBus.ConnectionString` anahtar bağlantı dizesi. 

    `<appSettings>` Bölümü, aşağıdaki gibi görünür. (Daha anlaşılır olması, biz satıra gibi ve bazı karakterler yetkilendirme belirtecinden kaldırıldı.)

    ![Olay hub adını ve bağlantı dizesini gösteren TelcoGenerator uygulama yapılandırma dosyası](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4.  Dosyayı kaydedin. 

### <a name="start-the-app"></a>Uygulamayı başlatın
1.  Bir komut penceresi açın ve TelcoGenerator uygulama sıkıştırması açılmış bulunduğu klasöre geçin.
2.  Aşağıdaki komutu girin:

        telcodatagen.exe 1000 .2 2

    Parametreler şunlardır: 

    * Saat başına CDR sayısı. 
    * SIM kart sahtekarlık olasılık: Ne sıklıkta uygulama sahte bir çağrı benzetimini tüm çağrıları yüzdesi. Değer 2'dir, yaklaşık %20 çağrısı kayıtların sahte görüneceğini anlamına gelir.
    * Saat cinsinden süre. Uygulama çalışması gerektiğini saat sayısı. Ayrıca uygulamayı, komut satırında Ctrl + C tuşlarına basarak istediğiniz zaman durdurabilirsiniz.

    Birkaç saniye sonra bunları olay hub'ına gönderir olarak telefon araması kayıtları ekranda görüntüleme uygulamayı başlatır.

Bu gerçek zamanlı sahtekarlık algılama uygulamada kullanarak anahtar alanlardan bazıları şunlardır:

|**Kayıt**|Tanımı|
|----------|--------------|
|`CallrecTime`|Arama için zaman damgası başlangıç saati. |
|`SwitchNum`|Çağrı bağlanmak için kullanılan telefon anahtarı. Bu örnekte, anahtarlar ülkeyi (ABD, Çin, İngiltere, Almanya veya Avustralya) temsil eden dizeleri şunlardır. |
|`CallingNum`|Arayanın telefon numarası. |
|`CallingIMSI`|Uluslararası mobil abone kimliği (IMSI). Arayanın benzersiz tanımlayıcısıdır. |
|`CalledNum`|Çağrı alıcının telefon numarası. |
|`CalledIMSI`|Uluslararası mobil abone kimliği (IMSI). Bu çağrı alıcının benzersiz tanımlayıcısıdır. |


## <a name="create-a-stream-analytics-job-to-manage-streaming-data"></a>Akış verilerini yönetmek için Stream Analytics işi oluşturma

Çağrı olayların bir akışa sahip olduğunuza göre bir akış analizi işi ayarlayabilirsiniz. İş verileri ayarladığınız olay hub'ı okuma. 

### <a name="create-the-job"></a>Proje oluşturma 

1. Azure portalında tıklatın **kaynak oluşturma** > **nesnelerin interneti** > **Stream Analytics işi**.

2. İş adı `sa_frauddetection_job_demo`, abonelik, kaynak grubunu ve konumu belirtin.

    En iyi performans için aynı bölgede iş ve olay hub'ı yerleştirmek için iyi bir fikirdir ve böylece bölgeler arasında veri aktarmak ödemeniz gerekmez.

    ![Yeni Stream Analytics işi oluşturma](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png)

3. **Oluştur**’a tıklayın.

    İş oluşturulur ve portal iş ayrıntılarını görüntüler. Henüz hiçbir şey ancak çalıştığı — bunu başlamadan önce iş yapılandırmanız gerekir.

### <a name="configure-job-input"></a>İş Girişi yapılandırın

1. Panoda veya **tüm kaynakları** bölmesinde bulun ve seçin, `sa_frauddetection_job_demo` Stream Analytics işi. 
2. İçinde **iş topoloji** bölüm Stream Analytics işi bölmesinde, tıklatın **giriş** kutusu.

    ![Akış analizi işi bölmesinde topoloji altında giriş kutusu](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Tıklatın  **+ &nbsp;Ekle** ve ardından bu değerleri bölmesiyle doldurun:

    * **Giriş diğer adı**: adını kullanmak `CallStream`. Farklı bir ad kullanırsanız, daha sonra ihtiyacınız olacak çünkü bunu not edin.
    * **Kaynak türünü**: seçin **veri akışı**. (**Başvuru verileri** Bu öğreticide kullanmayacaksa statik arama verileri ifade eder.)
    * **Kaynak**: seçin **olay hub'ı**.
    * **Alma seçeneği**: seçin **geçerli aboneliğe ilişkin kullanım olay hub'ı**. 
    * **Hizmet veri yolu ad alanı**: daha önce oluşturduğunuz olay hub'ı ad alanını seçin (`<yourname>-eh-ns-demo`).
    * **Olay hub'ı**: daha önce oluşturduğunuz olay hub'ı seçin (`sa-eh-frauddetection-demo`).
    * **Olay hub'ı ilke adı**: daha önce oluşturduğunuz erişim ilkesi seçin (`sa-policy-manage-demo`).

    ![Akış analizi işi için yeni giriş oluşturma](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png)

4. **Oluştur**’a tıklayın.

## <a name="create-queries-to-transform-real-time-data"></a>Gerçek zamanlı verileri dönüştürmek için sorgular oluşturun

Bu noktada, gelen bir veri akışı okumak için ayarlanmış bir akış analizi işi var. Sonraki adım, gerçek zamanlı verileri çözümler bir dönüşüm oluşturmaktır. Bunun için bir sorgu oluşturarak. Akış analizi dönüştürmeleri için gerçek zamanlı işleme açıklayan bir basit ve bildirim temelli sorgu modelini destekler. Sorguları akış analizi için belirli bazı uzantıları olan SQL benzeri bir dil kullanın. 

Basit bir sorgu yalnızca gelen tüm verileri okuyabilir. Ancak, genellikle özel veriler için veya verilerdeki ilişkileri arayın sorgular oluşturun. Öğreticinin bu bölümünde oluşturmak ve çözümleme için bir giriş akışı dönüştürebilirsiniz birkaç yollarını öğrenmek için birkaç sorguları sınayın. 

Burada oluşturduğunuz sorgular yalnızca dönüştürülmüş verilerin ekrana görüntüler. Sonraki bölümde, çıkış havuzu ve dönüştürülen veriler için bu havuzu Yazar bir sorgu yapılandıracaksınız.

Dili hakkında daha fazla bilgi için bkz: [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/dn834998.aspx).

### <a name="get-sample-data-for-testing-queries"></a>Sorguları test etmek için örnek veri al

TelcoGenerator uygulama olay hub'ına çağrısı kayıtları gönderiyor ve Stream Analytics işiniz olay hub'ından okumak için yapılandırılır. Doğru okuma emin olmak için iş test etmek için bir sorgu kullanabilirsiniz. Azure konsolunda bir sorguyu sınamak için örnek verileri gerekir. Bu kılavuz için event hub'ına gelen akıştan örnek verileri ayıklamak.

1. TelcoGenerator uygulaması çalıştıran ve çağrı kayıtları oluşturan emin olun.
2. Portalda, akış analizi işi bölmesine döndürür. (Bölmesinde kapattıysanız, arama `sa_frauddetection_job_demo` içinde **tüm kaynakları** bölmesinde.)
3. Tıklatın **sorgu** kutusu. Azure girişleri ve iş için yapılandırılmış olan çıkışları listeler ve çıktıyı gönderilmiş gibi giriş akışı dönüştürme olanak sağlayan bir sorgu oluşturmanızı sağlar.
4. İçinde **sorgu** bölmesinde nokta tıklayın `CallStream` girin ve ardından **örnek giriş verilerinden**.

    ![Akış analizi işi girişle "Seçili örnek verilerden Giriş" örnek verileri kullanılmak üzere menü seçenekleri](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)

    Bu giriş akışı okumayı açısından ne kadar süreyle tanımlanan Al ne kadar örnek verileri belirtmenize olanak sağlayan bir bölmesi açılır.

5. Ayarlama **dakika** 3 ve ardından **Tamam**. 
    
    ![Giriş akışı "3 Seçili dakika ile" örnekleme seçenekleri.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure Giriş akışı verilerden 3 dakika eşitleyeceğini örnekleri ve örnek verileri hazır olduğunda size bildirir. (Bu kısa biraz uzun sürebilir.) 

Örnek verileri geçici olarak depolanır ve açık sorgu penceresi açıkken kullanılabilir. Sorgu penceresini kapattığınızda, örnek veriler atılır ve yeni bir örnek veri kümesi oluşturmak zorunda kalırsınız. 

Alternatif olarak, örnek veriler olan bir .json dosyası alabilirsiniz [github'dan](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json)ve örnek veriler olarak kullanmak için bu .json dosyası karşıya yükleme `CallStream` giriş. 

### <a name="test-using-a-pass-through-query"></a>Bir doğrudan geçirilen sorgu kullanmadan test edin

Her olay arşivlemek istiyorsanız, olay yükü tüm alanları okumak için doğrudan sorgu kullanabilirsiniz.

1. Sorgu penceresinde, bu sorguyu girin:

        SELECT 
            *
        FROM 
            CallStream

    >[!NOTE]
    >İle SQL gibi anahtar sözcükler büyük küçük harfe duyarlı değildir ve boşluk önemli değildir.

    Bu sorguda `CallStream` giriş oluşturduğunuzda belirtilen diğer adı. Farklı bir diğer ad kullandıysanız, bunun yerine bu adı kullanın.

2. Tıklatın **Test**.

    Stream Analytics işi karşı örnek verileri sorguyu çalıştırır ve çıktıyı penceresinin alt kısmında görüntüler. Bu olay hub'ı ve akış analizi işi düzgün yapılandırılmış olduğunu bildirir. (Belirtildiği gibi daha sonra sorgu verileri yazabilen çıkış havuzu oluşturmayı öğreneceksiniz.)

    ![Stream Analytics iş çıktısı, oluşturulan 73 kaydı gösteriliyor](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    Tam kayıt sayısı, gördüğünüz kaç kayıt 3 dakika örneğinizi yakalanan bağlı olacaktır.
 
### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Bir sütun projeksiyonu kullanarak alan sayısını azaltın

Çoğu durumda, çözümleme Giriş akışı tüm sütunlardan gerekmez. Döndürülen alanlarında doğrudan sorgu daha küçük bir dizi proje için bir sorgu kullanabilirsiniz.

1. Kod düzenleyicisinde sorgu aşağıdaki gibi değiştirin:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
        FROM 
            CallStream

2. Tıklatın **Test** yeniden. 

    ![Stream Analytics işi çıkış oluşturulan 25 kayıtları gösteren projeksiyon için](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Count gelen bölgeye göre çağırır: atlayan pencere toplama ile

Her bölge gelen çağrıların sayısını istediğinizi varsayalım. Sayımı gibi toplama işlevleri gerçekleştirmek istediğiniz akış verisi (veri akışı etkili bir şekilde sonsuz olduğundan) akış zamana bağlı birimlerine segmentlere ayırmak ihtiyacınız var. Akış analizi kullanarak bunu [pencere işlevi](stream-analytics-window-functions.md). Bir birim olarak söz konusu pencereyi içindeki verileri sonra çalışabilirsiniz.

Bu dönüştürme için bir dizi çakışmadığından zamana bağlı windows istediğiniz — her pencere, gruplandırabilirsiniz veri ve toplama ayrık kümesine sahip. Bu pencere türü olarak adlandırılır bir *atlayan pencere* . Atlayan pencere içinde göre gruplandırılmış gelen çağrıların sayısını alabilir `SwitchNum`, çağrı geldiği ülke temsil eder. 

1. Kod düzenleyicisinde sorgu aşağıdaki gibi değiştirin:

        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Bu sorgu kullanan `Timestamp By` anahtar sözcük `FROM` atlayan pencere tanımlamak için kullanmak için giriş akışı hangi zaman damgası alanı belirtmek için yan tümcesi. Bu durumda, pencerenin verileri tarafından kesimleri böler `CallRecTime` her bir kayıttaki alan. (Hiçbir alan belirtilmezse, her olay Olay hub'ına ulaştığında zaman Pencereleme işlemi kullanır. "Geliş saati Vs uygulama saati" bölümüne bakın [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    Projeksiyon içeren `System.Timestamp`, her pencere sonuna için zaman damgasını döndürür. 

    Atlayan pencere kullanmak istediğinizi belirtmek için kullandığınız [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) işlevi `GROUP BY `yan tümcesi. İşlevinde bir zaman birimi (herhangi bir yerden bir güne milisaniyeye) ve pencere boyutunu (kaç birimleri) belirtin. Bu örnekte, nedenle ülkeye göre bir sayısı için her 5 saniyede eşitleyeceğini çağrılarının karşılaşırsınız atlayan pencere 5 saniyelik aralıklarına oluşur.

2. Tıklatın **Test** yeniden. Sonuçlarda dikkat zaman damgaları altında **WindowEnd** 5 saniyelik artışlarla şunlardır.

    ![Stream Analytics işi çıkış oluşturulan 13 kayıtları gösteren toplama için](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Kendi kendine birleşim kullanarak SIM sahtekarlık algılama

Bu örnekte, biz aynı kullanıcı ancak farklı konumlarda birbirleriyle 5 saniye içinde kaynaklanan çağrıları olmasını sahte kullanım düşünebilirsiniz. Örneğin, aynı kullanıcı yasal bir çağrı ABD ve Avustralya aynı anda yapamazsınız. 

İçin bu durumların denetlemek için veri akışı kendi kendine birleşim göre kendisine akış katılmak için kullanabileceğiniz `CallRecTime` değeri. Arama için sonra bakabilirsiniz kayıtları `CallingIMSI` değeri (kaynak numarası) aynıdır, ancak `SwitchNum` değeri (kaynağı ülke) aynı değil.

Veri akış ile bir birleşim kullandığınızda, birleştirme bazı sınırlamalar eşleşen satırları ne kadar süre ayrılabilir sağlamanız gerekir. (Daha önce belirtildiği gibi veri akışı etkili bir şekilde sınırsızdır.) İlişki için zaman sınırları içinde belirtilen `ON` JOIN yan tümcesi kullanılarak `DATEDIFF` işlevi. Bu durumda, birleştirme bir çağrı veri 5 saniye aralığını temel alır.

1. Kod düzenleyicisinde sorgu aşağıdaki gibi değiştirin: 

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

    Bu sorgu dışında herhangi bir SQL birleşimin gibidir `DATEDIFF` birleştirme işlevi. Bu bir sürümüdür `DATEDIFF` akış analizi özgü ve görünmemesi gerekir `ON...BETWEEN` yan tümcesi. Zaman birimi (Bu örnekte saniye) ve iki kaynakları katılım için diğer adlar parametrelerdir. (Bu standart SQL'den farklıdır `DATEDIFF` işlevi.) 

    `WHERE` Yan tümcesi içeren sahte çağrısı bayrakları koşul: kaynak anahtarlar aynı değildir. 

2. Tıklatın **Test** yeniden. 

    ![Stream Analytics işi çıkış oluşturulan kendi kendine birleşim, gösterme 6 kayıtlar için](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. **Kaydet**’e tıklayın. Bu, kendi kendine birleşim sorgu akış analizi işi bir parçası olarak kaydeder. (Bu örnek verileri kaydetmez.)

    ![Akış analizi işi Kaydet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png)

## <a name="create-an-output-sink-to-store-transformed-data"></a>Dönüştürülmüş verileri depolamak için çıkış havuzu oluşturma

Giriş olayları ve akış üzerinden bir dönüştürme gerçekleştirmek için bir sorgu alma için bir olay hub'ı bir olay akışı tanımladığınız. İş için çıkış havuzu tanımlamak için son adımdır — dönüştürülmüş akışa yazmak için başka bir deyişle, bir yer. 

Birçok kaynakları çıkış havuzlarını kullanabilirsiniz — bir SQL Server veritabanı, tablo depolama, Data Lake storage, Power BI ve hatta başka bir olay hub'ı. Bu öğretici için yapılandırılmamış verileri düzenler beri sonraki çözümleme için olay bilgilerini toplamak için tipik bir seçimdir Azure Blob depolama alanına akış yazacaksınız.

Blob depolama hesabınız varsa, kullanabilirsiniz. Bu öğretici için yalnızca Bu öğretici için yeni bir depolama hesabı oluşturulacağını göstereceğiz.

### <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob Storage hesabı oluşturma

1. Azure portalında akış analizi işi bölmesine döndürür. (Bölmesinde kapattıysanız, arama `sa_frauddetection_job_demo` içinde **tüm kaynakları** bölmesinde.)
2. İçinde **iş topoloji** 'yi tıklatın **çıkış** kutusu. 
3. İçinde **çıkışları** bölmesinde tıklatın  **+ &nbsp;Ekle** ve ardından bu değerleri bölmesiyle doldurun:

    * **Çıkış diğer adları**: adını kullanmak `CallStream-FraudulentCalls`. 
    * **Havuz**: seçin **Blob storage**.
    * **İçe aktarma seçenekleri**: seçin **blob storage'ı geçerli aboneliğe ilişkin kullanma**.
    * **Depolama hesabı**. Seçin **yeni depolama hesabı oluşturun.**
    * **Depolama hesabı** (ikinci kutusu). Girin `YOURNAMEsademo`, burada `YOURNAME` adınızı veya başka bir benzersiz bir dize. Ad yalnızca küçük harfler ve sayılar kullanabilirsiniz ve Azure arasında benzersiz olması gerekir. 
    * **Kapsayıcı**. Girin `sa-fraudulentcalls-demo`.
    Depolama hesabı adı ve kapsayıcı adı bir URI şöyle blob depolama sağlamak için birlikte kullanılır: 

    `http://yournamesademo.blob.core.windows.net/sa-fraudulentcalls-demo/...`
    
    ![Stream Analytics işi için "Yeni çıkış" bölmesi](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png)
    
4. **Oluştur**’a tıklayın. 

    Azure depolama hesabı oluşturur ve bir anahtarı otomatik olarak oluşturur. 

5. Kapat **çıkışları** bölmesi. 

## <a name="start-the-streaming-analytics-job"></a>Akış analizi işi Başlat

İşi şimdi yapılandırıldı. Bir giriş (olay hub), bir dönüştürme (sahte çağrıları için aranacak sorgu) ve bir çıkış (blob depolama) belirlediniz. İşi şimdi başlayabilirsiniz. 

1. TelcoGenerator uygulama çalıştığından emin olun.

2. İş bölmesinde **Başlat**.

    ![Stream Analytics işi Başlat](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-output.png)

3. İçinde **başlangıç işi** için proje çıktı başlangıç saati, seçim bölmesi **şimdi**. 

4. Tıklatın **Başlat**. 

    ![Stream Analytics işi için "işlemini Başlat" bölmesi](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-job-blade.png)

    Azure bildirir, size, iş başlatıldı ve iş bölmesini olarak durumu görüntülendiğinde **çalıştıran**.

    ![Stream Analytics iş durumu, "Çalışır" gösterme](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-running-status.png)
    

## <a name="examine-the-transformed-data"></a>Dönüştürülen veriler inceleyin

Artık tam bir akış analizi işi var. İş telefon araması meta veri akışı incelenerek, gerçek zamanlı sahte telefon aramaları için arama ve bu sahte çağrıları depolama hakkında bilgi yazma. 

Bu öğreticiyi tamamlamak için akış analizi işi tarafından yakalanan veriler bakmak isteyebilirsiniz. Verileri Azure blogu depolama birimine öbekleri (dosyaları) yazılmakta. Azure Blob Storage okuyan herhangi bir aracı kullanabilirsiniz. Önkoşullar bölümünde belirtildiği gibi Visual Studio'daki Azure uzantıları kullanabilir veya gibi bir araç kullanabilirsiniz [Azure Storage Gezgini](http://storageexplorer.com/) veya [Azure Gezgini](http://www.cerebrata.com/products/azure-explorer/introduction). 

Blob depolama birimindeki bir dosyanın içeriğini incelediğinizde, aşağıdakine benzer bir şey görürsünüz:

![Akış analizi çıktı ile Azure blob storage](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Biz, sahtekarlık algılama senaryoyla devam etmek ve Bu öğreticide oluşturduğunuz kaynakları kullanan diğer makaleler vardır. Devam etmek istiyorsanız, öneriler altında bkz **sonraki adımlar** daha sonra.

Böylece, gereksiz Azure ücretlendirme yok ancak, işiniz bittiğinde ve bunu oluşturduğunuz kaynakları gerekmiyorsa bunları silebilirsiniz. Bu durumda, şunları yapmanız önerilir:

1. Akış analizi işi durdurun. İçinde **işleri** bölmesinde tıklatın **durdurmak** en üstünde.
2. Telco Durdur Oluşturucu uygulama. Uygulama başlatıldığı komut penceresinde Ctrl + C tuşlarına basın.
3. Bu öğretici için yeni bir blob depolama hesabı oluşturduysanız, onu silin. 
4. Akış analizi işi silin.
5. Olay hub'ı silin.
6. Olay hub'ı ad alanını silin.

## <a name="get-support"></a>Destek alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici aşağıdaki makale ile devam edebilirsiniz:

* [Analizler ve Power BI akış: veri akışı için gerçek zamanlı analiz Pano](stream-analytics-power-bi-dashboard.md). Bu makalede nasıl gerçek zamanlı Görselleştirme ve analiz için Power BI Stream Analytics işi TelCo çıktısını gönderileceği gösterilmektedir.

Stream Analytics hakkında daha fazla bilgi için genel olarak, bu makalelere bakın:

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
