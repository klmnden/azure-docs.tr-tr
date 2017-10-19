---
title: "Machine Learning hizmetlerinde Iris öğreticisini sınıflandırmak için verileri hazırlama (önizleme) | Microsoft Docs"
description: "Bu eksiksiz öğreticide Azure Machine Learning hizmetlerinin (önizleme) uçtan uca nasıl kullanılacağı gösterilmektedir. Bu bölüm, verileri hazırlama işleminin 1. kısmıdır."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/28/2017
ms.openlocfilehash: 975a86c1f9d9692f6eadd232177f33cbbbeeff2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="classifying-iris-part-1-prepare-data"></a>Iris sınıflandırma bölüm 1: Verileri hazırlama
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğretici üç bölümden oluşan bir serinin birinci bölümüdür. Bu öğreticide, Azure Machine Learning hizmetleri (önizleme) hakkındaki temel bilgileri adım adım inceleyeceğiz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Machine Learning Workbench’te proje oluşturma
> * Veri hazırlama paketi oluşturma
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Bu öğreticide kolaylık sağlamak için değişmeyen [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak macOS deneyimi de çok benzerdir.

## <a name="prerequisites"></a>Ön koşullar
- Azure Machine Learning Denemesi Hesabı oluşturma
- Azure Machine Learning Workbench'i yükleme

Azure Machine Learning Workbench uygulamasını yüklemek için [Yükleme ve Oluşturma Hızlı Başlangıç](quickstart-installation.md) yönergelerini izleyebilirsiniz. Bu yükleme ayrıca komut satırı arabirimini (CLI) içerir.

## <a name="create-a-new-project-in-azure-ml-workbench"></a>Azure ML Workbench'te yeni bir proje oluşturma
1. Azure Machine Learning Workbench uygulamasını başlatın ve gerekirse oturum açın. **PROJELER** bölmesinde **+** simgesine tıklayarak bir **Yeni Proje** oluşturun.

   ![yeni çalışma alanı](media/tutorial-classifying-iris/new_ws.png)

2. **Yeni Proje Oluştur** ayrıntılarını doldurun. 

   ![Yeni Proje](media/tutorial-classifying-iris/new_project.png)

   - **Proje adı** alanına proje için bir ad girin. Örneğin, **myIris** değerini kullanın.
   - Projenin oluşturulduğu **Proje dizinini** seçin. Örneğin, **C:\Temp** değerini kullanın. 
   - **Proje açıklaması** girin. 
   - **Git deposu** alanı isteğe bağlıdır ve boş bırakılabilir. VSTS üzerinde var olan boş bir Git deposu (ana dalı olmayan bir depo) belirtebilirsiniz. Bunun yapılması, daha sonra dolaşım ve paylaşma senaryolarını etkinleştirmenize izin verir. Daha fazla bilgi için lütfen [Git deposu kullanma](using-git-ml-project.md) makalesine bakın. 
   - Bir **Çalışma alanı** seçin; örneğin, bu öğreticide **IrisGarden** kullanılır. 
   - Proje şablonu listesinden **Iris Sınıflandırma** şablonunu seçin. 

3. Projeyi oluşturmak için **Oluştur** düğmesine tıklayın. Proje oluşturulup açılır.

## <a name="create-a-data-preparation-package"></a>Veri hazırlama paketi oluşturma
1. **Dosya Görünümü**'nden **iris.csv** dosyasını açın. Dosya 5 sütun ve 150 satırdan oluşan basit bir tablodur. Dört sayısal özellik sütunu ve bir dize hedef sütunu vardır. Sütun üst bilgileri yoktur.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Özellikle dosya boyutu büyük olduğunda proje klasörünüze veri dosyaları eklemeniz önerilmez. Bu şablona **iris.csv** dosyasını çok küçük olduğu için gösterim amacıyla ekliyoruz. Daha fazla bilgi için lütfen [Büyük veri dosyalarını okuma ve yazma](how-to-read-write-files.md) makalesine bakın.

2. Yeni bir veri kaynağı eklemek için **Veri Görünümü**’nde **+** simgesine tıklayın. **Veri Kaynağı Ekleme** sihirbazı başlatılır. 

   ![veri görünümü](media/tutorial-classifying-iris/data_view.png)

3. Veri Hazırlama Sihirbazını tamamlayın. 
   - İlk ekranda **Dosyalar/Dizin** seçeneğini belirleyip **İleri**'ye tıklayın.
   - İkinci ekranda **iris.csv** yerel dosyasını (örneğin "C:\Temp\myIris\iris.csv") seçin.
   - **Dosya Ayrıntıları** adlı üçüncü ekranda varsayılan değerleri kabul edin.
   - **Veri Türleri** adlı dördüncü ekranda, _Sütun1_ ile _Sütun4_ arasındaki sütunlar sayısal değerler olduğundan, bu sütunların _Dize_ olan **VERİ TÜRÜ** değerini _Sayısal_ olarak değiştirin. 
   - Beşinci ve altıncı ekranlarda varsayılan ayarları kabul edin.
   - **Son** düğmesine tıklayın.

   ![iris seçme](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Bu alıştırma için geçerli proje dizininden **iris.csv** dosyasını seçtiğinizden emin olun; aksi takdirde sonraki adımlar başarısız olabilir. 

4. **iris-1.dsource** adlı yeni bir dosya oluşturulur. Örnek proje zaten numaralandırılmamış bir **iris.dsource** dosyası ile birlikte geldiğinden dosya bir tire ile benzersiz şekilde adlandırılmıştır.  

   Dosya açılır ve veriler gösterilir. **Sütun1** ile **Sütun5** arasında bir dizi sütun üst bilgisi bu veri kümesine otomatik olarak eklenir. Alt kısma kaydırın ve veri kümesinin son satırının boş olduğuna dikkat edin. Bunun nedeni, csv dosyasında fazladan bir satır sonu olmasıdır.

   ![iris veri görünümü](media/tutorial-classifying-iris/iris_data_view.png)

5. Şimdi **Ölçümler** düğmesine tıklayın. Her sütundaki histogramları ve sizin için hesaplanan istatistikleri inceleyin. Ayrıca **Veri** düğmesine tıklayarak verileri yeniden görebilirsiniz. 

   ![iris veri görünümü](media/tutorial-classifying-iris/iris_metrics_view.png)

6. **Hazırla** düğmesine tıklayın. **Hazırla** iletişim kutusu açılır. 

   Örnek proje bir **iris.dprep** dosyası ile birlikte geldiğinden, varsayılan olarak mevcut **iris.dprep** veri hazırlama paketinde yeni bir veri akışı oluşturmanızı istenir. 

   Açılan değeri **+Yeni Veri Hazırlık Paketi** olarak değiştirin ve yeni "iris-1" değerini girip **Tamam**’a tıklayın.

   ![Iris veri görünümü](media/tutorial-classifying-iris/new_dprep.png)

   Veri hazırlama düzenleyicisinde **iris-1.dprep** adlı yeni bir veri hazırlama paketi oluşturulup açılır.

7. Şimdi bazı temel veri hazırlıkları yapalım. Her bir sütun başlığına tıklayarak sütunları yeniden adlandırın ve üst bilgi metnini düzenlenebilir hale getirin. 

   Beş sütun için sırasıyla **Sepal Uzunluğu**, **Sepal Genişliği**, **Petal Uzunluğu**, **Petal Genişliği** ve **Türler** değerlerini girin.

   ![Sütunları yeniden adlandırma](media/tutorial-classifying-iris/rename_column.png)

8. Farklı değerleri saymak için **Türler** sütununu seçip sağ tıklayın. **Değer Sayıları**’nı seçin. 

   ![Değer Sayıları'na tıklayın](media/tutorial-classifying-iris/value_count.png)

   Bu işlem **Denetçiler** bölmesini açar ve dört çubuklu bir histogram gösterir. Hedef sütunda üç farklı değerin olduğuna dikkat edin: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** ve bir **(null)** değeri.

9. Grafikten null değerini temsil eden çubuğu seçerek null değerleri filtreleyerek dışarıda bırakın. Değeri **(null)** olan bir satır vardır. Bu satırdan kurtulmak için **-** filtre düğmesine tıklayın.

   ![Değer sayısı histogramı](media/tutorial-classifying-iris/filter_out.png)

10. **ADIMLAR** bölmesinde ayrıntıları verilen her bir adıma dikkat edin. Null değer satırlarını yeniden adlandırma ve filtreleme işlemleri için sütun üzerinde çalışırken, her eylem bir veri hazırlık adımı olarak kaydedilmiştir. Her bir adımı düzenleyerek ayarları değiştirebilir, adımları yeniden sıralayabilir ve hatta adımları kaldırabilirsiniz.

   ![Adımlar](media/tutorial-classifying-iris/steps.png)

11. Şimdi, grafik simgesini içeren **iris-1** adlı sekmede **X** simgesine tıklayarak veri hazırlığı düzenleyicisini kapatın. Çalışmanız **Veri Hazırlıkları** üst bilgisi altında gösterilen **iris-1.dprep** dosyasına otomatik olarak kaydedilir.

## <a name="generate-pythonpyspark-code-to-invoke-data-prep-package"></a>Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

1. **iris-1.dprep** dosyasına sağ tıklayarak açılır menüyü getirin ve **Veri Erişim Kodu Dosyası Oluştur**’u seçin. 

   ![kod oluştur](media/tutorial-classifying-iris/generate_code.png)

2. Aşağıdaki kod satırlarını içeren **iris-1.py** adlı yeni bir dosya açılır:

   ```python
   # This code snippet will load the referenced package and return a DataFrame.
   # If the code is run in a PySpark environment, the code will return a
   # Spark DataFrame. If not, the code will return a Pandas DataFrame.

   from azureml.dataprep.package import run
   df = run('iris.dprep', dataflow_idx=0)
   ```

   Bu kod parçacığı bir veri hazırlama paketi olarak yeni oluşturduğunuz mantığı çağırır. Bu kodun çalıştırıldığı bağlama göre `df` değeri farklı türde veri çerçevelerini temsil eder. Python çalışma zamanında yürütüldüğünde bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) kullanılır veya Spark bağlamında yürütüldüğünde bir [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) kullanılır. 

   Azure Machine Learning Workbench uygulamasında veri hazırlama hakkında daha fazla bilgi için [Veri Hazırlamaya Başlama](data-prep-getting-started.md) kılavuzuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
Üç bölümden oluşan öğretici serisinin bu ilk bölümünde Azure Machine Learning Workbench uygulamasını kullanarak şunları yaptınız:
> [!div class="checklist"]
> * Yeni bir proje oluşturma 
> * Veri hazırlama paketi oluşturma
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Serinin bir machine learning modeli oluşturmayı içeren sonraki kısmına geçmeye hazırsınız.
> [!div class="nextstepaction"]
> [Model oluşturma](tutorial-classifying-iris-part-2.md)
