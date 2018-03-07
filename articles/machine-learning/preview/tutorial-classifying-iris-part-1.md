---
title: "Azure Machine Learning hizmetlerinde Iris öğreticisini sınıflandırmak için verileri hazırlama (önizleme) | Microsoft Docs"
description: "Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu, birinci bölümdür ve veri hazırlığını açıklar."
services: machine-learning
author: hning86
ms.author: haining, j-martens
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 02/28/2018
ms.openlocfilehash: 0bef557ee1394e3c786fd2c54e821b5dea28fabf
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-classify-iris-part-1---preparing-the-data"></a>Öğretici: Iris Sınıflandırması bölüm 1 - Verileri hazırlama

Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğretici, üç bölümden oluşan bir serinin birinci bölümüdür. Bu öğreticide, Azure Machine Learning hizmetleri (önizleme) hakkındaki temel bilgileri adım adım inceleyeceğiz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure Machine Learning Workbench’te proje oluşturma
> * Veri hazırlama paketi oluşturma
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Bu öğreticide zamansız [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak macOS deneyimi de çok benzerdir.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:
- Bir Azure Machine Learning Denemesi hesabı
- Azure Machine Learning Workbench'in yüklü olması

Bu gereksinimleri zaten karşılamıyorsanız [Hızlı başlangıç: Yükleme ve başlatma](quickstart-installation.md) makalesindeki adımları izleyerek bu hesap ayarlayın ve Azure Machine Learning Workbench uygulamasını yükleyin. 

## <a name="create-a-new-project-in-workbench"></a>Workbench'te yeni proje oluşturma

[Hızlı başlangıç: Yükleme ve başlatma](quickstart-installation.md) makalesindeki adımları izlediyseniz bu projeye zaten sahipsiniz demektir ve bir sonraki bölüme atlayabilirsiniz.

1. Azure Machine Learning Workbench uygulamasını açın ve gerekirse oturum açın. 
   
   + Windows’da **Machine Learning Workbench** masaüstü kısayolunu kullanarak uygulamayı başlatın. 
   + macOS’ta Launchpad’den **Azure ML Workbench**’i seçin.

1. **PROJELER** bölmesinden artı işaretini (+) seçip **Yeni Proje**’yi seçin.  

   ![Yeni çalışma alanı](media/tutorial-classifying-iris/new_ws.png)

1. Form alanlarını doldurun ve **Oluştur** düğmesini seçerek Workbench’te yeni bir proje oluşturun.

   Alan|Öğretici için önerilen değer|Açıklama
   ---|---|---
   Proje adı | myIris |Hesabınızı tanımlayan benzersiz bir ad girin. Kendi adınızı veya denemeyi en iyi şekilde tanımlayan departman ya da proje adını kullanabilirsiniz. Adı 2-32 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve kısa çizgi (-) karakteri kullanılabilir. 
   Proje dizini | c:\Temp\ | Projenin oluşturulduğu dizini belirtin.
   Proje açıklaması | _boş bırakın_ | Projelerin açıklanması için kullanışlı olan isteğe bağlı alan.
   Visualstudio.com |_boş bırakın_ | İsteğe bağlı alan. Bir proje, kaynak denetimi ve işbirliği için isteğe bağlı olarak Visual Studio Team Services’da bir Git deposuyla ilişkilendirilebilir. [Bunun nasıl ayarlanacağını öğrenin. ](https://docs.microsoft.com/en-us/azure/machine-learning/preview/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo). 
   Çalışma alanı | IrisGarden (varsa) | Azure portalında Deneme hesabınız için oluşturduğunuz bir çalışma alanını seçin. <br/>Hızlı Başlangıç’ı izlediyseniz, IrisGarden adlı bir çalışma alanınız olmalıdır. Bu yoksa Deneme hesabınızı açtığınızda oluşturduğunuz bir çalışma alanını veya kullanmak istediğiniz başka bir çalışma alanını seçin.
   Proje şablonu | Iris Sınıflandırması | Şablonlar ürünü keşfetmek için kullanabileceğiniz komut dosyaları ve veriler içerir. Bu şablon, bu hızlı başlangıcın yanı sıra bu belge sitesindeki diğer öğreticiler için gereksinim duyduğunuz betikleri ve verileri içerir. 

   ![Yeni proje](media/tutorial-classifying-iris/new_project.png)
 
 Yeni bir proje oluşturulur ve bu projeyi içeren proje panosu açılır. Bu noktada proje giriş sayfası, veri kaynakları, dizüstü bilgisayarlar ve kaynak kodu dosyalarını keşfedebilirsiniz. 

## <a name="create-a-data-preparation-package"></a>Veri hazırlama paketi oluşturma

Öğreticinin bu bölümünde, verileri keşfeder ve veri hazırlama işlemine başlarsınız. Azure Machine Learning Workbench’te verilerinizi hazırladığınızda, Workbench’te gerçekleştirdiğiniz dönüştürme işlemlerinin JSON temsili yerel bir veri hazırlama paketinde (*.dprep dosyası) depolanır. Bu veri hazırlama paketi, Workbench’teki veri hazırlama çalışmalarınız için birincil kapsayıcıdır.

Bu veri hazırlama paketi, yürütülmek üzere bir çalışma zamanına (local-C#/CoreCLR, Scala/Spark veya Scala/HDI gibi) devredilebilir. Burada, yürütme için uygun çalışma zamanına yönelik kod oluşturulur. 

1. Klasör simgesini seçerek dosya görünümü açın ve **iris.csv**’yi seçerek dosyayı açın.  

   Bu dosya 5 sütun ve 150 satırdan oluşan bir tablodur. Dört sayısal özellik sütunu ve bir dize hedef sütunu vardır. Sütun üst bilgileri yoktur.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Özellikle dosya boyutu büyük olduğunda proje klasörünüze veri dosyaları eklemeyin. Bu şablona **iris.csv** dosyasını çok küçük olduğu için gösterim amacıyla ekliyoruz. Daha fazla bilgi için bkz. [Büyük veri dosyalarını okuma ve yazma](how-to-read-write-files.md).

2. Yeni bir veri kaynağı eklemek için **Veri Görünümü**’nde artı işaretini (**+**) seçin. **Veri Kaynağı Ekle** sayfası açılır. 

   ![Veri görünümü](media/tutorial-classifying-iris/data_view.png)

3. **Metin Dosyaları (*.csv, .json, .txt.,... )**’nı seçin ve **Sonraki**’ye tıklayın.
   ![Veri Kaynağı](media/tutorial-classifying-iris/data-source.png)
   

4. **iris.csv** dosyasına göz atıp **Sonraki**’ye tıklayın.  
 
   ![Iris seçme](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Bu alıştırma için geçerli proje dizininden **iris.csv** dosyasını seçtiğinizden emin olun. Aksi takdirde sonraki adımlar başarısız olabilir.
   
5. Varsayılan değerleri değiştirmeyin ve **Bitir**’e tıklayın.

6. **iris-1.dsource** adlı yeni bir dosya oluşturulur. Örnek proje zaten numaralandırılmamış bir **iris.dsource** dosyası ile birlikte geldiğinden dosya bir “-1” ile benzersiz şekilde adlandırılmıştır.  

   Dosya açılır ve veriler gösterilir. **Sütun1** ile **Sütun5** arasında bir dizi sütun üst bilgisi bu veri kümesine otomatik olarak eklenir. Alt kısma kaydırın ve veri kümesinin son satırının boş olduğuna dikkat edin. CSV dosyasında fazladan bir satır sonu olduğu için satır boştur.

   ![Iris veri görünümü](media/tutorial-classifying-iris/iris_data_view.png)

7. **Ölçümler** düğmesini seçin. Histogramları gözlemleyin. Her sütun için eksiksiz bir istatistik hesaplanmıştır. Verileri yeniden görmek için **Veriler** düğmesini de seçebilirsiniz. 

   ![Iris veri görünümü](media/tutorial-classifying-iris/iris_metrics_view.png)

8. **Hazırla** düğmesini seçin. **Hazırla** iletişim kutusu açılır. 

   Örnek proje, **iris.dprep** dosyasıyla birlikte gelir. Varsayılan olarak, zaten mevcut olan **iris.dprep** veri hazırlama paketinde yeni bir veri akışı oluşturmanızı ister. 

   Açılır menüden **+ Yeni Veri Hazırlama Paketi**’ni seçin, paket adı için yeni bir değer girin **iris-1**’i kullanın ve ardından **Tamam**’ı seçin.

   ![Iris veri görünümü](media/tutorial-classifying-iris/new_dprep.png)

   Veri hazırlama düzenleyicisinde **iris-1.dprep** adlı yeni bir veri hazırlama paketi oluşturulup açılır.

9. Şimdi bazı temel veri hazırlıkları yapalım. Üst bilgi metnini düzenlenebilir hale getirmek için her bir sütun başlığını seçin ve her bir sütunu aşağıdaki gibi yeniden adlandırın: 

   Beş sütun için sırasıyla **Sepal Uzunluğu**, **Sepal Genişliği**, **Petal Uzunluğu**, **Petal Genişliği** ve **Türler** değerlerini girin.

   ![Sütunları yeniden adlandırma](media/tutorial-classifying-iris/rename_column.png)

10. Farklı değerleri saymak için **Türler** sütununu seçip sağ tıklayın. Açılır menüden **Değer Sayıları**’nı seçin. 

   Bu eylem sonucunda, verilerin altında **Denetçiler** bölmesi açılır. Dört çubuklu bir histogram görüntülenir. Hedef sütunda üç farklı değer bulunur: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** ve bir **(null)** değeri.

   ![Değer Sayıları seçme](media/tutorial-classifying-iris/value_count.png)

11. Null değerleri filtrelemek için "Null" etiketini seçip eksi işaretini (**-**) seçin. Bunun üzerine, Null satırının rengi gri olur ve filtrenin uygulandığını gösterir. 

   ![Değer sayısı histogramı](media/tutorial-classifying-iris/filter_out.png)

12. **ADIMLAR** bölmesinde ayrıntıları verilen her bir adıma dikkat edin. Sütunları yeniden adlandırıp null değerli satırları filtreledikten sonra her eylem bir veri hazırlama adımı olarak kaydedilmiştir. Her bir adımı düzenleyerek ayarları değiştirebilir, adımları yeniden sıralayabilir ve adımları kaldırabilirsiniz.

   ![Adımlar](media/tutorial-classifying-iris/steps.png)

13. Veri hazırlama düzenleyicisini kapatın. Graf simgesi içeren **iris-1** sekmesindeki **Kapat** (x) öğesini seçin. Çalışmanız **Veri Hazırlıkları** üst bilgisi altında gösterilen **iris-1.dprep** dosyasına otomatik olarak kaydedilir.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

<!-- The output/results of a Package can be explored in Python or via a Jupyter Notebook. A Package can be executed across multiple runtimes including local Python, Spark (including in Docker), and HDInsight. A Package contains one or more Dataflows that are the steps and transforms applied to the data. A Package may use another Package as a Data Source (referred to as a Reference Data Flow). -->

1. Veri Hazırlıkları sekmesinden **iris-1.dprep** dosyasını bulun.

1. **iris-1.dprep** dosyasına sağ tıklayın ve bağlam menüsünden **Veri Erişim Kodu Dosyası Oluştur**’u seçin. 

   ![Kod oluşturma](media/tutorial-classifying-iris/generate_code.png)

   Aşağıdaki kod satırlarını içeren **iris-1.py** adlı yeni bir dosya açılır ve veri hazırlık paketi olarak oluşturduğunuz mantığı çağırır:

   ```python
   # Use the Azure Machine Learning data preparation package
   from azureml.dataprep import package

   # Use the Azure Machine Learning data collector to log various metrics
   from azureml.logging import get_azureml_logger
   logger = get_azureml_logger()

   # This call will load the referenced package and return a DataFrame.
   # If run in a PySpark environment, this call returns a
   # Spark DataFrame. If not, it will return a Pandas DataFrame.
   df = package.run('iris-1.dprep', dataflow_idx=0)

   # Remove this line and add code that uses the DataFrame
   df.head(10)
   ```

   Bu kodun çalıştırıldığı bağlama göre `df` değeri çeşitli türlerdeki veri çerçevelerini temsil eder. Python çalışma zamanında yürütüldüğünde bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) kullanılır veya Spark bağlamında yürütüldüğünde bir [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) kullanılır. 
   
   Azure Machine Learning Workbench uygulamasında verileri hazırlamayı öğrenmek için [Veri hazırlamaya başlama](data-prep-getting-started.md) kılavuzuna bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Machine Learning Workbench’i kullanarak aşağıdakileri yaptınız:
> [!div class="checklist"]
> * Yeni bir proje oluşturma
> * Veri hazırlama paketi oluşturma
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Öğretici serisinin Azure Machine Learning modeli derlemeyi öğreneceğiniz bir sonraki bölümüne geçmeye hazırsınız:
> [!div class="nextstepaction"]
> [Öğretici 2 - Derleme modelleri](tutorial-classifying-iris-part-2.md)
