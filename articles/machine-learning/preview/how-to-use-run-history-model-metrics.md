---
title: "Çalışma kullanılacak geçmişi ve Azure modeli ölçümlerini çalışma ekranı makine nasıl | Microsoft Docs"
description: "Azure Machine Learning çalışma ekranı çalıştırma geçmişi ve Model ölçümleri özelliklerini kullanmaya yönelik kılavuz"
services: machine-learning
author: jopela
ms.author: jopela
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 920155b2e70409fe1c7c3569326cc5a1fa01b222
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-use-run-history-and-model-metrics-in-azure-machine-learning-workbench"></a>Kullanım çalıştırma geçmişi ve Azure Machine Learning çalışma ekranındaki modeli ölçümleri

Azure Machine Learning çalışma ekranı destekleyen veri bilimi deneme aracılığıyla kendi **çalıştırma geçmişi** ve **modeli ölçümleri** özellikleri.
**Çalıştırma geçmişi** , machine learning denemeleri çıkışları izlemek için bir yol sağlar ve ardından filtreleme ve sonuçları karşılaştırması sağlar.
**Model ölçümleri** ne olursa olsun, veri bilimi denemeler en önemli değerleridir izleme komut dosyalarınızın, herhangi bir noktasından kaydedilebilir.
Bu makalede, oranı ve veri bilimi deneme kalitesini artırmak için bu özellikleri etkili kullanımını sağlamak açıklar.

## <a name="prerequisites"></a>Ön koşullar
Nasıl yapılır bu kılavuzu adım için aktarmanız gerekir:
* [Oluşturun ve Azure Machine Learning yükleyin](quickstart-installation.md)
- [Proje oluşturma](quickstart-installation.md)


## <a name="azure-ml-logging-api-overview"></a>Azure ML günlüğüne API genel bakış
[Azure ML günlüğü API](reference-logging-api.md) aracılığıyla kullanıma **azureml.logging** (sağlayan Azure ML çalışma ekranı ile yüklenir.) Python modülünde Bu modül içeri aktardıktan sonra kullanabilirsiniz **get_azureml_logger** yöntemi örneği oluşturmak için bir **Günlükçü** nesnesi.
Günlükçü ait daha sonra kullanabileceğiniz **günlük** anahtar/değer çiftlerini depolamak için yöntemi, Python komut dosyaları tarafından üretilen.
Şu anda modeli ölçümleri skaler, sözlük, liste günlüğü, dataframe türleri gösterildiği gibi desteklenir.

```Python
# create a logger instance in already set up environment 
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# log scalar (any integer or floating point type is fine)
logger.log("simple value", 7)


# log list
logger.log("all values", [5, 6, 7])
```
Azure ML çalışma ekranı projelerinizi içinde Günlükçü kullanmak kolaydır ve bu makalede, bunun nasıl yapılacağı gösterilmektedir.

## <a name="create-a-project-in-azure-ml-workbench"></a>Proje içinde Azure ML çalışma ekranı oluşturma
Bir proje zaten sahip değilseniz, birinden oluşturabilirsiniz [oluşturma ve yükleme Quickstart](quickstart-installation.md) gelen **proje Panosu**, açabilirsiniz **iris_sklearn.py** (komut dosyası gösterildiği gibi.)

![kod dosyaları sekmesinden erişme](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-01b.png)

Azure ML günlüğü modeli ölçüm beklenen uygulama için bir kılavuz olarak bu komut dosyasını kullanabilirsiniz.

## <a name="parameterize-and-log-model-metrics-from-script"></a>Parametreleştirme ve komut dosyasından modeli ölçümleri oturum
İçinde **iris_sklearn.py** komut dosyası, beklenen düzeni alma ve yapı Python Günlükçü aşağıdaki kod satırlarını sınırlı.

```Python
from azureml.logging import get_azureml_logger
run_logger = get_azureml_logger()
```

Bir kez oluşturduktan sonra çağırabilirsiniz **günlük** herhangi bir yöntemle ad/değer çifti.

Geliştirme tamamlandığında, genellikle değerleri komut satırı üzerinden geçirilebilir böylece betikleri Parametreleştirme kullanışlıdır.
Aşağıdaki örnek, standart Python kitaplıkları kullanarak komut satırı parametrelerini (varsa) kabul etmek gösterilmiştir.
Bu komut dosyası, Regularization oranı tek bir parametre alır (*reg*) artırmak için bir sınıflandırma modeline uyacak şekilde kullanılan *doğruluğu* overfitting olmadan.
Bu değişkenler, olarak günlüğe kaydedilen *Regularization oranı* ve *doğruluğu* böylece en iyi sonuçlar modeliyle kolayca belirlenebilir.

```Python
# change regularization rate and you will likely get a different accuracy.
reg = 0.01
# load regularization rate from argument if present
if len(sys.argv) > 1:
    reg = float(sys.argv[1])

print("Regularization rate is {}".format(reg))

# log the regularization rate
run_logger.log("Regularization Rate", reg)

# train a logistic regression model on the training set
clf1 = LogisticRegression(C=1/reg).fit(X_train, Y_train)
print (clf1)

# evaluate the test set
accuracy = clf1.score(X_test, Y_test)
print ("Accuracy is {}".format(accuracy))

# log accuracy
run_logger.log("Accuracy", accuracy)
```

Bu adımları komut dosyalarınızı alma'yı etkinleştirmek en iyi kullanımı yapmalarına **çalıştırma geçmişi**.

## <a name="launch-runs-from-project-dashboard"></a>Proje panodan başlatma çalıştırır
Dönme **proje Panosu**, çalıştırmanızı sağlayan bir **izlenen Çalıştır** seçerek **iris_sklearn.py** komut dosyası ve girerek **regularization oranı**  parametresinde **bağımsız değişkenleri** düzenleme kutusu.

![parametreleri girerek ve çalıştırmalarını başlatma](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-05.png)

Azure ML çalışma ekranı izlenen çalıştırır başlatmasını engellemez olduğundan, birkaç paralel olarak başlatılabilir.
İzlenen her çalıştırmayı görünür durumda durumundadır **işleri paneli** gösterildiği gibi.

![İzleme işleri panelinde çalıştırır](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-06.png)

Bu, en iyi kaynak kullanımı seri olarak çalıştırmak için her bir iş gerektirmeden sağlar.

## <a name="view-results-in-run-history"></a>Görünüm sonuçlarında çalıştırma geçmişi
İlerleme durumu ve izlenen çalıştırmaları sonucu Azure ML çalışma ekranı'nin analiz için kullanılabilir **çalıştırma geçmişi**.
**Çalıştırma geçmişi** üç farklı görünümleri sağlar:
- Pano
- Ayrıntılar
- Karşılaştırma

**Pano** görünüm hem grafik ve tablo formlarında çizilir belirli bir komut dosyasının tüm çalışmaları arasında verileri görüntüler.
**Ayrıntıları** oturum ölçümleri ve çıktı dosyaları (işlenmiş gibi çizer.) dahil olmak üzere belirli bir komut dosyasının belirli bir çalıştırma oluşturulan tüm veri görünümü görüntüler **Karşılaştırma** görünümü görüntülenen yan yana olması için iki veya üç çalıştırır sonuçlarını etkinleştirir, ölçümleri günlüğe ve çıkış dosyaları da dahil olmak üzere.

Sekiz üzerinde çalıştırılan izlenen **iris_sklearn.py**, değerleri **regularization oranı** parametre ve **doğruluğu** sonuç nasıl Çalıştır kullanılacağını göstermek için oturum açmış Geçmiş görünümleri.

### <a name="run-history-dashboard"></a>Çalıştırma geçmişi Panosu
Tüm sekiz metinler sonuçlarını görünür **çalıştırma geçmişi Pano**.
Olarak **iris_sklearn.py** günlükleri *Regularization oranı* ve *doğruluğu*, **çalıştırma geçmişi Pano** tarafından bu değerleri grafiklerde görüntüler Varsayılan.

![Çalıştırma geçmişi Panosu](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-07.png)

**Çalıştırma geçmişi Pano** oturum değerleri de kılavuzunda görünmesini sağlayacak şekilde özelleştirilebilir.  Tıklatarak **özelleştirme** simgesi görüntüler **liste görünümü özelleştirme** iletişim kutusunda gösterildiği gibi.

![çalıştırma geçmişi Pano kılavuz özelleştirme](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-08.png)

İzlenen çalışmaları sırasında oturum herhangi bir değeri görüntülemek için kullanılabilir ve seçme **Regularization oranı** ve **doğruluğu** kılavuza ekler.

![özelleştirilmiş kılavuzunda günlüğe kaydedilen değerler](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-09.png)

İlginç çalıştırır noktalarının grafiklerinde üzerine gelerek bulmak kolaydır.  Bu durumda, 7 Run düşük süre ile birlikte iyi bir doğruluk veriyor.

![bir çalıştırma ilginç bulma](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-10.png)

Herhangi bir grafiğin veya kılavuz görüntüler 7 Run bağlantıdaki 7 Run ile ilişkili bir noktayı tıklatma **çalıştırma geçmişi ayrıntıları**.

### <a name="run-history-details"></a>Geçmiş ayrıntıları çalıştırın
Bu görünümden 7 Run tarafından üretilen herhangi bir yapı ile birlikte çalıştırın 7'in tam sonuçları görüntülenir.

![Geçmiş ayrıntıları çalıştırın](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-11.png)

**Çalıştırma geçmişi ayrıntıları** görünümü de yeteneği sağlar **karşıdan** yazılan herhangi bir dosya **. / çıkarır** (Bu dosyaları yedeklenen Azure ML çalışma ekranı tarafından 's klasörü Bulut için depolama alanı başka bir makaleye konu çalıştırma geçmişi.)

Son olarak, **çalıştırma geçmişi ayrıntıları** bu çalışma zamanında durumuna projenizi geri yüklemek için bir yol sağlar.
Tıklatarak **geri** düğmesi gösterildiği gibi onay iletişim kutusunu görüntüler.

![çalıştırma geri yüklemeyi Onayla](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-13.png)

Onaylanan, dosyaların üzerine yazılabilir veya kaldırılır, böylece bu özellik dikkatli kullanın.

### <a name="run-history-comparison"></a>Çalıştırma geçmişi karşılaştırması
İki veya üç seçerek çalışan **çalıştırma geçmişi Pano** tıklatıp **karşılaştırmak** getirir **çalıştırma geçmişi karşılaştırma** görünümü.
Alternatif olarak, tıklatarak **karşılaştırmak** ve Çalıştır içinde seçerek **çalıştırma geçmişi ayrıntıları** görünümü de getirir, **çalıştırma geçmişi karşılaştırma** görünümü.
Her iki durumda da **çalıştırma geçmişi karşılaştırma** görünümü, iki veya üç çalıştırılan yan yana yapıları ve günlüğe kaydedilen sonuçları görmek için bir yol sağlar.

![çalıştırma geçmişi karşılaştırması](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-12.png)

Bu görünüm çizimler bir karşılaştırması için özellikle yararlı olur, ancak genel olarak, tüm özelliklerini çalıştırır burada karşılaştırılabilir.

### <a name="command-line-interface"></a>Command Line Interface
Azure Machine Learning çalışma ekranı de erişim sağlar çalıştırmak geçmişinde için kendi **komut satırı arabirimi**.
Erişim için **komut satırı arabirimi**, tıklatın **komut istemini açın** gösterildiği gibi menüsü.

![komut istemi açın](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-14.png)

Çalıştırma geçmişi için kullanılabilen komutları aracılığıyla erişilen `az ml history`, çevrimiçi Yardım ekleyerek kullanılabilir `-h` bayrağı.
```
$ az ml history -h

Group
    az ml history: View run history of machine learning experiments.

Commands:
    compare : Compare two runs.
    download: Download all the artifacts from a run into the destination path.
    info    : Details about one run.
    last    : Get detail about most recent run.
    list    : List runs.
    promote : Promote Artifacts.
```
Bu komutlar aynı özellikler sağlamak ve dönüş gösterilen aynı verileri **çalıştırma geçmişi görünümleri**.
Örneğin, son çalışmanın sonuçlarını bir JSON nesnesi olarak görüntülenebilir.
```
$ az ml history last
{
  "Accuracy": 0.6792452830188679,
  "Regularization Rate": 0.078125,
  "attachments": "control_log, control_log.txt, driver_log, driver_log.txt, pid.txt, dataprep/backgroundProcess.log, dataprep/backgroundProcess_Engine.log, dataprep/backgroundProcess_EngineHost.log, dataprep/backgroundProcess_ProjectProvider.log, dataprep/backgroundProcess_Sampling.log, dataprep/backgroundProcess_Telemetry.log, outputs/cm.png, outputs/model.pkl, outputs/sdk_debug.txt, outputs/roc.png",
  "created_utc": "2017-09-08T00:58:01.611105+00:00",
  "description": null,
  "duration": "0:00:04.892389",
  "end_time_utc": "2017-09-08T00:58:07.951403+00:00",
  "experiment_id": "ce92d0a9-3e2c-4d51-85de-93ef22249ce1",
  "heartbeat_enabled": true,
  "hidden": false,
  "name": null,
  "parent_run_id": null,
  "properties": {
    "CommitId": "e77a5f0df2af1a482bbe39b70bfbb16b62228cb3"
  },
  "run_id": "IrisDemo_1504832281116",
  "run_number": 8,
  "script_name": "iris_sklearn.py",
  "start_time_utc": "2017-09-08T00:58:03.059014+00:00",
  "status": "Completed",
  "target": "local",
  "user_id": "e9fafe06-b0e4-4154-8374-aae34f9977b2"
}
```
Ayrıca, tüm metinler listesi sekmeli biçimde görüntülenebilir.
```
$ az ml history list -o table
  Accuracy    Regularization Rate  Duration        Run_id                  Script_name      Start_time_utc                    Status
----------  ---------------------  --------------  ----------------------  ---------------  --------------------------------  ---------
  0.679245               0.078125  0:00:04.892389  IrisDemo_1504832281116  iris_sklearn.py  2017-09-08T00:58:03.059014+00:00  Completed
  0.679245               0.15625   0:00:04.734956  IrisDemo_1504832255198  iris_sklearn.py  2017-09-08T00:57:38.507196+00:00  Completed
  0.660377               0.3125    0:00:04.913762  IrisDemo_1504832234904  iris_sklearn.py  2017-09-08T00:57:16.849881+00:00  Completed
  0.660377               0.625     0:00:06.107764  IrisDemo_1504832210767  iris_sklearn.py  2017-09-08T00:56:54.602813+00:00  Completed
  0.641509               1.25      0:00:04.742727  IrisDemo_1504832171373  iris_sklearn.py  2017-09-08T00:56:13.879280+00:00  Completed
  0.660377               2.5       0:00:04.915401  IrisDemo_1504832148526  iris_sklearn.py  2017-09-08T00:55:52.937083+00:00  Completed
  0.641509               5         0:00:04.730627  IrisDemo_1504832127172  iris_sklearn.py  2017-09-08T00:55:29.612382+00:00  Completed
  0.641509              10         0:00:06.059082  IrisDemo_1504832109906  iris_sklearn.py  2017-09-08T00:55:14.739806+00:00  Completed

```
**Komut satırı arabirimi** Azure Machine Learning çalışma ekranı gücünü erişmek için alternatif bir yoludur.

## <a name="next-steps"></a>Sonraki Adımlar
Bu özellikler veri bilimi deneme işlemine yardımcı olmak kullanılabilir.
Kullanışlı olması için bulmalarını ve büyük ölçüde görüşlerinize değer veriyoruz umuyoruz.
Bu yalnızca ilk bizim uygulamaya ve planlanan geliştirmeleri büyük bir bölümünü sunuyoruz.
Sürekli olarak bunları Azure Machine Learning çalışma ekranına teslim etmek için umuyoruz. 
