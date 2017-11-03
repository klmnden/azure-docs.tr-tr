---
title: "Araç sistem durumu için power BI panosuna ve alışkanlık - Azure yürüten | Microsoft Docs"
description: "Araç sistem durumu ve yürüten gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence yeteneklerini kullanabilir alışkanlıklarınıza."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 39be936520d62cb1c1c28de9bd72f8f489166082
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Araç telemetri analizi çözüm şablon Power BI panosuna kurulum yönergeleri
Bu **menü** bu playbook bölümlerde bağlanır. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Araç Telemetri analiz çözümü nasıl araba dealerships, otomobil üreticileri ve sigorta şirketler araç sistem üzerinde gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence özelliklerini yararlanabilir ve müşteri alanında sürücü geliştirmeleri için yönlendirmeli alışkanlıklarınıza deneyimi, R & D ve pazarlama kampanyaları gösterir. Bu belge, çözüm aboneliğinizde dağıtıldıktan sonra Power BI raporları ve panoyu nasıl yapılandırabileceğiniz adım adım yönergeler içerir. 

## <a name="prerequisites"></a>Ön koşullar
1. Dağıtma [Telemetri analizi](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) çözümü  
2. [Microsoft Power BI Desktop yükleyin](http://www.microsoft.com/download/details.aspx?id=45331)
3. Bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/). Bir Azure aboneliğiniz yoksa, ücretsiz Azure aboneliği ile çalışmaya başlama
4. Microsoft Power BI hesabı

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence Suite bileşenleri
Araç Telemetri analizi çözüm şablonu bir parçası olarak, aşağıdaki Cortana Intelligence Hizmetleri aboneliğinize dağıtılır.

* **Olay hub'ı** Azure'da araç telemetri olayları milyonlarca alma için.
* **Akış analizi** araç sistem üzerinde gerçek zamanlı Öngörüler öğrenilmesi ve bu verileri daha zengin toplu analiz için uzun vadeli depolamaya devam eder.
* **Machine Learning** anomali algılama gerçek zamanlı ve toplu işleme Tahmine dayalı Öngörüler elde edin.
* **Hdınsight** ölçeğinde veri dönüştürme için de
* **Veri Fabrikası** planlama, kaynak yönetimi ve izleme toplu işleme ardışık orchestration işler.

**Power BI** gerçek zamanlı veri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano bu çözümü sunar. 

Çözüm iki farklı veri kaynakları kullanır: **benzetimli araç sinyalleri ve tanılama veri kümesi** ve **araç katalog**.

Bir araç telematik simulator bu çözümün bir parçası olarak dahil edilir. Tanılama bilgisi yayar ve araç durumuna karşılık gelen ve düzeni, zaman içinde belirli bir anda yürüten işaret eder. 

Başvuru dataset içeren Toplamıdır modeli eşleme için araç katalogdur

## <a name="power-bi-dashboard-preparation"></a>Power BI panosuna hazırlama
### <a name="setup-power-bi-real-time-dashboard"></a>Kurulum Power BI gerçek zamanlı Panosu

**Gerçek zamanlı Pano uygulamayı başlatmak** dağıtım tamamlandıktan sonra el ile işlem yönergeleri izlemelisiniz.

* Gerçek zamanlı Pano uygulaması RealtimeDashboardApp.zip indirin ve onu sıkıştırmasını açın.
*  Uygulama yapılandırma dosyası 'RealtimeDashboardApp.exe.config', Eventhub, Blob Storage ve ML hizmet bağlantıları el ile işlem yönergeleri ve değişikliklerinizi kaydedin değerlerle değiştirin appSettings sıkıştırması açılmış klasöründe açın.
* Uygulama RealtimeDashboardApp.exe çalıştırın. Bir oturum açma penceresi açılır, geçerli Powerbı kimlik bilgilerinizi sağlayın ve tıklatın **kabul** düğmesi. Daha sonra uygulama çalışmaya başlar.

   ![Power BI'da oturum açın](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI panosuna izinleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Powerbı Web sitesine, oturum açma ve gerçek zamanlı bir pano oluşturun.

Artık Power BI panosu ile zengin Görselleştirmelerini gerçek zamanlı kazanmak için yapılandırmaya hazırsınız ve Tahmine dayalı Öngörüler araç sistem durumu ve yürüten alışkanlıklarınıza. Tüm raporları oluşturmak ve Pano yapılandırmak için bir saat için yaklaşık 45 dakika sürer. 

### <a name="configure-power-bi-reports"></a>Power BI raporları yapılandırın
Gerçek zamanlı raporları ve panoyu tamamlanması yaklaşık 30-45 dakika sürebilir. Gözat [http://powerbi.com](http://powerbi.com) ve oturum açma.

![Power BI'da oturum açın](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Yeni bir veri kümesi Power BI'da oluşturulur. Tıklatın **ConnectedCarsRealtime** veri kümesi.

![Bağlı eçildiğindeki araba gerçek zamanlı veri kümesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Boş bir rapor kullanarak Kaydet **Ctrl + s**.

![Boş raporu kaydedin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Rapor adı sağlayın *araç Telemetri analizi gerçek zamanlı - raporları*.

![Rapor adı belirtin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Gerçek zamanlı raporları
Bu çözümde üç gerçek zamanlı raporları vardır:

1. İşlemde araçları
2. Bakım gerektiren araçları
3. Araçlar sağlık istatistikleri

Tüm üç gerçek zamanlı raporlar yapılandırmak veya her aşama sonra durdurup toplu raporları yapılandırma, bir sonraki bölüme geçin seçebilirsiniz. Çözümün gerçek zamanlı yolunun tam Öngörüler görselleştirmek için üç tüm raporları oluşturmanızı öneririz.  

### <a name="1-vehicles-in-operation"></a>1. İşlemde araçları
Çift **sayfa 1** ve "Araçlar" işlemde yeniden adlandırın  
    ![Araba - bağlı Taşıtlardan işleminde](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Seçin **toplamıdır** alanının **alanları** ve görselleştirme türü olarak seçin **"Kart"**.  

Kart görselleştirme şekilde gösterildiği gibi oluşturulur.  
    ![Bağlı araba - Select toplamıdır](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Boş alan yeni bir görsel öğe eklemek için tıklatın.  

Seçin **Şehir** ve **toplamıdır** alanlarından. Değiştirmek için görselleştirme **"Harita"**. Sürükleme **toplamıdır** değerleri alanında. Sürükleme **Şehir** için alanlarından **gösterge** alanı.   
    ![Araba - kart görselleştirme bağlı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Seçin **biçimi** konusundaki **görselleştirmeleri**, tıklatın **başlık** değiştirip **metin** için **"Araçlar işlemde şehre göre"**.  
    ![Araba - bağlı Taşıtlardan işlemde şehre göre](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Son görselleştirme şekilde gösterildiği gibi görünüyor.    
    ![Bağlı araba - son Görselleştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Boş alan yeni bir görsel öğe eklemek için tıklatın.  

Seçin **Şehir** ve **toplamıdır**, görselleştirme türünü değiştirme **kümelenmiş sütun grafiği**. Sağlamak **Şehir** alanındaki **eksen alanı** ve **toplamıdır** içinde **alan değeri**  

Sıralama grafik tarafından **"Toplamıdır sayısı"**  
    ![Bağlı araba - toplamıdır sayısı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Değişiklik grafik **başlık** için **"Araçlar işlemde şehre göre"**  

Tıklatın **biçimi** bölümünde sonra seçin **veri renkleri**, tıklatın **"Açık"** için **Tümünü Göster**  
    ![Bağlı araba - tüm veri renkleri göster](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Renk simgesine tıklayarak tek tek Şehir rengini değiştirin.  
    ![Araba - değişiklik renkleri bağlı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Boş alan yeni bir görsel öğe eklemek için tıklatın.  

Seçin **kümelenmiş sütun grafiği** görselleştirmeleri, gelen görselleştirme sürükleyin **Şehir** alanındaki **eksen** alanı **modeli** içinde **gösterge** alan ve **toplamıdır** içinde **değeri** alanı.  
    ![Bağlı araba - kümelenmiş sütun grafiği](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Bağlı araba - işleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Bu sayfadaki tüm görselleştirme şekilde gösterildiği gibi yeniden düzenleyin.  
    ![Bağlı araba - görselleştirmeleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

"Araçlar işleminde" gerçek zamanlı rapor başarıyla yapılandırdınız. Gerçek zamanlı bir sonraki rapor oluşturmak veya buraya durdurmak ve Pano yapılandırmak için devam edebilirsiniz. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Bakım gerektiren araçları
Tıklatın ![Ekle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) yeni bir rapor eklemek için yeniden adlandırma **"Taşıtlardan gerektiren bakım"**

![Bağlı araba - bakım gerektiren araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Seçin **toplamıdır** alan ve görselleştirme türünü değiştirmek **kartı**.  
    ![Bağlı araba - toplamıdır kartı Görselleştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Biz kümesinde "MaintenanceLabel" adlı bir alana sahip. Bu alan "0" veya "1" değeri olabilir." Çözümün bir parçası sağlandı ve gerçek zamanlı yolu ile tümleştirilmiş Azure Machine Learning modeli tarafından ayarlanır. Bir araç bakım gerektirir "1" değeri gösterir. 

Eklemek için bir **sayfa düzeyi** bakım gerektiren taşıtlardan verileri göstermek için filtre: 

1. Sürükleme **"MaintenanceLabel"** içine alan **sayfa düzeyi filtrelerini**.  
   ![Bağlı araba - sayfa düzeyi filtreleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Tıklatın **temel filtreleme** menü MaintenanceLabel sayfa düzeyi filtresi altındaki mevcut.  
   ![Araba - bağlı temel filtreleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Filtre değeri ayarlamak **"1"**    
   ![Bağlı araba - filtre değeri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Boş alan yeni bir görsel öğe eklemek için tıklatın.  

Seçin **kümelenmiş sütun grafiği** görselleştirmeleri gelen  
![Bağlı araba - Vind kartı Görselleştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Bağlı araba - kümelenmiş sütun grafiği](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Sürükleme alan **modeli** içine **eksen** alanı **toplamıdır** için **değeri** alanı. Görselleştirme tarafından sıralama **toplamıdır sayısı**.  Değişiklik grafik **başlık** için **"modeli tarafından bakım gerektiren Taşıtlardan"**  

Sürükleme **toplamıdır** içine alanları **doygunluğu** adresindeki mevcut **alanları** ![alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) bölümünü **görselleştirme** sekmesi  
![Bağlı araba - doygunluğu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Değişiklik **veri renkleri** görselleştirmeleri içinde **biçimi** bölümü  
Minimum rengini: **F2C812**  
Maksimum rengini: **FF6300**  
![Araba - renk değişiklikleri bağlı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Yeni görselleştirme renkleri araba - bağlı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Boş alan yeni bir görsel öğe eklemek için tıklatın.  

Seçin **kümelenmiş sütun grafiği** görselleştirmeleri sürükleyin **toplamıdır** içine alan **değeri** alanında, sürükle **Şehir** içine alan **eksen** alanı. Sıralama grafik tarafından **"Toplamıdır sayısı"**. Değişiklik grafik **başlık** için **"şehre göre bakım gerektiren Taşıtlardan"**   
![Bağlı araba - şehre göre bakım gerektiren araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Boş alan yeni bir görsel öğe eklemek için tıklatın.  

Seçin **çok satır kartı** görselleştirmeleri, gelen görselleştirme sürükleyin **modeli** ve **toplamıdır** içine **alanları** alanı.  
![Bağlı araba - birden çok satır kartı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Tüm görselleştirme yeniden düzenleme, son raporu şu şekilde görünür:  
![Bağlı araba - birden çok satır kartı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

"Araçlar bakım gerektiren" gerçek zamanlı rapor başarıyla yapılandırdınız. Gerçek zamanlı bir sonraki rapor oluşturmak veya buraya durdurmak ve Pano yapılandırmak için devam edebilirsiniz. 

### <a name="3-vehicles-health-statistics"></a>3. Araçlar sağlık istatistikleri
Tıklatın ![Ekle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) yeni bir rapor eklemek için yeniden adlandırma **"Taşıtlardan sağlık İstatistikleri"**  

Seçin **ölçer** görselleştirme görselleştirmeleri, öğesinden sonra sürükleyin **hızı** içine alan **değeri Minimum, maksimum değer** alanları.  
![Bağlı araba - birden çok satır kartı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Olan varsayılan toplama değiştirme **hızı** içinde **değeri alanı** için **ortalama** 

Olan varsayılan toplama değiştirme **hızı** içinde **Minimum alan** için **en az**

Olan varsayılan toplama değiştirme **hızı** içinde **maksimum alan** için **maksimum**

![Bağlı araba - birden çok satır kartı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Yeniden Adlandır **ölçer başlık** için **"Ortalama hızı"** 

![Bağlı araba - ölçer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Boş alan yeni bir görsel öğe eklemek için tıklatın.  

Benzer şekilde ekleme bir **ölçer** için **ortalama altyapısı Petrol**, **ortalama yakıt**, ve **ortalama altyapısı Sıcaklığın**.  

Varsayılan toplama adımlarda yukarıda göre her ölçer alanların değiştirme **"Ortalama hızı"** ölçer.

![Bağlı araba - ölçerler](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Boş alan yeni bir görsel öğe eklemek için tıklatın.

Seçin **çizgi ve kümelenmiş sütun grafiği** görselleştirmeleri sonra sürükleyin **Şehir** içine alan **paylaşılan eksen**, sürükleyin **hızı**, **tirepressure ve engineoil alanları** içine **sütun değerlerini** alanı, kendi toplama türünü değiştirme **ortalama**. 

Sürükleme **engineTemperature** içine alan **satır değerleri** alanında, toplama türü değiştirme **ortalama**. 

![Bağlı araba - görselleştirmeleri alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Grafiği değiştirmek **başlık** için **"Ortalama hızı, lastiği baskısı, altyapısı Petrol ve altyapısı Sıcaklık"**.  

![Bağlı araba - görselleştirmeleri alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Boş alan yeni bir görsel öğe eklemek için tıklatın.

Seçin **Treemap** görselleştirmeleri, gelen görselleştirme sürükleyin **modeli** içine alan **grup** alan ve sürükleme alan **MaintenanceProbability** içine **değerleri** alanı.

Grafiği değiştirmek **başlık** için **"araç modelleri bakım gerektiren"**.

![Araba - Grafik Başlığı Değiştir bağlı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Boş alan yeni bir görsel öğe eklemek için tıklatın.

Seçin **% 100 Yığılmış grafik** görselleştirme sürükleyin **Şehir** içine alan **eksen** alan ve sürükleme **MaintenanceProbability**, **RecallProbability** içine alanları **değeri** alanı.

![Bağlı araba - yeni görselleştirme ekleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Tıklatın **biçimi**seçin **veri renkleri**ve **MaintenanceProbability** renk değerine **"F2C80F"**.

Değişiklik **başlık** için grafiğin **"Olasılık, araç bakım & geri çağırma tarafından Şehir"**.

![Bağlı araba - yeni görselleştirme ekleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Boş alan yeni bir görsel öğe eklemek için tıklatın.

Seçin **alan grafiği** görselleştirmeleri gelen görselleştirme sürükleyin **modeli** içine alan **eksen** alan ve sürükleme **engineOil, tirepressure, hız ve MaintenanceProbability** içine alanları **değerleri** alanı. Toplama tipine değiştirme **"Ortalama"**. 

![Bağlı araba - toplama türünü değiştir](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Grafik başlığını değiştirmek **"Ortalama altyapısı petrol, lastiği baskısı, hızlı ve Bakım olasılık modeli tarafından"**.

![Araba - Grafik Başlığı Değiştir bağlı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Boş alan yeni bir görsel öğe eklemek için tıklatın:

1. Seçin **dağılım grafiği** görselleştirme görselleştirmeleri gelen.
2. Sürükleme **modeli** içine alan **ayrıntıları** ve **gösterge** alanı.
3. Sürükle **yakıt** içine alan **x ekseni** alanı için toplama değiştirmek **ortalama**.
4. Sürükleme **engineTemparature** içine **y ekseni alanında**, toplama değiştirmek **ortalama**
5. Sürükleme **toplamıdır** içine alan **boyutu** alanı.

![Bağlı araba - yeni görselleştirme ekleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Grafiği değiştirmek **başlık** için **"Yakıt ortalamalar, modeli tarafından altyapısı Sıcaklık"**.

![Araba - Grafik Başlığı Değiştir bağlı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Son rapor gibi aşağıda gösterildiği gibi görünür.

![Bağlı araba son raporu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Gerçek zamanlı panoya raporlardan PIN görselleştirmeleri
Panolar yanındaki artı simgesine tıklayarak boş bir pano oluşturun. "Araç Telemetri analizi Pano" adı

![Bağlı araba Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Yukarıdaki raporlardan görselleştirme panoya sabitleyin. 

![Bağlı araba Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Pano, tüm üç raporlar oluşturduğunuzda ve karşılık gelen görselleştirmeleri panosuna sabitlenmiş aşağıdaki gibi görünmelidir. Tüm raporları oluşturmadıysanız panonuz farklı görünebilir. 

![Bağlı araba Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Tebrikler! Gerçek zamanlı Pano başarıyla oluşturdunuz. CarEventGenerator.exe ve RealtimeDashboardApp.exe yürütmek devam ederken, panosunda canlı güncelleştirmeler görmeniz gerekir. Aşağıdaki adımları tamamlamak için yaklaşık 10-15 dakika sürer.

## <a name="setup-power-bi-batch-processing-dashboard"></a>Power BI toplu işleme Pano Kurulumu
> [!NOTE]
> Uçtan uca toplu işleme ardışık düzeni yürütülmesi biter ve değerinde bir yıl oluşturulan veri işlemek yaklaşık iki saat (dağıtım başarılı şekilde tamamlandığını) sürer. Bu nedenle işleme sonraki adımlara devam etmeden önce tamamlanmasını bekleyin. 
> 
> 

**Power BI Tasarımcısı dosyasını indirin**

* Önceden yapılandırılmış bir Power BI Tasarımcısı dosyasına el ile işlem yönergeleri dağıtımının bir parçası olarak dahil edilir
* 2 arayın. Kurulum Powerbı toplu işleme Pano burada adlı toplu işlem Pano için Powerbı şablonu yükleyebilir **ConnectedCarsPbiReport.pbix**.
* Yerel olarak Kaydet

**Power BI raporları yapılandırın**

* Tasarımcı dosyasını açın '**ConnectedCarsPbiReport.pbix**' Power BI Desktop kullanma. Power BI masaüstünden zaten yoksa, yükleme [Power BI Desktop yükleme](http://www.microsoft.com/download/details.aspx?id=45331). 
* Tıklatın **Düzenle sorguları**.

![Power BI Sorgu Düzenle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Çift **kaynak**.

![Kümesi Power BI kaynağı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Sunucu bağlantı dizesi dağıtımının bir parçası sağlanan Azure SQL server ile güncelleştirin.  El ile işlem yönergeleri altında bakın 

    4. Azure SQL Database
    
    * Sunucu: somethingsrv.database.windows.net
    * Veritabanı: connectedcar
    * Kullanıcı adı: kullanıcı adı
    * Parola: SQL server parolanızı Azure portalından yönetebilirsiniz

* Bırakın **veritabanı** olarak *connectedcar*.

![Set Power BI veritabanı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* **Tamam** düğmesine tıklayın.
* Görürsünüz **Windows kimlik bilgileri** sekmesi seçili varsayılan olarak, kendisine değiştirme **veritabanı kimlik bilgileri** tıklayarak **veritabanı** sekmesini sağ.
* Sağlamak **kullanıcıadı** ve **parola** onun dağıtım kurulumu sırasında belirtilen, Azure SQL veritabanı'nın.

![Veritabanı kimlik bilgileri sağlayın](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Tıklatın **Bağlan**
* Her sağ bölmesinde mevcut üç kalan sorgular için yukarıdaki adımları yineleyin ve ardından veri kaynağı bağlantı ayrıntıları güncelleştirin.
* Tıklatın **kapatın ve yük**. Power BI Desktop dosyası veri kümelerini SQL Azure veritabanı tablolarında bağlanır.
* **Kapat** Power BI Desktop dosyası.

![Power BI desktop Kapat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Tıklatın **kaydetmek** değişiklikleri kaydetmek için düğmesi. 

Toplu işleme yolun çözümdeki karşılık gelen tüm raporlar artık yapılandırdınız. 

## <a name="upload-to-powerbicom"></a>Karşıya *powerbi.com*
1. Power BI web portalında http://powerbi.com ve oturum açma gidin.
2. Tıklatın **veri alma**  
3. Power BI Desktop dosyasını karşıya yükleyin.  
4. Karşıya yüklemek için tıklayın **Veri Al -> dosya alma yerel dosya ->**  
5. Gidin **"**ConnectedCarsPbiReport.pbix**"**  
6. Dosya yüklendikten sonra geri Power BI çalışma alanınıza yönlendirileceksiniz.  

Bir veri kümesi, rapor ve boş bir Pano sizin için oluşturulur.  

Yeni bir Pano PIN grafiklere adlı **araç Telemetri analizi Pano** içinde **Power BI**. Yukarıda oluşturduğunuz boş Pano tıklayın ve ardından gidin **raporları** bölüm, yeni yüklenen raporun'ı tıklatın.  

![Araç Telemetri güç BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Not rapor altı sayfa vardır:**  
Sayfa 1: Araç yoğunluğu  
Sayfa 2: Gerçek zamanlı araç sistem durumu  
Sayfa 3: Titizlikle Taşıtlardan güdümlü   
Sayfa 4: Geri çekilen araçları  
Sayfa 5: Verimli bir şekilde Taşıtlardan güdümlü yakıt  
Sayfa 6: Contoso logosu  

![Bağlı araba güç BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**Sayfa 3**, aşağıdaki PIN:  

1. Toplamıdır sayısı  
   ![Bağlı araba güç BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Agresif taşıtlardan Waterfall grafik model tarafından yönetilen  
   ![Araç Telemetri - PIN grafikleri 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Sayfa 5**, aşağıdaki PIN: 

1. Toplamıdır sayısı    
   ![Araç Telemetri - PIN grafikleri 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Verimli taşıtlardan modeli tarafından Yakıt Al: Kümelenmiş sütun grafiği  
   ![Araç Telemetri - PIN grafik 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Sayfa 4**, aşağıdaki PIN:  

1. Toplamıdır sayısı  
   ![Araç Telemetri - PIN grafikleri 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Şehir tarafından geri çekilen taşıtlardan model: Treemap  
   ![Araç Telemetri - PIN grafikleri 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Sayfa 6**, aşağıdaki PIN:  

1. Contoso Motors logosu  
   ![Araç Telemetri - PIN grafikleri 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Pano düzenleme**  

1. Panoya gidin
2. Her bir grafik ve tam Pano görüntüde sağlanan adlandırma üzerinde temel adlandırma üzerine gelin. Ayrıca grafikleri Pano gibi aramak için hareket ettirin.  
   ![Araç Telemetri - Pano 2 düzenleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Araç Telemetri güç BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Bu belgede belirtildiği gibi tüm raporları oluşturduysanız, son tamamlanan Pano aşağıdaki şekilde gibi görünmelidir. 

![Araç Telemetri - Pano 2 düzenleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Tebrikler! Raporları başarıyla oluşturdunuz ve gerçek zamanlı, öngörü kazanmak ve araç sistem durumu ve yürüten Öngörüler toplu Pano alışkanlıklarınıza.  
