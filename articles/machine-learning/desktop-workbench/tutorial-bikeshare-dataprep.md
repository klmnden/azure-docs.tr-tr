---
title: Bisiklet paylaşımı öğreticisi - Azure Machine Learning Workbench ile gelişmiş veri hazırlama
description: Bu öğreticide, Azure Machine Learning Workbench'i kullanarak uçtan uca veri hazırlama görevini gerçekleştireceksiniz
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 09/21/2017
ms.openlocfilehash: 2a50350b9ba49d82a20b92804ffb92ec6906186d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439909"
---
# <a name="tutorial-use-azure-machine-learning-workbench-for-advanced-data-preparation-bike-share-data"></a>Öğretici: Azure Machine Learning Workbench'i kullanarak gelişmiş veri hazırlığı (Bisiklet paylaşımı verileri)
Azure Machine Learning (önizleme), uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve gelişmiş bir analiz çözümüdür.

Bu öğreticide Machine Learning (önizleme) kullanarak aşağıdakileri yapmayı öğrenirsiniz:
> [!div class="checklist"]
> * Azure Machine Learning veri hazırlama aracı ile verileri etkileşimli olarak hazırlama.
> * Test veri kümesini içeri aktarma, dönüştürme ve oluşturma.
> * Veri hazırlama paketi oluşturma.
> * Python kullanarak veri hazırlama paketini çalıştırma.
> * Ek giriş dosyaları için veri hazırlama paketini yeniden kullanarak bir eğitim veri kümesi oluşturma.
> * Betikleri yerel bir Azure CLI penceresinde yürütme.
> * Betikleri bulut üzerindeki Azure HDInsight ortamında yürütme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning Workbench'in yerel yüklemesi. Daha fazla bilgi için [yükleme hızlı başlangıç](../service/quickstart-installation.md) başlığı altındaki yönergeleri izleyin.
* Azure CLI'yi yüklemediyseniz, yönergeleri izleyerek [en son Azure CLI sürümünü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Azure'da bir [HDInsights Spark kümesi](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal) oluşturulur.
* Bir Azure depolama hesabı.
* Workbench’te yeni proje oluşturma bilgisi.
* Depolama hesabınızda blobları karşıya yükleyebilmeniz, indirebilmeniz ve görüntüleyebilmeniz için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)'nin yüklü olması gerekli olmasa bile yararlı olacaktır.

## <a name="data-acquisition"></a>Veri alma
Bu öğreticide [Boston hubway veri kümesi](https://s3.amazonaws.com/hubway-data/index.html) ve [NOAA](http://www.noaa.gov/)’daki Boston hava durumu verileri kullanılır.

1. Veri dosyalarını aşağıdaki bağlantılardan yerel geliştirme ortamınıza indirin:

   * [Boston hava durumu verileri](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv)

   * Hubway web sitesindeki hubway seyahat verileri:

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

1. İndirdiğiniz her .zip dosyasını açın.

## <a name="upload-data-files-to-azure-blob-storage"></a>Veri dosyalarını Azure Blob depolamaya yükleme
Azure Blob depolamayı veri dosyalarınızı barındırmak için kullanabilirsiniz.

1. Kullandığınız HDInsight kümesi için kullanılan depolama hesabını kullanın.

    ![HDInsight küme depolama hesabı](media/tutorial-bikeshare-dataprep/hdinsightstorageaccount.png)

1. **BikeShare** veri dosyalarını depolamak için **data-files** adlı yeni bir kapsayıcı oluşturun.

1. Veri dosyalarını karşıya yükleyin. `BostonWeather.csv` dosyasını `weather` adlı klasöre yükleyin. Seyahat veri dosyalarını `tripdata` adlı klasöre yükleyin.

    ![Veri dosyalarını karşıya yükleme](media/tutorial-bikeshare-dataprep/azurestoragedatafile.png)

> [!TIP]
> Ayrıca, blobları karşıya yüklemek için Depolama Gezgini'ni kullanabilirsiniz. Öğreticide oluşturulan dosyalarını içeriğini görüntülemek istediğinizde de bu aracı kullanın.

## <a name="learn-about-the-datasets"></a>Veri kümeleri hakkında bilgi edinme
1. __Boston weather__ adlı dosya, hava durumu ile ilgili aşağıdaki alanlarda saatlik olarak bildirilen verileri içerir:

   * **DATE**

   * **REPORTTPYE**

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

1. __Hubway__ verileri, yıl ve aya göre dosyalar halinde düzenlenmiştir. Örneğin, `201501-hubway-tripdata.zip` adlı dosya Ocak 2015’e ait verilerin bulunduğu bir .csv dosyası içerir. Verilerde aşağıdaki alanlar bulunur ve her satır bir bisiklet seyahatini temsil eder:

   * **Trip Duration (in seconds)**

   * **Start Time and Date**

   * **Stop Time and Date**

   * **Start Station Name & ID**

   * **End Station Name & ID**

   * **Bike ID**

   * **User Type (Casual = 24-Hour or 72-Hour Pass user; Member = Annual or Monthly Member)**

   * **ZIP Code (if user is a member)**

   * **Gender (self-reported by member)**

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma
1. Başlangıç menünüzden veya başlatıcıdan **Machine Learning Workbench**’i başlatın.

1. Yeni bir Machine Learning projesi oluşturun. **Projeler** sayfasındaki **+** düğmesini veya **Dosya** > **Yeni**'yi seçin.

   * **Bike Share** şablonunu kullanın.

   * Projenizi **BikeShare** olarak adlandırın. 

## <a id="newdatasource"></a>Yeni veri kaynağı oluşturma

1. Yeni bir veri kaynağı oluşturun. Sol araç çubuğundaki **Veri** düğmesini (silindir simgesi) seçerek **Veri** görünümünü görüntüleyin.

   ![Veri görünümü sekmesi](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

1. Bir veri kaynağı ekleyin. **+** simgesini ve sonra da **Veri Kaynağı Ekle**’yi seçin.

   ![Veri Kaynağı Ekle seçeneği](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Hava durumu verilerini ekleme

1. **Veri Deposu**: Verileri içeren veri deposunu seçin. Dosyaları kullandığınız için **Dosyalar/Dizin**’i seçin. Devam etmek için **İleri**’yi seçin.

   ![Dosyalar/Dizin girdisi](media/tutorial-bikeshare-dataprep/datasources.png)

1. **Dosya Seçimi**: Hava durumu verilerini ekleyin. Daha önce Blob Depolama'ya yüklemiş olduğunuz `BostonWeather.csv` dosyasına göz atın ve bu dosyayı seçin. **İleri**’yi seçin.

   ![BostonWeather.csv dosyasının seçili olduğu dosya seçimi](media/tutorial-bikeshare-dataprep/azureblobpickweatherdatafile.png)

1. **Dosya Ayrıntıları**: Algılanan dosya şemasını doğrulayın. Machine Learning Workbench, dosyadaki verileri analiz eder ve kullanılacak şemayı algılar.

   ![Dosya ayrıntılarını doğrulama](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > Workbench bazı durumlarda doğru şemayı algılamayabilir. Parametrelerin veri kümeniz için doğru olduğundan her zaman emin olun. Hava durumu verilerinin aşağıdaki değerlere göre ayarlandığını doğrulayın:
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

1. **Veri Türleri**: Otomatik olarak algılanan veri türlerini gözden geçirin. Machine Learning Workbench, dosyadaki verileri analiz eder ve kullanılacak veri türlerini algılar.

   a. Bu veriler için, tüm sütunlarda **DATA TYPE** değerini **Dize** olarak değiştirin.

   > [!NOTE]
   > Dize, bu öğreticinin sonraki bölümlerinde Workbench’in özelliklerini vurgulamak için kullanılır. 

   ![Veri türlerini gözden geçirme](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   b. Devam etmek için __İleri__’yi seçin. 

1. **Örnekleme**: Örnekleme şeması oluşturmak için **Düzenle**'yi seçin. Yeni eklenen __İlk 10000__ satırını ve sonra __Düzenle__’yi seçin. __Örnek Stratejisi__’ni **Tam Dosya** olarak ayarladıktan sonra **Uygula**’yı seçin.

   ![Yeni örnekleme stratejisi ekleme](media/tutorial-bikeshare-dataprep/weatherdatasamplingfullfile.png)

   __Tam Dosya__ stratejisini kullanmak için __Tam Dosya__ girdisini ve sonra __Etkin Olarak Ayarla__’yı seçin. Bunun etkin strateji olduğunu göstermek üzere __Tam Dosya__’nın yanında bir yıldız gösterilir.

   ![Etkin strateji olarak Tam Dosya ayarlanmış](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Devam etmek için **İleri**’yi seçin.

1. **Yol Sütunu**: İçeri aktarılan verilere tam dosya yolunu bir sütun olarak eklemek için __Yol Sütunu__ bölümünü kullanın. __Yol Sütununu Ekleme__’yi seçin.

   > [!TIP]
   > Farklı dosya adlarına sahip çok sayıda dosya içeren bir klasörü içeri aktarıyorsanız, yolun bir sütun olarak eklenmesi yararlı olur. Dosya adlarının daha sonra ayıklamak istediğiniz bilgileri içerdiği durumlarda da bu özellikten yararlanabilirsiniz.

   ![Ekleme olarak ayarlanmış Yol Sütunu](media/tutorial-bikeshare-dataprep/pathcolumn.png)

1. **Son**: Veri kaynağı oluşturmayı tamamlamak için **Son**'u seçin.

    __BostonWeather__ adlı yeni bir veri kaynağı sekmesi açılır. Verilerin bir örneği, kılavuz görünümünde gösterilir. Örnek, daha önce belirtilmiş olan etkin örnekleme şemasını temel alır.

    Ekranın sağ tarafındaki **Adımlar** bölmesinde, bu veri kaynağı oluşturulurken gerçekleştirilen adımların tek tek gösterildiğine dikkat edin.

   ![Veri kaynağı, örnek ve adımları görüntüleme](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Veri kaynağı ölçümlerini görüntüleme

Tablonun kılavuz görünümünde sol üst kısımdaki __Ölçümler__'i seçin. Bu görünüm, örneklenmiş verilerin dağılımını ve diğer toplu istatistiklerini gösterir.

![Ölçümler görüntüsü](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> İstatistiklerin görünürlüğünü yapılandırmak için **Ölçüm Seç** açılan listesini kullanın. Kılavuz görünümünü değiştirmek için oradaki ölçümleri seçin ve temizleyin.

__Veri__ görünümüne dönmek için sayfanın sol üst kısmındaki __Veri__ öğesini seçin.

## <a name="add-a-data-source-to-the-data-preparation-package"></a>Veri hazırlama paketine veri kaynağı ekleme

1. Verileri hazırlamaya başlamak için __Hazırla__’yı seçin. 

1. İstendiğinde, veri hazırlama paketi için **BikeShare Data Prep** gibi bir ad girin. 

1. Devam etmek için __Tamam__'ı seçin.

   ![Hazırla iletişim kutusu](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

1. __Veri__ sekmesinin __Veri Hazırlığı__ bölümünün altında **BikeShare Data Prep** adlı yeni bir paket gösterilir. 

   Paketi görüntülemek için bu girdiyi seçin. 

1. __Veri Akışları__'nı genişletmek ve pakette yer alan veri akışlarını görüntülemek için **>>** düğmesini seçin. Bu örnekte yalnızca __BostonWeather__ veri akışı bulunur.

   > [!IMPORTANT]
   > Bir paket birden çok veri akışı içerebilir.

   ![Paketteki veri akışları](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Verileri değere göre filtreleme
1. Verileri filtrelemek için belirli bir değer içeren bir hücreye sağ tıklayın ve __Filtrele__’yi seçin. Ardından filtre türünü seçin.

1. Bu öğretici için, `FM-15` değerini içeren hücreyi seçin. Ardından filtreyi **eşittir** olarak ayarlayın.  Bundan böyle veriler yalnızca __REPORTTYPE__ değeri `FM-15` olan satırları döndürür.

   ![Filtre iletişim kutusu](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15, bir Rutin Meydan Meteorolojik (METAR) hava raporu türüdür. FM-15 raporlarının, en kapsamlı rapor türü olduğu ve çok az verinin eksik olduğu deneysel olarak gözlenmiştir.

## <a name="remove-a-column"></a>Sütun kaldırma

__REPORTTYPE__ sütunu artık gerekli değildir. Sütun üst bilgisine sağ tıklayın ve **Sütunu Kaldır**’ı seçin.

   ![Sütunu Kaldır seçeneği](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Veri türlerini değiştirme ve hataları kaldırma
1. Aynı anda birden fazla sütun seçmek için sütun üst bilgilerini belirlerken Ctrl’yi (Mac’te ⌘ komutu) seçin. Aşağıdaki sütun üst bilgilerini seçmek için bu tekniği kullanın:

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

1. Seçili sütun üst bilgilerinden birine sağ tıklayın ve **Alan Türünü Sayısala Dönüştür**’ü seçin. Bu seçenek sütunların veri türünü sayısala dönüştürür.

   ![Birden çok sütunu sayısala dönüştürme](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

1. Hata değerlerini filtreleyin. Bazı sütunlarda veri türü dönüştürme sorunları vardır. Bu sorun, sütunun __Veri Kalitesi Çubuğu__’nda kırmızı renkle gösterilir.

   Hata içeren satırları kaldırmak için **HOURLYDRYBULBTEMPF** sütun üst bilgisine sağ tıklayın. **Sütunu Filtrele**’yi seçin. Varsayılan **İstiyorum** değerini **Satırları Tutmak** olarak kullanın. **Koşullar** açılır listesini değiştirerek **hata değil**’i seçin. Filtreyi uygulamak için **Tamam**’ı seçin.

   ![Hata değerlerini filtreleme](media/tutorial-bikeshare-dataprep/filtererrorvalues.png)

1. Diğer sütunlarda kalan hata satırlarını kaldırmak için, bu filtreleme işlemini **HOURLYRelativeHumidity** ve **HOURLYWindSpeed** sütunlarında da tekrarlayın.

## <a name="use-by-example-transformations"></a>Örnek dönüşümlere göre kullanma

Verileri iki saatlik bloklara yönelik bir tahminde kullanmak için, iki saatlik sürelere ilişkin ortalama hava durumu koşullarını hesaplamanız gerekir. Aşağıdaki eylemleri kullanın:

* **DATE** sütununu ayrı **Date** ve **Time** sütunlarına bölün. Ayrıntılı adımlar için sonraki bölüme bakın.

* **Time** sütunundan bir **Hour_Range** sütunu türetin. Ayrıntılı adımlar için sonraki bölüme bakın.

* **DATE** ve **Hour_Range** sütunlarından bir **Date\_Hour\_Range** sütunu türetin. Ayrıntılı adımlar için sonraki bölüme bakın.

### <a name="split-column-by-example"></a>Sütunu örneğe göre bölme

1. **DATE** sütununu ayrı **Date** ve **Time** sütunlarına bölün. **DATE** sütun başlığına sağ tıklayın ve **Sütunu Örneğe Göre Böl**’ü seçin.

   ![Sütunu Örneğe Göre Böl girdisi](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

1. Machine Learning Workbench otomatik olarak anlamlı bir sınırlayıcı tanımlar ve verileri tarih ile saat değerlerine bölerek iki sütun oluşturur. 

1. Bölme işlemi sonuçlarını kabul etmek için __Tamam__’ı seçin.

   ![Bölünmüş DATE_1 ve DATE_2 sütunları](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Sütunu örneğe göre türetme

1. İki saatlik bir aralık türetmek için __DATE\_2__ sütun üst bilgisine sağ tıklayın ve **Sütunu Örneğe Göre Türet**’i seçin.

   ![Sütunu Örneğe Göre Türet girdisi](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Null değerlerle yeni bir boş sütun eklenir.

1. Yeni sütundaki ilk boş hücreyi seçin. İstenen zaman aralığının bir örneğini sağlamak için, yeni sütuna **12:00-02:00** yazın ve Enter'ı seçin.

   ![Değeri 12.00-02.00 aralığında olan yeni sütun](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Machine Learning Workbench, sizin sağladığınız örnekleri temel alan bir program sentezler ve aynı programı kalan satırlara uygular. Diğer tüm satırlar, sağladığınız örneğe göre otomatik olarak doldurulur. Workbench ayrıca verilerinizi analiz eder ve istisnai durumları tanımlamaya çalışır. 

   > [!IMPORTANT]
   > Workbench'in geçerli sürümünde Mac'te istisnai durumların tanımlanması çalışmayabilir. Mac bilgisayarlarda, aşağıdaki 3. ve 4. adımları atlayın. Bunun yerine, tüm satırlar türetilen değerlerle doldurulduktan sonra __Tamam__'ı seçin.
   
1. Kılavuzun üzerindeki **Veriler Analiz Ediliyor** iletisi, Workbench’in istisnai durumları algılamaya çalıştığını belirtir. İşlem bittiğinde, durum **Önerilen sonraki satırı gözden geçirin** veya **Öneri yok** olarak değişir. Bu örnekte, **Önerilen sonraki satırı gözden geçirin** durumu döndürülür.

1. Önerilen değişiklikleri gözden geçirmek için **Önerilen sonraki satırı gözden geçirin** öğesini seçin. Gözden geçirmeniz ve gerekirse düzeltmeniz gereken hücre ekranda vurgulanır.

   ![Önerilen sonraki satırı gözden geçirin](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Dönüştürmeyi kabul etmek için __Tamam__’ı seçin.
 
1. __BostonWeather__ verilerinin kılavuz görünümüne geri dönersiniz. Kılavuz bu durumda önceden eklenen üç sütunu içerir.

   ![Eklenen satırlarla birlikte kılavuz görünümü](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   > Yaptığınız tüm değişiklikler **Adımlar** bölmesinde saklanır. **Adımlar** bölmesinde oluşturduğunuz adıma gidin, aşağı oku seçin ve sonra da **Düzenle**’yi seçin. Gelişmiş **Sütunu Örneğe Göre Türet** penceresi gösterilir. Tüm örnekleriniz burada saklanır. Ayrıca, aşağıdaki kılavuzda bir satıra çift tıklayarak örnekleri el ile ekleyebilirsiniz. Değişiklikleri uygulamadan ana kılavuza geri dönmek için **İptal**’i seçin. Bu görünüme, **Sütunu Örneğe Göre Türet** dönüşümünü gerçekleştirirken **Gelişmiş mod**’u seçerek de erişebilirsiniz.

1. Sütunu yeniden adlandırmak için sütun üst bilgisine çift tıklayın ve **Hour Range** yazın. Değişikliği kaydetmek için Enter'ı seçin.

   ![Sütunları yeniden adlandırma](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

1. Tarih ve saat aralığını türetmek için **Date\_1** ve **Hour Range** sütunlarını birlikte seçin, sonra da **Sütunu Örneğe Göre Türet**’i seçin.

   ![Sütunu Örneğe Göre Türet](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   İlk satıra örnek olarak **1 Ocak 2015 12:00-02:00** yazın ve Enter'ı seçin.

   Workbench, sağladığınız örneği temel alarak dönüşümü belirler. Bu örnekte sonuç olarak tarih biçimi değiştirilir ve iki saatlik zaman penceresi ile birleştirilir.

   ![Örnek 1 Ocak 2015 12:00-02:00](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

   > [!IMPORTANT]
   > Mac bilgisayarlarda, 8. adım yerine aşağıdaki adımı izleyin:
   >
   > * **1 Şubat 2015 12:00-02:00** değerini içeren ilk hücreye gidin. Bu 15. satır olmalıdır. Değeri **2 Ocak 2015 12:00-02:00** olarak düzeltin ve Enter'ı seçin. 
   

1. **Veriler Analiz Ediliyor** durumunun **Önerilen sonraki satırı gözden geçirin** olarak değişmesini bekleyin. Bu değişiklik birkaç saniye sürebilir. Önerilen satıra gitmek için durum bağlantısını seçin. 

   ![Gözden geçirmek için önerilen satır](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   Kaynak tarih değeri gg/aa/yyyy veya aa/gg/yyyy biçiminde olabileceğinden satır null değere sahiptir. Doğru değer olan **13 Ocak 2015 02:00-04:00** değerini yazın ve Enter'ı seçin. Workbench kalan satırlarda türetmeyi iyileştirmek için iki örneği kullanır.

   ![Doğru biçimlendirilmiş veriler](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

1. Dönüştürmeyi kabul etmek için **Tamam**’ı seçin.

   ![Tamamlanan dönüştürme kılavuzu](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Bu adımda **Sütunu Örneğe Göre Türet** öğesinde **Gelişmiş mod**'u kullanmak için, **Adımlar** bölmesinde aşağı oku seçin. Veri kılavuzunda, **DATE\_1** ve **Hour Range** sütunlarının yanında onay kutuları bulunur. Çıkışın nasıl değiştiğini görmek için **Hour Range** sütununun yanındaki onay kutusunu temizleyin. Girdi olarak **Hour Range** sütunu mevcut olmadığında, **12.00-2.00** sabit olarak kabul edilir ve türetilmiş değerlere eklenir. Değişikliklerinizi uygulamadan ana kılavuza geri dönmek için **İptal**’i seçin.
   ![Gelişmiş mod](media/tutorial-bikeshare-dataprep/derivedcolumnadvancededitdeselectcolumn.png)

1. Sütunu yeniden adlandırmak için üst bilgiye çift tıklayın. Adı **Date Hour Range** olarak değiştirin ve Enter'ı seçin.

1. **DATE**, **DATE\_1**, **DATE\_2** ve **Hour Range** sütunlarını birlikte seçin. Sağ tıklayın ve sonra **Sütunu kaldır**’ı seçin.

## <a name="summarize-data-mean"></a>Verileri özetleme (ortalama)

Sonraki adımda, değerlerin saat aralığına göre gruplandırılmış ortalaması alınarak hava durumu koşulları özetlenir.

1. **Date Hour Range** sütununu ve sonra **Dönüşümler** menüsünden **Özetle**'yi seçin.

    ![Dönüşümler menüsü](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

1. Verileri özetlemek için, sütunları sayfanın alt kısmındaki kılavuzdan üstteki sol ve sağ bölmelere sürükleyin. Sol bölmede **Verileri gruplandırmak için sütunları buraya sürükleyin** iletisi bulunur. Sağ bölmede **Verileri özetlemek için sütunları buraya sürükleyin** iletisi bulunur. 

    a. **Date Hour Range** sütununu alttaki kılavuzdan sol bölmeye sürükleyin. **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** ve **HOURLYWindSpeed** sütunlarını sağ bölmeye sürükleyin. 

    b. Sağ bölmede her bir sütunun **Toplam** ölçümü olarak **Ortalama**’yı seçin. **Tamam**'ı seçerek özetlemeyi bitirin.

    ![Özetlenmiş veri ekranı](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-by-using-script"></a>Betik kullanarak veri akışını dönüştürme

Sayısal sütunlardaki verilerin 0 - 1 aralığında olacak şekilde değiştirilmesi, bazı modellerin hızla yakınsanmasını sağlar. Şu anda, bu dönüşümü genel olarak yapabilecek yerleşik bir dönüştürme yoktur. Bu işlemi gerçekleştirmek için Python betiği kullanın.

1. **Dönüşüm** menüsünde **Dönüşüm Veri Akışı (Betik)** öğesini seçin.

1. Görüntülenen metin kutusuna aşağıdaki kodu girin. Sütun adlarını kullandıysanız, kod değişiklik yapılmadan çalışmalıdır. Python’da basit bir min-maks normalleştirme mantığı yazıyorsunuz.

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
    
   ![Dönüşüm Veri Akışı (Betik) iletişim kutusu](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

1. Betiği kullanmak için __Tamam__’ı seçin. Kılavuzdaki sayısal sütunlar artık 0 -1 aralığındaki değerleri içerir.

    ![0 ile 1 arasındaki değerleri içeren kılavuz](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Hava durumu verilerini hazırlamayı tamamladınız. Şimdi seyahat verilerini hazırlayın.

## <a name="load-trip-data"></a>Seyahat verilerini yükleme

1. `201701-hubway-tripdata.csv` dosyasını içeri aktarmak için [Yeni veri kaynağı oluşturma](#newdatasource) bölümündeki adımları kullanın. İçeri aktarma işlemi sırasında aşağıdaki seçenekleri kullanın:

    * __Dosya Seçimi__: Seçmek üzere dosyaya göz atarken **Azure Blobu**'nu seçin.

    * __Örnekleme Şeması__: **Tam Dosya** örnekleme şemasını seçin ve örneği etkin hale getirin.

    * __Veri Türü__: Varsayılanları kabul edin.

1. Verileri içeri aktardıktan sonra hazırlamaya başlamak için __Hazırla__’yı seçin. Var olan **BikeShare Data Prep.dprep** paketini ve ardından __Tamam__’ı seçin.

    Bu işlem yeni bir dosya oluşturmak yerine var olan **Data Preparation** dosyasına bir **Veri Akışı** ekler.

    ![Mevcut paketi seçme](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

1. Kılavuz yüklendikten sonra __DATAFLOWS__ öğesini genişletin. Artık iki veri akışı vardır: **BostonWeather** ve **201701-hubway-tripdata**. **201701-hubway-tripdata** girdisini seçin.

    ![201701-hubway-tripdata girdisi](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-the-map-inspector"></a>Harita denetçisini kullanma

Veri hazırlığı için, dize verileriyle sayısal ve coğrafi verilere yönelik denetçi olarak adlandırılan kullanışlı görselleştirmeler sağlanır. Bunlar, verileri daha iyi anlamanıza ve aykırı değerleri belirlemenize yardımcı olur. Harita denetçisini kullanmak için şu adımları izleyin.

1. **Start station latitude** ve **start station longitude** sütunlarını birlikte seçin. Sütunlardan birine sağ tıklayın ve sonra **Harita**’yı seçin.

    > [!TIP]
    > Çoklu seçimi etkinleştirmek için Ctrl tuşunu (Mac’te ⌘ komutu) basılı tutun ve her sütuna ait üst bilgiyi seçin.

    ![Harita görselleştirme](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

1. Harita görselleştirmeyi en üst düzeye çıkarmak **Ekranı Kapla** simgesini seçin. Haritayı pencereye sığdırmak için görselleştirmenin sol üst kısmındaki **E** simgesini seçin.

    ![Tam ekran görüntüsü](media/tutorial-bikeshare-dataprep/maximizedmap.png)

1. Kılavuz görünümüne dönmek için **Simge Durumuna Küçült** düğmesini seçin.

## <a name="use-the-column-statistics-inspector"></a>Sütun istatistikleri denetçisini kullanma

Sütun istatistikleri denetçisini kullanmak için __tripduration__ sütununa sağ tıklayıp __Sütun İstatistikleri__’ni seçin.

![Sütun istatistikleri girdisi](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Bu işlem __INSPECTORS__ bölmesine __tripduration İstatistikleri__ başlıklı yeni bir görselleştirme ekler.

![Tripduration İstatistikleri denetçisi](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> Seyahat süresinin en yüksek değeri 961.814 dakikadır ve bu da yaklaşık olarak iki yıla denk gelir. Görünüşe göre veri kümesinde bazı aykırı değerler bulunmaktadır.

## <a name="use-the-histogram-inspector"></a>Histogram denetçisini kullanma

Aykırı değerleri tanımlamayı denemek için __tripduration__ sütununa sağ tıklayıp __Histogram__’ı seçin.

![Histogram denetçisi](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Histogram yararlı bir sonuç vermez çünkü aykırı değerler grafı eğer.

## <a name="add-a-column-by-using-script"></a>Betik kullanarak sütun ekleme

1. **Tripduration** sütununa sağ tıklayıp **Sütun Ekle (Betik)** öğesini seçin.

    ![Sütun Ekle (Betik) menüsü](media/tutorial-bikeshare-dataprep/computecolscript.png)

1. __Sütun Ekle (Betik)__ iletişim kutusunda aşağıdaki değerleri kullanın:

    * __Yeni Sütun Adı__: logtripduration

    * __Bu Yeni Sütunu Şundan Sonra Ekle__: tripduration

    * __Yeni Sütun Kodu__: `math.log(row.tripduration)`

    * __Kod Bloğu Türü__: İfade

   ![Sütun Ekle (Betik) iletişim kutusu](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

1. **Logtripduration** sütununu eklemek için __Tamam__’ı seçin.

1. Sütuna sağ tıklayın ve **Histogram**’ı seçin.

    ![Logtripduration sütununun histogramı](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

    Görsel olarak, bu histogram anormal kuyruklu bir normal dağılım gibi görünür.

## <a name="use-an-advanced-filter"></a>Gelişmiş filtre kullanma

Veriler üzerinde bir filtre kullanılması, denetçileri yeni dağıtımla güncelleştirir. 

1. **Logtripduration** sütununa sağ tıklayın ve **Sütunu Filtrele**'yi seçin. 

1. __Düzenle__ iletişim kutusunda aşağıdaki değerleri kullanın:

    * __Bu Sayı Sütununu Filtrele__: logtripduration

    * __İstiyorum__: Satırları Tut

    * __Ne Zaman__: Aşağıdaki Koşullardan herhangi biri True (mantıksal OR) olduğunda

    * __Bu Sütun__: küçüktür ise

    * __Değer__: 9

    ![Filtre seçenekleri](media/tutorial-bikeshare-dataprep/loftripfilter.png)

1. Filtreyi uygulamak için __Tamam__’ı seçin.

    ![Filtre uygulandıktan sonra güncelleştirilmiş histogramlar](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Halo etkisi

1. **logtripduration** histogramını büyütün. Gri histogramın üzerine mavi bir histogram yayılır. Bu ekran **Halo Etkisi** olarak adlandırılır:

    * Gri histogram işlemden (bu örnekte filtreleme işlemi) önceki dağılımı temsil eder.

    * Mavi histogram işlemden sonraki histogramı temsil eder. 

   Halo etkisi, bir işlemin veriler üzerindeki etkisini görselleştirmeye yardımcı olur.

   ![Halo etkisi](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Mavi histogram, öncekine göre daha kısa görünür. Bu farkın nedeni, yeni aralıkta verilerin yeniden demetlenmesidir.

1. Haloyu kaldırmak için __Düzenle__’yi seçin ve __Halo göster__ seçeneğinin işaretini kaldırın.

    ![Histogram seçenekleri](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

1. **Tamam**'ı seçenek halo etkisini devre dışı bırakın. Ardından histogramı küçültün.

### <a name="remove-columns"></a>Sütunları kaldırma

Seyahat verilerinde her satır bir bisiklet alma olayını temsil eder. Bu öğretici için yalnızca **starttime** ve **start station id** sütunları gereklidir. Diğer sütunları kaldırmak için bu iki sütunu birlikte seçin, sütun üst bilgisine sağ tıklayın ve sonra **Sütunu Tut**’u seçin. Diğer sütunlar kaldırılır.

![Sütunu Tut seçeneği](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Verileri özetleme (sayı)

İki saatlik bir sürenin bisiklet talebini özetlemek için türetilmiş sütunlar kullanın.

1. **Starttime** sütununa sağ tıklayın ve **Sütunu Örneğe Göre Türet**’i seçin.

    ![Sütunu Örneğe Göre Türet seçeneği](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

1. Örneğin, ilk satır için **1 Ocak 2017 12:00-02:00** değerini girin.

    > [!IMPORTANT]
    > Türetilen sütunlara ilişkin önceki örnekte, tarih ve saati içeren bir sütun türetmek için birden çok adım kullandınız. Bu örnekte, son çıktının bir örneğini sağlayarak bu işlemin tek bir adımda gerçekleştirilebileceğini görebilirsiniz.

    > [!NOTE]
    > Satırların herhangi birine göre bir örnek verebilirsiniz. Örneğin, ilk veri satırı için **1 Ocak 2017 12:00-02:00** geçerli bir değerdir.

    ![Örnek veriler](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

   > [!IMPORTANT]
   > Mac bilgisayarlarda, 3. adım yerine bu adımı izleyin:
   >
   > * **1 Ocak 2017 1:00-2:00** değerini içeren ilk hücreye gidin. Bu 14. satır olmalıdır. Değeri **1 Ocak 2017 12:00-02:00** olarak düzeltin ve Enter'ı seçin. 

1. Uygulamanın değerleri tüm satırlara göre hesaplamasını bekleyin. İşlem birkaç saniye sürebilir. Analiz bittikten sonra verileri gözden geçirmek için __Önerilen sonraki satırı gözden geçirin__ bağlantısını kullanın.

   ![Biten analiz ve gözden geçirme bağlantısı](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Hesaplanan değerlerin doğru olduğundan emin olun. Doğru değilse, değeri beklenen değerle güncelleştirin ve Enter tuşuna basın. Sonra analizin bitmesini bekleyin. **Öneri yok** ifadesini görene kadar **Önerilen sonraki satırı gözden geçirin** işlemini tamamlayın. **Öneri yok** ifadesi, uygulamanın istisnai durumlara baktığı ve sentezlenen programı uygun bulduğu anlamına gelir. Dönüşümü kabul etmeden önce görsel denetim gerçekleştirmek iyi bir yöntemdir. 

1. Dönüştürmeyi kabul etmek için **Tamam**’ı seçin. Yeni oluşturulan sütunu **Date Hour Range** olarak adlandırın.

    ![Yeniden adlandırılan sütun](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

1. **Starttime** sütun üst bilgisine sağ tıklayın ve **Sütunu kaldır**’ı seçin.

1. Verileri özetlemek için __Dönüşüm__ menüsünde __Özetle__’yi seçin. Dönüşümü oluşturmak için aşağıdaki adımları kullanın:

    * __Date Hour Range__ ve __start station id__ sütunlarını soldaki **Gruplandırma Ölçütü** bölmesine sürükleyin.

    * __Start station id__ sütununu sağdaki **verileri özetle** bölmesine sürükleyin.

   ![Özetleme seçenekleri](media/tutorial-bikeshare-dataprep/tripdatacount.png)

1. Özet sonucunu kabul etmek için **Tamam**’ı seçin.

## <a name="join-dataflows"></a>Veri akışlarını birleştirme

Hava durumu verilerini seyahat verileriyle birleştirmek için aşağıdaki adımları kullanın:

1. __Dönüşümler__ menüsünde __Birleştir__’i seçin.

1. __Tablolar__: **Sol** veri akışı olarak **BostonWeather**’ı, **Sağ** veri akışı olarak **201701-hubway-tripdata** öğesini seçin. Devam etmek için **İleri**’yi seçin.

    ![Tablo seçimleri](media/tutorial-bikeshare-dataprep/jointableselection.png)

1. __Anahtar Sütunları__: Tabloların her ikisinde **Date Hour Range** sütununu ve sonra __İleri__’yi seçin.

    ![Anahtar Sütunu seçimleri](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

1. __Birleştirme Türü__: Birleştirme türü olarak __Eşleşen satırlar__’ı seçin ve sonra da __Son__’u seçin.

    ![Eşleşen satırlar birleştirme türü](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Bu işlem __Birleştirme Sonucu__ adlı yeni bir veri akışı oluşturur.

## <a name="create-additional-features"></a>Ek özellikler oluşturma

1. Haftanın gününü içeren bir sütun oluşturmak için **Date Hour Range** sütununa sağ tıklayıp **Sütunu Örneğe Göre Türet**’i seçin. Pazar günü gerçekleşen bir tarih için __Paz__ değerini kullanın. Örnek olarak **1 Ocak 2017 12:00-02:00** verilebilir. Enter'ı seçin ve sonra da **Tamam**'ı seçin. Bu sütunu __Weekday__ olarak adlandırın.

    ![Haftanın günü için yeni sütun oluşturma](media/tutorial-bikeshare-dataprep/featureweekday.png)

1. Bir satırın saat aralığını içeren bir sütun oluşturmak için **Date Hour Range** sütununa sağ tıklayın ve **Sütunu Örneğe Göre Türet**’i seçin. **1 Ocak 2017 12:00-02:00** değerini içeren satır için **12:00-02:00** değerini kullanın. Enter'ı seçin ve sonra da **Tamam**'ı seçin. Bu sütunu **Period** olarak adlandırın.

    ![Dönem sütunu](media/tutorial-bikeshare-dataprep/featurehourrange.png)

1. **Date Hour Range** ve **r_Date Hour Range** sütunlarını kaldırmak için Ctrl’yi (Mac’te ⌘ komutu) ve ardından her bir sütun üst bilgisini seçin. Sağ tıklayın ve **Sütunu Kaldır**’ı seçin.

## <a name="read-data-from-python"></a>Python'dan veri okuma

Python veya PySpark’tan bir veri hazırlama paketi çalıştırabilir ve sonucu bir **Veri Çerçevesi** olarak alabilirsiniz.

Örnek Python betiği oluşturmak için __BikeShare Data Prep__’e sağ tıklayıp __Veri Erişim Kodu Dosyası Oluştur__’u seçin. Örnek Python dosyası **Proje Klasörünüzde** oluşturulur ve ayrıca Workbench’teki bir sekmeye yüklenir. Oluşturulan kodun bir örneği, aşağıdaki Python betiğidir:

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

**Birleştirme Sonucu** veri akışını bir .csv dosyasına kaydetmek için `BikeShare Data Prep.py` betiğini değiştirmeniz gerekir. 

1. Visual Studio Code'da projeyi düzenleme için açın.

    ![Visual Studio Code'da projeyi açma](media/tutorial-bikeshare-dataprep/openprojectinvscode.png)

1. Aşağıdaki kodu kullanarak `BikeShare Data Prep.py` dosyasındaki Python betiğini güncelleştirin:

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/testata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

1. `Your Azure Storage blob path` değerini, oluşturulacak çıkış dosyasının yoluyla değiştirin. `blobfolder` ve `csvfiles` değişkenlerinin ikisini de değiştirin.

## <a name="create-an-hdinsight-run-configuration"></a>HDInsight çalıştırma yapılandırması oluşturma

1. Machine Learning Workbench'te komut satırı penceresini açın, **Dosya** menüsünü ve sonra **Komut İstemini Aç**'ı seçin. Komut isteminiz `C:\Projects\BikeShare>` istemiyle proje klasöründe başlatılır.

    ![Komut istemini açma](media/tutorial-bikeshare-dataprep/opencommandprompt.png)

   >[!IMPORTANT]
   >Aşağıdaki adımları gerçekleştirmek için komut satırı penceresini (Workbench'ten açılan) kullanmanız gerekir.

1. Komut istemini kullanarak Azure'da oturum açın. 

   Siz Azure kaynaklarında kimlik doğrulaması yaparken, Workbench uygulaması ve CLI bağımsız kimlik bilgisi önbellekleri kullanır. Bu işlemi yalnızca bir kez yapmanız gerekir; önbelleğe alınan belirtecin süresi dolana kadar geçerli olacaktır. `az account list` komutu, oturum açma bilgilerinizle kullanılabilen aboneliklerin listesini döndürür. Birden fazla varsa, istenen abonelikteki kimlik değerini kullanın. Bu aboneliği `az account set -s` komutuyla birlikte kullanılacak varsayılan hesap olarak ayarlayın ve sonra abonelik kimliği değerini sağlayın. Ardından hesap `show` komutunu kullanarak ayarı doğrulayın.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

1. HDInsight çalıştırma yapılandırmasını oluşturun. Kümenizin adını ve `sshuser` parolasını biliyor olmalısınız.

    ```azurecli
    az ml computetarget attach cluster --name hdinsight --address <yourclustername>.azurehdinsight.net --username sshuser --password <your password>
    az ml experiment prepare -c hdinsight
    ```
> [!NOTE]
> Boş bir proje oluşturulduğunda, varsayılan çalıştırma yapılandırmaları **local** ve **docker**'dır. Bu adım, betiklerinizi çalıştırırken Workbench'te kullanılabilen yeni bir çalıştırma yapılandırması oluşturur. 

## <a name="run-in-an-hdinsight-cluster"></a>HDInsight kümesinde çalıştırma

Betiğinizi HDInsight kümesinde çalıştırmak için Machine Learning Workbench uygulamasına dönün.

1. Sol taraftaki **Giriş** simgesini seçerek projenizin giriş ekranına dönün.

1. Betiğinizi HDInsight kümesinde çalıştırmak için açılan listeden **hdinsight** öğesini seçin.

1. **Çalıştır**'ı seçin. Betik, bir iş olarak gönderilir. Dosya depolama kapsayıcınızda belirtilen konuma yazıldıktan sonra iş durumu __Tamamlandı__ olarak değişir.

    ![HDInsight betik çalıştırma](media/tutorial-bikeshare-dataprep/hdinsightrunscript.png)


## <a name="substitute-data-sources"></a>Yedek veri kaynakları

Önceki adımlarda `201701-hubway-tripdata.csv` ve `BostonWeather.csv` veri kaynaklarını kullanarak test verilerini hazırladınız. Paketi diğer seyahat veri dosyalarıyla birlikte kullanmak için aşağıdaki adımları uygulayın:

1. Daha önce verilen adımları aşağıdaki değişikliklerle kullanarak, yeni bir veri kaynağı oluşturun:

    * __Dosya Seçimi__: Dosya seçerken, kalan altı seyahat tripdata .csv dosyasını birlikte seçin.

    ![Kalan altı dosyayı yükleme](media/tutorial-bikeshare-dataprep/browseazurestoragefortripdatafiles.png)

    > [!NOTE]
     > __+5__ girdisi, listelenen dosyaya ek olarak beş dosyanın daha bulunduğunu belirtir.

    * __Dosya Ayrıntıları__: __Üst Bilgileri Yükseltme Modu__’unu **Tüm Dosyalar Aynı Üst Bilgilere Sahiptir** olarak ayarlayın. Bu değer, dosyaların her birinin aynı üst bilgiyi içerdiğini gösterir.

    ![Dosya ayrıntıları seçimi](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Sonraki adımlarda kullanılacağı için bu veri kaynağının adını kaydedin.

1. Dosyaları projenizde görüntülemek için klasör simgesini seçin. __aml\_config__ dizinini genişletip `hdinsight.runconfig` dosyasını seçin.

    ![Hdinsight.runconfig dosyasının konumu](media/tutorial-bikeshare-dataprep/hdinsightsubstitutedatasources.png) 

1. Dosyayı Visual Studio Code'da açmak için **Düzenle** düğmesini seçin.

1. `hdinsight.runconfig` dosyasının sonuna aşağıdaki satırları ekleyin ve sonra disk simgesini seçerek dosyayı kaydedin.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Bu değişiklik, özgün veri kaynağını altı seyahat veri dosyasını içeren veri kaynağıyla değiştirir.

## <a name="save-training-data-as-a-csv-file"></a>Eğitim verilerini CSV dosyası olarak kaydetme

1. Daha önce düzenlemiş olduğunuz `BikeShare Data Prep.py` Python dosyasına göz atın. Eğitim verilerini kaydetmek için farklı bir dosya yolu sağlayın.

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/traindata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

1. Eğitim verileri çıkışı için `traindata` adlı klasörü kullanın.

1. Yeni bir iş göndermek için **Çalıştır**'ı seçin. **hdinsight** öğesinin seçili olduğundan emin olun. Yeni yapılandırmayla bir iş gönderilir. Bu işin çıkışı, eğitim verileridir. Bu veriler, daha önce izlediğiniz veri hazırlama adımları kullanılarak oluşturulur. İşin tamamlanması birkaç dakika sürebilir.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bisiklet paylaşımı veri hazırlama öğreticisini bitirdiniz. Bu öğreticide Machine Learning (önizleme) kullanarak aşağıdakileri yapmayı öğrendiniz:
> [!div class="checklist"]
> * Azure Machine Learning veri hazırlama aracı ile verileri etkileşimli olarak hazırlama.
> * Test veri kümesini içeri aktarma, dönüştürme ve oluşturma.
> * Veri hazırlama paketi oluşturma.
> * Python kullanarak veri hazırlama paketini çalıştırma.
> * Ek giriş dosyaları için veri hazırlama paketini yeniden kullanarak bir eğitim veri kümesi oluşturma.

Bundan sonra, veri hazırlama hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Veri hazırlama kullanıcı kılavuzu](data-prep-user-guide.md)
