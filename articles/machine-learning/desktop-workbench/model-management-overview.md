---
title: Azure Machine Learning Model Yönetimi kavramsal genel bakış | Microsoft Docs
description: Bu belge için Azure Machine Learning Model yönetimi kavramlarını açıklar.
services: machine-learning
author: hjerezmsft
ms.author: hjerez
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ROBOTS: NOINDEX
ms.openlocfilehash: d3f7e206e7f4aa61a8ec1272ff2670d81bb7a33e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974689"
---
# <a name="azure-machine-learning-model-management"></a>Azure Machine Learning Model Yönetimi

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Azure Machine Learning Model yönetimi, yönetmek ve makine öğrenimi iş akışları ve modelleri dağıtmak sağlar. 

Model yönetimi için özellikleri sağlar:
- Model sürüm oluşturma
- Modelleri üretimde izleme
- Modelleri üretime AzureML işlem ortamıyla aracılığıyla dağıtma [Azure Container Service](https://azure.microsoft.com/services/container-service/) ve [Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Docker kapsayıcıları ile modelleri oluşturma ve yerel olarak test etme
- Otomatik modeli yeniden eğitme
- Eyleme dönüştürülebilir Öngörüler için model telemetri yakalama. 

Azure Machine Learning Model yönetimi, bir kayıt defteri model sürümleri sağlar. Ayrıca paketleme ve REST API'leri olarak makine öğrenimi kapsayıcıları dağıtma için otomatik iş akışları sağlar. Modeller ve çalışma zamanı bağımlılıklarını Linux tabanlı bir Docker kapsayıcısında tahmin API ile paketlenmiştir. 

Azure Machine Learning işlem ortamları ayarlama ve modellerini barındırmak için ölçeklenebilir kümelerini yönetmek için yardımcı olur. İşlem ortamı, Azure Container Service üzerinde temel alır. Azure Container Services REST API uç noktaları aşağıdaki özelliklere sahip olarak Machine Learning API'leri otomatik riskini sağlar:

- Kimlik Doğrulaması
- Yük dengeleme
- Otomatik ölçeklendirme
- Şifreleme

Azure Machine Learning Model Yönetimi CLI, API ve Azure portalı üzerinden bu özellikleri sağlar. 

Azure Machine Learning model Yönetimi aşağıdaki bilgiler kullanılmaktadır:

 - Model dosya veya dizin ile model dosyaları
 - Kullanıcı bir model Puanlama işlevi uygulama Python dosyası oluşturuldu
 - Çalışma zamanı bağımlılıklarını listeleme Conda bağımlılık dosyası
 - Çalışma zamanı ortamı seçeneği ve 
 - API parametreler için şema dosyası 

Bu bilgiler aşağıdaki eylemleri gerçekleştirirken kullanılır:

- Model kaydediliyor
- Bir kapsayıcı oluştururken bir bildirim oluşturma kullanılır
- Bir Docker kapsayıcı görüntüsü oluşturma
- Azure Container Service için bir kapsayıcı dağıtma
 
Kümeye dağıtılan modelleri nasıl kayıtlı ve bir genel bakış aşağıdaki şekilde gösterilmiştir. 

![](media/model-management-overview/modelmanagement.png)

## <a name="create-and-manage-models"></a>Modelleri oluşturmak ve yönetmek 
Model sürümleri üretimde izleme için modeller Azure Machine Learning Model yönetimi ile kaydedebilirsiniz. İçin yeniden üretilebilirliğini ve yönetim kolaylığı, hizmetin tüm bağımlılıkları ve ilişkili bilgileri yakalar. Performans, daha ayrıntılı bilgiler için sağlanan SDK'sını kullanarak model telemetri yakalayabilirsiniz. Kullanıcı tarafından sağlanan depolama modeli telemetri arşivlenir. Model telemetri, daha sonra model performansını analiz etme, yeniden eğitme ve işletmeniz için öngörü için de kullanılabilir.

## <a name="create-and-manage-manifests"></a>Oluşturma ve bildirimleri yönetme 
Modelleri üretime dağıtmak için ek yapıtları gerektirir. Sistem modeli, bağımlılıkları, çıkarım betik (diğer adıyla Puanlama betik), örnek verileri, şema vb. kapsayan bir bildirim oluşturma olanağı sağlar. Bu bildirimi bir Docker kapsayıcı görüntüsü oluşturmak için tarif görev yapar. Kuruluşların bildirimi otomatik olarak oluşturmak, farklı sürümlerini oluşturun ve bunların bildirimlerini yönetme. 

## <a name="create-and-manage-docker-container-images"></a>Oluşturma ve Docker kapsayıcı görüntülerini yönetin 
Önceki adımdan gelen bildirim ilgili ortamlarındaki Docker tabanlı kapsayıcı görüntülerini oluşturmak için kullanabilirsiniz. Kapsayıcılı, Docker tabanlı görüntüleri kuruluşların bu görüntüler aşağıdaki işlem ortamlarını üzerinde çalıştırma esnekliğini sağlar:

- [Azure Container Service Kubernetes tabanlı](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Şirket içi kapsayıcı Hizmetleri
- Geliştirme ortamları
- IoT cihazları

Bu Docker tabanlı kapsayıcı görüntüleri Öngörüler oluşturmak için gereken tüm gerekli bağımlılıkları olan diğerlerinden bağımsızdır. 

## <a name="deploy-docker-container-images"></a>Docker kapsayıcı görüntüleri dağıtma 
Azure Machine Learning Model yönetimi ile tek bir komutla Docker tabanlı kapsayıcı görüntüleri, ML işlem ortamı tarafından yönetilen Azure Container Service'e dağıtabilirsiniz. Bu dağıtımları, aşağıdaki özellikleri sağlayan bir ön uç sunucusu ile oluşturulur:

- Uygun ölçekte düşük gecikme süresi Öngörüler
- Yük dengeleme
- ML uç noktaları otomatik olarak ölçeklendirme
- API anahtarı kimlik doğrulama
- API swagger belgesi

Dağıtım ölçek ve telemetri aracılığıyla aşağıdaki yapılandırma ayarları kontrol edebilirsiniz:

- Sistem günlük kaydı ve her web hizmet düzeyi için model telemetri. Etkinleştirilirse, tüm stdout günlüklerini akışla [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Model telemetri, sağladığınız depolama arşivlenir. 
- Otomatik ölçeklendirme ve eşzamanlılık sınırları. Bu ayarlar otomatik olarak dağıtılan kapsayıcılar mevcut küme içindeki yük sayısını artırın. Aktarım hızı ve tutarlılık tahmin gecikme de denetler.

## <a name="consumption"></a>Tüketim 
Azure Machine Learning Model Yönetimi REST API'si için swagger belgesinin birlikte dağıtılan modeli oluşturur. API ile REST API anahtarı ve satır iş kolu uygulamaları bir parçası olarak tahminler elde etmek alınacağı girişleri model arama tarafından dağıtılan modellerinde kullanabilir. Örnek kod diller için Java, Github'da kullanılabilir [Python](https://github.com/CortanaAnalyticsGallery-Int/digit-recognition-cnn-tf/blob/master/client.py)ve REST API'leri çağırmak için C#. Azure Machine Learning Model Yönetimi CLI bu REST API'leri ile çalışmak için kolay bir yol sağlar. Tek bir CLI komutu, bir swagger özellikli uygulamalar veya curl kullanarak API'leri kullanabilir. 

## <a name="retraining"></a>Yeniden eğitme 
Azure Machine Learning Model yönetimi, modelleri yeniden eğitme için kullanabileceğiniz API'ler sağlar. API'leri, model güncelleştirilmiş sürümlerini güncelleştirme mevcut dağıtımları için de kullanabilirsiniz. Veri bilimi iş akışının bir parçası olarak, modeli deneme ortamınıza yeniden. Ardından, modelin Model yönetimi ile kaydolmak ve güncelleştirme mevcut dağıtımları. Güncelleştirmeleri, tek bir güncelleştirme CLI komutu kullanarak gerçekleştirilir. Güncelleştirme komut API URL'si veya anahtar değişikliği yapmadan mevcut dağıtımları güncelleştirir. Modeli kullanan uygulamalar herhangi bir kod değişikliği çalışabilir ve yeni modeli kullanarak daha iyi tahmin başlanması devam.

Bu kavramları açıklayan bir tam iş akışı, aşağıdaki şekilde kapsanır.

![](media/model-management-overview/modelmanagementworkflow.png)

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS) 
- **Hangi veri türleri desteklenir? NumPy dizileri doğrudan web hizmeti giriş olarak geçirebilirsiniz?**

   Ardından generate_schema SDK kullanılarak oluşturulmuş bir şema dosyası sağlıyorsanız NumPy ve/veya Pandas DF geçirebilirsiniz. Herhangi bir JSON seri hale getirilebilir girişleri de geçirebilirsiniz. Görüntü ikili kodlanmış dize olarak de geçirebilirsiniz.

- **Web hizmetini birden fazla giriş veya desteklemez farklı girdiler ayrıştırma?**

   Evet, birden çok girdiyi bir JSON istekteki bir sözlük olarak paketlenmiş alabilir. Her bir giriş tek benzersiz sözlük anahtarına karşılık gelir.

- **Olan bir web isteğine etkinleştiren çağrı hizmet engelleme çağrısının veya zaman uyumsuz bir çağrı?**

   Hizmet API ve CLI bir parçası olarak gerçek zamanlı seçeneği kullanılarak oluşturulmuş, ardından engelleme/zaman uyumlu çağrıyı olur. Gerçek zamanlı hızlı olması beklenir. Bunu önlemek için zaman uyumsuz HTTP kitaplığını kullanarak çağırabilirsiniz istemci tarafında istemci iş parçacığı engelleme rağmen.

- **Web hizmeti kaç istek eşzamanlı olarak işleyebilirsiniz?**

   Bu, küme ve web hizmeti ölçeğini bağlıdır. 100 kat çoğaltmaların, hizmetinizin genişletebilir ve ardından onu çok sayıda istek eşzamanlı olarak işleyebilirsiniz. Hizmet verimliliğini artırmak için en fazla eş zamanlı istek başına çoğaltma da yapılandırabilirsiniz.

- **Kaç istek web hizmeti kuyrukta?**

   Yapılandırılabilir. Varsayılan olarak, tek bir çoğaltma başına yaklaşık 10 ayarlanmış, ancak, artış /, uygulama gereksinimlerinize azaltabilir. Genellikle bunu artırmak sıralanmış isteklerin sayısı hizmet verimliliğini artırır ancak gecikmeleri daha da kötüsü daha yüksek yüzdebirliklerini yapar. Gecikme süreleri tutarlı tutmak için düşük bir değere (1-5), sıraya alma ayarlamak istediğiniz ve aktarım hızını işlemek için yineleme sayısını artırın. Ayrıca ayarlama çoğaltmaların sayısı üzerindeki yükü temel alınarak otomatik hale getirmek için otomatik ölçeklendirmeyi etkinleştirebilirsiniz. 

- **Birden fazla web hizmeti uç noktası için aynı makine ya da küme için kullanılabilir mi?**

   Kesinlikle. 100 kat Hizmetleri/uç noktalar aynı kümede çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Model yönetimi ile başlamak için bkz: [Model yönetimi yapılandırma](deployment-setup-configuration.md).
