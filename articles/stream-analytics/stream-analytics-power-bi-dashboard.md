---
title: Azure Stream Analytics ile Power BI Panosu tümleştirme
description: Bu makalede, bir Azure akış analizi işi dışında görselleştirmek için gerçek zamanlı bir Power BI panosuna kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2017
ms.openlocfilehash: 15b8548e8b5b6ff8d2f5722d2a4031f8e52d044b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Analizler ve Power BI akış: veri akışı için gerçek zamanlı analiz Panosu
Azure Stream Analytics, başında iş zekası araçları birini yararlanmak tanır [Microsoft Power BI](https://powerbi.com/). Bu makalede, bilgi nasıl iş zekası araçları, Azure akış analizi işi için çıkış olarak Power BI kullanarak oluşturun. Ayrıca oluşturmak ve gerçek zamanlı bir Panoda kullanmak hakkında bilgi edinin.

Bu makalede Stream Analytics'ten devam [gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) Öğreticisi. Bu öğreticide oluşturduğunuz akışında oluşturur ve böylece bir akış analizi işi tarafından algılanan sahte telefon aramaları görselleştirebilirsiniz çıktı Power BI ekler. 

İzleyebilir [video](https://www.youtube.com/watch?v=SGUpT-a99MA) bu senaryo gösterilmektedir.


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

* Bir Azure hesabı.
* Power BI için bir hesap. Bir iş veya Okul hesabı kullanabilirsiniz.
* Tamamlanmış bir sürümünü [gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md) Öğreticisi. Öğretici kurgusal telefonla meta verileri oluşturan bir uygulamayı içerir. Öğreticide, bir olay hub'ı oluşturun ve telefon araması veri akışı event hub'ına gönderin. Sahte aramaları (aynı anda aynı sayıdan farklı konumlarda) algıladığı için bir sorgu yazın. 


## <a name="add-power-bi-output"></a>Power BI çıkışı ekleme
Gerçek zamanlı sahtekarlık algılama öğreticide çıktı Azure Blob depolama alanına gönderilir. Bu bölümde, Power BI bilgilerini gönderen bir çıktı ekleyin.

1. Azure Portal'da, daha önce oluşturduğunuz akış analizi işi'ni açın. Önerilen ad kullandıysanız, iş adlı `sa_frauddetection_job_demo`.

2. Seçin **çıkışları** ortasında işi Panosu kutusuna ve ardından **+ Ekle**.

3. İçin **çıkış diğer adları**, girin `CallStream-PowerBI`. Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, adı daha sonra gerektiğinden, not edin. 

4. Altında **havuzu**seçin **Power BI**.

   ![Power BI için bir çıktı oluşturmak](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. Tıklatın **yetkilendirmek**.

    Burada bir iş veya Okul hesabı için Azure kimlik bilgilerinizi sağlayabilir penceresi açılır. 

    ![Power BI erişim için kimlik bilgilerini girin](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. Kimlik bilgilerinizi girin. Kimlik bilgilerinizi girdiğinizde Power BI alanınızı erişim izni akış analizi işi de yaparken sonra unutmayın.

7. Ne zaman, döndürülen için **yeni çıktı** dikey penceresinde, aşağıdaki bilgileri girin:

    * **Çalışma grubu**: Power BI kiracınızın, veri kümesi oluşturmak istediğiniz bir çalışma alanı seçin.
    * **Veri kümesi adı**: girin `sa-dataset`. Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, bunu daha sonra kullanmak üzere not.
    * **Tablo adı**: girin `fraudulent-calls`. Şu anda, akış analizi işleri Power BI çıkışı bir veri kümesinde yalnızca bir tablo olabilir.

    ![PBI çalışma](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > Power BI veri kümesi ve Stream Analytics işinde belirttiğiniz olanları olarak aynı ada sahip bir tablo sahipse, var olanları üzerine yazılır.
    > Siz açıkça bu veri kümesi ve tablo Power BI hesabınızı oluşturmamanızı öneririz. İş pompa çıkış Power BI'da oturum başlatır ve akış analizi işi başlattığınızda otomatik olarak oluşturulur. Sorgu iş herhangi bir sonuç döndürmezse, veri kümesi ve tablo oluşturulmaz.
    >

8. **Oluştur**’a tıklayın.

Veri kümesi aşağıdaki ayarlara sahip oluşturulur:

* **defaultRetentionPolicy: BasicFIFO**: FIFO, en çok 200.000 satırları içeren verilerdir.
* **defaultMode: pushStreaming**: akış döşeme ve geleneksel rapor tabanlı görsel veri kümesi (paketini destekler anında iletme).

Şu anda diğer bayraklarıyla veri kümesi oluşturulamıyor.

Power BI veri kümeleri hakkında daha fazla bilgi için bkz: [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) başvuru.


## <a name="write-the-query"></a>Sorgu yazma

1. Kapat **çıkışları** dikey ve iş dikey penceresine dönün.

2. Tıklatın **sorgu** kutusu. 

3. Aşağıdaki sorgu girin. Bu sorgu, sahtekarlık algılama öğreticide oluşturduğunuz kendi kendine birleşim sorgu benzerdir. Bu sorgu sonuçları, oluşturduğunuz yeni çıkış gönderir farktır (`CallStream-PowerBI`). 

    >[!NOTE]
    >Giriş değil adlandırırsanız `CallStream` sahtekarlık algılama öğreticide için adınızı alternatif `CallStream` içinde **FROM** ve **katılma** sorgu yan tümcelerinde.

        /* Our criteria for fraud:
        Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where the switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. **Kaydet**’e tıklayın.


## <a name="test-the-query"></a>Sorgu testi
Bu bölümde, isteğe bağlı ancak önerilir. 

1. TelcoStreaming uygulama şu anda çalışmıyorsa başlatın, aşağıdaki adımları izleyerek:

    * Bir komut penceresi açın.
    * Değiştirilen telcodatagen.exe.config dosya ve telcogenerator.exe olduğu klasöre gidin.
    * Şu komutu çalıştırın:

            telcodatagen.exe 1000 .2 2

2. İçinde **sorgu** dikey penceresinde nokta tıklayın `CallStream` girin ve ardından **örnek giriş verilerinden**.

3. Veri ve tıklatın üç dakika eşitleyeceğini istediğinizi belirtin **Tamam**. Veri örneklendirilen haberdar kadar bekleyin.

4. Tıklatın **Test** ve sonuçları alınırken emin olun.


## <a name="run-the-job"></a>İşini çalıştır

1. TelcoStreaming uygulama çalıştığından emin olun.

2. Kapat **sorgu** dikey.

3. İş dikey penceresinde tıklayın **Başlat**.

    ![Stream Analytics işi Başlat](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Gelen akış sahte çağrılarında arayan, akış analizi işi başlatır. İş ayrıca veri kümesi ve tablo Power BI'da oluşturur ve bunları sahte çağrıları hakkında veri gönderme başlatır.


## <a name="create-the-dashboard-in-power-bi"></a>Power BI'da pano oluşturun

1. Git [Powerbi.com](https://powerbi.com) ve iş veya Okul hesabınızla oturum açın. Akış analizi işi sorgu sonuçları çıkışı yapıyorsa, veri kümesi zaten oluşturulmuş bakın:

    ![Power bı'da akış veri kümesi](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. Çalışma alanınızda tıklatın  **+ &nbsp;oluşturma**.

    ![Power BI çalışma alanında oluştur düğmesi](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Yeni bir pano oluşturun ve adlandırın `Fraudulent Calls`.

    ![Bir pano oluşturun ve Power BI çalışma alanında bir ad verin](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Pencerenin üstündeki **Ekle döşeme**seçin **özel akış veri**ve ardından **sonraki**.

    ![Özel veri kümesi akış](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. Altında **bilgisayarınızı DATSETS**, kümenizi seçin ve ardından **sonraki**.

    ![Akış Veri kümenizi](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. Altında **görselleştirme türü**seçin **kartı**ve ardından **alanları** listesinde **fraudulentcalls**.

    ![Yeni kutucuk için görselleştirme ayrıntıları](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. **İleri**’ye tıklayın.

8. Başlık ve alt başlık gibi kutucuğu ayrıntıları doldurun.

    ![Başlık ve yeni kutucuk için alt başlığı](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. **Uygula**'ya tıklayın.

    Şimdi bir sahtekarlık sayaç var!

    ![Sahtekarlık sayacı](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. Yeniden (4. adım ile başlayarak) bir kutucuk eklemek için adımları izleyin. Bu süre, aşağıdakileri yapın:

    * Ulaştığınızda **görselleştirme türü**seçin **çizgi grafiği**. 
    * Eksen ekleme ve seçin **windowend**. 
    * Bir değer ekleyip seçin **fraudulentcalls**.
    * İçin **zaman penceresini görüntülemek için**, son 10 dakika seçin.

    ![Bölme çizgi grafiği için oluşturma](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Tıklatın **sonraki**, başlık ve alt başlık eklemek ve tıklayın **Uygula**.

    Power BI panosunu şimdi, iki veri görünümlerini sahte çağrıları hakkında akış verilerinde algılanan olarak sağlar.

    ![Power BI panosuna sahte çağrıları için iki kutucuk gösteren tamamlandı](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Power BI hakkında bilgi alın

Bu öğretici görselleştirmeleri bir veri kümesi için yalnızca birkaç türlü oluşturmak nasıl gösterir. Power BI, kuruluşunuzun diğer müşteri iş zekası araçları oluşturmanıza yardımcı olabilir. Daha fazla fikir için aşağıdaki kaynaklara bakın:

* Power BI panosuna başka bir örneği için izleme [Power BI ile çalışmaya başlama](https://youtu.be/L-Z_6P56aas?t=1m58s) video.
* Power BI çıkışı iş akış analizi yapılandırma hakkında daha fazla bilgi ve Power BI gruplarını kullanarak gözden [Power BI](stream-analytics-define-outputs.md#power-bi) bölümünü [Stream Analytics çıkarır](stream-analytics-define-outputs.md) makalesi. 
* Power BI genellikle kullanma hakkında daha fazla bilgi için bkz: [Power BI panoları](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Sınırlamalar ve en iyi uygulamalar hakkında bilgi edinin
Şu anda Power BI kabaca saniye başına bir kez çağrılabilir. Akış görselleri 15 KB paketlerini destekler. Ötesinde, akış görselleri başarısız (ancak anında çalışmaya devam eder). Bu sınırlamalar nedeniyle Power BI kendisini en doğal burada Azure akış analizi önemli veri yükünü azaltma mu çalışmaları için uygundur. Veri gönderimi en fazla saniyede bir itme olduğunu ve sorgunuzu içinde üretilen iş gereksinimlerini adlandırıldığını emin olmak için bir atlayan pencere ya da Hopping penceresini kullanmanızı öneririz.

Saniye cinsinden pencerenizi vermek için değeri hesaplamak için aşağıdaki eşitliği kullanabilirsiniz:

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Örneğin:

* Verileri bir saniyelik aralıklarda gönderme 1.000 cihazları var.
* Power BI Pro saat başına 1.000.000 satır destekleyen SKU kullanıyor.
* Aygıt başına ortalama veri miktarı için Power BI yayımlamak istediğiniz.

Sonuç olarak, denklemi olur:

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Bu yapılandırmayı göz önüne alarak, özgün sorgu aşağıdaki değiştirebilirsiniz:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a>Yetkilendirmeyi yenile
Parola işinizi oluşturulmuş veya son kimliği doğrulanmış oluşturulmasından sonra değiştirilmişse, Power BI hesabınızı yeniden kimlik doğrulamaya gerekir. Azure Active Directory (Azure AD) kiracınızın Azure çok faktörlü kimlik doğrulaması yapılandırılmışsa, ayrıca Power BI yetkilendirme iki haftada yenilemek gerekir. Yenilemezseniz, iş çıktısı eksikliği gibi Belirtiler görebilir veya bir `Authenticate user error` işlem günlüğüne kaydeder.

Benzer şekilde, belirtecin süresi dolduktan sonra bir iş başlatılır, bir hata oluşur ve işi başarısız olur. Bu sorunu çözmek için çalışan işini durdurmak ve Power BI çıktısına gidin. Veri kaybını önlemek için seçin **yetkilendirmeyi yenileyin** işinin dışında yeniden başlatın ve bağlama **son durdurulma zamanı**.

Power BI ile yetkilendirme yenilendikten sonra sorunu Çözümlendi yansıtacak şekilde yetkilendirme alanında yeşil bir uyarı görüntülenir.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
