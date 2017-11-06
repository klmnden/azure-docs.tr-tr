---
title: "Visual Studio için Azure Stream Analytics araçlarını kullanın | Microsoft Docs"
description: "Başlangıç öğreticisi için Visual Studio için Azure Stream Analytics araçları"
keywords: Visual studio
documentationcenter: 
services: stream-analytics
author: su-jie
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: sujie
ms.openlocfilehash: 8e3f1ae6739896dfd1329561dbcede38a6069546
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Visual Studio için Azure Stream Analytics araçlarını kullanın
Visual Studio için Azure Stream Analytics araçları artık genel olarak kullanılabilir. Bu araçları yanı sıra gidermek karmaşık sorgular yazmak Stream Analytics kullanıcılar için daha zengin bir deneyim sağlamak ve hatta sorguları yerel olarak yazma. Ayrıca, bir Visual Studio projeye Stream Analytics işi dışa aktarabilirsiniz.

## <a name="introduction"></a>Giriş
Bu öğreticide, Visual Studio için Stream Analytics araçları oluşturmak, yazar, yerel olarak test, yönetmek ve akış analizi işleri hata ayıklamak için nasıl kullanılacağını öğrenin. 

Bu öğreticiyi tamamladıktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* Visual Studio için Stream Analytics araçları edinin.
* Yapılandırın ve akış analizi işi dağıtın.
* İşinizi yerel örnek verilerle yerel olarak test edin.
* Sorunları gidermek için izleme'yi kullanın.
* Var olan işleri projelerine verin.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşullar gerekir:

* Öğreticide adımları "Stream Analytics işi oluştur" en son [akış analizi kullanarak bir IOT çözümü derleme](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics). 
* Visual Studio 2017, Visual Studio 2015 ve Visual Studio 2013 güncelleştirme 4 yükleyin. Enterprise (Ultimate/Premium), Professional ve topluluk sürümleri desteklenir. Express sürümü desteklenmiyor. 
* İzleyin [yükleme yönergeleri](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install) Visual Studio için Stream Analytics araçlarını yüklemek için.

## <a name="create-a-stream-analytics-project"></a>Stream Analytics projesi oluşturma
Visual Studio'da seçin **dosya** > **yeni proje**. Sol taraftaki şablonları listesinden seçin **Stream Analytics**ve ardından **Azure Stream Analytics uygulaması**.
Sayfanın alt kısmındaki proje giriş **adı**, **konumu**, ve **çözüm adı** için diğer projeler yaptığınız gibi.

![Yeni proje oluşturma](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

Proje **Ücretli** üretildi **Çözüm Gezgini**.

![Çözüm Gezgini'nde Ücretli projesi](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a>Doğru abonelik seçin
1. Üzerinde **Görünüm** menüsünde, select **Sunucu Gezgini** Visual Studio.

2. Azure hesabınızla oturum açın. 

## <a name="define-input-sources"></a>Giriş kaynağı tanımlayın
1. İçinde **Çözüm Gezgini**, genişletin **girişleri** düğümü ve yeniden adlandırma **Input.json** için **EntryStream.json**. Çift **EntryStream.json**.

2. İçin **giriş diğer adı**, girin **EntryStream**. Giriş diğer adı sorgu komut dosyasında kullanıldığını unutmayın.

3. İçin **kaynak türü**seçin **veri akışı**.

4. İçin **kaynak**seçin **olay hub'ı**.

5. İçin **Service Bus Namespace**seçin **TollData** aşağı açılan listede seçeneği.

6. İçin **olay hub'ı adı**seçin **girişi**.

7. İçin **olay hub'ı ilke adı**seçin **RootManageSharedAccessKey** (varsayılan değer).

8. İçin **olayı seri hale getirme biçimi**seçin **Json**ve **kodlama**seçin **UTF8**.
   
   Ayarlarınızı şöyle görünür:
   
   ![Giriş ayarları](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
   
9. Sayfanın alt kısmındaki seçin **kaydetmek** Sihirbazı tamamlamak için. Artık çıkış akışı oluşturmak için başka bir giriş kaynağı ekleyebilirsiniz. Sağ **girişleri** düğümü ve select **yeni öğe**.
   
   ![Yeni öğe](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
   
10. Açılan pencerede seçin **Stream Analytics giriş**, değiştirip **adı** için **ExitStream.json**. **Add (Ekle)** seçeneğini belirleyin.
   
    ![Yeni Öğe Ekle](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
   
11. Çift **ExitStream.json** proje ve alanları doldurmak için giriş akışı olarak aynı adımları izleyin. İçin **olay hub'ı adı**, girilmelidir **çıkmak**aşağıdaki ekran görüntüsünde gösterildiği gibi:
   
    ![ExitStream ayarları](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)
   
   Şimdi iki giriş akışları tanımladınız.
   
   ![İki giriş akışları](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
   
   Ardından, car kayıt verileri içeren blob dosyası için başvuru veri girişi ekleyin.
   
12. Sağ **girişleri** düğümünü proje ve aynı işlemi akış girdileri sonra izleyin. İçin **kaynak türü**seçin **başvuru verileri**ve **giriş diğer adı**, girin **kayıt**.
   
    ![Kayıt ayarları](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)
   
13. Seçin **depolama** seçeneğiyle içeren hesabını **TollData**. Kapsayıcı adı **TollData**ve **yol deseni** olan **registration.json**. Bu dosya adı büyük/küçük harfe duyarlıdır ve küçük harf olmalıdır.

14. Seçin **kaydetmek** Sihirbazı tamamlamak için.

Şimdi tüm girişleri tanımlanır.

## <a name="define-output"></a>Çıktı tanımlayın
1. İçinde **Çözüm Gezgini**, genişletin **girişleri** düğümü ve çift **Output.json**.

2. İçin **çıkış diğer adları**, girin **çıkış**. İçin **havuzu**seçin **SQL veritabanı**.

3. İçin **veritabanı** adı, girin **TollDataDB**.

4. İçin **kullanıcı adı**, girin **tolladmin**. İçin **parola**, girin **123toll!**. İçin **tablo**, girin **TollDataRefJoin**.

5. **Kaydet**’i seçin.

   ![Çıkış ayarları](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="stream-analytics-query"></a>Stream Analytics sorgusu
Bu öğreticide, veri hat ilgili birkaç iş soruları yanıtlamak çalışır. Biz, Stream Analytics ilgili yanıt vermek için kullanılan sorgu oluşturulur. İlk Stream Analytics işiniz başlamadan önce basit bir senaryoyu ve sorgu söz dizimi inceleyelim.

### <a name="introduction-to-stream-analytics-query-language"></a>Stream Analytics sorgu dili giriş
Ücretli Stand girin taşıtlardan sayısını gerektiğini varsayalım. Bu akış olayların sürekli olduğundan, bir süre tanımlamak zorunda. "Kaç taşıtlardan üç dakikada Ücretli Stand girin?" olarak soru değiştirelim Bu ölçüm, genellikle dönen sayım olarak da adlandırılır.

Bu soruya yanıt veren Stream Analytics sorgu bakalım:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

Gördüğünüz gibi akış analizi gibi SQL sorgu dili kullanır. Sorgu saati ile ilgili yönlerini belirlemek için birkaç uzantıları ekler.

Hakkında daha fazla ayrıntı için okuma [süresi management](https://msdn.microsoft.com/library/azure/mt582045.aspx) ve [Pencereleme](https://msdn.microsoft.com/library/azure/dn835019.aspx) MSDN'den sorguda kullanılan yapılar.

İlk Stream Analytics sorgu yazılmış, TollApp klasörü şu yol içinde yer alan örnek veri dosyalarını kullanarak test edin:

**..\TollApp\TollApp\Data**

Bu klasör, aşağıdaki dosyaları içerir:

* Entry.JSON
* Exit.JSON
* Registration.JSON

## <a name="question-number-of-vehicles-entering-a-toll-booth"></a>Soru: Ücretli Stand girme taşıtlardan sayısı
Projede çift **Script.asaql** Kod düzenleyicisinde açın. Komut dosyası önceki bölümde düzenleyiciye yapıştırın. Sorgu Düzenleyicisi'ni IntelliSense, söz dizimi renklendirme ve bir hata işaretleyici destekler.

![Sorgu Düzenleyicisi](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>Stream Analytics sorguları yerel olarak test etme
Herhangi bir sözdizimi hatası olup olmadığını görmek için sorguyu ilk derleyebilirsiniz.

1. Bu örnek verileri sorgusu doğrulamak için yerel örnek verileri giriş sağ tıklayıp seçerek kullanın **yerel girdisi eklemek**.
   
   ![Yerel giriş Ekle](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
   
2. Açılan pencerede, yerel yolundan örnek verileri seçin. **Kaydet**’i seçin.
   
   ![Yerel giriş Ekle](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
   
   Adlı bir dosya **local_EntryStream.json** girişleri klasörünüze otomatik olarak eklenir.
   
   ![Yerel giriş klasörü dosya listesi](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
   
3. Seçin **yerel olarak çalıştır** sorgu Düzenleyicisi'nde. Veya F5 tuşuna basın.
   
   ![Yerel olarak çalıştırma](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)
   
   Konsol çıktısı çıkış yolunda bulabilirsiniz. Sonuç klasörünü açmak için herhangi bir tuşa basın.
   
   ![Yerel çalıştırma](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)
   
4. Yerel klasör içinde sonuçlarını denetleyin.
   
   ![Yerel klasör sonucu](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-03.png)
   
   
### <a name="sample-input"></a>Örnek Giriş
Yerel dosya giriş kaynaklardan giriş verilerini de örnek oluşturabilirsiniz. Giriş yapılandırma dosyasını sağ tıklatın ve seçin **örnek verileri**. 

![Örnek Veri](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

Yalnızca olay hub'ları veya IOT hub'ları şu an için örnek oluşturabilirsiniz olduğunu unutmayın. Diğer giriş kaynağı desteklenmez. Açılan iletişim kutusunda örnek verileri kaydetmek için yerel yolunda doldurun. Seçin **örnek**.

![Örnek veri yapılandırması](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
Devam eden görebilirsiniz **çıkış** penceresi. 

![Örnek veri çıkışı](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a>Azure Stream Analytics sorgu Gönder
1. İçinde **sorgu Düzenleyicisi'ni**seçin **göndermek için Azure** komut dosyası Düzenleyicisi'nde.

   ![Azure'a Gönder](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Seçin **yeni bir Azure Stream Analytics işi oluşturmak**. İçin **iş adı**, girin **TollApp**. Doğru seçin **abonelik** aşağı açılan listesinde. Seçin **gönderme**.

   ![İşi gönderin](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-the-job"></a>İşi Başlat
İşinizi şimdi oluşturulur ve iş görünümünde otomatik olarak açılır. 
1. İşlemi başlatmak için yeşil ok düğmesini seçin.

   ![Başlat iş düğmesi](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Varsayılan ayar seçip **Başlat**.
 
   ![İşi Başlat](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

   Dönüştürülen iş durumunu görebilir **çalıştıran**, ve giriş/çıkış olay vardır.

   ![İş özeti ve ölçümler](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-results-in-visual-studio"></a>Visual Studio'da denetim sonuçları
1. Visual Studio sunucu Gezgini'ni açın ve sağ **TollDataRefJoin** tablo.

2. Seçin **Show Table Data** işinizi çıkışını görmek için.
   
   ![Tablo verileri göster](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)
   

### <a name="view-job-metrics"></a>İş metrikleri görüntüleyin
Bazı temel iş istatistiklerinde gösterilen **iş ölçümleri**. 

![İş ölçümleri](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a>Sunucu Gezgininde iş listesi
İçinde **Sunucu Gezgini**seçin **akış analizi işleri** ve ardından **yenileme**. İşinizi altında görünür **akış analizi işleri**.

![İşlerini listesi](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a>Proje görünümü Aç
İş düğümünü genişletin ve çift **iş görünümünde** iş görünümünde açmak için düğüm.

![İşi görüntüle](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a>Var olan bir projeyi
Var olan bir projeye verebilirsiniz iki yolu vardır.
* İçinde **Sunucu Gezgini**altında **akış analizi işleri** düğümü, proje düğümüne sağ tıklayın. Seçin **verme yeni Stream Analytics projeye**.
   
   ![Yeni Stream Analytics projesine aktarma](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)
   
   Oluşturulan proje görünür **Çözüm Gezgini**.
   
    ![Çözüm Gezgini proje](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
   
* Proje Görünümü'nde seçin **proje oluştur**.
   
   ![Proje oluştur](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)
   
## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar
 
* Sorgunuz coğrafi uzamsal işlevleri varsa, yerel test işe yaramaz.
* Düzenleyici destek ekleyerek veya değiştirerek JavaScript UDF için kullanılamaz.
* Yerel test çıktı JSON biçiminde kaydetme desteklemiyor. 
* Destek, Power BI çıkışı ve ADLS çıktı için kullanılamaz.



## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Stream Analytics'i kullanmaya başlama](stream-analytics-get-started.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)


