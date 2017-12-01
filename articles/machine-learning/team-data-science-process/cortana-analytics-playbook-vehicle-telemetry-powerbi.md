---
title: "Power BI panosuna araç sağlık ve yürüten alışkanlıklarınıza - Azure | Microsoft Docs"
description: "Araç sistem durumu ve yürüten gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence yeteneklerini kullanabilir alışkanlıklarınıza."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: bradsev
ms.openlocfilehash: 626987ec0648f9e770499b4a48bc4ca2d175d2b4
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Araç Telemetri analizi çözüm şablonu Power BI Panosu kurulum yönergeleri
Bu playbook bölümlerde bu menü bağlantılar: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Araç Telemetri analiz çözümü araba dealerships, otomobil üreticileri ve sigorta şirketler Cortana Intelligence özelliklerini nasıl kullanacağınızı gösterir. Bunlar, araç sistem durumu ve müşteri deneyimi, araştırma ve geliştirme geliştirmek için alışkanlıklarınıza yürüten ve pazarlama kampanyaları gerçek zamanlı ve Tahmine dayalı Öngörüler elde edebilirsiniz. Bu adım adım yönergeler aboneliğinizde çözümü dağıttıktan sonra nasıl Power BI raporları ve panoyu yapılandırabilirsiniz gösterir. 

## <a name="prerequisites"></a>Ön koşullar
* Dağıtma [araç Telemetri analizi](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) çözümü. 
* [Power BI Desktop yüklemek](http://www.microsoft.com/download/details.aspx?id=45331).
* Elde bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/). Bir Azure aboneliğiniz yoksa, Azure ücretsiz aboneliği ile çalışmaya başlayın.
* Power BI hesabınızı açın.

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence suite bileşenleri
Araç Telemetri analizi çözüm şablonu bir parçası olarak, aşağıdaki Cortana Intelligence Hizmetleri aboneliğinize dağıtılır:

* **Azure Event Hubs** Azure'da araç telemetri olayları milyonlarca alır.
* **Azure Stream Analytics** araç sistem durumunu gerçek zamanlı Öngörüler sağlar ve bu verileri daha zengin toplu analiz için uzun vadeli depolamaya devam eder.
* **Azure Machine Learning** anormallikleri gerçek zamanlı olarak algılar ve toplu işleme Tahmine dayalı Öngörüler sağlamak için kullanır.
* **Azure Hdınsight** ölçeğinde veri dönüştürür.
* **Azure Data Factory** işler orchestration, planlama, kaynak yönetimi ve toplu işleme ardışık düzeni izleme.

**Power BI** gerçek zamanlı veri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano bu çözümü sunar. 

Çözüm iki farklı veri kaynakları kullanır:

* Benzetimli araç sinyalleri ve tanılama veri kümeleri
* Araç Kataloğu

Bir araç telematik simulator bu çözümün bir parçası olarak dahil edilir. Tanılama bilgilerini ve durumunu araç ve belirli bir noktada yönlendirmeli desenleri zamanında karşılık gelen sinyalleri yayar. 

Araç, modelleri için VINs eşleyen bir başvuru veri kümesi kataloğudur.

## <a name="power-bi-dashboard-preparation"></a>Power BI Panosu hazırlama
### <a name="set-up-the-power-bi-real-time-dashboard"></a>Power BI gerçek zamanlı Pano ayarlayın

#### <a name="start-the-real-time-dashboard-application"></a>Gerçek zamanlı Pano uygulama Başlat
Dağıtım tamamlandıktan sonra el ile işlem yönergeleri izleyin.

1. Gerçek zamanlı Pano uygulaması RealtimeDashboardApp.zip indirin ve onu sıkıştırmasını açın.

2.  Uygulama yapılandırma dosyasını RealtimeDashboardApp.exe.config sıkıştırması açılmış klasöründe açın. Olay hub'ları, Azure Blob Depolama ve Azure Machine Learning hizmeti bağlantıları el ile işlem yönergeleri değerleriyle appSettings değiştirin. Yaptığınız değişiklikleri kaydedin.

3. RealtimeDashboardApp.exe uygulamayı çalıştırın. Pencere üzerinde geçerli Power BI kimlik bilgilerinizi girin. 

   ![Oturum açma Power BI penceresi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
4. Seçin **kabul**. Uygulama çalışmaya başlar.

   ![Power BI Panosu izinleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

5. Power BI Web sitesine oturum açabilir ve gerçek zamanlı bir pano oluşturun.

Artık Power BI panosunu yapılandırmak hazırsınız.  

### <a name="configure-power-bi-reports"></a>Power BI raporları yapılandırın
Gerçek zamanlı raporları ve panoyu tamamlanması yaklaşık 30-45 dakika sürebilir. 

1. Gözat [Power BI](http://powerbi.com) Web sayfası ve oturum açın.

    ![Power BI oturum açma sayfası](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

2. Yeni veri kümesi Power BI'da oluşturulur. Seçin **ConnectedCarsRealtime** veri kümesi.

    ![ConnectedCarsRealtime veri kümesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

3. Boş raporu kaydetmek için Ctrl + S tuşlarına basın.

    ![Boş raporu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

4. Rapor adı girin **araç Telemetri analizi gerçek zamanlı - raporları**.

    ![Rapor adı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Gerçek zamanlı raporları
Üç gerçek zamanlı raporları bu çözümde şunlardır:

* İşlemde araçları
* Bakım gerektiren araçları
* Araç sağlık istatistikleri

Her aşama sonra durdurabilir veya gerçek zamanlı raporları üçü yapılandırabilirsiniz. Toplu raporlarının nasıl yapılandırılacağını sonraki bölümüne geçebilirsiniz. Gerçek zamanlı yolun çözümün tam Öngörüler görselleştirmek için tüm üç raporları oluşturmanızı öneririz.  

### <a name="vehicles-in-operation-report"></a>İşlemi rapordaki araçları
1. Çift **sayfa 1**ve yeniden adlandırmak **Taşıtlardan işleminde**.

    ![İşlemde araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

2. Üzerinde **alanları** sekmesine **toplamıdır**. Üzerinde **görselleştirmeleri** sekmesine **kartı** görselleştirme.  

    **Kartı** görselleştirme aşağıdaki resimde gösterildiği gibi oluşturulur:

    ![Toplamıdır seçin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

3. Yeni bir görsel öğe eklemek için boş alanı seçin.  

4. Üzerinde **alanları** sekmesine **Şehir** ve **toplamıdır**. Üzerinde **görselleştirmeleri** sekmesine **harita** görselleştirme. Sürükleme **toplamıdır** için **değerleri** alanı. Sürükleme **Şehir** için **gösterge** alanı. 

    ![Kart Görselleştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

5. Üzerinde **görselleştirmeleri** sekmesine **biçimi** bölümü. Seçin **başlık**, değiştirip **metin** için **Taşıtlardan işlemde şehre göre**.

    ![İşlemde şehre göre araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

    Son görselleştirme aşağıdaki gibi görünür:

    ![Son Görselleştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

6. Yeni bir görsel öğe eklemek için boş alanı seçin.  

7. Üzerinde **alanları** sekmesine **Şehir** ve **toplamıdır**. Üzerinde **görselleştirmeleri** sekmesine **kümelenmiş sütun grafiği** görselleştirme. Sürükleme **Şehir** için **eksen** alanı. Sürükleme **toplamıdır** için **değeri** alanı.

8. Grafik tarafından sıralama **toplamıdır sayısı**.

    ![Toplamıdır sayısı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

9. Grafiği değiştirmek **başlık** için **Taşıtlardan işlemde şehre göre**. 

10. Seçin **biçimi** bölümünde ve ardından **veri renkleri**. Değişiklik **Tümünü Göster** için **üzerinde**.

    ![Veri renkleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

11. Tek bir şehir rengi renk simgesi seçerek değiştirin.

    ![Renk Değiştir](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

12. Yeni bir görsel öğe eklemek için boş alanı seçin.  

13. Üzerinde **görselleştirmeleri** sekmesine **kümelenmiş sütun grafiği** görselleştirme. Üzerinde **alanları** sekmesinde, sürükleyin **Şehir** için **eksen** alanı. Sürükleme **modeli** için **gösterge** alanı. Sürükleme **toplamıdır** için **değeri** alanı.

    ![Kümelenmiş sütun grafiği](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)

    Grafik aşağıdaki görüntü gibi görünür:

    ![İşleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

14. Tüm görselleştirmeler sayfası aşağıdaki gibi görünen şekilde yeniden düzenleyin:

    ![Görselleştirmelerle Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

"İşlem Taşıtlardan" gerçek zamanlı rapor başarıyla yapılandırıldı. Gerçek zamanlı bir sonraki rapor oluşturabilir veya buraya durdurun ve Pano yapılandırın. 

### <a name="vehicles-requiring-maintenance-report"></a>Araçlar bakım gerektiren raporu

1. Seçin ![Ekle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) yeni bir rapor eklemek için. Yeniden adlandırmak **Taşıtlardan gerektiren Bakım**.

    ![Bakım gerektiren araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

2. Üzerinde **alanları** sekmesine **toplamıdır**. Üzerinde **görselleştirmeleri** sekmesine **kartı** görselleştirme.

    ![Toplamıdır kartı Görselleştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

    Veri kümesi adında bir alan içeriyor. **MaintenanceLabel**. Bu alan "0" veya "1" değerine sahip Değer, çözümün bir parçası sağlanan makine öğrenimi modeline göre ayarlanır. Gerçek zamanlı yolu ile tümleşiktir. "1" değeri bir araç bakım gerektirdiğini gösterir. 

3. Eklemek için bir **sayfa düzeyi filtresi** bakım gerektiren araçları için verileri görüntülemek için: 

   a. Sürükleme **MaintenanceLabel** alanı **sayfa düzeyi filtrelerini**.
  
      ![Sayfa düzeyi filtreleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)

    b. Ekranın alt kısmındaki **sayfa düzeyi filtreleri MaintenanceLabel**seçin **temel filtreleme**.

      ![Temel filtreleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png) 

    c. Filtre değeri ayarlamak **1**.

      ![Filtre değeri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

4. Yeni bir görsel öğe eklemek için boş alanı seçin.  

5. Üzerinde **görselleştirmeleri** sekmesine **kümelenmiş sütun grafiği** görselleştirme. 

    ![Toplamıdır kartı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)

    ![Kümelenmiş sütun grafiği](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

6. Üzerinde **alanları** sekmesinde, sürükleyin **modeli** için **eksen** alanı. Sürükleme **toplamıdır** için **değeri** alanı. Görselleştirme tarafından sıralama **toplamıdır sayısı**. Grafiği değiştirmek **başlık** için **modeli tarafından bakım gerektiren Taşıtlardan**. 

7. Üzerinde **alanları** ![alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) bölümünü **görselleştirmeleri** sekmesinde, sürükleyin **toplamıdır** için **doygunluğu**.

    ![Renk doygunluğunu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

8. Üzerinde **biçimi** bölümünde **veri renkleri** görseldeki: 

    a. Değişiklik **Minimum** için renk **F2C812**.

    b. Değişiklik **maksimum** için renk **FF6300**.

    ![Yeni veri renkler](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)

    Yeni görselleştirme renkleri aşağıdaki gibi görünür:

    ![Yeni görselleştirme renkler](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

9. Yeni bir görsel öğe eklemek için boş alanı seçin.  

10. Üzerinde **görselleştirmeleri** sekmesine **kümelenmiş sütun grafiği**. Sürükleme **toplamıdır** için **değeri** alanı. Sürükleme **Şehir** için **eksen** alanı. Grafik tarafından sıralama **toplamıdır sayısı**. Grafiği değiştirmek **başlık** için **şehre göre bakım gerektiren Taşıtlardan**.

    ![Şehre göre bakım gerektiren araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

11. Yeni bir görsel öğe eklemek için boş alanı seçin.  

12. Üzerinde **görselleştirmeleri** sekmesine **çok satır kartı** görselleştirme. Sürükleme **modeli** ve **toplamıdır** için **alanları** alanı.

    ![Birden fazla satır kartı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

13. Tüm görselleştirmeler son rapor aşağıdaki gibi görünen şekilde yeniden düzenleyin: 

    ![Düzenlenmiş son raporu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

"Taşıtlardan gerektiren bakım" gerçek zamanlı rapor başarıyla yapılandırıldı. Gerçek zamanlı bir sonraki rapor oluşturabilir veya buraya durdurun ve Pano yapılandırın. 

### <a name="vehicle-health-statistics-report"></a>Araç sağlık istatistikleri rapor

1. Seçin ![Ekle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) yeni bir rapor eklemek için. Yeniden adlandırmak **Taşıtlardan sağlık istatistikleri**. 

2. Üzerinde **görselleştirmeleri** sekmesine **ölçer** görselleştirme. Sürükleme **hızı** için **değeri**, **Minimum değer**, ve **en büyük değer** alanları.

   ![Araçlar sağlık istatistikleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

3. İçinde **değeri** alanı olan varsayılan toplama değiştirme **hızı** için **ortalama**.

4. İçinde **Minimum değer** alanı olan varsayılan toplama değiştirme **hızı** için **Minimum**.

5. İçinde **en büyük değer** alanı olan varsayılan toplama değiştirme **hızı** için **maksimum**.

   ![Hızı değerleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

6. Yeniden Adlandır **ölçer başlık** için **ortalama hızı**.

   ![Gösterge](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

7. Yeni bir görsel öğe eklemek için boş alanı seçin.  

    Benzer şekilde, eklemek bir **ölçer** için **ortalama altyapısı Petrol**, **ortalama yakıt**, ve **ortalama altyapısı sıcaklık**.  

8. Önceki adımlarında yaptığınız gibi her ölçer alanlarının varsayılan toplama değiştirme **ortalama hızı** ölçer.

    ![Ek ölçerler](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

9. Yeni bir görsel öğe eklemek için boş alanı seçin.

10. Üzerinde **görselleştirmeleri** sekmesine **çizgi ve kümelenmiş sütun grafiği** görselleştirme. Sürükleme **Şehir** için **paylaşılan eksen**. Sürükleme **tirepressure**, **engineoil**, ve **hızı** için **sütun değerlerini** alanı. Toplama tipine değiştirme **ortalama**. 

11. Sürükleme **engineTemperature** için **satır değerleri** alanı. Toplama türü değiştirme **ortalama**. 

    ![Sütun ve çizgi değerleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

12. Grafiği değiştirmek **başlık** için **ortalama hızı, lastiği baskısı, altyapısı Petrol ve altyapısı sıcaklık**.  

    ![Çizgi ve kümelenmiş sütun grafiği başlığı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

13. Yeni bir görsel öğe eklemek için boş alanı seçin.

14. Üzerinde **görselleştirmeleri** sekmesine **Treemap** görselleştirme. Sürükleme **modeli** için **grup** alanı. Sürükleme **MaintenanceProbability** için **değerleri** alanı.

15. Grafiği değiştirmek **başlık** için **bakım gerektiren araç modelleri**.

    ![Treemap başlığı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

16. Yeni bir görsel öğe eklemek için boş alanı seçin.

17. Üzerinde **görselleştirmeleri** sekmesine **% 100 Yığılmış grafik** görselleştirme. Sürükleme **Şehir** için **eksen** alanı. Sürükleme **MaintenanceProbability** ve **RecallProbability** için **değeri** alanı.

    ![Eksen ve değer alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

18. Üzerinde **biçimi** bölümünde, select **veri renkleri**. Ayarlama **MaintenanceProbability** renk değerine **F2C80F**.

19. Grafiği değiştirmek **başlık** için **araç bakım olasılık & geri çağırma şehre göre**.

    ![% 100 Yığılmış çubuk grafik başlığı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

20. Yeni bir görsel öğe eklemek için boş alanı seçin.

21. Üzerinde **görselleştirmeleri** sekmesine **alan grafiği** görselleştirme. Sürükleme **modeli** için **eksen** alanı. Sürükleme **engineOil**, **tirepressure**, **hızı**, ve **MaintenanceProbability** için **değerleri** alan. Toplama tipine değiştirme **ortalama**. 

    ![Toplama türü](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

22. Grafiği değiştirmek **başlık** için **ortalama altyapısı petrol, lastiği baskısı, hızlı ve Bakım olasılık modeli tarafından**.

    ![Alan grafik başlığı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

23. Yeni bir görsel öğe eklemek için boş alanı seçin.

24. Üzerinde **görselleştirmeleri** sekmesine **dağılım grafiği** görselleştirme. Sürükleme **modeli** için **ayrıntıları** ve **gösterge** alanları. Sürükleme **yakıt** için **X ekseni** alanı. Toplama değiştirme **ortalama**. Sürükleme **engineTemperature** için **Y ekseni** alanı. Toplama değiştirme **ortalama**. Sürükleme **toplamıdır** için **boyutu** alanı.

    ![Ayrıntılar, gösterge, eksen ve boyutu alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

25. Grafiği değiştirmek **başlık** için **yakıt, engineTemperature ortalaması ve Model ve Model tarafından toplamıdır sayısı, ortalama**.

    ![Grafik başlığını dağılım](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

    Son rapor aşağıdaki gibi görünür:

    ![Son raporu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Gerçek zamanlı panoya raporlardan PIN görselleştirmeleri
1. Artı simgenin yanındaki seçerek boş bir pano oluşturun **panolar**. Bir ad girin **araç Telemetri analizi Pano**.

    ![Pano artı simgesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

2. Önceki raporlardan panoya görselleştirmeler sabitleyin. 

    ![Pano PIN simgesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

    Tüm üç raporları panosuna sabitlenmiş olduğunda, aşağıdaki gibi görünmelidir. Tüm raporları oluşturmadıysanız, Panonuzda farklı görünebilir. 

    ![Panoyu raporlar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gerçek zamanlı Pano oluşturuldu. CarEventGenerator.exe ve RealtimeDashboardApp.exe yürütmek devam ederken, panosunda canlı güncelleştirmeler bakın. Aşağıdaki adımları tamamlamak için yaklaşık 10-15 dakika sürebilir.

## <a name="set-up-the-power-bi-batch-processing-dashboard"></a>Power BI toplu işleme Pano ayarlayın
> [!NOTE]
> Uçtan uca toplu iş yürütme ve üretilen veri bir yılın tutarında işlem ardışık düzen işleme için yaklaşık iki saat (dağıtım başarılı şekilde tamamlandığını) sürer. İşleme aşağıdaki adımlarla devam etmeden önce tamamlanmasını bekleyin. 
> 
> 

### <a name="download-the-power-bi-designer-file"></a>Power BI Tasarımcısı dosyasını indirin

1. Önceden yapılandırılmış bir Power BI Tasarımcısı dosyasına dağıtım el ile işlem yönergeleri bir parçası olarak bulunur. "2 arayın. Powerbı toplu işleme Pano ayarlayın."

2. Burada adlı toplu işlem Pano için Power BI şablonu indirme **ConnectedCarsPbiReport.pbix**.

3. Yerel olarak kaydedin.

### <a name="configure-power-bi-reports"></a>Power BI raporları yapılandırın

1. Tasarımcı dosyasını açın **ConnectedCarsPbiReport.pbix** Power BI Desktop kullanarak. Zaten sahip değilseniz, Power BI masaüstünden yüklemek [Power BI Desktop yükleme](http://www.microsoft.com/download/details.aspx?id=45331) Web sitesi.

2. Seçin **Düzenle sorguları**.

    ![Sorguları düzenleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

3. Çift **kaynak**.

    ![Kaynak](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

4. Sunucu bağlantı dizesi dağıtımının bir parçası sağlanan Azure SQL server ile güncelleştirin. Azure SQL veritabanı altında el ile işlem yönergelere bakın:

    * Sunucu: somethingsrv.database.windows.net
    * Veritabanı: connectedcar
    * Kullanıcı adı: kullanıcı adı
    * Parola: Azure portalından, SQL Server parolanızı yönetebilirsiniz.

5. Bırakın **veritabanı** olarak **connectedcar**.

    ![Database](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

6. **Tamam**’ı seçin.

7. **Windows kimlik bilgileri** sekmesi, varsayılan olarak seçilidir. Şekilde değiştirin **veritabanı kimlik bilgileri** seçerek **veritabanı** sekmesini sağ.

8. Girin **kullanıcıadı** ve **parola** Azure SQL veritabanınızın onun dağıtım Kurulum sırasında belirtildi.

    ![Veritabanı kimlik bilgileri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

9. **Bağlan**’ı seçin.

10. Her sağ bölmede mevcut üç kalan sorgular için önceki adımları yineleyin. Ardından veri kaynağı bağlantı ayrıntıları güncelleştirin.

11. Seçin **kapatın ve yük**. Power BI Desktop dosyası veri kümelerini SQL veritabanı tablolarında bağlanır.

12. Seçin **kapatmak** Power BI Desktop dosyasını kapatın.

    ![Kapat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

13. Seçin **kaydetmek** değişiklikleri kaydedin. 

Toplu işleme çözümü yolunda karşılık gelen tüm raporlar artık yapılandırdınız. 

## <a name="upload-to-powerbicom"></a>Powerbı.com adresinde karşıya yükle
1. Git [Power BI web portalı](http://powerbi.com)ve oturum açın.

2. Seçin **veri alma**.

3. Power BI Desktop dosyasını karşıya yükleyin. Seçin **Veri Al** > **dosyalarını almak** > **yerel dosya**.

4. Git **ConnectedCarsPbiReport.pbix**.

5. Dosyayı karşıya yüklendikten sonra Power BI çalışma alanınıza geri dönün. Bir veri kümesi, rapor ve boş bir Pano sizin için oluşturulur.  

6. Yeni bir Pano PIN grafiklere adlı **araç Telemetri analizi Pano** Power bı'da. Daha önce oluşturulmuş ve gidin boş bir Pano seçin **raporları** bölümü. Yeni yüklenen raporu seçin.  

    ![Yeni Power BI Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

    Raporun altı sayfa vardır:

    Sayfa 1: Araç yoğunluğu  
    Sayfa 2: Gerçek zamanlı araç sistem durumu  
    Sayfa 3: Titizlikle taşıtlardan güdümlü   
    Sayfa 4: Geri çekilen araçları  
    Sayfa 5: verimli bir şekilde taşıtlardan güdümlü yakıt  
    Sayfa 6: Contoso Motors logosu  

    ![Altı sayfaları ile Power BI raporu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

7. Gelen **sayfa 3**, aşağıdaki içeriği PIN:  

    a. **Toplamıdır sayısı**  

   ![Sayfa 3 toplamıdır sayısı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png)

    b. **Agresif taşıtlardan Waterfall grafik model tarafından yönetilen** 

   ![Sayfa 3 grafik 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

8. Gelen **sayfa 5**, aşağıdaki içeriği PIN: 

    a. **Toplamıdır sayısı**

   ![Sayfa 5 grafik 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)

    b. **Model tarafından Fuel-Efficient taşıtlardan: Kümelenmiş sütun grafiği**

   ![Sayfa 5 grafik 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

9. Gelen **sayfa 4**, aşağıdaki içeriği PIN:  

    a. **Toplamıdır sayısı** 

   ![Sayfa 4 Grafik 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

    b. **Şehir tarafından geri çekilen taşıtlardan model: Treemap**

   ![Sayfa 4 Grafik 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

10. Gelen **sayfa 6**, aşağıdaki içeriği PIN:  

    * **Contoso Motors logosu**

    ![Contoso Motors logosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

### <a name="organize-the-dashboard"></a>Pano düzenleme  

1. Panoya gidin.

2. Her grafiğinin üzerine getirin. Tamamlanmış Pano aşağıda sağlanan adlandırma üzerinde göre her bir grafik yeniden adlandırın:

   ![Pano kuruluş](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png) 
   
3. Aşağıdaki Pano gibi yaklaşık aramak için grafikler Taşı:

    ![Düzenlenmiş Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)

4. Bu belgede belirtilen tüm raporları oluşturduktan sonra son tamamlanmış Panosu aşağıdaki gibi görünür: 

   ![Son Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Başarıyla raporları ve panoyu kazanmak için oluşturduğunuz gerçek zamanlı, Tahmine dayalı ve toplu işlem ınsights araç sistem durumu ve yürüten alışkanlıklarınıza.  
