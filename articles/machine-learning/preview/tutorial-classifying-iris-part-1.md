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
ms.date: 09/25/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e88b6bf74b7492353e5d5d004bde12fa4787e405
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---

# <a name="classifying-iris-part-1-prepare-data"></a>Iris sınıflandırma bölüm 1: Verileri hazırlama
Azure Machine Learning hizmetleri (önizleme) uzman veri bilimcilerinin bulut ölçeğinde veri hazırlamasını, deney geliştirmesini ve model dağıtmasını sağlayan tümleşik, uçtan uca ve genişmiş analiz çözümüdür.

Bu öğretici üç bölümden oluşan bir serinin birinci bölümüdür. Bu öğreticide, Azure Machine Learning hizmetleri (önizleme) hakkındaki temel bilgileri adım adım inceleyeceğiz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Machine Learning Workbench’te proje oluşturma
> * Veri hazırlama paketi oluşturma
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Bu öğreticide kolaylık sağlamak için değişmeyen [Iris çiçeği veri kümesi](https://en.wikipedia.org/wiki/Iris_flower_data_set) kullanılmıştır. Ekran görüntüleri Windows'a özgüdür ancak macOS deneyimi de çok benzerdir.

## <a name="launch-azure-machine-learning-workbench"></a>Azure Machine Learning Workbench’i başlatma
Komut satırı arabirimini (CLI) de içeren Azure Machine Learning Workbench uygulamasını yüklemek için [Yükleme ve Oluşturma Hızlı Başlangıcı](quickstart-installation.md)’nı izleyin. Azure Machine Learning Workbench uygulamasını başlatın ve gerekirse oturum açın.

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma
1. **PROJELER** bölmesinde **+** simgesine tıklayarak bir **Yeni Proje** oluşturun.

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
1. **Dosya Görünümü**’nden `iris.csv` dosyasını açın. Dosya 5 sütun ve 150 satırdan oluşan basit bir tablodur. Dört sayısal özellik sütunu ve bir dize hedef sütunu vardır. Sütun üst bilgileri yoktur.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   >Not: Özellikle dosya boyutu büyük olduğunda proje klasörünüze veri dosyaları eklemeniz önerilmez. Bu şablona `iris.csv` dosyasını çok küçük olduğu için gösterim amacıyla ekliyoruz. Daha fazla bilgi için lütfen [Büyük veri dosyalarını okuma ve yazma](how-to-read-write-files.md) makalesine bakın.

2. Yeni bir veri kaynağı eklemek için **Veri Görünümü**’nde **+** simgesine tıklayın. **Veri Kaynağı Ekleme** sihirbazı başlatılır. 

   ![veri görünümü](media/tutorial-classifying-iris/data_view.png)

3. **Dosyalar/Dizin** seçeneğini belirleyip `iris.csv` yerel dosyasını seçin. Her ekran için varsayılan ayarları kabul edin ve son olarak **Son** öğesine tıklayın. 

   ![iris seçme](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Bu alıştırma için geçerli proje dizininden `iris.csv` dosyasını seçtiğinizden emin olun; aksi takdirde sonraki adımlar başarısız olabilir. 

4. Yeni `iris-1.dsource` dosyası oluşturulur. Örnek proje zaten numaralandırılmamış bir `iris.dsource` dosyası ile birlikte geldiğinden dosya bir tire ile benzersiz şekilde adlandırılmıştır.  Dosya açılır ve veriler gösterilir. `Column1` ile `Column5` arasında bir dizi sütun bilgisi bu veri kümesine otomatik olarak eklenir. Alt kısma kaydırın ve veri kümesinin son satırının boş olduğuna dikkat edin. Bunun nedeni, csv dosyasında fazladan bir satır sonu olmasıdır.

   ![iris veri görünümü](media/tutorial-classifying-iris/iris_data_view.png)

5. Şimdi **Ölçümler** düğmesine tıklayın. Her sütundaki histogramları ve sizin için hesaplanan istatistikleri inceleyin. Ayrıca **Veri** düğmesine tıklayarak verileri yeniden görebilirsiniz. 

   ![iris veri görünümü](media/tutorial-classifying-iris/iris_metrics_view.png)

6. **Ölçümler** düğmesinin yanındaki **Hazırla** düğmesine (veya ölçüm görünümündeyseniz **Veri** düğmesine) tıklayın. **Hazırla** iletişim kutusu açılır. Örnek proje zaten bir `iris.dprep` dosyası ile birlikte gelir; bu nedenle, varsayılan olarak o mevcut **iris.dprep** veri hazırlama paketinde yeni bir veri akışı oluşturmanızı ister. Açılan değeri **+Yeni Veri Hazırlık Paketi** olarak değiştirin ve yeni "iris-1" değerini girip **Tamam**’a tıklayın.

   ![iris veri görünümü](media/tutorial-classifying-iris/new_dprep.png)

Veri hazırlama düzenleyicisinde `iris-1.dprep` adlı yeni bir veri hazırlama paketi oluşturulup açılır.

Şimdi bazı basit veri hazırlıkları yapalım. Her bir sütun başlığına tıklayarak sütunları yeniden adlandırın ve üst bilgi metnini düzenlenebilir hale getirin. Beş sütun için sırasıyla `Sepal Length`, `Sepal Width`, `Petal Length`, `Petal Width` ve `Species` değerlerini girin.

![sütunları yeniden adlandırma](media/tutorial-classifying-iris/rename_column.png)

`Species` sütununu seçip sağ tıklayın. **Değer Sayıları**’nı seçin. 

![değer sayısı](media/tutorial-classifying-iris/value_count.png)

Bu işlem dört çubuku bir histogram oluşturur. Hedef sütunumuzun `Iris_virginica`, `Iris_versicolor`, `Iris-setosa` şeklinde üç farklı değere sahip olduğuna dikkat edin. Ayrıca değeri `(null)` olan bir satır da mevcuttur. Null değerini temsil eden çubuğu seçerek bu satırdan kurtulun ve kaldırmak için **-** filtre düğmesine tıklayın. 

![değer sayısı](media/tutorial-classifying-iris/filter_out.png)

Null değer satırını yeniden adlandırıp filtreleyerek sütun üzerinde çalıştığınız için, yaptığınız her işlem **STEPS** bölmesinde bir veri hazırlama adımı olarak kaydedilir. Bunları düzenleyebilir (ayarlarını düzenlemek için), yeniden sıralayabilir ve hatta kaldırabilirsiniz.

![adımlar](media/tutorial-classifying-iris/steps.png)

## <a name="generate-pythonpyspark-code-to-invoke-data-prep-package"></a>Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Şimdi DataPrep düzenleyicisini kapatın. (Endişelenmeyin, otomatik olarak kaydedilir.) **iris-1.dprep** dosyasına sağ tıklayarak açılır menüyü getirin ve **Veri Erişim Kodu Dosyası Oluştur**’u seçin. 

![kod oluştur](media/tutorial-classifying-iris/generate_code.png)

Sonraki iki kod satırı önceden doldurulmuş halde bir **iris-1.py** dosyası oluşturulur (bazı yorumlarla birlikte):

```python
# This code snippet will load the referenced package and return a DataFrame.
# If the code is run in a PySpark environment, the code will return a
# Spark DataFrame. If not, the code will return a Pandas DataFrame.

from azureml.dataprep.package import run
df = run('iris.dprep', dataflow_idx=0)
```
Bu kod parçacığı bir veri hazırlama paketi olarak yeni oluşturduğunuz mantığı çağırır. Bu kodun çalıştırıldığı bağlama göre `df`, Python çalışma zamanında yürütülüyorsa bir [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) ya da bir Spark bağlamında yürütülüyorsa [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) olabilir. Azure Machine Learning Workbench uygulamasında veri hazırlama hakkında daha fazla bilgi için [Veri Hazırlamaya Başlama](data-prep-getting-started.md) kılavuzuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
Üç bölümden oluşan öğretici serisinin bu ilk bölümünde Azure Machine Learning Workbench uygulamasını kullanarak şunları yaptınız:
> [!div class="checklist"]
> * Yeni bir proje oluşturma 
> * Veri hazırlama paketi oluşturma
> * Veri hazırlama paketini çağırmak için Python/PySpark kodu oluşturma

Serinin bir machine learning modeli oluşturmayı içeren sonraki kısmına geçmeye hazırsınız.
> [!div class="nextstepaction"]
> [Model oluşturma](tutorial-classifying-iris-part-2.md)

