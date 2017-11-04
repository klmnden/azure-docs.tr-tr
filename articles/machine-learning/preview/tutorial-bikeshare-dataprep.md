---
title: "Bisiklet paylaşımı öğretici - Azure Machine Learning çalışma ekranı ile gelişmiş veri hazırlama"
description: "Ve Azure Machine Learning çalışma ekranı kullanarak uçtan uca veri hazırlık öğretici"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: article
ms.date: 09/21/2017
ms.openlocfilehash: 4918a820ca07a7d83dfb544ee9f0bbeb66d22722
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Bisiklet paylaşımı Öğreticisi: Azure Machine Learning çalışma ile verileri hazırlama Gelişmiş
Azure Machine Learning Hizmetleri (Önizleme) olan bir tümleşik, uçtan uca veri bilimi ve verileri hazırlama, denemeler geliştirmek ve bulut ölçeğinde modelleri dağıtmak profesyonel veri bilimcilerine için Gelişmiş analiz çözümü.

Bu öğreticide, Azure Machine Learning Hizmetleri (Önizleme) öğrenmek için kullandığınız nasıl yapılır:
> [!div class="checklist"]
> * Azure Machine Learning veri Hazırlık Aracı etkileşimli olarak ile verileri hazırlama
> * İçeri aktarma, dönüştürme ve test veri kümesi oluşturma
> * Veri hazırlama Paket Oluştur
> * Veri hazırlama Python kullanarak paketini çalıştırın
> * Ek giriş dosyaları için veri hazırlığı paketi yeniden kullanarak bir eğitim veri kümesi oluştur

> [!IMPORTANT]
> Bu öğretici yalnızca verileri hazırlar, tahmin modeli oluşmuyor.
>
> Kendi tahmin modeli eğitmek için hazırlanan verileri kullanabilirsiniz. Örneğin, bisiklet isteğe bağlı bir 2 saatlik penceresi sırasında tahmin etmek için bir model oluşturma.

## <a name="prerequisites"></a>Ön koşullar
* Azure Machine Learning çalışma ekranı yerel olarak yüklü olması gerekir. Daha fazla bilgi için izleyin [yükleme Hızlı Başlangıç](quickstart-installation.md).
* Çalışma ekranı içinde yeni bir proje oluşturma ile benzer.

## <a name="data-acquisition"></a>Veri alma
Bu öğretici kullanır [Boston Hubway dataset](https://s3.amazonaws.com/hubway-data/index.html) ve Boston hava durumu verilerini [NOAA](http://www.noaa.gov/).

1. Veri dosyaları, yerel geliştirme ortamınız için aşağıdaki bağlantılardan birini indirin. 
   * [Boston hava veri](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Hubway Web sitesinden Hubway seyahat verileri.

      - [201501 hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504 hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510 hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601 hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604 hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610 hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701 hubway tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Her .zip dosyasını karşıdan yükleme sonrasında sıkıştırmasını açın.

## <a name="learn-about-the-datasets"></a>Veri kümeleri hakkında bilgi edinin
1. __Boston hava__ dosya saatlik olarak bildirilen aşağıdaki hava durumu ile ilgili alanlar içeriyor:
   * TARİH
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. __Hubway__ yıl ve ay tarafından veri dosyalarıyla düzenlenir. Örneğin, adlı dosyayı `201501-hubway-tripdata.zip` Ocak 2015 için veri içeren bir .csv dosyası içerir. Verileri bir bisiklet seyahati temsil eden her satır aşağıdaki alanları içerir:

   * Seyahat süresi (saniye)
   * Başlangıç saati ve tarihi
   * Bitiş saati ve tarihi
   * İstasyon Adı & kimliği Başlat
   * Uç istasyon adı & kimliği
   * Bisiklet kimliği
   * Kullanıcı türü (sıradan = 24 saat veya 72 saat geçişi kullanıcı; Üye = yıllık veya aylık üyesi)
   * POSTA kodu (kullanıcı üyesi ise)
   * Cinsiyeti (üyesi tarafından otomatik olarak bildirilen)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma
1. Başlatma **Azure Machine Learning çalışma ekranı** Başlat menüsünden veya Başlatıcısı.

2. Yeni bir Azure Machine Learning projesi oluşturun.  Tıklatın  **+**  düğmesini **projeleri** sayfasında veya **dosya** > **yeni**.
   - Kullanım **boş proje** şablonu.
   - Projenizin adı **BikeShare**. 

## <a id="newdatasource"></a>Yeni bir veri kaynağı oluşturun

1. Yeni bir veri kaynağı oluşturun. Tıklatın **veri** sol araç çubuğu düğmesini (silindir simgesi). Bu görüntüler **veri görünümü**.

   ![Veri Görünümü sekmesinde görüntüsü](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Bir veri kaynağı ekleyin. Seçin  **+**  simgesine ve ardından **veri kaynağı Ekle**.

   ![Veri Kaynağı Ekle giriş görüntüsü](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Hava durumu veri ekleme

1. **Veri deposu**: verileri içeren veri deposu seçin. Dosyaları kullandığından seçin **dosyaları / Directory**. Seçin **sonraki** devam etmek için.

   ![Dosya görüntüsü / dizin girişi](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Dosya Seçimi**: hava veri ekleyin. Göz atın ve seçim `BostonWeather.csv` daha önce indirilen dosya. **İleri**’ye tıklayın.

   ![Seçili BostonWeater.csv dosya seçimle görüntüsü](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **Dosya ayrıntıları**: algılandığında dosyası şemasını doğrulayın. Azure Machine Learning çalışma ekranı dosyasındaki verileri analiz eder ve kullanmak için şema oluşturur.

   ![Dosya Ayrıntıları görüntüsü](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > Çalışma ekranı, bazı durumlarda doğru şemayı algılayamayabilir. Her zaman parametreleri, veri kümesi için doğru olduğunu doğrulamanız gerekir. Hava durumu verileri için aşağıdaki değerlere ayarlandığını doğrulayın:
   >
   > * __Dosya türü__: ayrılmış değerler dosyası (csv, tsv, txt, vb.)
   > * __Ayırıcı__: virgül [,]
   > * __Açıklama satırı karakter__: hiçbir değer ayarlanır.
   > * __Atla satırları modu__: atlamayın
   > * __Kodlama dosya__: utf-8
   > * __Üstbilgi modu Yükselt__: üst bilgileri ilk dosyasından kullanın

   Verilerin önizlemesini aşağıdaki sütunlar görüntülenir:
   * **Path**
   * **TARİH**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Devam etmek için seçin **sonraki**.

4. **Veri türleri**: otomatik olarak algılanır veri türlerini gözden geçirin. Azure Machine Learning çalışma ekranı dosyasındaki verileri analiz eder ve kullanmak için veri türleri oluşturur.

   Bu veriler için değiştirme `DATA TYPE` için tüm sütunların `String`.

   > [!NOTE]
   > `String`Daha sonra Bu öğreticide çalışma ekranı özelliklerini vurgulamak için kullanılır. 

   ![Veri türleri görüntüsü](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Devam etmek için seçin __sonraki__. 

5. **Örnekleme**: örnekleme düzeni oluşturmak için seçin **+ yeni** düğmesi. Yeni __üst 10000__ eklendi ve ardından satır __Düzenle__. Ayarlama __örnek stratejisi__ için **tam dosya**ve ardından **Uygula**.

   ![Yeni bir örnekleme stratejisi ekleme resmi](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   Kullanılacak __tam dosya__ stratejisi, select __tam dosya__ giriş ve ardından __etkin ayarlanmış__. Bir yıldız görünür __tam dosya__ etkin stratejisi olduğunu belirtmek için.

   ![Tam dosya etkin stratejisi olarak görüntüsü](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Devam etmek için seçin **sonraki**.

6. **Yol sütun**: __yolu sütun__ bölümü alınan verileri bir sütun olarak tam dosya yolunu eklemenizi sağlar. Seçin __yolu sütun içermez__.

   > [!TIP]
   > Yolun bir sütun olarak dahil olmak üzere birçok farklı dosya adları dosyalarıyla klasörünü alıyorsanız yararlı olur. Dosya adları daha sonra ayıklamak istediğiniz bilgileri içeriyorsa de yararlıdır.

   ![Görüntü yolu sütunun içermeyen ayarlayın](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Son**: veri kaynağı oluşturmayı tamamlamak için seçin **son** düğmesi.

    Yeni bir veri kaynağı sekmesinde adlı __BostonWeather__ açar. Bir örnek veri ızgara görünümünde görüntülenir. Örnek daha önce belirtilen etkin örnekleme düzenini temel alır.

    Bildirim **adımları** ekranın sağ taraftaki bölmesinde bu veri kaynağı oluşturulurken yapılan tek tek Eylemler görüntüler.

   ![Veri kaynağı, örnek ve adımları görüntüleme görüntüsü](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Veri kaynağı metrikleri görüntüleyin

Seçin __ölçümleri__ üstündeki düğmesi sekmenin ızgara görünümünde sol. Bu görünüm dağıtım ve diğer toplu istatistikler örneklenen verileri görüntüler.

![Ölçümleri görüntüsünü görüntüler](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> İstatistikleri görünürlüğünü yapılandırmak için kullanın **seçin ölçüm** açılır. Denetleyin ve ızgara görünümü değiştirmek için ölçümleri var. seçeneğinin işaretini kaldırın.

Döndürülecek __veri görünümü__seçin __veri__ sayfanın sol.

## <a name="add-data-source-to-data-preparation-package"></a>Veri hazırlama paketi için veri kaynağı ekleme

1. Seçin __Prepare__ verileri hazırlama başlamak için. 

2. İstendiğinde, gibi veri hazırlığı paketi için bir ad girin `BikeShare Data Prep`. 

3. Seçin __Tamam__ devam etmek için.

   ![Prepare iletişim kutusunun görüntüsü](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Adlı yeni bir paket **BikeShare veri hazırlığı** altında görünür __veri hazırlığı__ bölümünü __veri__ sekmesi. 

   Paket görüntülemek için bu girişi seçin. 

5. Seçin  **>>**  görüntülenecek genişletmek için düğmesini __veri akışları__ pakette yer alan. Bu örnekte, __BostonWeather__ yalnızca veri akışı değil.

   > [!IMPORTANT]
   > Bir paket birden çok veri akışları içerebilir.

   ![Pakette yer alan veri akışları görüntüsü](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Veri değere göre filtreleme
1. Verilere filtre, belirli bir değere sahip bir hücre sağ tıklayın ve seçmek için __filtre__ve ardından filtre türü.

2. Bu öğretici için değeri içeren bir hücre seçin `FM-15` ve ardından filtre oluşan bir filtre **eşittir**.  Veriler yalnızca döndürür için satır filtrelenmiş şimdi burada __REPORTTYPE__ olan `FM-15`.

   ![Filtre iletişim kutusunun görüntüsü](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15, Meteorological Terminal Aviation rutin hava durumu raporu (METAR) bir türüdür. FM 15 raporları empirically en, listtle eksik verilerle tamamlanması gözetilir.

## <a name="remove-a-column"></a>Bir sütunu kaldırmak

Artık ihtiyaç duymadığınız __REPORTTYPE__ sütun. Sütun başlığına sağ tıklayın ve **sütun Kaldır**.

   ![Kaldır sütun seçeneği görüntüsü](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Veri türlerini değiştirme ve hatalarını kaldırın.%0
1. Tuşuna basarak __Ctrl (komutu ⌘ Mac üzerinde)__ sütun başlıklarını seçerek aynı anda birden çok sütun seçmenize olanak sağlar, ancak. Bu, aşağıdaki sütun üst bilgileri seçmek için kullanın:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Sağ** aşağıdakilerden birini seçin ve seçili sütun üst bilgileri **Dönüştür alan türü sayısal**. Bu sütun için veri türü için sayısal dönüştürür.

   ![Birden çok sütun için sayısal dönüştürme](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Hata değerlere filtre. Bazı sütunlar veri dönüştürme sorunları türüne sahip. Bu sorun, kırmızı rengi belirttiği __veri kalitesi çubuğu__ sütun için.

   Hatalar içeren satırların kaldırmak için sağ **HOURLYDRYBULBTEMPF** sütun başlığı. Seçin **sütununa filtre**. Varsayılan kullanmak **t istediğiniz** olarak **tutmak satırları**. Değişiklik **koşullar** seçmek için aşağı **değil hata**. Seçin **Tamam** filtre uygulamak için.

4. Diğer sütunlardaki kalan hata satırları ortadan kaldırmak için bu filtre işlemi için yineleyin **HOURLYRelativeHumidity** ve **HOURLYWindSpeed** sütun.

## <a name="use-by-example-transformations"></a>Tarafından örnek Dönüşümleri kullanma

Verileri bir tahmini için iki saat blokları kullanmak için iki saatlik dönem için ortalama hava koşulları işlem gerekir. Bunu yapmak için aşağıdaki işlemleri kullanabilirsiniz:

* Bölünmüş **tarih** ayrı sütununa **tarih** ve **zaman** sütun. Ayrıntılı adımları için sonraki bölüme bakın.

* Türetilen bir **Hour_Range** sütundan **zaman** sütun. Ayrıntılı adımlar için aşağıdaki bölüme bakın.

* Türetilen bir **tarih\_saat\_aralığı** sütundan **tarih** ve **Hour_Range** sütun. Ayrıntılı adımlar için aşağıdaki bölüme bakın.

### <a name="split-column-by-example"></a>Sütunu örneğe göre bölme

1. Bölünmüş **tarih** ayrı tarih ve saat sütunları sütununa. Sağ tıklayın **tarih** sütun başlığını ve select **örneğe göre bölünmüş sütun**.

   ![Örnek giriş tarafından bölünmüş sütun görüntüsü](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Azure Machine Learning çalışma ekranı otomatik olarak anlamlı bir sınırlayıcı tanımlar ve tarih ve saat değerleri veri bölerek iki sütun oluşturur. 

3. Seçin __Tamam__ bölme işlemi sonuçları kabul etmek için.

   ![Bölünmüş sütunları DATE_1 ve DATE_2 görüntüsü](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Sütunu örneğe göre türetme

1. İki saatlik aralığı çıkarmaya sağ __tarih\_2__ sütun başlığını ve seçin **türetilen sütun örneğe göre**.

   ![Örnek giriş tarafından derive sütun görüntüsü](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Yeni bir boş sütun, null değerlere sahip eklenir.

2. Yeni bir sütun ilk boş hücreyi tıklatın. Yazarak istenen zaman aralığı için bir örnek sağlar `12AM-2AM` yeni bir sütun ve ENTER tuşuna basın.

   ![Yeni bir sütun görüntüsü bir değerle, 00: 00-02: 00](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Azure ML çalışma ekranı tarafından sağlanan örnekleri dayalı bir program ölçekli harfleri sentezler ve aynı programın kalan satırlarda uygular. Diğer tüm satırlar, sağlanan örnek üzerinde otomatik olarak doldurulur. Çalışma ekranı de verilerinizi analiz eder ve kenar durumlarını tanımlamak çalışır. 

   > [!IMPORTANT]
   > Edge durumlarda tanımlaması Mac ekranının geçerli sürümde çalışmayabilir. Atla __3. adım__ ve __4. adım__ aşağıda Mac üzerinde Bunun yerine, basın __Tamam__ tüm satırları türetilmiş değerlerle doldurulmuş sonra.
   
3. Metin **verilerini analiz etme** kılavuz çalışma ekranı kenar durumlarda algılamak yaptığını belirtir. İşiniz bittiğinde, durum değişikliklerini **gözden geçirme sonraki önerilen satır** veya **öneri**. Bu örnekte, **gözden geçirme sonraki önerilen satır** döndürülür.

4. Önerilen değişikliklerin gözden geçirmek için seçin **gözden geçirme sonraki önerilen satır**. Gözden geçirmeli ve (gerekirse) düzeltmek hücre ekranda vurgulanır.

   ![Gözden geçirme satır görüntüsü](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Seçin __Tamam__ dönüştürme kabul etmek için.
 
5. Veri Izgara Görünümü döndürülürsünüz __BostonWeather__. Kılavuz artık önceden eklenen üç sütun içerir.

   ![Izgara Görünümü eklenmiş satırlar ile görüntüsü](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Üzerinde yaptığınız tüm değişiklikleri korunur **adımları** bölmesi. Oluşturduğunuz adıma **adımları** bölmesinde, aşağı oka ve seçim tıklatın **Düzenle**. Gelişmiş penceresi **türetilen sütun örneğe göre** görüntülenir. Tüm örnekleri burada korunur. Kılavuzda bir satırı çift tıklayarak örnekler el ile de ekleyebilirsiniz. Seçin **iptal** değişiklikleri uygulamadan ana kılavuza dönün. Seçerek bu görünüme erişebilen **Gelişmiş mod** gerçekleştirirken bir **türetilen sütun örneğe göre** dönüştürün.

6. Sütunu yeniden adlandırmak için sütun başlığını ve türü çift **saat aralığı**. Tuşuna **Enter** değişikliği kaydetmek için.

   ![Sütun yeniden adlandırma](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Tarih ve saat aralığı, çoklu seçim çıkarmaya **tarih\_1** ve **saat aralığı** sütunları, sağ tıklatın ve ardından **Derive sütun örneğe göre**.

   ![Sütun örnekle türetme](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Tür `Jan 01, 2015 12AM-2AM` ilk satır ve tuşuna karşı örnek olarak **Enter**.

   Çalışma ekranı tabanlı sağladığınız örnek üzerinde dönüştürme belirler. Bu örnekte, sonuç tarih biçimi değişti ve iki saatlik bir aralık ile birleştirilmiş olmasıdır.

   ![Örnek görüntüsü ' 01 Ocak 2015 00: 00-02: 00](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

   > [!IMPORTANT]
   > Mac üzerinde yerine aşağıdaki adımı izleyin __adım 8__ aşağıda.
   >
   > * İçeren ilk hücrenin gidin `Feb 01, 2015 12AM-2AM`. Olmalıdır __satır 15__. Değerine düzeltmek `Jan 02, 2015 12AM-2AM`ve basın __Enter__. 
   

8. Durumu değiştirmek bekleyin **verilerini analiz etme** için **gözden geçirme sonraki önerilen satır**. Bu, birkaç saniye sürebilir. Önerilen satırına gidin durumunu bağlantısını seçin. 

   ![Gözden geçirmek için önerilen satır görüntüsü](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Kaynak tarih değeri gg/aa/yyyy veya aa/gg/yyyy olabileceğinden satır bir null değer içeriyor. Doğru değeri yazın `Jan 13, 2015 2AM-4AM` ve basın **Enter**. Çalışma ekranı iki örnek kalan satırlar için türetme geliştirmek için kullanır.

   ![Doğru biçimlendirilmiş veriler görüntüsü](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Seçin **Tamam** dönüştürme kabul etmek için.

   ![Tamamlanan dönüştürme kılavuz görüntüsü](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Gelişmiş modunu kullanabilirsiniz **Derive sütun örneğe göre** aşağı oka tıklayarak bu adım için **adımları** bölmesi. Veri kılavuzunda sütun adlarının yanındaki onay kutularını vardır **tarih\_1** ve **saat aralığı** sütun. Yanındaki onay kutusunun işaretini **saat aralığı** Bu çıktı nasıl değiştiğini görmek için sütun. Olmadığında **saat aralığı** sütunu giriş olarak **00: 00-02: 00** bir sabit değer olarak kabul edilir ve türetilen değerlere eklenir. Seçin **iptal** yaptığınız değişiklikleri uygulamadan ana kılavuza dönün.

10. Sütunu yeniden adlandırmak için başlığını çift tıklatın. Adına değiştirme **tarih saat aralığı** ve tuşuna basarak **Enter**.

11. Çoklu seçim **tarih**, **tarih\_1**, **tarih\_2**, ve **saat aralığı** sütun. Sağ tıklayın ve ardından **Kaldır sütun**.

## <a name="summarize-data-mean"></a>(Ortalama) verilerini özetle

Sonraki adım, saat aralığına göre gruplandırılmış değerlerin ortalamasını gerçekleştirerek hava koşulları sunmaktır.

1. Seçin **tarih saat aralığı** sütun ve ardından **özetleme** gelen **dönüştüren** menüsü.

    ![Menü dönüştürür](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Özetlemek için sütun sayfanın sonundaki kılavuz üst sol ve sağ bölmeleri için sürükleyin. Sol bölmede metni içeren **veri gruplandırmak için sütunları buraya sürükleyin**. Sağ bölmede metni içeren **özetlemek için sütunları buraya sürükleyin**. 

    Sürükleme **tarih saat aralığı** sol bölmenin altındaki kılavuz sütundan. Sürükleme **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity**, ve **HOURLYWindSpeed** sağ bölmede için. 

    Sağ bölmede seçin **anlamına** olarak **toplama** ölçü her sütun için. Tıklatın **Tamam** özetleme tamamlamak için.

   ![Özetlenmiş veri ekran görüntüsü](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Veri akışı komut dosyası kullanarak dönüştürme

0-1 aralığına sayısal sütunlarda verileri değiştirme bazı modelleri hızla yakınsamasını sağlar. Şu anda bu dönüşüm genel yapmak için yerleşik hiçbir dönüştürme yoktur, ancak bir Python komut dosyası bu işlemi gerçekleştirmek için kullanılabilir.

1. Gelen **dönüştürme** menüsünde, select **dönüştürme veri akışı**.

2. Aşağıdaki kod görüntülenen metin kutusuna girin. Sütun adlarını kullandıysanız, kodu değişiklik yapmadan çalışması gerekir. Python içinde bir basit min-max normalleştirme mantığı yazarsınız.

    > [!WARNING]
    > Komut dosyasını daha önce bu öğreticide kullanılan sütun adları bekliyor. Farklı bir sütun adları varsa, komut dosyası adlarında değiştirmeniz gerekir.

   ```python
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   minVal = min(df["HOURLYDRYBULBTEMPF_Mean"])
   df["HOURLYDRYBULBTEMPF_Mean"] = (df["HOURLYDRYBULBTEMPF_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYDRYBULBTEMPF_Mean":"N_DryBulbTemp"},inplace=True)

   maxVal = max(df["HOURLYRelativeHumidity_Mean"])
   minVal = min(df["HOURLYRelativeHumidity_Mean"])
   df["HOURLYRelativeHumidity_Mean"] = (df["HOURLYRelativeHumidity_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYRelativeHumidity_Mean":"N_RelativeHumidity"},inplace=True)

   maxVal = max(df["HOURLYWindSpeed_Mean"])
   minVal = min(df["HOURLYWindSpeed_Mean"])
   df["HOURLYWindSpeed_Mean"] = (df["HOURLYWindSpeed_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYWindSpeed_Mean":"N_WindSpeed"},inplace=True)

   df
   ```

    > [!TIP]
    > Python betiğini döndürmelidir `df` sonunda. Bu değer, kılavuz doldurmak için kullanılır.
    
    ![Veri akışı komut dosyası iletişimi dönüştürme](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Seçin __Tamam__ betik kullanmak için. Kılavuz sayısal sütunlarda artık 0-1 aralığında değerleri içerir.

    ![Değerler 0 ile 1 arasında içeren kılavuz](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Hava durumu verileri hazırlama tamamladınız. Ardından, seyahat veri hazırlayın.

## <a name="load-trip-data"></a>Seyahat veri yükleme

1. İçeri aktarmak için `201701-hubway-tripdata.csv` dosya, içindeki adımları kullanın [yeni bir veri kaynağı oluşturun](#newdatasource) bölümü. İçeri aktarma işlemi sırasında aşağıdaki seçenekleri kullanın:

    * __Örnekleme düzeni__: **tam dosya** düzeni örnekleme olun örnek etkin ve 
    * __Veri türü__: Varsayılanları kabul edin.

2. Verileri aldıktan sonra Seç __Prepare__ düğmesi veri noktalarını hazırlamaya başla. Varolanı seç **BikeShare veri Prep.dprep** paketini ve ardından __Tamam__.

    Bu işlem ekler bir **veri akışı** varolan **veri hazırlığı** yeni bir tane oluşturmak yerine dosya.

    ![Varolan paket seçme görüntüsü](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Kılavuz yüklendikten sonra genişletin __veri akışları__. Şimdi iki veri akışları vardır: **BostonWeather** ve **201701 hubway tripdata**. Seçin **201701 hubway tripdata** girişi.

    ![201701 hubway tripdata giriş görüntüsü](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Harita denetçisi kullanın

Veri hazırlama için çok sayıda adlı yararlı görsel öğe vardır **denetçiler** verileri daha iyi anlamak ve aykırı değerlerini tanımlayan Yardım dize, sayısal ve Geographical veriler için. Harita denetçisi kullanmak için aşağıdaki adımları kullanın:

1. Çoklu seçim **istasyon enlem Başlat** ve **istasyon boylam Başlat** sütun. Sütunları birine sağ tıklayın ve ardından **harita**.

    > [!TIP]
    > Çoklu seçim etkinleştirmek için basılı __Ctrl (komutu ⌘ Mac üzerinde)__ ve her sütunun başlığını seçin.

    ![Harita görselleştirme görüntüsü](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Harita görselleştirme en üst düzeye çıkarmak seçin **Ekranı Kapla** simgesi. Harita penceresine sığacak şekilde seçin **E** görselleştirme sol üst tarafındaki simgesi.

    ![Tam ekran görüntüsü](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Tıklayın **simge durumuna küçült** kılavuz görünümüne dönmek için düğmesi.

## <a name="use-column-statistics-inspector"></a>Sütun istatistikleri denetçisi kullanın

Sütun istatistikleri denetçisi kullanmak için sağ __tripduration__ sütun ve select __sütun istatistikleri__.

![Sütun istatistikleri giriş](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Bu işlem başlıklı yeni bir görsel öğe ekler __tripduration istatistikleri__ içinde __DENETÇİLER__ bölmesi.

![Tripduration istatistikleri denetçisi görüntüsü](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> En yüksek değer seyahat süresi **961,814 dakika**, yaklaşık iki yıllık olduğu. Veri kümesini bazı aykırı değerlerini olduğunuz görünüyor.

## <a name="use-histogram-inspector"></a>Histogram denetçisi kullanın

Aykırı değerlerini belirlemeye çalışmak için sağ tıklatın __tripduration__ sütun ve select __Histogram__.

![Histogram denetçisi](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Histogram aykırı değerlerini grafik eğriltme yararlı, değildir.

## <a name="add-column-using-script"></a>Komut dosyası kullanarak sütun ekleme

1. Sağ **tripduration** sütun ve select **Sütun Ekle (komut)**.

    ![Ekle sütun (komut) menü görüntüsü](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. İçinde __Sütun Ekle (komut)__ iletişim kutusunda, aşağıdaki değerleri kullanın:

    * __Yeni sütun adı__: logtripduration
    * __Bu yeni sütun After INSERT__: tripduration
    * __Yeni sütun kodu__:`math.log(row.tripduration)`
    * __Kod bloğunun türü__: ifade

   ![Sütun (komut) Ekle iletişim kutusu](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Seçin __Tamam__ eklemek için **logtripduration** sütun.

4. Sağ tıklatın ve sütun **Histogram**.

    ![Histogram logtripduration sütunun](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Görsel olarak, bu Histogramı anormal kuyruklu Normal dağıtım gibi görünüyor.

## <a name="use-advanced-filter"></a>Gelişmiş Filtre kullanın

Verileri bir filtre kullanarak denetçiler yeni dağıtım ile güncelleştirir. Sağ **logtripduration** sütun ve select **filtre sütunu**. İçinde __Düzenle__ iletişim kutusunda, aşağıdaki değerleri kullanın:

* __Bu sayı sütun filtre__: logtripduration
* __Şunu yapmak istiyorum__: satırları tut
* __Zaman__: aşağıdaki koşullardan herhangi biri True (mantıksal OR) olan
* __Varsa bu sütun__: küçüktür
* __Değer__: 9

![Filtre Seçenekleri](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Seçin __Tamam__ filtre uygulamak için.

![Filtre uygulandıktan sonra güncelleştirilmiş çubuk grafikler](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Hale efekti

1. En üst düzeye **logtripduration** Histogram oluşturur. Gri histogram yayılan mavi bir çubuk grafik yoktur. Bu görüntü adlı **hale efekti**:

    * **Gri histogram** (Bu durumda, filtreleme işlemi) işleminden önce dağıtım temsil eder.
    * **Mavi histogram** histogram sonra işlemi temsil eder. 

   Hale efekti veriler üzerinde bir işlemi etkisini görselleştirme ile yardımcı olur.

   ![Hale efekti görüntüsü](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Mavi histogram kısa görünen bildirim Öncekine karşılaştırılan. Otomatik veri yeni aralığındaki yeniden bucketing nedeni budur.

2. Hale kaldırmayı seçin __Düzenle__ ve işaretini __Göster hale__.

    ![Histogram seçenekleri](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Seçin **Tamam** hale efekti devre dışı bırakın ve histogram en aza indirmek için.

### <a name="remove-columns"></a>Sütunları kaldırma

Seyahat verilerde her satır bir bisiklet alma olayı temsil eder. Bu öğretici için yalnızca gereksinim duyduğunuz **starttime** ve **İstasyon Kimliği Başlat** sütun. Diğer sütunlara göre kaldırmak birden fazla seçerek bu iki sütun sütun başlığına sağ tıklayın ve ardından **tutmak sütun**. Diğer sütunlara kaldırılır.

![Canlı sütun seçeneği görüntüsü](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Özetlemek (sayı)

2 saatlik bir süre için bisiklet talep özetlemek için türetilmiş sütunlar kullanın.

1. Sağ tıklayın **starttime** sütun ve select **türetilen sütun örneğe göre**

    ![Görüntüsü türetilen örnek seçeneğiyle](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Örneğin, değeri girin `Jan 01, 2017 12AM-2AM` ilk satır için.

    > [!IMPORTANT]
    > Türetilen sütunların önceki örnekte, tarih ve zaman dilimi bulunan bir sütun türetmek için birden çok adımı kullanılır. Bu örnekte, son çıktı örneği sağlayarak bu işlem tek bir adım gerçekleştirilebilir görebilirsiniz.

    > [!NOTE]
    > Bir örnek satır karşı verebilirsiniz. Bu örneğin, değeri `Jan 01, 2017 12AM-2AM` verilerin ilk satırında için geçerlidir.

    ![Örnek veri görüntüsü](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

   > [!IMPORTANT]
   > Mac üzerinde yerine aşağıdaki adımı izleyin __3. adım__ aşağıda.
   >
   > * İçeren ilk hücrenin gidin `Jan 01, 2017 1AM-2AM`. Olmalıdır __satır 14__. Değerine düzeltmek `Jan 01, 2017 12AM-2AM`, yerleştirip __Enter__. 

3. Uygulamanın tüm satırları karşı değerlerini ölçer kadar bekleyin. Birkaç saniye sürebilir. Çözümleme tamamlandıktan sonra kullanmak __gözden geçirme sonraki önerilen satır__ verileri gözden geçirmek için bağlantı.

   ![Gözden geçirme bağlantıyla tamamlanmış analiz görüntüsü](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Hesaplanan değerler doğru olduğundan emin olun. Aksi durumda, beklenen değerle değeri güncelleştirin ve ENTER tuşuna basın. Daha sonra analiz tamamlanmasını bekleyin. Tamamlamak **gözden geçirme sonraki önerilen satır** görene kadar işlem **öneri**. Gördüğünüz sonra **öneri**, uygulama kenar durumlarda incelemiştir ve birleştirilen programla memnun. Dönüştürme kabul etmeden önce visual denetleme dönüştürülmüş verilerin gerçekleştirmek için en iyi bir uygulamadır. 

4. Seçin **Tamam** dönüştürme kabul etmek için. Yeni oluşturulan sütunu yeniden adlandırmak **tarih saat aralığı**.

    ![Adlandırılan sütun görüntüsü](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Sağ **starttime** sütun başlığını ve select **Kaldır sütun**.

6. Özetlemek için seçin __özetleme__ gelen __dönüştürme__ menüsü. Dönüştürme oluşturmak için aşağıdaki eylemleri gerçekleştirin:

    * Sürükleme __tarih saat aralığı__ ve __İstasyon Kimliği Başlat__ sol (Grupla) bölmesine.
    * Sürükleme __İstasyon Kimliği Başlat__ sağa (verilerini özetle) bölmesi.

   ![Özetleme seçenekleri görüntüsü](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Seçin **Tamam** Özet sonuç kabul etmek için.

## <a name="join-dataflows"></a>Veri akışları katılma

Seyahat veri hava durumu verilerle katılmak için aşağıdaki adımları kullanın:

1. Seçin __katılma__ gelen __dönüştüren__ menüsü.

2. __Tablolar__: seçin **BostonWeather** sol veri akışı olarak ve **201701 hubway tripdata** doğru veri akışı olarak. Devam etmek için seçin **sonraki**.

    ![Tabloları seçimleri görüntüsü](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Anahtar sütun__: seçin **tarih saat aralığı** tabloları hem seçip sütununda __sonraki__.

    ![Anahtar sütunlar üzerinde birleştirme görüntüsü](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Birleştirme türü__: seçin __eşleşen satırları__ birleştirme türünü ve ardından olarak __son__.

    ![Eşleşen satırları birleştirme türü](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Bu işlem adlı yeni bir veri akışı oluşturur __birleştirme sonucu__.

## <a name="create-additional-features"></a>Ek özellikleri oluşturma

1. Haftanın gününü içeren bir sütun oluşturmak için sağ tıklayın **tarih saat aralığı** sütun ve select **Derive sütun örneğe göre**. Değeri kullanın __Sun__ Pazar günü oluşan bir tarih için. Örneğin, **01 Ocak 2017 00: 00-02: 00**. Tuşuna **Enter** ve ardından **Tamam**. Bu sütunu yeniden adlandırmak __haftanın günü__.

    ![Haftanın gününü içeren yeni bir sütun oluşturma resmi](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Zaman aralığı için bir satır içeren bir sütun oluşturmak için sağ **tarih saat aralığı** sütun ve Seç **Derive sütun örneğe göre**. Değeri kullanın **00: 00-02: 00** satır içeren için **01 Ocak 2017 00: 00-02: 00**. Tuşuna **Enter** ve ardından **Tamam**. Bu sütunu yeniden adlandırmak **süresi**.

    ![Dönem sütunu görüntüsü](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Kaldırmak için **tarih saat aralığı** ve **rDate saat aralığı** sütunları basın **Ctrl (komutu ⌘ Mac üzerinde)** ve her sütun başlığını seçin. Sağ tıklayın ve ardından **sütun Kaldır**.

## <a name="read-data-from-python"></a>Python'dan veri okuma

Veri hazırlama paketi Python veya PySpark çalıştırın ve sonuç olarak almak bir **veri çerçevesi**.

Python komut dosyası örneği oluşturmak için sağ __BikeShare veri hazırlığı__ seçip __veri erişim kodu dosyası oluştur__. Örnek Python dosyası oluşturulur, **proje klasörünü**ve ayrıca çalışma ekranının içinden bir sekmede yüklenir. Aşağıdaki Python komut dosyası örneği oluşturulan kod yer almaktadır:

```python
# Use the Azure Machine Learning data preparation package
from azureml.dataprep import package

# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# This call will load the referenced package and return a DataFrame.
# If run in a PySpark environment, this call returns a
# Spark DataFrame. If not, it will return a Pandas DataFrame.
df = package.run('BikeShare Data Prep.dprep', dataflow_idx=0)

# Remove this line and add code that uses the DataFrame
df.head(10)
```

Bu öğretici için dosyanın adıdır `BikeShare Data Prep.py`. Bu dosya daha sonra öğreticide kullanılır.

## <a name="save-test-data-as-a-csv-file"></a>Test verileri bir CSV dosyası olarak kaydedin

Kaydetmek için **birleştirme sonucu** veri akışı için bir. CSV dosyası değiştirmelisiniz `BikeShare Data Prep.py` komut dosyası. Aşağıdaki kodu kullanarak Python komut dosyasını güncelleştirin:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Seçin **çalıştırmak** ekranın en üstünden. Komut dosyası olarak gönderilen bir **iş** yerel makine üzerinde. İş durumu değişikliklerini sonra __tamamlandı__, dosyanın belirtilen konuma yazıldı.

## <a name="substitute-data-sources"></a>Alternatif veri kaynakları

Önceki adımlarda kullandığınız `201701-hubway-tripdata.csv` ve `BostonWeather.csv` Test verileri hazırlamak için veri kaynakları. Diğer seyahat veri dosyalarıyla paketini kullanmak için aşağıdaki adımları kullanın:

1. Yeni bir **veri kaynağı** işlemi yapılan aşağıdaki değişiklikler ile daha önce verilen adımları kullanarak:

    * __Dosya Seçimi__: dosya, çoklu seçim altı seyahat tripdata kalan seçerken. CSV dosyaları.

        ![Altı kalan dosyaları yükleme](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > __+ %5 arasında__ giriş belirtir listelenen bir ötesinde beş ek dosyalar vardır.

    * __Dosya ayrıntıları__: ayarlamak __üstbilgileri modu Yükselt__ için **tüm dosyalar aynı üstbilgileri sahip**. Bu değer, her dosya aynı başlığını içerdiğini gösterir.

        ![Dosya Ayrıntıları seçimi](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Sonraki adımlarda kullanılmak üzere bu veri kaynağının adını kaydedin.

2. Projenizde dosyaları görüntülemek için klasör simgesini seçin. Genişletme __aml\_config__ dizin ve ardından `local.runconfig` dosya.

    ![Local.runconfig konumunu görüntüsü](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. Sonuna şu satırları ekleyin `local.runconfig` dosya ve ardından dosyayı kaydetmek için disk simgesini seçin.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Bu değişiklik, özgün veri kaynağı ile altı seyahat veri dosyalarını içeren bir değiştirir.

## <a name="save-training-data-as-a-csv-file"></a>Eğitim verileri bir CSV dosyası olarak kaydedin

Python dosyaya gidin `BikeShare Data Prep.py` önceki düzenlenmesi ve eğitim verileri kaydetmek için farklı bir dosya yolu belirtin.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Yeni bir işi göndermek için kullanmak **çalıştırmak** sayfasının simgesine tıklayın. A **iş** yeni yapılandırmayla gönderildi. Bu iş çıktısı eğitim verilerdir. Bu veriler, daha önce oluşturduğunuz aynı veri hazırlık adımları kullanılarak oluşturulur. İşi tamamlamak için birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bisiklet paylaşımı veri hazırlığı öğreticisini tamamladınız. Bu öğreticide, Azure Machine Learning Hizmetleri (Önizleme) öğrenmek için kullandığınız nasıl yapılır:
> [!div class="checklist"]
> * Azure Machine Learning veri Hazırlık Aracı etkileşimli olarak ile verileri hazırlama
> * İçeri aktarma, dönüştürme ve test veri kümesi oluşturma
> * Veri hazırlama Paket Oluştur
> * Veri hazırlama Python kullanarak paketini çalıştırın
> * Ek giriş dosyaları için veri hazırlığı paketi yeniden kullanarak bir eğitim veri kümesi oluştur

Ardından, veri hazırlığı hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Veri hazırlama Kullanıcı Kılavuzu](data-prep-user-guide.md)
