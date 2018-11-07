---
title: Power BI panosu için araç durumu ve sürüş alışkanlıkları - Azure | Microsoft Docs
description: Araç durumu ve sürüş üzerinde gerçek zamanlı ve Tahmine dayalı Öngörüler elde etmek için Cortana Intelligence'nın özelliklerini kullanmaya alışkanlıkları.
services: machine-learning
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: deguhath
ms.openlocfilehash: b703cb4d3ddd8b62895c9c40c7fa2fba728e884e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262293"
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Araç Telemetri analizi çözüm şablonu Power BI Panosu kurulum yönergeleri
Playbook'u bölümlerde bu menü bağlantılar: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Araba bayileri, otomobil üreticileri ve sigorta şirketlerinin yapabildiğinizi nasıl araç Telemetri analizi çözüm nu tanıtarak Cortana Intelligence yeteneklerini kullanın. Bunlar, araç durumu ve sürüş alışkanlıkları müşteri deneyimi, araştırma ve geliştirme ve pazarlama kampanyaları gerçek zamanlı ve Tahmine dayalı Öngörüler elde edebilirsiniz. Bu adım adım yönergeleri, aboneliğinizde çözümü dağıttıktan sonra Power BI raporları ve panoyu yapılandırma gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
* Dağıtma [araç Telemetri analizi](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) çözüm. 
* [Power BI Desktop uygulamasını](https://www.microsoft.com/download/details.aspx?id=45331).
* Elde bir [Azure aboneliği](https://azure.microsoft.com/pricing/free-trial/). Azure aboneliğiniz yoksa, ücretsiz Azure aboneliği ile başlayın.
* Power BI hesabınızı açın.

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence suite bileşenleri
Araç Telemetri analizi çözüm şablonuyla bir parçası olarak, aşağıdaki Cortana Intelligence Hizmetleri aboneliğinize dağıtılır:

* **Azure Event Hubs** Azure'a milyonlarca araç telemetri alır.
* **Azure Stream Analytics** araç durumu üzerinde gerçek zamanlı Öngörüler sağlar ve bu verileri daha ayrıntılı toplu analizler için uzun vadeli depolamaya devam ettirir.
* **Azure Machine Learning** anormallikleri gerçek zamanlı olarak algılar ve toplu işlem, Tahmine dayalı Öngörüler sağlamak için kullanır.
* **Azure HDInsight** uygun ölçekte verileri dönüştürür.
* **Azure Data Factory** düzenleme, planlama, kaynak yönetimini ve toplu işlem hattının işler.

**Power BI** Bu çözüm, verileri ve Tahmine dayalı analiz görselleştirmeleri için zengin bir Pano sağlar. 

Çözüm iki farklı veri kaynaklarını kullanmaktadır:

* Simülasyon araç sinyaller ve tanılama veri kümeleri
* Araç Kataloğu

Bir araç telematik simülatörü, bu çözümün bir parçası olarak dahil edilir. Tanılama bilgileri ve durumu araç ve belirli bir noktada sürüş desenleri sürede karşılık gelen sinyalleri yayar. 

Araç, başvuru veri kümesi VINs modeller için eşlenen kataloğudur.

## <a name="power-bi-dashboard-preparation"></a>Power BI Panosu hazırlama
### <a name="set-up-the-power-bi-real-time-dashboard"></a>Power BI gerçek zamanlı Pano Ayarla

#### <a name="start-the-real-time-dashboard-application"></a>Gerçek zamanlı Pano uygulamayı başlatın
Dağıtım tamamlandıktan sonra el ile işlem yönergeleri izleyin.

1. Gerçek zamanlı Pano uygulama RealtimeDashboardApp.zip indirin ve sıkıştırmasını açın.

1.  Sıkıştırması açılmış klasöründe RealtimeDashboardApp.exe.config uygulama yapılandırma dosyasını açın. AppSettings Event Hubs, Azure Blob Depolama ve Azure Machine Learning hizmeti bağlantıları el ile işlem yönergeleri değerleriyle değiştirin. Yaptığınız değişiklikleri kaydedin.

1. RealtimeDashboardApp.exe uygulamayı çalıştırın. Oturum açma penceresinde, geçerli bir Power BI kimlik bilgilerinizi girin. 

   ![Power BI oturum açma penceresini](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
1. **Kabul Et**’i seçin. Uygulama çalışmaya başlar.

   ![Power BI Pano izinleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

1. Power BI Web sitesine oturum açın ve gerçek zamanlı bir pano oluşturun.

Artık Power BI Panosu yapılandırmak hazırsınız.  

### <a name="configure-power-bi-reports"></a>Power BI raporlarını yapılandırma
Gerçek zamanlı raporlar ve Pano tamamlanması yaklaşık 30-45 dakika yararlanın. 

1. Gözat [Power BI](http://powerbi.com) Web sayfasını ve oturum açın.

    ![Power BI oturum açma sayfası](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

1. Power BI'da yeni bir veri kümesi oluşturulur. Seçin **ConnectedCarsRealtime** veri kümesi.

    ![ConnectedCarsRealtime veri kümesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

1. Boş rapor kaydetmek için Ctrl + S tuşuna basın.

    ![Boş rapor](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

1. Rapor adı girin **araç Telemetri analizi gerçek zamanlı - raporları**.

    ![Rapor adı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Gerçek zamanlı raporları
Bu çözümde üç gerçek zamanlı raporlar şunlardır:

* İşlemde araçları
* Bakım gerektiren araçları
* Aracı sistem durumu istatistikleri

Tüm aşama sonra durdurabilirsiniz veya üçünü raporları yapılandırabilirsiniz. Batch raporlarının nasıl yapılandırılacağını sonraki bölümüne geçebilirsiniz. Tam öngörü çözümün gerçek zamanlı yolunun görselleştirmek için üç tüm raporlar oluşturmanızı öneririz.  

### <a name="vehicles-in-operation-report"></a>İşlem rapordaki araçları
1. Çift **sayfa 1**ve yeniden adlandırmak **işleminde Taşıtlardan**.

    ![İşlemde araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

1. Üzerinde **alanları** sekmesinde **toplamıdır**. Üzerinde **görselleştirmeler** sekmesinde **kart** görselleştirme.  

    **Kart** görselleştirme aşağıdaki şekilde gösterildiği gibi oluşturulur:

    ![Toplamıdır seçin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

1. Yeni bir görselleştirme eklemek için boş alanı seçin.  

1. Üzerinde **alanları** sekmesinde **Şehir** ve **toplamıdır**. Üzerinde **görselleştirmeler** sekmesinde **harita** görselleştirme. Sürükleme **toplamıdır** için **değerleri** alan. Sürükleme **Şehir** için **gösterge** alan. 

    ![Kart görselleştirmesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

1. Üzerinde **görselleştirmeler** sekmesinde **biçimi** bölümü. Seçin **başlık**, değiştirip **metin** için **şehre göre işleminde Taşıtlardan**.

    ![Şehre göre işleminde araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

    Son görselleştirme aşağıdaki örnekteki gibi görünür:

    ![Son Görselleştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

1. Yeni bir görselleştirme eklemek için boş alanı seçin.  

1. Üzerinde **alanları** sekmesinde **Şehir** ve **toplamıdır**. Üzerinde **görselleştirmeler** sekmesinde **kümelenmiş sütun grafik** görselleştirme. Sürükleme **Şehir** için **eksen** alan. Sürükleme **toplamıdır** için **değer** alan.

1. Grafiği sıralamak **toplamıdır sayısı**.

    ![Toplamıdır sayısı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

1. Grafik değiştirin **başlık** için **şehre göre işleminde Taşıtlardan**. 

1. Seçin **biçimi** bölümüne ve ardından **veri renkleri**. Değişiklik **Tümünü Göster** için **üzerinde**.

    ![Veri renkleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

1. Tek bir şehir rengini, renk simgeyi seçerek değiştirin.

    ![Rengi değiştirme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

1. Yeni bir görselleştirme eklemek için boş alanı seçin.  

1. Üzerinde **görselleştirmeler** sekmesinde **kümelenmiş sütun grafik** görselleştirme. Üzerinde **alanları** sekmesinde, sürükleyin **Şehir** için **eksen** alan. Sürükleme **modeli** için **gösterge** alan. Sürükleme **toplamıdır** için **değer** alan.

    ![Kümelenmiş sütun grafik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)

    Grafiği aşağıdaki gibi görünür:

    ![İşleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

1. Tüm görselleştirmeler yeniden düzenleyebilir, böylece sayfa aşağıdaki örnekteki gibi görünür:

    ![Görselleştirmeler içeren Pano](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

"İşlem Araçları" raporu başarıyla yapılandırıldı. Gerçek zamanlı bir sonraki rapor oluşturabilir veya burada durabilir ve Pano yapılandırın. 

### <a name="vehicles-requiring-maintenance-report"></a>Aracı bakım gerektiren raporu

1. Seçin ![Ekle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) yeni bir rapor eklemek için. Yeniden adlandırmak **Taşıtlardan bakım gerektiren**.

    ![Bakım gerektiren araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

1. Üzerinde **alanları** sekmesinde **toplamıdır**. Üzerinde **görselleştirmeler** sekmesinde **kart** görselleştirme.

    ![Toplamıdır kart görselleştirmesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

    Veri kümesi içeren adlı bir alan **MaintenanceLabel**. Bu alanın değeri "0" veya "1" olabilir. Değer, çözümün bir parçası sağlanan machine learning modeli tarafından ayarlanır. Gerçek zamanlı yol ile tümleştirilir. "1" değeri, bir araç bakım yapılması gerektiğini belirtir. 

1. Eklemek için bir **sayfa düzeyi filtresi** bakım gerektiren araçları için verileri göstermek için: 

   a. Sürükleme **MaintenanceLabel** alanı **sayfa düzeyi filtreleri**.
  
      ![Sayfa düzeyi filtreleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)

    b. Sayfanın alt kısmında **sayfa düzeyi filtreleri MaintenanceLabel**seçin **temel filtreleme**.

      ![Temel filtreleme](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png) 

    c. Filtre değeri ayarlamak **1**.

      ![Filtre değeri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

1. Yeni bir görselleştirme eklemek için boş alanı seçin.  

1. Üzerinde **görselleştirmeler** sekmesinde **kümelenmiş sütun grafik** görselleştirme. 

    ![Toplamıdır kartı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)

    ![Kümelenmiş sütun grafik](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

1. Üzerinde **alanları** sekmesinde, sürükleyin **modeli** için **eksen** alan. Sürükleme **toplamıdır** için **değer** alan. Ardından görselleştirme tarafından sıralama **toplamıdır sayısı**. Grafik değiştirin **başlık** için **modeli tarafından bakım gerektiren Taşıtlardan**. 

1. Üzerinde **alanları** ![alanları-image](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) bölümünü **görselleştirmeler** sekmesinde, sürükleyin **toplamıdır** için **RenkDoygunluğu**.

    ![Renk Doygunluğu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

1. Üzerinde **biçimi** bölümünde **veri renkleri** görselleştirmede: 

    a. Değişiklik **Minimum** için renk **F2C812**.

    b. Değişiklik **maksimum** için renk **FF6300**.

    ![Yeni veri renkleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)

    Yeni görselleştirme renklerini, aşağıdaki örnekteki gibi görünür:

    ![Yeni görselleştirme renklerini](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

1. Yeni bir görselleştirme eklemek için boş alanı seçin.  

1. Üzerinde **görselleştirmeler** sekmesinde **kümelenmiş sütun grafik**. Sürükleme **toplamıdır** için **değer** alan. Sürükleme **Şehir** için **eksen** alan. Grafiği sıralamak **toplamıdır sayısı**. Grafik değiştirin **başlık** için **şehre göre bakım gerektiren Taşıtlardan**.

    ![Şehre göre bakım gerektiren araçları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

1. Yeni bir görselleştirme eklemek için boş alanı seçin.  

1. Üzerinde **görselleştirmeler** sekmesinde **çok satırlı kart** görselleştirme. Sürükleme **modeli** ve **toplamıdır** için **alanları** alan.

    ![Çok satırlı kart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

1. Raporun son hali aşağıdaki örnekteki gibi görünür, böylece tüm görselleştirmeler yeniden düzenleyin: 

    ![Düzenlenmiş raporun son hali](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

"Aracı Bakımı gerektiren" gerçek zamanlı rapor başarıyla yapılandırıldı. Gerçek zamanlı bir sonraki rapor oluşturabilir veya burada durabilir ve Pano yapılandırın. 

### <a name="vehicle-health-statistics-report"></a>Aracı sistem durumu istatistiklerini rapor

1. Seçin ![Ekle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) yeni bir rapor eklemek için. Yeniden adlandırmak **Taşıtlardan sağlık istatistikleri**. 

1. Üzerinde **görselleştirmeler** sekmesinde **ölçer** görselleştirme. Sürükleme **hızı** için **değer**, **Minimum değer**, ve **maksimum değer** alanları.

   ![Aracı sistem durumu istatistikleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

1. İçinde **değer** alanında varsayılan toplama değiştirme **hızı** için **ortalama**.

1. İçinde **Minimum değer** alanında varsayılan toplama değiştirme **hızı** için **Minimum**.

1. İçinde **maksimum değer** alanında varsayılan toplama değiştirme **hızı** için **maksimum**.

   ![Hızı değerleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

1. Yeniden adlandırma **ölçer başlık** için **ortalama hızı**.

   ![Gösterge](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

1. Yeni bir görselleştirme eklemek için boş alanı seçin.  

    Benzer şekilde, eklemek bir **ölçer** için **ortalama altyapısı Petrol**, **ortalama yakıt**, ve **ortalama altyapısı sıcaklık**.  

1. Varsayılan toplama işlemi önceki adımlarda yaptığınız gibi her ölçer alanların değiştirme **ortalama hızı** ölçer.

    ![Ek ölçekler](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

1. Yeni bir görselleştirme eklemek için boş alanı seçin.

1. Üzerinde **görselleştirmeler** sekmesinde **çizgi ve kümelenmiş sütun grafiği** görselleştirme. Sürükleme **Şehir** için **paylaşılan eksen**. Sürükleme **tirepressure**, **engineoil**, ve **hızı** için **sütun değerleri** alan. Toplama tipine değiştirme **ortalama**. 

1. Sürükleme **engineTemperature** için **çizgi değerleri** alan. Toplama türü ile değiştirmek **ortalama**. 

    ![Sütun ve çizgi değerleri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

1. Grafik değiştirin **başlık** için **ortalama hızı, Lastik baskısı altyapısı Petrol ve altyapısı sıcaklık**.  

    ![Çizgi ve kümelenmiş sütun grafik başlığı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

1. Yeni bir görselleştirme eklemek için boş alanı seçin.

1. Üzerinde **görselleştirmeler** sekmesinde **ağaç Haritası** görselleştirme. Sürükleme **modeli** için **grubu** alan. Sürükleme **MaintenanceProbability** için **değerleri** alan.

1. Grafik değiştirin **başlık** için **bakım gerektiren Vehicle modelleri**.

    ![Ağaç Haritası başlığı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

1. Yeni bir görselleştirme eklemek için boş alanı seçin.

1. Üzerinde **görselleştirmeler** sekmesinde **% 100 Yığılmış grafik** görselleştirme. Sürükleme **Şehir** için **eksen** alan. Sürükleme **MaintenanceProbability** ve **RecallProbability** için **değer** alan.

    ![Eksen ve değer alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

1. Üzerinde **biçimi** bölümünden **veri renkleri**. Ayarlama **MaintenanceProbability** renk değerine **F2C80F**.

1. Grafik değiştirin **başlık** için **Vehicle olasılık bakım & geri çağırma şehre göre**.

    ![% 100 Yığılmış çubuk grafik başlığı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

1. Yeni bir görselleştirme eklemek için boş alanı seçin.

1. Üzerinde **görselleştirmeler** sekmesinde **alan grafiği** görselleştirme. Sürükleme **modeli** için **eksen** alan. Sürükleme **engineOil**, **tirepressure**, **hızı**, ve **MaintenanceProbability** için **değerleri** alan. Toplama tipine değiştirme **ortalama**. 

    ![Toplama türü](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

1. Grafik değiştirin **başlık** için **modeli altyapısı petrol, Lastik baskısı, hızı ve Bakım olasılık ortalama**.

    ![Grafik başlığı alanı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

1. Yeni bir görselleştirme eklemek için boş alanı seçin.

1. Üzerinde **görselleştirmeler** sekmesinde **dağılım grafiği** görselleştirme. Sürükleme **modeli** için **ayrıntıları** ve **gösterge** alanları. Sürükleme **yakıt** için **x ekseni** alan. Toplama işlemini değiştirme **ortalama**. Sürükleme **engineTemperature** için **y ekseni** alan. Toplama işlemini değiştirme **ortalama**. Sürükleme **toplamıdır** için **boyutu** alan.

    ![Ayrıntılar, açıklama, eksen ve boyutu alanları](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

1. Grafik değiştirin **başlık** için **yakıt engineTemperature ortalama ve Model tarafından toplamıdır sayısı, ortalama**.

    ![Grafik başlığı dağılım](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

    Raporun son hali aşağıdaki örnekteki gibi görünür:

    ![Raporun son hali](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Gerçek zamanlı panoya raporlardaki görselleştirmeleri
1. Yanındaki artı simgesini seçerek bir boş Pano oluşturma **panolar**. Bir ad girin **araç Telemetri analizi Panosu**.

    ![Pano artı simgesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

1. Önceki raporlar görselleştirmeler panoya sabitleyin. 

    ![Panoya Sabitle simgesi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

    Panoya Sabitlenen üç tüm raporlar, aşağıdaki örnekteki gibi görünmesi gerekir. Tüm raporları oluşturmadıysanız, panonuzun farklı görünebilir. 

    ![Raporlar içeren Pano](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gerçek zamanlı Pano oluşturuldu. CarEventGenerator.exe ve RealtimeDashboardApp.exe yürütülecek devam ederken, panosunda canlı güncelleştirmeler bakın. Aşağıdaki adımları tamamlanması yaklaşık 10-15 dakika sürer.

## <a name="set-up-the-power-bi-batch-processing-dashboard"></a>Power BI toplu işleme Pano Ayarla
> [!NOTE]
> Uçtan uca toplu yürütme ve bir yıl değerinde oluşturulan veri işleme ardışık düzen işleme için (Başlangıç, dağıtımın başarılı olarak tamamlanmasına) yaklaşık iki saat sürer. İşleme, aşağıdaki adımlarla devam etmeden önce tamamlanması için bekleyin:
> 
> 

### <a name="download-the-power-bi-designer-file"></a>Power BI Tasarımcısı dosyasını indir

1. Önceden yapılandırılmış bir Power BI Tasarımcısı dosyası, dağıtım el ile işlem yönergeleri bir parçası olarak dahil edilir. "2 arayın. Power BI toplu işleme Pano ayarlayın."

1. Burada adlı toplu işleme Pano için Power BI şablon indirme **ConnectedCarsPbiReport.pbix**.

1. Yerel olarak kaydedin.

### <a name="configure-power-bi-reports"></a>Power BI raporlarını yapılandırma

1. Tasarımcı dosyası açın **ConnectedCarsPbiReport.pbix** Power BI Desktop'ı kullanarak. Zaten sahip değilseniz, Power BI Desktop'tan yükleme [Power BI Desktop yükleme](https://www.microsoft.com/download/details.aspx?id=45331) Web sitesi.

1. Seçin **sorguları Düzenle**.

    ![Sorguları Düzenle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

1. Çift **kaynak**.

    ![Kaynak](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

1. Azure SQL Server dağıtımının bir parçası sağlanan server bağlantı dizesini güncelleştirin. Azure SQL veritabanı altında el ile işlem yönergeleri bakın:

    * Sunucu: somethingsrv.database.windows.net
    * Veritabanı: connectedcar
    * Kullanıcı adı: kullanıcı adı
    * Parola: SQL Server parolanız Azure portalından yönetebilirsiniz.

1. Bırakın **veritabanı** olarak **connectedcar**.

    ![Database](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

1. **Tamam**’ı seçin.

1. **Windows kimlik bilgisi** sekmesi, varsayılan olarak seçilidir. Değiştirin **veritabanı kimlik bilgileri** seçerek **veritabanı** sekmesine sağ.

1. Girin **kullanıcıadı** ve **parola** onun dağıtım Kurulum sırasında belirtilen Azure SQL veritabanınızın.

    ![Veritabanı kimlik bilgileri](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

1. **Bağlan**’ı seçin.

1. Sağ bölmede mevcut üç kalan sorguların her önceki adımı yineleyin. Ardından veri kaynağı bağlantı bilgilerini güncelleştirin.

1. Seçin **kapatın ve yükleme**. Power BI Desktop dosyası veri kümelerini SQL veritabanı tablolarına bağlı.

1. Seçin **kapatmak** Power BI Desktop dosyasını kapatın.

    ![Kapat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

1. Seçin **Kaydet** değişiklikleri kaydedin. 

Toplu iş çözümdeki yolu işleme karşılık gelen tüm raporları yapılandırdınız. 

## <a name="upload-to-powerbicom"></a>Powerbi.com üzerinde karşıya yükleme
1. Git [Power BI web portalı](http://powerbi.com)ve oturum açın.

1. **Veri Al**’ı seçin.

1. Power BI Desktop dosyasını karşıya yükleyin. Seçin **Veri Al** > **dosyaları Al** > **yerel dosya**.

1. Git **ConnectedCarsPbiReport.pbix**.

1. Dosya karşıya yüklendikten sonra Power BI çalışma alanınıza geri dönün. Bir veri kümesi, rapor ve boş bir Pano sizin için oluşturulur.  

1. Adlı yeni bir panoya Sabitle grafikleri **araç Telemetri analizi Panosu** Power bı'da. Daha önceden oluşturuldu ve ardından Git boş panoyu seçin **raporları** bölümü. Yeni yüklenen raporu seçin.  

    ![Yeni Power BI Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

    Rapor altı sayfası vardır:

    1. sayfası: Araç yoğunluğu  
    2. sayfası: Gerçek zamanlı araç durumu  
    3. sayfası: Agresif taşıtlardan temelli   
    4. sayfası: Yükümlülüğümüz araçları  
    Sayfa 5: verimli Araçlar temelli yakıt  
    Sayfa 6: Contoso Motors logosu  

    ![Altı sayfaları ile Power BI raporu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

1. Gelen **sayfa 3**, aşağıdaki içeriği Sabitle:  

    a. **Toplamıdır sayısı**  

   ![Sayfa 3 toplamıdır sayısı](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png)

    b. **Agresif modeli – Şelale grafik araçları temelli** 

   ![3. sayfası grafik 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

1. Gelen **sayfa 5**, aşağıdaki içeriği Sabitle: 

    a. **Toplamıdır sayısı**

   ![Sayfa 5 grafik 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)

    b. **Model Fuel-Efficient araçların: Kümelenmiş sütun grafik**

   ![Sayfa 5 grafik 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

1. Gelen **sayfa 4**, aşağıdaki içeriği Sabitle:  

    a. **Toplamıdır sayısı** 

   ![4. sayfası grafik 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

    b. **Şehir, yükümlülüğümüz araçların modeli: ağaç Haritası**

   ![4. sayfası grafik 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

1. Gelen **sayfa 6**, aşağıdaki içeriği Sabitle:  

    * **Contoso Motors logosu**

    ![Contoso Motors logosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

### <a name="organize-the-dashboard"></a>Panoyu Düzenle  

1. Panoya gidin.

1. Her grafik üzerine gelin. Tamamlanmış Pano aşağıda sağlanan adlandırma üzerinde göre her bir grafik yeniden adlandırın:

   ![Pano kuruluş](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png) 
   
1. Pano aşağıdaki örnekteki gibi yaklaşık aramak için grafikler taşıyın:

    ![Düzenlenmiş Panosu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)

1. Bu belgede belirtilen tüm raporları oluşturduktan sonra son tamamlanmış Pano aşağıdaki örnekteki gibi görünür: 

   ![Son Pano](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Başarıyla raporlar ve Pano elde etmek için oluşturduğunuz gerçek zamanlı ve Tahmine dayalı ve batch Öngörüler araç durumu ve sürüş alışkanlıkları.  
