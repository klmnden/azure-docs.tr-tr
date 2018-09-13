---
title: Çalıştırma kullanılacak geçmişini ve Model ölçümlerini azure'da Workbench Machine Learning nasıl | Microsoft Docs
description: Azure Machine Learning Workbench'i çalıştırma geçmişi ve Model ölçümlerini özelliklerini kullanma Kılavuzu
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 34fe72087a3de133d65ea4a4737ab5dba45242f4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647280"
---
# <a name="how-to-use-run-history-and-model-metrics-in-azure-machine-learning-workbench"></a>Çalıştırma geçmişini kullanın ve Azure Machine Learning workbench'te modeli ölçümleri

Azure Machine Learning Workbench destekleyen veri bilimi deneme ile kendi **çalıştırma geçmişi** ve **Model ölçümlerini** özellikleri.
**Çalıştırma geçmişi** makine öğrenimi denemelerini çıkışlarına izlemek için bir yol sağlar ve sonra da filtreleme ve sonuçları karşılaştırma sağlar.
**Model ölçümlerini** hangi veri bilimi denemelerinizi en önemli değerleridir izleme betikleriniz, herhangi bir noktasından kaydedilebilir.
Bu makalede, etkili kullanan oranı ve, veri bilimi deneme kalitesini artırmak için bu özellikleri açıklar.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için için gerekir:
* [Oluşturma ve Azure Machine Learning'i yükleme](../service/quickstart-installation.md)
- [Proje oluşturma](../service/quickstart-installation.md)


## <a name="azure-ml-logging-api-overview"></a>Azure ML günlük API'sine genel bakış
[Azure ML API günlüğü](reference-logging-api.md) aracılığıyla kullanılabilir **azureml.logging** (Bu Azure ML Workbench'te yüklüdür.) bir Python Modülü Bu modül içeri aktardıktan sonra kullanabileceğiniz **get_azureml_logger** örneklemek için yöntemi bir **Günlükçü** nesne.
Günlükçü'nın daha sonra kullanabileceğiniz **günlük** anahtar/değer çiftlerini depolamak için yöntemi, Python betikleri tarafından üretilen.
Gösterildiği gibi şu anda, skaler günlük model ölçümlerini ve liste türleri desteklenir.

```Python
# create a logger instance in already set up environment 
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# log scalar (any integer or floating point type is fine)
logger.log("simple value", 7)


# log list
logger.log("all values", [5, 6, 7])
```
Azure ML Workbench projeleriniz içinde Günlükçü kullanmak daha kolaydır ve bu makalede bunun nasıl yapılacağı gösterilmektedir.

## <a name="create-a-project-in-azure-ml-workbench"></a>Azure ML Workbench'te proje oluşturma
Bir proje zaten yoksa, bir oluşturabilirsiniz [oluşturma ve yükleme Hızlı Başlangıç](../service/quickstart-installation.md) gelen **proje Panosu**, açabileceğiniz **iris_sklearn.py** (komut dosyası gösterildiği gibi.)

![kod dosyaları sekmesinden erişme](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-01b.png)

Bu betik, Azure ML üzerinde oturum modeli ölçüm beklenen uygulaması için bir kılavuz olarak kullanabilirsiniz.

## <a name="parameterize-and-log-model-metrics-from-script"></a>Parametreleştirme ve Model ölçümlerini Betiğinden oturum
İçinde **iris_sklearn.py** komut dosyası, beklenen desene alma ve yapı python'da Günlükçü aşağıdaki kod satırlarını sınırlı.

```Python
from azureml.logging import get_azureml_logger
run_logger = get_azureml_logger()
```

Oluşturulduktan sonra çağırabilirsiniz **günlük** herhangi bir yöntemle ad/değer çifti.

Geliştirme tamamlandığında, genellikle betikleri böylece değerleri komut satırı geçirilebilir parametreleştirin kullanışlıdır.
Aşağıdaki örnek, standart Python kitaplıkları kullanarak komut satırı parametrelerini (varsa) kabul etmek nasıl gösterir.
Bu betik, tek bir parametre için normalleştirme oranını alır (*reg*) artırmak için bir sınıflandırma modeline uyacak şekilde kullanılan *doğruluğu* overfitting olmadan.
Bu değişkenler olarak kaydedilir *normalleştirme oranını* ve *doğruluğu* böylece en iyi sonuçlar modeliyle bir kolayca belirlenebilir.

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

Betiğinizde bu adımların atılmasından, etkinleştirme en iyi kullanımı yapmalarına **çalıştırma geçmişi**.

## <a name="launch-runs-from-project-dashboard"></a>Proje panodan başlatma çalıştırır
Dönme **proje Panosu**, başlatabilirsiniz bir **izlenen Çalıştır** seçerek **iris_sklearn.py** betik ve girerek **normalleştirme oranını**  parametresinde **bağımsız değişkenleri** düzenleme kutusu.

![parametreleri girip çalıştırma başlatılıyor](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-05.png)

Azure ML Workbench izlenen çalışmaları başlatma engellemez olduğundan, birkaç paralel olarak başlatılabilir.
İzlenen her bir çalıştırmanın durumunu görülebilir **işler panelindeki** gösterildiği gibi.

![işler panelindeki İzleme çalıştırır](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-06.png)

Bu, en iyi kaynak kullanımını seri olarak çalıştırmak için her bir iş gerek kalmadan sağlar.

## <a name="view-results-in-run-history"></a>Çalıştırma geçmişi sonuçlarını görüntüle
İlerleme durumu ve izlenen çalıştırma sonuçlarını analiz Azure ML workbench'te'nın **çalıştırma geçmişi**.
**Çalıştırma geçmişi** üç farklı görünümleri sağlar:
- Pano
- Ayrıntılar
- Karşılaştırma

**Pano** görünümü, tüm çalıştırmalar hem grafik ve tablo formlarında işlenen, belirli bir betiğin arasında verileri görüntüler.
**Ayrıntıları** oturum ölçümleri ve çıkış dosyaları (işlenmiş gibi çizer.) dahil olmak üzere, belirli bir betiğin belirli bir çalıştırma işleminden oluşturulan tüm verileri görüntüleyen bir görünüm **Karşılaştırma** görünümü olarak görüntülenen yan yana iki veya üç çalıştırma sonuçlarını etkinleştirir, günlüğe kaydedilmiş metrikleri ve çıkış dosyaları da dahil olmak üzere.

Sekiz üzerinde çalıştırılan izlenen **iris_sklearn.py**, değerleri **normalleştirme oranını** parametresi ve **doğruluğu** sonucu günlüğe Çalıştır'ı kullanmayı göstermek için Geçmiş görüntüleme.

### <a name="run-history-dashboard"></a>Çalıştırma geçmişi Panosu
Sekiz tüm çalışmaların sonuçlarını görülebilir **çalıştırma geçmişi Panosu**.
Olarak **iris_sklearn.py** günlükleri *normalleştirme oranını* ve *doğruluğu*, **çalıştırma geçmişi Panosu** bu değerlere göre grafiklerde görüntüler Varsayılan.

![çalıştırma geçmişi Panosu](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-07.png)

**Çalıştırma geçmişi Panosu** günlüğe kaydedilen değerleri de kılavuzda görünmesini sağlayacak şekilde özelleştirilebilir.  Tıklayarak **özelleştirme** simgesi görüntüler **liste görünümü özelleştirme** iletişim kutusunda gösterildiği gibi.

![çalıştırma geçmişi Panosu kılavuz özelleştirme](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-08.png)

İzlenen çalışmaları sırasında günlüğe kaydedilen tüm değerleri görüntülemek için kullanılabilir ve seçme **normalleştirme oranını** ve **doğruluğu** bunları kılavuza ekler.

![özelleştirilmiş kılavuzunda günlüğe kaydedilen değerleri](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-09.png)

Grafiklerde noktaları üzerine gelerek ilginç çalıştırmaları bulmak daha kolaydır.  Bu durumda, 7'yi çalıştırın, düşük bir süre ile birlikte iyi bir doğruluk veriyor.

![bir çalıştırma ilginç bulma](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-10.png)

Herhangi bir grafiğe veya kılavuz görüntüler 7 çalıştırmak için bağlantıyı 7 çalıştırması ile ilişkili bir noktaya tıklayarak **çalıştırma geçmişi ayrıntıları**.

### <a name="run-history-details"></a>Çalıştırma geçmişi ayrıntıları
Bu görünümde, 7 çalıştırma tarafından oluşturulan yapıtların yanı sıra 7 Run tam sonuçları görüntülenir.

![çalıştırma geçmişi ayrıntıları](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-11.png)

**Çalıştırma geçmişi ayrıntıları** görünümü de yeteneği sağlar **indirme** yazılan tüm dosyaları **. / çıkışlar** (Bu dosyalar desteklenen Azure ML Workbench tarafından 's klasörü başka bir makalenin konusu çalıştırma geçmişi için bulut depolama.)

Son olarak, **çalıştırma geçmişi ayrıntıları** projenizi geri yükleme olanağı, bu çalışma zamanında durumuna sağlar.
Tıklayarak **geri** düğmesi gösterildiği gibi onay iletişim kutusunu görüntüler.

![çalıştırma geri yüklemeyi Onayla](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-13.png)

Onaylandı, dosyaların üzerine yazılabilir veya kaldırıldıysa, bu nedenle bu özelliği dikkatle kullanın.

### <a name="run-history-comparison"></a>Çalıştırma geçmişi karşılaştırması
İki veya üç seçerek çalışan **çalıştırma geçmişi Panosu** tıklayıp **karşılaştırma** getirir **çalıştırma geçmişi karşılaştırma** görünümü.
Alternatif olarak, tıklayarak **karşılaştırma** ve içinde bir Farklı Çalıştır'ı seçerek **çalıştırma geçmişi ayrıntıları** görünümü de getirir, **çalıştırma geçmişi karşılaştırma** görünümü.
Her iki durumda da **çalıştırma geçmişi karşılaştırma** görünümü, iki veya üç çalıştırma yan yana yapıtları ve günlüğe kaydedilen sonuçları görmek için bir yol sağlar.

![çalıştırma geçmişi karşılaştırması](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-12.png)

Bu çizimler bir karşılaştırması için özellikle yararlı bir görünümdür ancak genel olarak, tüm çalıştırmalar özelliklerini burada karşılaştırılabilir.

### <a name="command-line-interface"></a>Command Line Interface
Azure Machine Learning Workbench de erişim sağlar geçmişinde çalıştırmak için kendi **komut satırı arabirimi**.
Erişim için **komut satırı arabirimi**, tıklayın **komut istemini Aç** gösterildiği menüsü.

![Komut istemini açın](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-14.png)

Çalıştırma geçmişi için kullanılabilir komutları aracılığıyla erişilir `az ml history`, çevrimiçi Yardım ekleyerek kullanılabilir `-h` bayrağı.
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
Bu komutlar aynı özellikleri sağlar ve dönüş gösterilen aynı verileri **çalıştırma geçmişi görünümleri**.
Örneğin, son çalıştırılmasının sonuçlarını bir JSON nesnesi olarak görüntülenebilir.
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
Ayrıca, tüm çalıştırmaların listesi, tablo biçiminde görüntülenebilir.
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
**Komut satırı arabirimi** Azure Machine Learning Workbench gücünü erişmek için alternatif bir yoludur.

## <a name="next-steps"></a>Sonraki Adımlar
Bu özellikler, veri bilimi deneme işlemine yardımcı olmak kullanılabilir.
Bunları kullanışlı olması için bulmak ve büyük ölçüde geri bildirim göndermenizi rica umuyoruz.
Bu yalnızca başlangıç kararlılığımızın ve planlı geliştirmeleri büyük ölçüde sahibiz.
Bunları Azure Machine Learning Workbench için sürekli olarak sunmak için hazırız. 
