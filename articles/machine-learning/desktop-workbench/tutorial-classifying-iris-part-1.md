---
title: Azure Machine Learning hizmetinde Iris öğreticisini sınıflandırmak için verileri hazırlama (önizleme) | Microsoft Docs
description: Bu eksiksiz öğreticide Azure Machine Learning hizmetinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu, birinci bölümdür ve veri hazırlığını açıklar.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs, gcampanella
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 03/07/2018
ROBOTS: NOINDEX
ms.openlocfilehash: dd10581888da64114debec40cba8564023033864
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53278517"
---
# <a name="tutorial-1-classify-iris---preparing-the-data"></a>Öğretici 1: Iris - verileri hazırlama sınıflandırma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Azure Machine Learning hizmeti (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve gelişmiş bir analiz çözümüdür.

Bu öğretici, **üç bölümden oluşan bir serinin birinci bölümüdür**. Bu öğreticide, Azure Machine Learning hizmeti (önizleme) hakkındaki temel bilgileri adım adım inceleyerek şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Machine Learning Workbench’te proje oluşturma
> * Veri hazırlama paketi oluşturma
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Bu öğreticide zamansız [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. 

[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:
- Bir Azure Machine Learning Denemesi hesabı
- Azure Machine Learning Workbench'in yüklü olması

Bu Önkoşullar zaten yoksa, adımları [hızlı başlangıç: Yükleme ve başlatma](quickstart-installation.md) makale hesaplarınızı ayarlayın ve Azure Machine Learning Workbench uygulamasını yükleyin. 

## <a name="create-a-new-project-in-workbench"></a>Workbench'te yeni proje oluşturma

Adımları izlediyseniz [hızlı başlangıç: Yükleme ve başlatma](quickstart-installation.md) makale zaten bu proje yüklü olmalıdır ve sonraki bölüme atlayabilirsiniz.

1. Azure Machine Learning Workbench uygulamasını açın ve gerekirse oturum açın. 
   
   + Windows’da **Machine Learning Workbench** masaüstü kısayolunu kullanarak uygulamayı başlatın. 
   + macOS’ta Launchpad’den **Azure ML Workbench**’i seçin.

1. **PROJELER** bölmesinden artı işaretini (+) seçip **Yeni Proje**’yi seçin.  

   ![Yeni çalışma alanı](./media/tutorial-classifying-iris/new_ws.png)

1. Form alanlarını doldurun ve **Oluştur** düğmesini seçerek Workbench’te yeni projeyi oluşturun.

   Alan|Öğretici için önerilen değer|Açıklama
   ---|---|---
   Proje adı | myIris |Hesabınızı tanımlayan benzersiz bir ad girin. Kendi adınızı veya denemeyi en iyi şekilde tanımlayan departman ya da proje adını kullanabilirsiniz. Adı 2-32 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler ve kısa çizgi (-) karakteri kullanılabilir. 
   Proje dizini | c:\Temp\ | Projenin oluşturulduğu dizini belirtin.
   Proje açıklaması | _boş bırakın_ | Projeleri açıklamak için kullanışlı bir isteğe bağlı alan.
   Visualstudio.com GIT Deposu URL’si |_boş bırakın_ | İsteğe bağlı alan. Bir projeyi, kaynak denetimi ve işbirliği için isteğe bağlı olarak Azure DevOps’ta bir Git deposuyla ilişkilendirebilirsiniz. [Bunun nasıl ayarlanacağını öğrenin](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo). 
   Seçili çalışma alanı | IrisGarden (varsa) | Azure portalında Deneme hesabınız için oluşturduğunuz bir çalışma alanını seçin. <br/>Hızlı Başlangıç’ı izlediyseniz, IrisGarden adlı bir çalışma alanınız olmalıdır. Bu çalışma alanı yoksa, Deneme hesabınızı açtığınızda oluşturduğunuz çalışma alanını veya kullanmak istediğiniz başka bir çalışma alanını seçin.
   Proje şablonu | Classifying Iris | Şablonlar, ürünü keşfetmek için kullanabileceğiniz betikleri ve verileri içerir. Bu şablon, bu hızlı başlangıcın yanı sıra bu belge sitesindeki diğer öğreticiler için gereksinim duyduğunuz betikleri ve verileri de içerir. 

   ![Yeni proje](media/tutorial-classifying-iris/new_project.png)
 
 Yeni proje oluşturulur ve bu projeyi içeren proje panosu açılır. Bu noktada proje giriş sayfası, veri kaynakları, dizüstü bilgisayarlar ve kaynak kodu dosyalarını keşfedebilirsiniz. 

   ![Projeyi açma](media/tutorial-classifying-iris/project-open.png)
 

## <a name="create-a-data-preparation-package"></a>Veri hazırlama paketi oluşturma

Ardından verileri inceleyebilir ve Azure Machine Learning Workbench’te hazırlamaya başlayabilirsiniz. Workbench’te uyguladığınız her dönüştürme bir yerel veri hazırlama paketinde (*dprep dosyası) JSON dosyası biçimde depolanır. Bu veri hazırlama paketi, Workbench’teki veri hazırlama çalışmalarınız için birincil kapsayıcıdır.

Bu veri hazırlama paketi, daha sonra bir çalışma zamanına (local-C#/CoreCLR, Scala/Spark veya Scala/HDI gibi) devredilebilir. 

1. Klasör simgesini seçerek Dosya görünümü açın ve **iris.csv**’yi seçerek dosyayı açın.

   Bu dosya 5 sütun ve 50 satırdan oluşan bir tablo içerir. Dört sütun, sayısal özellik sütunlarıdır. Beşinci sütun dize hedef sütunudur. Sütunların hiçbirinin üst bilgi adı yoktur.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Özellikle dosya boyutu büyük olduğunda proje klasörünüze veri dosyaları eklemeyin. **iris.csv** veri dosyası küçük olduğu için tanıtım amacıyla bu şablona eklenmiştir. Daha fazla bilgi için bkz. [Büyük veri dosyalarını okuma ve yazma](how-to-read-write-files.md).

2. Yeni bir veri kaynağı eklemek için **Veri görünümü**’nde artı işaretini (**+**) seçin. **Veri Kaynağı Ekle** sayfası açılır. 

   ![Azure Machine Learning Workbench’teki Veri görünümü](media/tutorial-classifying-iris/data_view.png)

3. **Metin Dosyaları (\*.csv, \*.json, \*.txt., ...)** seçimini yapıp **İleri**’ye tıklayın.
   ![Azure Machine Learning Workbench’teki Veri Kaynağı](media/tutorial-classifying-iris/data-source.png)

4. **iris.csv** dosyasına göz atıp **Son**’a tıklayın. Bu işlem, ayırıcı ve veri türleri gibi parametreler için varsayılan değerleri kullanır.

   >[!IMPORTANT]
   >Bu alıştırma için geçerli proje dizininden **iris.csv** dosyasını seçtiğinizden emin olun. Aksi takdirde sonraki adımlar başarısız olabilir.
 
   ![Iris seçme](media/tutorial-classifying-iris/select_iris_csv.png)
   
5. **iris-1.dsource** adlı yeni bir dosya oluşturulur. Örnek proje zaten numaralandırılmamış bir **iris.dsource** dosyası ile birlikte geldiğinden dosya bir “-1” ile benzersiz şekilde adlandırılmıştır.  

   Dosya açılır ve veriler gösterilir. **Sütun1** ile **Sütun5** arasında bir dizi sütun üst bilgisi bu veri kümesine otomatik olarak eklenir. Alt kısma kaydırın ve veri kümesinin son satırının boş olduğuna dikkat edin. CSV dosyasında fazladan bir satır sonu olduğu için satır boştur.

   ![Iris veri görünümü](media/tutorial-classifying-iris/iris_data_view.png)

1. **Ölçümler** düğmesini seçin. Histogramlar oluşturulur ve görüntülenir.

   **Veri** düğmesini seçerek veri görünümüne geri dönebilirsiniz.
   
   ![Iris veri görünümü](media/tutorial-classifying-iris/iris_data_view_metrics.png)

1. Histogramları gözlemleyin. Her sütun için eksiksiz bir istatistik hesaplanmıştır. 

   ![Iris veri görünümü](media/tutorial-classifying-iris/iris_metrics_view.png)

8. **Hazırla** düğmesini seçerek bir veri hazırlama paketi oluşturmaya başlayın. **Hazırla** iletişim kutusu açılır. 

   Örnek proje varsayılan olarak seçilen bir **iris.dprep** veri hazırlama dosyası içerir. 

   ![Iris veri görünümü](media/tutorial-classifying-iris/prepare.png)

1. Açılan menüden **+ Yeni Veri Hazırlama Paketi**’ni seçerek yeni bir veri hazırlama paketi oluşturun.

   ![Iris veri görünümü](media/tutorial-classifying-iris/prepare_new.png)

1. Paket adı için yeni bir değer girin (**iris-1**’i kullanın) ve ardından **Tamam**’ı seçin.

   Veri hazırlama düzenleyicisinde **iris-1.dprep** adlı yeni bir veri hazırlama paketi oluşturulup açılır.

   ![Iris veri görünümü](media/tutorial-classifying-iris/prepare_iris-1.png)

   Şimdi bazı temel veri hazırlıkları yapalım. 

1. Üst bilgi metnini düzenlenebilir hale getirmek için her bir sütun başlığını seçin. Ardından her sütunu aşağıdaki gibi yeniden adlandırın: 

   Beş sütun için sırasıyla **Sepal Uzunluğu**, **Sepal Genişliği**, **Petal Uzunluğu**, **Petal Genişliği** ve **Türler** değerlerini girin.

   ![Sütunları yeniden adlandırma](media/tutorial-classifying-iris/rename_column.png)

1. Benzersiz değerleri sayın:
   1. **Türler** sütununu seçin
   1. Seçmek için sağ tıklayın. 
   1. Açılır menüden **Değer Sayıları**’nı seçin. 

   Verilerin altında **Denetçiler** bölmesi açılır. Dört çubuklu bir histogram görüntülenir. Hedef sütunda dört farklı değer bulunur: **Iris-virginica**, **Iris-versicolor**, **Iris-setosa**ve **(null)** değeri.

   ![Değer Sayıları seçme](media/tutorial-classifying-iris/value_count.png)

   ![Değer sayısı histogramı](media/tutorial-classifying-iris/filter_out.png)

1. Null değerleri filtreleyip dışarıda bırakmak için "(null)" çubuğunu ve sonra da eksi işaretini (**-**) seçin. 

   Bunun üzerine, (null) satırının rengi gri olur ve filtrenin uygulandığını gösterir. 

   ![Null değerleri filtrenin dışında bırakma](media/tutorial-classifying-iris/filter_out2.png)

1. **ADIMLAR** bölmesinde ayrıntılı olarak anlatılan tek tek veri hazırlama adımlarına dikkat edin. Sütunları yeniden adlandırıp null değerli satırları filtreledikten sonra her eylem bir veri hazırlama adımı olarak kaydedilmiştir. Her bir adımı düzenleyerek ayarlarını değiştirebilir, adımları yeniden sıralayabilir ve adımları kaldırabilirsiniz.

   ![Adımlar](media/tutorial-classifying-iris/steps.png)

1. Veri hazırlama düzenleyicisini kapatın. Grafik simgesiyle gösterilen **iris-1** sekmesini kapatmak için sekmedeki **x** simgesini seçin. Çalışmanız **Veri Hazırlıkları** üst bilgisi altında gösterilen **iris-1.dprep** dosyasına otomatik olarak kaydedilir.

   ![Kapat](media/tutorial-classifying-iris/close.png)

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

 Veri hazırlama paketinin çıkışı doğrudan Python'da veya Jupyter Notebook’ta incelenebilir. Paketler, yerel Python, Spark (Docker dahil) ve HDInsight gibi birden fazla çalışma zamanı arasında çalıştırılabilir. 

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

   Bu kodun çalıştırıldığı bağlama göre `df` değeri farklı bir DataFrame türünü temsil eder:
   + Python çalışma zamanında yürütülürken bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) kullanılır.
   + Spark bağlamında çalıştırılırken bir [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) kullanılır. 
   
   Azure Machine Learning Workbench'te verileri hazırlama hakkında daha fazla bilgi için [Veri hazırlamaya başlama](data-prep-getting-started.md) kılavuzuna bakın.

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
