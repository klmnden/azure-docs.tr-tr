---
title: Oluşturun, görüntüleyin ve yönetin Azure İzleyici'yi kullanarak uyarıları
description: Yazar, görüntülemek ve ölçüm yönetmek ve tek bir yerden günlük uyarısı kuralları için yeni birleşik Azure uyarı deneyiminden kullanın.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: a913075c051c6b784495917b7edbd7340254a212
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952639"
---
# <a name="create-view-and-manage-alerts-using-azure-monitor"></a>Oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme  

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portalı içinde yeni uyarılar arabirimini kullanarak uyarıları ayarlama işlemini göstermektedir. Bir uyarı kuralı tanımı üç bölümlerinde verilmiştir:
- Hedef: izlenecek belirli Azure kaynağı
- Ölçüt: Belirli bir koşulu veya mantıksal, sinyalin görülen, tetikleyici
- Eylem: Bir - bildirim alıcısı için belirli çağrı gönderilen, SMS, Web kancası vb. e-posta.

Azure uyarıları, ayrıca tüm uyarı kuralları ve bunları tek bir yerde yönetme becerisini birleşik bir görünümünü sağlar. Çözümlenmemiş tüm uyarıları görüntüleme dahil olmak üzere. İşlevi hakkında daha fazla bilgi [Azure uyarıları - genel bakış](monitoring-overview-unified-alerts.md).

Uyarı kullanan terimi **günlük uyarıları** dayalı özel sorgu olduğu sinyal uyarılarını açıklamak için [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-analytics.md). [Yeni ölçüm uyarısı özelliğinin](monitoring-near-real-time-metric-alerts.md) uyarı yeteneği sağlar [çok boyutlu ölçümler](monitoring-metric-charts.md) belirli Azure kaynakları için. Bu kaynak için uyarıları oluşturma boyutlara ek filtreler kullanabilirsiniz **çok boyutlu ölçüm uyarıları**.


> [!NOTE]
> Azure uyarılarını Azure'da uyarılar oluşturmak için yeni ve geliştirilmiş bir deneyim sunar. Varolan [uyarılar (Klasik)](monitoring-overview-alerts.md) deneyimi kullanılabilir kalır.
>

Ayrıntılı sonraki Azure Uyarıları'nı kullanarak adım adım kılavuzdur.

## <a name="create-an-alert-rule-with-the-azure-portal"></a>Azure Portal ile bir uyarı kuralı oluşturma
1. İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.  
    ![İzleme](./media/monitor-alerts-unified/AlertsPreviewMenu.png)

2. Seçin **yeni uyarı kuralı** Azure'da yeni bir uyarı oluşturmak için.
    ![Uyarı Ekle](./media/monitor-alerts-unified/AlertsPreviewOption.png)

3. Uyarı oluşturma bölümünde, üç bölümden oluşan ile gösterilir: *uyarı koşulunu tanımlama*, *uyarı ayrıntılarını tanımlama*, ve *tanımla eylem grubu*.

    ![Kural oluşturma](./media/monitor-alerts-unified/AlertsPreviewAdd.png)

4.  Kullanarak uyarı koşulunu tanımlama **seçin kaynak** bağlantı ve hedef bir kaynak seçerek belirtme. Filtre belirleyerek * abonelik * kaynak türüne ve son olarak seçerek gerekli *kaynak*.

    >[!NOTE]

    > Devam etmeden önce seçili kaynak için sinyal doğrulayın.

    ![Kaynak seçin](./media/monitor-alerts-unified/Alert-SelectResource.png)

 Kullanıcı arabirimi, çeşitli uyarı türleri tek bir yerden oluşturmanıza olanak sağlar. İstenen uyarı türüne göre sonraki adım olarak seçin:

    - İçin **ölçüm uyarıları**: 5-7. adımları uygulayın; ardından doğrudan 11. adıma gidin
    - İçin **günlük uyarıları**, 8. adımına atlayabilirsiniz.

    > [!NOTE]

    > Etkinlik günlüğü uyarıları da desteklenir, ancak Önizleme aşamasındadır. [Daha fazla bilgi edinin](monitoring-activity-log-alerts-new-experience.md).

5. *Ölçüm uyarıları*: olun **kaynak türü** sinyal türü ile seçilen **ölçüm**sonra bir kez uygun **kaynak** tıklatın seçilir  *Bitti* oluşturma uyarısı döndürülecek düğmesi. Ardından **Ölçüt Ekle** daha önce seçilen kaynak için kullanılabilen sinyal seçenekleri, izleme hizmeti ve listelenen - türü listesinden belirli bir sinyal seçmek için düğme.

    ![Bir kaynak seçin](./media/monitor-alerts-unified/AlertsPreviewResourceSelection.png)

    > [!NOTE]

    >  Tüm [neredeyse gerçek zamanlı uyarılar](monitoring-near-real-time-metric-alerts.md) olabilecek kaynakların İzleyici hizmetiyle listelenen **Platform** ve sinyal türü olarak **ölçüm**

6. *Ölçüm uyarıları*: sinyal seçtikten sonra uyarı vermek için mantık belirtilebilir. Başvuru için geçmiş veri sinyali zaman penceresini kullanarak ince seçeneğiyle gösterilir **Geçmişi Göster**, geçen hafta için son altı saat farklı. Bir yerde görselleştirme ile **Alert Logic** koşulu, toplama ve son olarak eşiği gösterilen seçeneklerden seçilebilir. Sağlanan mantığı önizleme olarak uyarı tetiklemesi belirten koşul sinyal geçmişi yanı sıra görselleştirme gösterilir. Son olarak hangi süre için uyarı için belirtilen koşul seçerek nasıl olması gerektiğini belirlemek **süresi** ne sıklıkta uyarı seçerek çalışması gereken birlikte seçeneği **sıklığı**.

    ![Ölçüm için sinyal mantığını yapılandırma](./media/monitor-alerts-unified/AlertsPreviewCriteria.png)

7. *Ölçüm uyarıları*: varsa sinyaldir bir ölçüm ve ardından söz konusu Azure kaynağı için birden çok veri noktaları veya boyutları kullanarak uyarı pencere filtrelenebilir. 

    a. Bir süreye seçin **Geçmişi Göster** farklı bir süre görselleştirmek için açılır. Zaman serisinde bulunan filtre uygulamak desteklenen ölçümler için boyut seçebilirsiniz; Boyutları seçme isteğe bağlıdır ve yukarı-beş boyutlarının kullanılabilir. 

    b. **Uyarı mantığı** gösterilen seçenekler arasından seçilebilecek *koşul*, *, toplama ve *eşiği*. Sağlanan mantıksal önizleme olarak, geçmişte uyarı Tetiklenmiş zamanı belirten koşul görselleştirmeyi sinyal geçmişi ile birlikte gösterilir. 

    c. Süreyi belirtmek için **süresi** uyarı seçerek ne sıklıkla birlikte çalışması gereken **sıklığı**.

    ![Çok boyutlu ölçüm için sinyal mantığını yapılandırma](./media/monitor-alerts-unified/AlertsPreviewCriteriaMultiDim.png)

8. *Günlük uyarıları*: olun **kaynak türü** gibi bir analytics kaynak *Log Analytics* veya *Application Insights* ve sinyal türü olarak **günlüğü** sonra bir kez uygun **kaynak** olduğundan seçilen, tıklayın *Bitti*. Ardından **Ölçüt Ekle** sinyal sinyal listeden ve kaynak için kullanılabilir seçenekler listesini görüntüleyin düğmesine **özel günlük araması** seçilen gibi hizmetini izleme günlüğü için seçenek *günlük Analytics* veya *Application Insights*.

   ![Kaynak - özel bir günlük araması'nı seçin](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Liste sinyal türü - analytics sorgusuna alma uyarılar **günlük (kayıtlı sorgu)**, çizimde görüldüğü gibi. Böylece kullanıcılar Analytics sorgunuzda mükemmel ve gelecekte kullanılmak üzere uyarılar - kaydetmek daha fazla ayrıntı bulunabilir sorgu kaydetme kullanarak [log analytics'te günlük arama özelliğini kullanarak](../log-analytics/log-analytics-log-searches.md) veya [application ınsights'ta paylaşılan sorgu Analytics](../log-analytics/log-analytics-overview.md). 

9.  *Günlük uyarıları*: içinde seçildiğinde, uyarı için sorgu belirtilebilir **arama sorgusu** sorgu söz dizimi yanlışsa alanda hata kırmızı renkte görüntülenir; alan. Sorgu Sözdizimi doğruysa - başvuru için belirtilen sorgu geçmiş veri son altı saat zaman penceresinden geçen hafta için ince seçeneğiyle bir grafik olarak gösterilir.

 ![Uyarı kuralını yapılandırın](./media/monitor-alerts-unified/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Geçmiş verileri görselleştirme, sorgu sonuçları saati ayrıntıları varsa yalnızca gösterilebilir. Özetlenmiş veriler veya belirli bir sütun değerleri -, sorgu sonuçları aynı tekil bir çizim gösterilir.

    >  Application ınsights'ı kullanarak günlük uyarıları ölçüm ölçüsü türü için kullanarak verileri gruplandırmak için hangi belirli bir değişken belirtebilirsiniz **bulunan** ; aşağıda gösterildiği gibi seçeneği:

    ![toplama seçeneği](./media/monitor-alerts-unified/aggregate-on.png)

10.  *Günlük uyarıları*: bir yerde görselleştirme ile **Alert Logic** koşulu, toplama ve son olarak eşiği gösterilen seçeneklerden seçilebilir. Son olarak mantığında belirtin belirtilen koşulun değerlendirme süresi kullanarak **süresi** seçeneği. Uyarı seçerek ne sıklıkta çalıştırılacağını birlikte **sıklığı**.
İçin **günlük uyarıları** uyarılar temelinde:
   - *Kayıt sayısı*: sorgu tarafından döndürülen kayıt sayısını büyüktür veya belirtilen değerden daha az ise bir uyarı oluşturulur.
   - *Ölçüm ölçüsü*: her değilse bir uyarı oluşturulur *toplam değer* sonuçlarda sağlanan eşik değerini aşıyor ve bu *göre gruplandırılmış* değer seçildi. Seçili zaman aralığındaki eşiğini kaç kez uyarısının ihlallerini sayısıdır. Sonuç kümesi veya ihlallerini ardışık örnekler içinde gerçekleşmelidir gerektirecek şekilde ardışık ihlaller genelinde toplam ihlal sayısı için herhangi bir birleşimini ihlallerini belirtebilirsiniz. Daha fazla bilgi edinin [günlük uyarıları ve bunların türlerini](monitor-alerts-unified-log.md).

    > [!TIP]
    > Uyarılar - günlük arama uyarıları şu anda özel sürebilir *süresi* ve *sıklığı* dakika değeri. Değerleri 1440 dakika 24 saat (yani) 5 dakika arasında değişebilir. Uyarı dönemi deyin üç saat olmasını istiyorsanız, bu nedenle dönüştürmeden dakika içinde - kullanmadan önce 180 dakika

11. İkinci bir adım olarak, bir ad, uyarının tanımlayın **uyarı kuralı adı** alanı ile birlikte bir **açıklama** uyarı özellikleri gerçekleşen ve **önem derecesi** değerini Sağlanan seçenekler. Bu ayrıntılar, tüm uyarı e-postaları, bildirimleri veya Azure İzleyici tarafından gerçekleştirilen anında iletme tanımlarlar. Kullanıcı uygun şekilde değiştirerek uyarı kuralı oluşturma hemen etkinleştirmeye ek olarak, seçebilir **oluşturulduktan sonra kuralı etkinleştir** seçeneği.

    İçin **günlük uyarıları** yalnızca, bazı ilave işlevler uyarı ayrıntıları kullanılabilir:

    - **Uyarıları bastır**: gizleme için uyarı kuralı etkinleştirdiğinizde, Eylemler kural için yeni bir uyarı oluşturduktan sonra tanımlı bir süre için devre dışı bırakılır. Kural hala çalışıyor ve ölçütler karşılanıyorsa sağlanan uyarı kayıtları oluşturur. Yinelenen eylemler çalıştırmadan sorunu düzeltmek için izin verme size zaman.

        ![Günlük uyarıları için uyarıları bastır](./media/monitor-alerts-unified/AlertsPreviewSuppress.png)

        > [!TIP]
        > Bildirimleri çakışma durdurulduğundan emin olmak için uyarı sıklığını büyüktür bastır bir uyarı değerini belirtin

12. Üçüncü ve son adım olarak belirtmek varsa **eylem grubu** uyarı kuralı için Uyarı koşulu karşılandığında tetiklenen gerekir. Uyarı ile mevcut bir eylem grubu seçin veya yeni bir eylem grubu oluşturun. Şunlara göre bir uyarı tetikleyicisi Azure olur olduğunda bir eylem grubu seçili: email(s) göndermek, SMS(s) göndermek, Web kancası çağrısı, Azure runbook'ları, anında iletme ITSM aracına, vb. kullanarak düzeltin. Daha fazla bilgi edinin [Eylem grupları](monitoring-action-groups.md).

    İçin **günlük uyarıları** bazı ilave işlevler şu varsayılan eylemleri geçersiz kılmak kullanılabilir:

    - **E-posta bildirimi**: geçersiz kılan *e-posta konusu* eylem grubu aracılığıyla; söz konusu eylem grubuna bir veya daha fazla e-posta eylemleri varsa gönderilen e-postadaki. Postanın gövdesini değiştiremezsiniz ve bu alan **değil** e-posta adresi.
    - **Özel Json yükü dahil**: söz konusu eylem grubuna bir veya daha fazla Web kancası işlemleri mevcut Web kancası Eylem grupları tarafından; kullanılan JSON geçersiz kılar. Kullanıcı, ilişkili eylem grubunda yapılandırılmış tüm Web kancaları için kullanılmak üzere JSON biçimi belirtebilirsiniz; Web kancası biçimleri hakkında daha fazla bilgi için bkz. [günlük uyarıları için Web kancası eylemi](monitor-alerts-unified-log-webhook.md). Test Web kancası seçeneği, biçimi ve işleme örnek JSON kullanarak hedef tarafından denetlenecek sağlanır ve etiketli gibi bu seçenek yalnızca kuruluşunuz **test** amaçlar.

        ![Günlük uyarıları için eylem geçersiz kılmaları](./media/monitor-alerts-unified/AlertsPreviewOverrideLog.png)

        > [!NOTE]
        > İçin **Test Web kancası** çalışmaya seçeneğinde, uç nokta desteklemelidir [Cross Origin kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/) ve kullanıcının "Access-Control-Allow-Origin üst bilgisi" sorunları almak için CORS Ara kullanabilir

13. Tüm alanları geçerliyse ve yeşil onay **uyarı kuralı oluşturma** düğmesini ve Azure İzleyici - uyarılar bir uyarı oluşturulur. Tüm uyarıları Pano uyarılardan görüntülenebilir.

    ![Kural oluşturma](./media/monitor-alerts-unified/AlertsPreviewCreate.png)

    Birkaç dakika içinde uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="view-your-alerts-in-azure-portal"></a>Azure portalında uyarıları görüntüleme

1. İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.  

2. **Uyarılar Panosu** görüntülenen - tekil panosunda görüntülenen tüm Azure uyarıları burada görüntülerle birleşik ve ![uyarı Panosu](./media/monitoring-alerts-unified-usage/alerts-preview-overview.png)
3. Üst soldan sağa, panoyu ayrıntılı bir listesi görmek üzere tıklanabilecek aşağıdaki - bir bakışta gösterir:
    - *Tetiklenen uyarılar*: mantıksal karşılanması ve içinde durum harekete sahip uyarılar şu anda sayısı
    - *Toplam uyarı kuralları*: oluşturulan uyarı kuralları ve alt, şu anda etkin numarası sayısı 
    

        > [!NOTE]
        > Günlük uyarıları için application ınsights ve log analytics de dahil olmak üzere tüm tetiklenen uyarılar hakkında ayrıntılar içeren tutarlı Pano sağlamak; [Geliştirilmiş birleşik uyarı (Önizleme)](monitoring-overview-unified-alerts.md#enhanced-unified-alerts-public-preview) kullanılmalıdır
  
  
4. Tüm tetiklenen uyarılar listesini, kullanıcının ayrıntılarını görüntülemek için tıklayabileceği gösterilir.
5. Bulma belirli uyarıları kuruluşlara yardımcı olmaktayız; bir kullanabileceğiniz üstteki açılan seçenekleri belirli bir filtreleme için *abonelik, kaynak grubu ve/veya kaynak*. Çözümlenmemiş herhangi bir uyarı için bir kullanım başka *filtre uyarı* seçeneği bulmak için sağlanan anahtar sözcüğü - belirli eşleşen uyarılarla *adı, Uyarı ölçütleri, kaynak grubu ve hedef kaynak*

## <a name="managing-your-alerts-in-azure-portal"></a>Azure portalındaki uyarılarınızı yönetme
1. İçinde [portalı](https://portal.azure.com/)seçin **İzleyici** ve izleme bölümü altında - **uyarılar**.  
2. Seçin **yönetme kuralları** oluşturulan uyarı kurallarının tümünü burada listelenen kural Yönetim bölümüne - gitmek için üst taraftaki çubukta, düğmesine; devre dışı bırakıldı uyarıları da dahil olmak üzere.
3. Özel uyarı kuralları için bulmak için bir ya da açılan filtreleri shortlist uyarı kuralları için özel izin en üste kullanabilirsiniz *abonelik, kaynak grupları ve/veya kaynak*. Yukarıdaki uyarı kuralı listesi bölmesinde arama'yı kullanarak üzerinde alternatif olarak işaretlenmiş *filtre uyarıları*, bir anahtar sözcüğü karşı eşleşen sağlayabilir *uyarı adı, koşul ve hedef kaynak*; yalnızca göstermek için eşleştirme kuralları.
   ![Uyarı kurallarını yönet](./media/monitoring-alerts-unified-usage/alerts-preview-rules.png)
4. Tıklatılabilir bir bağlantı gösterilen adını görüntülemek veya belirli bir uyarı kuralı değiştirmek için tıklayın.
5. Tanımlanan bir uyarı gösterilir - içinde üç aşama yapısı: 1) uyarı durumu 2) uyarı ayrıntısı (3) eylem grubu. **Hedef ölçütleri** uyarı mantığı ya da yeni bir değiştirmek üzere tıklanabilecek ölçütleri Kutusu'nu önceki mantığı silmek için kullandıktan sonra eklenebilir. Benzer şekilde, uyarı Ayrıntıları bölümündeki - **açıklama** ve **önem derecesi** değiştirilebilir. Eylem grubunu değiştirilebilir ve yeni bir uyarı kullanmaya bağlama için hazırlanmış **yeni eylem grubu** düğmesi.

   ![Uyarı kuralı değiştirin](./media/monitor-alerts-unified/AlertsPreviewEdit.png)

6. Üst panelde kullanarak değişiklikler uyarıya yansıtıcı dahil olabilir: **Kaydet** uyarı için yapılan tüm değişiklikleri kaydetmek için **at** uyarı için yapılan tüm değişiklikler taahhüt vermek zorunda kalmadan geri dönmek için **devre dışı bırak**  sonrasında artık çalışır veya herhangi bir eylemi tetikleyen uyarısı - devre dışı bırakmak için. Son olarak, **Sil** Azure'dan tüm uyarı kuralı kalıcı olarak kaldırılacak.

## <a name="next-steps"></a>Sonraki adımlar

- Yeni hakkında daha fazla bilgi [neredeyse gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md)
- Alma bir [ölçümleri koleksiyonun genel bakış](insights-how-to-customize-monitoring.md) hizmetinizin kullanılabilir ve yanıt verdiğinden emin olmak için.
- Hakkında bilgi edinin [oturum uyarılar Azure uyarıları](monitor-alerts-unified-log.md)
- [Etkinlik günlüğü uyarıları uyarılar (Önizleme) deneyiminde hakkında bilgi edinin](monitoring-activity-log-alerts-new-experience.md)
