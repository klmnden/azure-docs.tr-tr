---
title: "Azure Machine Learning hizmetlerinde Iris öğreticisini sınıflandırmak için verileri hazırlama (önizleme) | Microsoft Docs"
description: "Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu, birinci bölümdür ve veri hazırlığını açıklar."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 09/28/2017
ms.openlocfilehash: 4e558518a5a1fb7b4cd0a58fe2453fd4c083b46a
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="classify-iris-part-1-prepare-the-data"></a>Iris Sınıflandırma bölüm 1: Verileri hazırlama
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğretici, üç bölümden oluşan bir serinin birinci bölümüdür. Bu öğreticide, Azure Machine Learning hizmetleri (önizleme) hakkındaki temel bilgileri adım adım inceleyeceğiz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]
> * Azure Machine Learning Workbench’te proje oluşturma.
> * Bir veri hazırlama paketi oluşturma.
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma.

Bu öğreticide zamansız [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak Mac OS deneyimi de çok benzerdir.

## <a name="prerequisites"></a>Ön koşullar
- Bir Azure Machine Learning Denemesi hesabı oluşturun.
- Azure Machine Learning Workbench'i yükleyin.

Azure Machine Learning Workbench uygulamasını yüklemek için [Yükleme ve Oluşturma Hızlı Başlangıç](quickstart-installation.md) makalesindeki yönergeleri izleyebilirsiniz. Bu yükleme ayrıca Azure platformlar arası komut satırı aracı veya Azure CLI’yi içerir.

## <a name="create-a-new-project-in-azure-machine-learning-workbench"></a>Azure Machine Learning Workbench’te yeni proje oluşturma
1. Azure Machine Learning Workbench uygulamasını açın ve gerekirse oturum açın. **PROJELER** bölmesinde artı işaretini (**+**) seçerek **Yeni Proje** oluşturun.

   ![Yeni çalışma alanı](media/tutorial-classifying-iris/new_ws.png)

2. **Yeni Proje Oluştur** ayrıntılarını doldurun: 

   ![Yeni proje](media/tutorial-classifying-iris/new_project.png)

   - **Proje adı** kutusuna proje için bir ad girin. Örneğin, **myIris** değerini kullanın.
   - Projenin oluşturulduğu **Proje dizinini** seçin. Örneğin, `C:\Temp\` değerini kullanın. 
   - İsteğe bağlı olarak **Proje açıklaması** girin. 
   - **Git Deposu** alanı da isteğe bağlıdır ve boş bırakılabilir. Visual Studio Team Services üzerinde var olan boş bir Git deposu (ana dalı olmayan bir depo) belirtebilirsiniz. Zaten var olan bir Git deposu kullanıyorsanız, gezinti ve paylaşım senaryolarını daha sonra etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [Git deposu kullanma](using-git-ml-project.md). 
   - Bir **Çalışma alanı** seçin; örneğin, bu öğreticide **IrisGarden** kullanılır. 
   - Proje şablonu listesinden **Iris Sınıflandırma** şablonunu seçin. 

3. **Oluştur** düğmesini seçin. Proje oluşturulup açılır.

## <a name="create-a-data-preparation-package"></a>Veri hazırlama paketi oluşturma
1. **Dosya Görünümü**'nden **iris.csv** dosyasını açın. Bu dosya 5 sütun ve 150 satırdan oluşan bir tablodur. Dört sayısal özellik sütunu ve bir dize hedef sütunu vardır. Sütun üst bilgileri yoktur.

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

9. Şimdi bazı temel veri hazırlıkları yapalım. Sütunları yeniden adlandırın. Üst bilgi metnini düzenlenebilir hale getirmek için her bir sütun başlığını seçin. 

   Beş sütun için sırasıyla **Sepal Uzunluğu**, **Sepal Genişliği**, **Petal Uzunluğu**, **Petal Genişliği** ve **Türler** değerlerini girin.

   ![Sütunları yeniden adlandırma](media/tutorial-classifying-iris/rename_column.png)

10. Farklı değerleri saymak için **Türler** sütununu seçip sağ tıklayın. Açılır menüden **Değer Sayıları**’nı seçin. 

   ![Değer Sayıları seçme](media/tutorial-classifying-iris/value_count.png)

   Bu işlem **Denetçiler** bölmesini açar ve dört çubuklu bir histogram gösterir. Hedef sütunda üç farklı değer bulunur: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** ve bir **(null)** değeri.

11. Null değerleri filtrelemek için grafikten null değerini temsil eden çubuğu seçin. Değeri **(null)** olan bir satır vardır. Bu satırı kaldırmak için eksi işaretini seçin (**-**).

   ![Değer sayısı histogramı](media/tutorial-classifying-iris/filter_out.png)

12. **ADIMLAR** bölmesinde ayrıntıları verilen her bir adıma dikkat edin. Sütunları yeniden adlandırıp null değerli satırları filtreledikten sonra her eylem bir veri hazırlama adımı olarak kaydedilmiştir. Her bir adımı düzenleyerek ayarları değiştirebilir, adımları yeniden sıralayabilir ve adımları kaldırabilirsiniz.

   ![Adımlar](media/tutorial-classifying-iris/steps.png)

13. Veri hazırlama düzenleyicisini kapatın. Graf simgesi içeren **iris-1** sekmesindeki **Kapat** (x) öğesini seçin. Çalışmanız **Veri Hazırlıkları** üst bilgisi altında gösterilen **iris-1.dprep** dosyasına otomatik olarak kaydedilir.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

1. **iris-1.dprep** dosyasına sağ tıklayarak açılır menüyü getirin ve sonra **Veri Erişim Kodu Dosyası Oluştur**’u seçin. 

   ![Kod oluşturma](media/tutorial-classifying-iris/generate_code.png)

2. Aşağıdaki kod satırlarını içeren **iris-1.py** adlı yeni bir dosya açılır:

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

   Bu kod parçacığı bir veri hazırlama paketi olarak yeni oluşturduğunuz mantığı çağırır. Bu kodun çalıştırıldığı bağlama göre `df` değeri çeşitli türlerdeki veri çerçevelerini temsil eder. Python çalışma zamanında yürütüldüğünde bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) kullanılır veya Spark bağlamında yürütüldüğünde bir [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) kullanılır. 

   Azure Machine Learning Workbench uygulamasında veri hazırlama hakkında daha fazla bilgi için [Veri hazırlamaya başlama](data-prep-getting-started.md) kılavuzuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
Üç bölümden oluşan öğretici serisinin bu ilk bölümünde Azure Machine Learning Workbench uygulamasını kullanarak şunları yaptınız:
> [!div class="checklist"]
> * Yeni bir proje oluşturma. 
> * Bir veri hazırlama paketi oluşturma.
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma.

Serinin bir Azure Machine Learning modeli derlemeyi öğreneceğiniz sonraki kısmına geçmeye hazırsınız:
> [!div class="nextstepaction"]
> [Model oluşturma](tutorial-classifying-iris-part-2.md)
