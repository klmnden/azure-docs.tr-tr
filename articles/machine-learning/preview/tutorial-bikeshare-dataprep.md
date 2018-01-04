---
title: "Bisiklet paylaşımı öğreticisi - Azure Machine Learning Workbench ile gelişmiş veri hazırlama"
description: "Azure Machine Learning Workbench ile uçtan uca veri hazırlama öğreticisi"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: tutorial
ms.date: 09/21/2017
ms.openlocfilehash: 69f6911a95be382b06313d984f09c7e85aec10df
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Bisiklet paylaşımı öğreticisi: Azure Machine Learning Workbench ile gelişmiş veri hazırlama
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve gelişmiş bir analiz çözümüdür.

Bu öğreticide Azure Machine Learning hizmetlerini (önizleme) kullanarak aşağıdakileri yapmayı öğrenirsiniz:
> [!div class="checklist"]
> * Azure Machine Learning Veri Hazırlama aracı ile etkileşimli olarak veri hazırlama
> * Test veri kümesini içeri aktarma, dönüştürme ve oluşturma
> * Veri Hazırlama paketi oluşturma
> * Python kullanarak Veri Hazırlama Paketini çalıştırma
> * Ek girdi dosyaları için Veri Hazırlama paketini yeniden kullanarak bir eğitim veri kümesi oluşturma

> [!IMPORTANT]
> Bu öğretici yalnızca verileri hazırlar; tahmin modeli oluşturmaz.
>
> Hazırlanan verileri, kendi tahmin modellerinizi eğitmek için kullanabilirsiniz. Örneğin, 2 saatlik bir zaman aralığındaki bisiklet talebini tahmin etmeye yönelik bir model oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* Azure Machine Learning Workbench’in yerel olarak yüklü olması gerekir. Daha fazla bilgi için [Yükleme Hızlı Başlangıç](quickstart-installation.md) makalesindeki yönergeleri izleyin.
* Workbench’te yeni proje oluşturma bilgisi.

## <a name="data-acquisition"></a>Veri alma
Bu öğreticide [Boston Hubway veri kümesi](https://s3.amazonaws.com/hubway-data/index.html) ve [NOAA](http://www.noaa.gov/)’daki Boston hava durumu verileri kullanılır.

1. Veri dosyalarını aşağıdaki bağlantılardan yerel geliştirme ortamınıza indirin. 
   * [Boston hava durumu verileri](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Hubway web sitesindeki Hubway seyahat verileri.

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. İndirdiğiniz her .zip dosyasını açın.

## <a name="learn-about-the-datasets"></a>Veri kümeleri hakkında bilgi edinme
1. __Boston weather__ adlı dosya, hava durumu ile ilgili aşağıdaki alanlarda saatlik olarak bildirilen verileri içerir:
   * DATE
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. __Hubway__ verileri, yıl ve aya göre dosyalar halinde düzenlenmiştir. Örneğin, `201501-hubway-tripdata.zip` adlı dosya Ocak 2015’e ait verilerin bulunduğu bir .csv dosyası içerir. Veriler aşağıdaki alanları içerir ve her satır bir bisiklet seyahatini temsil eder:

   * Seyahat Süresi (saniye)
   * Başlangıç Saati ve Tarihi
   * Bitiş Saati ve Tarihi
   * Başlangıç İstasyonunun Adı ve Kimliği
   * Bitiş İstasyonunun Adı ve Kimliği
   * Bisiklet Kimliği
   * Kullanıcı Türü (Normal = 24 Saat veya 72 Saat Geçişli kullanıcı; Üye = Yıllık veya Aylık Üye)
   * Posta Kodu (kullanıcı üye ise)
   * Cinsiyet (üye tarafından bildirilir)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma
1. Başlangıç menüsünden veya başlatıcıdan **Azure Machine Learning Workbench**’i başlatın.

2. Yeni bir Azure Machine Learning projesi oluşturun.  **Projeler** sayfasındaki **+** düğmesine veya **Dosya** > **Yeni** öğesine tıklayın.
   - **Boş Proje** şablonunu kullanın.
   - Projenizi **BikeShare** olarak adlandırın. 

## <a id="newdatasource"></a>Yeni veri kaynağı oluşturma

1. Yeni bir veri kaynağı oluşturun. Sol araç çubuğundaki **Veri** düğmesine (silindir simgesi) tıklayın. **Veri Görünümü** gösterilir.

   ![Veri görünümü sekmesinin görüntüsü](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Bir veri kaynağı ekleyin. **+** simgesini ve sonra **Veri Kaynağı Ekle**’yi seçin.

   ![Veri Kaynağı Ekle girdisinin görüntüsü](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Hava durumu verilerini ekleme

1. **Veri Deposu**: Verileri içeren veri deposunu seçin. Dosya kullandığınız için **Dosyalar/Dizin**’i seçin. Devam etmek için **İleri**’yi seçin.

   ![Dosyalar/Dizin girdisinin görüntüsü](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Dosya Seçimi**: Hava durumu verilerini ekleyin. Daha önce indirdiğiniz `BostonWeather.csv` dosyasına gözatıp bu dosyayı seçin. **İleri**’ye tıklayın.

   ![BostonWeater.csv dosyasının seçili olduğu dosya seçimi görüntüsü](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **Dosya Ayrıntıları**: Algılanan dosya şemasını doğrulayın. Azure Machine Learning Workbench, dosyadaki verileri analiz eder ve kullanılacak şemayı algılar.

   ![Dosya Ayrıntıları görüntüsü](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > Workbench bazı durumlarda doğru şemayı algılamayabilir. Parametrelerin veri kümeniz için doğru olduğunu her zaman onaylamanız gerekir. Hava durumu verilerinin aşağıdaki değerlere göre ayarlandığını doğrulayın:
   >
   > * __Dosya Türü__: Ayrılmış Değerler Dosyası (csv, tsv, txt vb.)
   > * __Ayırıcı__: Virgül [,]
   > * __Açıklama Satırı Karakteri__: Bir değer ayarlanmaz.
   > * __Satır Atlama Modu__: Atlama
   > * __Dosya Kodlama__: utf-8
   > * __Üst Bilgi Yükseltme Modu__: Birinci Dosyadaki Üst Bilgileri Kullan

   Verilerin önizlemesinde aşağıdaki sütunlar gösterilmelidir:
   * **Path**
   * **DATE**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Devam etmek için **İleri**’yi seçin.

4. **Veri Türleri**: Otomatik olarak algılanan veri türlerini gözden geçirin. Azure Machine Learning Workbench, dosyadaki verileri analiz eder ve kullanılacak veri türlerini algılar.

   Bu veriler için tüm sütunların `DATA TYPE` değerini `String` olarak değiştirin.

   > [!NOTE]
   > `String`, bu öğreticinin sonraki bölümlerinde Workbench’in özelliklerini vurgulamak için kullanılır. 

   ![Veri türlerinin görüntüsü](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Devam etmek için __İleri__’yi seçin. 

5. **Örnekleme**: Bir örnekleme şeması oluşturmak için **+ Yeni** düğmesini seçin. Yeni eklenen __İlk 10000__ satırını ve sonra __Düzenle__’yi seçin. __Örnek Stratejisi__’ni **Tam Dosya** olarak ayarladıktan sonra **Uygula**’yı seçin.

   ![Yeni bir örnekleme stratejisi ekleme görüntüsü](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   __Tam Dosya__ stratejisini kullanmak için __Tam Dosya__ girdisini ve sonra __Etkin Olarak Ayarla__’yı seçin. Bunun etkin strateji olduğunu göstermek üzere __Tam Dosya__’nın yanında bir yıldız görünür.

   ![Etkin strateji olarak Tam Dosya görüntüsü](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Devam etmek için **İleri**’yi seçin.

6. **Yol Sütunu**: __Yol Sütunu__ bölümü, dosya yolunu içeri aktarılan verilere bir sütun olarak eklemenize olanak tanır. __Yol Sütununu Ekleme__’yi seçin.

   > [!TIP]
   > Farklı dosya adlarına sahip çok sayıda dosya içeren bir klasörü içeri aktarıyorsanız, yolun bir sütun olarak eklenmesi yararlıdır. Dosya adlarının daha sonra ayıklamak istediğiniz bilgileri içerdiği durumlarda da bu özellikten yararlanabilirsiniz.

   ![Ekleme olarak ayarlanmış Yol Sütunu görüntüsü](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Son**: Veri kaynağı oluşturmayı tamamlamak için **Son** düğmesini seçin.

    __BostonWeather__ adlı yeni bir veri kaynağı sekmesi açılır. Verilerin bir örneği, kılavuz görünümünde gösterilir. Örnek, daha önce belirtilen etkin örnekleme şemasını temel alır.

    Ekranın sağ tarafındaki **Adımlar** bölmesinde, bu veri kaynağı oluşturulurken gerçekleştirilen adımların tek tek gösterildiğine dikkat edin.

   ![Veri kaynağı, örnek ve adımları gösteren görüntü](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Veri kaynağı ölçümlerini görüntüleme

Sekmenin kılavuz görünümünün sol üst tarafında bulunan __Ölçümler__ düğmesini seçin. Bu görünüm, örneklenmiş verilerin dağılımını ve diğer toplu istatistiklerini gösterir.

![Ölçümleri ekranının görüntüsü](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> İstatistiklerin görünürlüğünü yapılandırmak için **Ölçüm Seç** açılan listesini kullanın. Kılavuz görünümünü değiştirmek için oradaki ölçümleri işaretleyin veya işaretini kaldırın.

__Veri Görünümü__’ne geri dönmek için sayfanın sol üst kısmındaki __Veri__ öğesini seçin.

## <a name="add-data-source-to-data-preparation-package"></a>Veri hazırlama paketine veri kaynağı ekleme

1. Verileri hazırlamaya başlamak için __Hazırla__’yı seçin. 

2. İstendiğinde, Veri Hazırlama paketi için `BikeShare Data Prep` gibi bir ad girin. 

3. Devam etmek için __Tamam__'ı seçin.

   ![Hazırla iletişim kutusunun görüntüsü](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. __Veri__ sekmesinin __Veri Hazırlama__ bölümü altında **BikeShare Data Prep** adlı yeni bir paket görünür. 

   Paketi görüntülemek için bu girdiyi seçin. 

5. Ekranı pakette yer alan __Veri akışlarını__ gösterecek şekilde genişletmek için **>>** düğmesini seçin. Bu örnekte yalnızca __BostonWeather__ veri akışı bulunur.

   > [!IMPORTANT]
   > Bir paket, birden fazla Veri Akışı içerebilir.

   ![Pakette yer alan veri akışlarının görüntüsü](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Verileri değere göre filtreleme
1. Verileri filtrelemek için belirli bir değere sahip bir hücreye sağ tıklayıp __Filtrele__’yi ve sonra filtre türünü seçin.

2. Bu öğreticide, `FM-15` değerini içeren bir hücre seçin ve sonra filtreyi **Equals** filtresine ayarlayın.  Bundan böyle veriler yalnızca __REPORTTYPE__ değeri `FM-15` olan satırları döndürür.

   ![Filtre iletişim kutusunun görüntüsü](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15, bir Meydan Rutin Hava Raporu (METAR) türüdür. FM-15 raporlarının, en kapsamlı rapor türü olduğu ve çok az verinin eksik olduğu deneysel olarak gözlenmiştir.

## <a name="remove-a-column"></a>Sütun kaldırma

__REPORTTYPE__ sütunu artık gerekli değildir. Sütun üst bilgisine sağ tıklayıp **Sütunu Kaldır**’ı seçin.

   ![Sütunu kaldır seçeneğinin görüntüsü](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Veri türlerini değiştirme ve hataları kaldırma
1. Sütun üst bilgilerini seçerken __Ctrl (Mac’te Command ⌘)__ tuşuna basarak tek seferde birden fazla sütunu seçebilirsiniz. Aşağıdaki sütun üst bilgilerini seçmek için bu tuşu kullanın:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. Seçili sütun üst bilgilerinden birine **sağ tıklayın** ve **Alan Türünü Sayısala Dönüştür**’ü seçin. Bu işlem, sütunların veri türünü sayısala dönüştürür.

   ![Birden çok sütunu sayısala dönüştürme](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Hata değerlerini filtreleyin. Bazı sütunlarda veri türü dönüştürme sorunları vardır. Bu sorun, sütunun __Veri Kalitesi Çubuğu__’nda kırmızı renkle gösterilir.

   Hata içeren satırları kaldırmak için **HOURLYDRYBULBTEMPF** sütun üst bilgisine sağ tıklayın. **Sütunu Filtrele**’yi seçin. Varsayılan **İstiyorum** değerini **Satırları Tutmak** olarak kullanın. **Koşullar** açılır listesini değiştirerek **hata değil**’i seçin. Filtreyi uygulamak için **Tamam**’ı seçin.

4. Diğer sütunlarda kalan hata satırlarını kaldırmak için, bu filtreleme işlemini **HOURLYRelativeHumidity** ve **HOURLYWindSpeed** sütunlarında tekrarlayın.

## <a name="use-by-example-transformations"></a>Örnek dönüşümlere göre kullanma

Verileri iki saatlik bloklara yönelik bir tahminde kullanmak için, iki saatlik sürelere ilişkin ortalama hava durumu koşullarını hesaplamanız gerekir. Bunu yapmak için aşağıdaki işlemleri kullanabilirsiniz:

* **DATE** sütununu ayrı **Date** ve **Time** sütunlarına bölün. Ayrıntılı adımlar için sonraki bölüme bakın.

* **Time** sütunundan bir **Hour_Range** sütunu türetin. Ayrıntılı adımlar için sonraki bölüme bakın.

* **DATE** ve **Hour_Range** sütunlarından bir **Date\_Hour\_Range** sütunu türetin. Ayrıntılı adımlar için sonraki bölüme bakın.

### <a name="split-column-by-example"></a>Sütunu örneğe göre bölme

1. **DATE** sütununu ayrı tarih ve saat sütunlarına bölün. **DATE** sütun başlığına sağ tıklayın ve **Sütunu Örneğe Göre Böl**’ü seçin.

   ![Sütunu örneğe göre böl girdisinin görüntüsü](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Azure Machine Learning Workbench otomatik olarak anlamlı bir sınırlayıcı tanımlar ve verileri tarih ile saat değerlerine bölerek iki sütun oluşturur. 

3. Bölme işlemi sonuçlarını kabul etmek için __Tamam__’ı seçin.

   ![Bölünmüş DATE_1 ve DATE_2 sütunlarının görüntüsü](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Sütunu örneğe göre türetme

1. İki saatlik aralık türetmek için __DATE\_2__ sütun üst bilgisine sağ tıklayıp **Sütunu Örneğe Göre Türet**’i seçin.

   ![Sütunu örneğe göre türet girdisinin görüntüsü](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Null değerlerle yeni bir boş sütun eklenir.

2. Yeni sütundaki ilk boş hücreye tıklayın. Yeni sütuna `12AM-2AM` yazıp Enter tuşuna basarak istenilen zaman aralığının bir örneğini sağlayın.

   ![Değeri 12.00-02.00 aralığında olan yeni sütunun görüntüsü](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Azure ML Workbench, sizin sağladığınız örnekleri temel alan bir programı sentezler ve aynı programı kalan satırlara uygular. Diğer tüm satırlar, sağladığınız örneğe göre otomatik olarak doldurulur. Workbench ayrıca verilerinizi analiz eder ve istisnai durumları tanımlamaya çalışır. 
  
3. Kılavuzun üzerindeki **Veriler Analiz Ediliyor** iletisi, Workbench’in istisnai durumları algılamaya çalıştığını belirtir. İşlem bittiğinde, durum **Önerilen sonraki satırı gözden geçirin** veya **Öneri yok** olarak değişir. Bu örnekte, **Önerilen sonraki satırı gözden geçirin** durumu döndürülür.

4. Önerilen değişiklikleri gözden geçirmek için **Önerilen sonraki satırı gözden geçirin** öğesini seçin. Gözden geçirmeniz ve (gerekirse) düzeltmeniz gereken hücre ekranda vurgulanır.

   ![Gözden geçirme satırının görüntüsü](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Dönüştürmeyi kabul etmek için __Tamam__’ı seçin.
 
5. __BostonWeather__ verilerinin kılavuz görünümüne geri dönersiniz. Kılavuz bu durumda önceden eklenen üç sütunu içerir.

   ![Eklenen satırlarla birlikte kılavuz görünümünün görüntüsü](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Yaptığınız tüm değişiklikler **Adımlar** bölmesinde saklanır. **Adımlar** bölmesinde oluşturduğunuz adıma gidin, aşağı oka tıklayın ve **Düzenle**’yi seçin. Gelişmiş **Sütunu Örneğe Göre Türet** penceresi gösterilir. Tüm örnekleriniz burada saklanır. Ayrıca, aşağıdaki kılavuzda bir satıra çift tıklayarak örnekleri el ile ekleyebilirsiniz. Değişiklikleri uygulamadan ana kılavuza geri dönmek için **İptal**’i seçin. Bu görünüme, **Sütunu Örneğe Göre Türet** dönüşümü gerçekleştirirken **Gelişmiş Mod**’u seçerek de erişebilirsiniz.

6. Sütunu yeniden adlandırmak için sütun üst bilgisine çift tıklayın ve **Hour Range** yazın. Değişikliği kaydetmek için **Enter** tuşuna basın.

   ![Sütunu yeniden adlandırma](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Tarih ve saat aralığını türetmek için **Date\_1** ve **Hour Range** sütunlarını birlikte seçin ve sonra **Sütunu örneğe göre türet**’i seçin.

   ![Sütunu örneğe göre türetme](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Birinci satırın karşısına örnek olarak `Jan 01, 2015 12AM-2AM` yazın ve **Enter** tuşuna basın.

   Workbench, sağladığınız örneği temel alarak dönüşümü belirler. Bu örnekte sonuç olarak tarih biçimi değiştirilir ve iki saatlik zaman penceresi ile birleştirilir.

   ![`1 Ocak 2015 12.00-02.00 örneğinin görüntüsü](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)


8. **Veriler Analiz Ediliyor** durumunun **Önerilen sonraki satırı gözden geçirin** olarak değişmesini bekleyin. Bu işlem birkaç saniye sürebilir. Önerilen satıra gitmek için durum bağlantısını seçin. 

   ![Gözden geçirmek için önerilen satırın görüntüsü](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Kaynak tarih değeri gg/aa/yyyy veya aa/gg/yyyy biçiminde olabileceğinden satır null değere sahiptir. `Jan 13, 2015 2AM-4AM` için uygun değeri yazıp **Enter** tuşuna basın. Workbench kalan satırlar için türetmeyi iyileştirmek üzere iki örneği kullanır.

   ![Doğru biçimlendirilmiş verilerin görüntüsü](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Dönüştürmeyi kabul etmek için **Tamam**’ı seçin.

   ![Tamamlanan dönüştürme kılavuzunun görüntüsü](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > **Adımlar** bölmesindeki aşağı oka tıklayarak bu adım için **Sütunu örneğe göre türet** gelişmiş modunu kullanabilirsiniz. Veri kılavuzunda, **DATE\_1** ve **Hour Range** sütun adlarının yanında onay kutuları bulunur. Bu işlemin çıktıyı nasıl değiştirdiğini görmek için **Hour Range** sütununun yanındaki onay kutusunun işaretini kaldırın. Girdi olarak **Hour Range** sütunu mevcut olmadığında, **12.00-2.00** sabit olarak kabul edilir ve türetilmiş değerlere eklenir. Değişikliklerinizi uygulamadan ana kılavuza geri dönmek için **İptal**’i seçin.

10. Sütunu yeniden adlandırmak için üst bilgiye çift tıklayın. Adı **Date Hour Range** olarak değiştirip **Enter** tuşuna basın.

11. **DATE**, **DATE\_1**, **DATE\_2** ve **Hour Range** sütunlarını birlikte seçin. Sağ tıklayın ve sonra **Sütunu kaldır**’ı seçin.

## <a name="summarize-data-mean"></a>Verileri özetleme (ortalama)

Sonraki adımda, değerlerin saat aralığına göre gruplandırılmış ortalaması alınarak hava durumu koşulları özetlenir.

1. **Date Hour Range** sütununu ve sonra **Dönüşümler** menüsünden **Özetle** öğesini seçin.

    ![Dönüşümler menüsü](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Verileri özetlemek için sayfanın alt kısmındaki kılavuzdan üstteki sol ve sağ bölmelere sütunları sürükleyin. Sol bölmede **Verileri gruplandırmak için sütunları buraya sürükleyin** iletisi bulunur. Sağ bölmede **Verileri özetlemek için sütunları buraya sürükleyin** iletisi bulunur. 

    **Date Hour Range** sütununu alttaki kılavuzdan sol bölmeye sürükleyin. **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** ve **HOURLYWindSpeed** sütunlarını sağ bölmeye sürükleyin. 

    Sağ bölmede her bir sütunun **Toplam** ölçümü olarak **Ortalama**’yı seçin. Özetlemeyi tamamlamak için **Tamam**’a tıklayın.

   ![Özetlenmiş veri ekranının görüntüsü](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Betik kullanarak veri akışını dönüştürme

Sayısal sütunlardaki verilerin 0-1 aralığında olacak şekilde değiştirilmesi, bazı modellerin hızla yakınsanmasını sağlar. Şu anda bu dönüşümü genel olarak yapmak için yerleşik bir dönüşüm bulunmamaktadır, ancak bu işlemi gerçekleştirmek için bir Python betiği kullanılabilir.

1. **Dönüşüm** menüsünde **Dönüşüm Veri Akışı**’nı seçin.

2. Açılan metin kutusuna aşağıdaki kodu girin. Sütun adlarını kullandıysanız, kod değişiklik yapılmadan çalışabilir. Python’da basit bir min-maks normalleştirme mantığı yazıyorsunuz.

    > [!WARNING]
    > Betik daha önce bu öğreticide kullanılan sütun adlarını bekler. Farklı sütun adları kullandıysanız, betikteki adları değiştirmeniz gerekir.

   ```python
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
    > Python betiğinin sonunda `df` döndürülmelidir. Bu değer, kılavuzu doldurmak için kullanılır.
    
    ![Veri akışı dönüştürme betiği iletişim kutusu](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Betiği kullanmak için __Tamam__’ı seçin. Kılavuzdaki sayısal sütunlar artık 0-1 aralığındaki değerleri içerir.

    ![0 ile 1 arasındaki değerleri içeren kılavuz](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Hava durumu verilerini hazırlamayı tamamladınız. Şimdi seyahat verilerini hazırlayın.

## <a name="load-trip-data"></a>Seyahat verilerini yükleme

1. `201701-hubway-tripdata.csv` dosyasını içeri aktarmak için [Yeni Veri Kaynağı Oluşturma](#newdatasource) bölümündeki adımları kullanın. İçeri aktarma işlemi sırasında aşağıdaki seçenekleri kullanın:

    * __Örnekleme şeması__: **Tam Dosya** örnekleme şemasını seçin, örneği etkin hale getirin ve 
    * __Veri Türü__: Varsayılanları kabul edin.

2. Verileri içeri aktardıktan sonra __Hazırla__ düğmesini seçerek verileri hazırlamaya başlayın. Var olan **BikeShare Data Prep.dprep** paketini ve ardından __Tamam__’ı seçin.

    Bu işlem yeni bir dosya oluşturmak yerine var olan **Data Preparation** dosyasına bir **Veri Akışı** ekler.

    ![Var olan paketi seçme görüntüsü](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Kılavuz yüklendikten sonra __DATAFLOWS__ öğesini genişletin. Artık iki veri akışı vardır: **BostonWeather** ve **201701-hubway-tripdata**. **201701-hubway-tripdata** girdisini seçin.

    ![201701-hubway-tripdata girdisinin görüntüsü](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Harita denetçisini kullanma

Veri hazırlamada, verileri daha iyi anlamaya ve aykırı değerleri tanımlaya yardımcı olan Dize, Sayısal ve Coğrafi değerler için **Denetçi** adı verilen birkaç yararlı görselleştirme bulunur. Harita denetçisini kullanmak için aşağıdaki adımları kullanın:

1. **Start station latitude** ve **start station longitude** sütunlarını birlikte seçin. Sütunlardan birine sağ tıklayın ve sonra **Harita**’yı seçin.

    > [!TIP]
    > Çoklu seçimi etkinleştirmek için __Ctrl (Mac’te Command ⌘)__ tuşunu basılı tutarak her bir sütunun üst bilgisini seçin.

    ![Harita görselleştirme görüntüsü](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Harita görselleştirmeyi en üst düzeye çıkarmak **Ekranı Kapla** simgesini seçin. Haritayı pencereye sığdırmak için görselleştirmenin sol üst kısmındaki **E** simgesini seçin.

    ![Tam ekran görüntüsü](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Kılavuz görünümüne geri dönmek için **Simge Durumuna Küçült** düğmesine tıklayın.

## <a name="use-column-statistics-inspector"></a>Sütun İstatistikleri Denetçisini kullanma

Sütun istatistikleri denetçisini kullanmak için __tripduration__ sütununa sağ tıklayıp __Sütun İstatistikleri__’ni seçin.

![Sütun istatistikleri girdisi](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Bu işlem __INSPECTORS__ bölmesine __tripduration İstatistikleri__ başlıklı yeni bir görselleştirme ekler.

![Tripduration İstatistikleri denetçisinin görüntüsü](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> Seyahat süresinin en yüksek değeri **961.814 dakikadır** ve bu da yaklaşık olarak iki yıla denk gelir. Görünüşe göre veri kümesinde bazı aykırı değerler bulunmaktadır.

## <a name="use-histogram-inspector"></a>Histogram denetçisini kullanma

Aykırı değerleri tanımlamayı denemek için __tripduration__ sütununa sağ tıklayıp __Histogram__’ı seçin.

![Histogram denetçisi](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Aykırı değerler grafiği eğdiği için histogram faydalı değildir.

## <a name="add-column-using-script"></a>Betik kullanarak sütun ekleme

1. **Tripduration** sütununa sağ tıklayıp **Sütun Ekle (Betik)** öğesini seçin.

    ![Sütun ekle (betik) menüsünün görüntüsü](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. __Sütun Ekle (Betik)__ iletişim kutusunda aşağıdaki değerleri kullanın:

    * __Yeni Sütun Adı__: logtripduration
    * __Bu Yeni Sütunu Şundan Sonra Ekle__: tripduration
    * __Yeni Sütun Kodu__: `math.log(row.tripduration)`
    * __Kod Bloğu Türü__: İfade

   ![Sütun Ekle (Betik) iletişim kutusu](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. **Logtripduration** sütununu eklemek için __Tamam__’ı seçin.

4. Sütuna sağ tıklayıp **Histogram**’ı seçin.

    ![Logtripduration sütununun histogramı](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Görsel olarak, bu histogram anormal kuyruklu bir Normal Dağıtım gibi görünür.

## <a name="use-advanced-filter"></a>Gelişmiş Filtre kullanma

Veriler üzerinde bir filtre kullanılması, denetçileri yeni dağıtımla güncelleştirir. **Logtripduration** sütununa sağ tıklayıp **Sütunu Filtrele** öğesini seçin. __Düzenle__ iletişim kutusunda aşağıdaki değerleri kullanın:

* __Bu Sayı Sütununu Filtrele__: logtripduration
* __İstiyorum__: Satırları Tut
* __Ne Zaman__: Aşağıdaki Koşullardan herhangi biri True (mantıksal OR) olduğunda
* __Bu Sütun__: küçüktür ise
* __Değer__: 9

![Filtre seçenekleri](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Filtreyi uygulamak için __Tamam__’ı seçin.

![Filtre uygulandıktan sonra güncelleştirilmiş histogramlar](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Halo etkisi

1. **logtripduration** histogramını büyütün. Gri histogram üzerine yayılan mavi bir histogram bulunur. Bu ekran **Halo Etkisi** olarak adlandırılır:

    * **Gri histogram** işlemden (bu örnekte filtreleme işlemi) önceki dağılımı temsil eder.
    * **Mavi histogram** işlemden sonraki histogramı temsil eder. 

   Halo etkisi, bir işlemin veriler üzerindeki etkisini görselleştirmeye yardımcı olur.

   ![Halo etkisinin görüntüsü](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Mavi histogramın öncekinden daha kısa göründüğüne dikkat edin. Bunun nedeni, yeni aralıkta verilerin yeniden demetlenmesidir.

2. Haloyu yok etmek için __Düzenle__’yi seçin ve __Halo göster__’in işaretini kaldırın.

    ![Histogram seçenekleri](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Halo etkisini devre dışı bırakmak için **Tamam**’ı seçin ve sonra histogramı küçültün.

### <a name="remove-columns"></a>Sütunları kaldırma

Seyahat verilerinde her satır bir bisiklet alma olayını temsil eder. Bu öğretici için yalnızca **starttime** ve **start station id** sütunları gereklidir. Bu iki sütunu birlikte seçerek diğer sütunları kaldırın, sütun üst bilgisine sağ tıklayın ve sonra **Sütunu Tut**’u seçin. Diğer sütunlar kaldırılır.

![Sütunu tut seçeneğinin görüntüsü](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Verileri özetleme (Sayı)

2 saatlik süreye ilişkin bisiklet talebini özetlemek için türetilmiş sütunlar kullanın.

1. **Starttime** sütununa sağ tıklayıp **Sütunu Örneğe Göre Türet**’i seçin

    ![Örneğe göre türet seçeneğinin görüntüsü](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Örneğin, ilk satır için `Jan 01, 2017 12AM-2AM` değeri girin.

    > [!IMPORTANT]
    > Türetilen sütunlara ilişkin önceki örnekte, tarih ve saati içeren bir sütun türetmek için birden çok adım kullandınız. Bu örnekte, son çıktının bir örneğini sağlayarak bu işlemin tek bir adımda gerçekleştirilebileceğini görebilirsiniz.

    > [!NOTE]
    > Satırların herhangi birine göre bir örnek verebilirsiniz. Bu örnekte, birinci veri satırı için `Jan 01, 2017 12AM-2AM` değeri geçerlidir.

    ![Örnek verilerin görüntüsü](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)
   
3. Uygulamanın değerleri tüm satırlara göre hesaplamasını bekleyin. Bu işlem birkaç saniye sürebilir. Analiz tamamlandıktan sonra verileri gözden geçirmek için __Önerilen sonraki satırı gözden geçirin__ bağlantısını kullanın.

   ![Gözden geçirme bağlantıyla birlikte tamamlanmış analizin görüntüsü](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Hesaplanan değerlerin doğru olduğundan emin olun. Doğru değilse, değeri beklenen değerle güncelleştirip Enter tuşuna basın. Sonra analizin tamamlanmasını bekleyin. **Öneri yok** ifadesini görene kadar **Önerilen sonraki satırı gözden geçirin** işlemini tamamlayın. **Öneri yok** ifadesini gördüğünüzde, uygulamanın istisnai durumlara baktığını ve sentezlenen programın uygun olduğunu anlayabilirsiniz. Dönüşümü kabul etmeden önce görsel denetim gerçekleştirilmesi en iyi uygulamalardan biridir. 

4. Dönüştürmeyi kabul etmek için **Tamam**’ı seçin. Yeni oluşturulan sütunu **Date Hour Range** olarak adlandırın.

    ![Adlandırılan sütun görüntüsü](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. **starttime** sütun üst bilgisine sağ tıklayıp **Sütunu Kaldır**’ı seçin.

6. Verileri özetlemek için __Dönüşüm__ menüsünden __Özetle__’yi seçin. Dönüşümü oluşturmak için aşağıdaki eylemleri gerçekleştirin:

    * __Date Hour Range__ ve __start station id__ sütunlarını sol (gruplandırma ölçütü) bölmesine sürükleyin.
    * __start station id__ sütununu sağ (veri özetleme) bölmesine sürükleyin.

   ![Özetleme seçeneklerinin görüntüsü](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Özet sonucunu kabul etmek için **Tamam**’ı seçin.

## <a name="join-dataflows"></a>Veri akışlarını birleştirme

Hava durumu verilerini seyahat verileriyle birleştirmek için aşağıdaki adımları kullanın:

1. __Dönüşümler__ menüsünden __Birleştir__’i seçin.

2. __Tablolar__: Sol veri akışı olarak **BostonWeather**’ı, sağ veri akışı olarak **201701-hubway-tripdata** öğesini seçin. Devam etmek için **İleri**’yi seçin.

    ![Tablolar seçimlerinin görüntüsü](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Anahtar Sütunları__: Tabloların her ikisinde **Date Hour Range** sütununu ve sonra __İleri__’yi seçin.

    ![Anahtar sütunlarında birleştirme görüntüsü](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Birleştirme Türü__: Birleştirme türü olarak __Eşleşen satırlar__’ı seçin ve sonra __Son__’u seçin.

    ![Eşleşen satırlar birleştirme türü](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Bu işlem __Birleştirme Sonucu__ adlı yeni bir veri akışı oluşturur.

## <a name="create-additional-features"></a>Ek özellikler oluşturma

1. Haftanın gününü içeren bir sütun oluşturmak için **Date Hour Range** sütununa sağ tıklayıp **Sütunu Örneğe Göre Türet**’i seçin. Pazar günü gerçekleşen bir tarih için __Paz__ değerini kullanın. Örneğin, **1 Ocak 2017 12:00-02:00**. **Enter** tuşuna basın ve ardından **Tamam**’ı seçin. Bu sütunu __Weekday__ olarak adlandırın.

    ![Haftanın gününü içeren yeni bir sütun oluşturma görüntüsü](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Bir satırın saat aralığını içeren bir sütun oluşturmak için **Date Hour Range** sütununa sağ tıklayıp **Sütunu örneğe göre türet**’i seçin. **1 Ocak 2017 12:00-2:00** değerini içeren satır için **12:00-2:00** değerini kullanın. **Enter** tuşuna basın ve ardından **Tamam**’ı seçin. Bu sütunu **Period** olarak adlandırın.

    ![Period sütunu görüntüsü](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. **Date Hour Range** ve **rDate Hour Range** sütunlarını kaldırmak için **Ctrl (Mac’te Command ⌘)** tuşuna basıp her bir sütun üst bilgisini seçin. Sağ tıklayın ve sonra **Sütunu Kaldır**’ı seçin.

## <a name="read-data-from-python"></a>Python'dan veri okuma

Python veya PySpark’tan bir veri hazırlama paketi çalıştırabilir ve sonucu bir **Veri Çerçevesi** olarak alabilirsiniz.

Örnek br Python betiği oluşturmak için __BikeShare Data Prep__’e sağ tıklayıp __Veri Erişim Kodu Dosyası Oluştur__’u seçin. Örnek Python dosyası **Proje Klasörünüzde** oluşturulur ve ayrıca Workbench’teki bir sekmeye yüklenir. Oluşturulan kodun bir örneği, aşağıdaki Python betiğidir:

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

Bu öğreticide dosyanın adı `BikeShare Data Prep.py` şeklindedir. Bu dosya öğreticinin sonraki bölümlerinde kullanılacaktır.

## <a name="save-test-data-as-a-csv-file"></a>Test verilerini CSV dosyası olarak kaydetme

**Birleştirme Sonucu** Veri Akışını bir .CSV dosyasına kaydetmek için `BikeShare Data Prep.py` betiğini değiştirmeniz gerekir. Aşağıdaki kodu kullanarak Python betiğini güncelleştirin:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Ekranın üst kısmından **Çalıştır**’ı seçin. Betik, yerel makine üzerindeki bir **İş** olarak gönderilir. İş durumu __Tamamlandı__ olarak değiştikten sonra dosya belirtilen konuma yazılmıştır.

## <a name="substitute-data-sources"></a>Yedek veri kaynakları

Önceki adımlarda `201701-hubway-tripdata.csv` ve `BostonWeather.csv` veri kaynaklarını kullanarak Test verilerini hazırladınız. Paketi diğer seyahat veri dosyalarıyla birlikte kullanmak için aşağıdaki adımları uygulayın:

1. Daha önce verilen adımları aşağıdaki değişikliklerle birlikte kullanarak yeni bir **Veri Kaynağı** oluşturun:

    * __Dosya Seçimi__: Dosya seçerken, kalan altı seyahat tripdata .CSV dosyasını birlikte seçin.

        ![Kalan altı dosyayı yükleme](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > __+5__ girdisi, listelenen dosyaya ek olarak beş dosyanın daha bulunduğunu belirtir.

    * __Dosya Ayrıntıları__: __Üst Bilgileri Yükseltme Modu__’unu **Tüm Dosyalar Aynı Üst Bilgilere Sahiptir** olarak ayarlayın. Bu değer, dosyaların her birinin aynı üst bilgiyi içerdiğini gösterir.

        ![Dosya ayrıntıları seçimi](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Sonraki adımlarda kullanılmak üzere bu veri kaynağının adını kaydedin.

2. Dosyaları projenizde görüntülemek için klasör simgesini seçin. __aml\_config__ dizinini genişletip `local.runconfig` dosyasını seçin.

    ![Local.runconfig konumunun görüntüsü](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. `local.runconfig` dosyasının sonuna aşağıdaki satırları ekleyin ve sonra disk simgesini seçerek dosyayı kaydedin.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Bu değişiklik, özgün veri kaynağını altı seyahat veri dosyasını içeren veri kaynağıyla değiştirir.

## <a name="save-training-data-as-a-csv-file"></a>Eğitim verilerini CSV dosyası olarak kaydetme

Daha önce oluşturduğunuz `BikeShare Data Prep.py` adlı Python dosyasına gidin ve Eğitim Verilerini kaydetmek için farklı bir Dosya Yolu belirtin.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Yeni bir iş göndermek için sayfanın üstündeki **Çalıştır** simgesini kullanın. Yeni yapılandırma ile bir **İş** gönderilir. Bu işin çıktısı Eğitim Verileridir. Bu veriler daha önce oluşturduğunuz veri hazırlama adımlarının aynısı kullanılarak oluşturulur. İşin tamamlanması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bisiklet Paylaşımı Veri Hazırlama öğreticisini tamamladınız. Bu öğreticide Azure Machine Learning hizmetlerini (önizleme) kullanarak aşağıdakileri yapmayı öğrendiniz:
> [!div class="checklist"]
> * Azure Machine Learning Veri Hazırlama aracı ile etkileşimli olarak veri hazırlama
> * Test veri kümesini içeri aktarma, dönüştürme ve oluşturma
> * Veri Hazırlama paketi oluşturma
> * Python kullanarak Veri Hazırlama Paketini çalıştırma
> * Ek girdi dosyaları için Veri Hazırlama paketini yeniden kullanarak bir eğitim veri kümesi oluşturma

Bundan sonra, veri hazırlama hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Veri hazırlama kullanıcı kılavuzu](data-prep-user-guide.md)
