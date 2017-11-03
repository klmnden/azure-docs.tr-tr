---
title: "Azure Machine Learning modeli Yönetimi kavramsal genel bakış | Microsoft Docs"
description: "Bu belge, Azure Machine Learning için Model yönetimi kavramlarını açıklar."
services: machine-learning
author: nk773
ms.author: neerajkh, padou
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 31b859d86e82c92839462280721c5f84f1d923cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-machine-learning-model-management"></a>Azure Machine Learning Model Yönetimi

Azure Machine Learning modeli yönetim, yönetmek ve machine learning iş akışları ve modelleri dağıtmanızı sağlar. 

Model yönetim yeteneklerini sağlar:
- Model sürüm oluşturma
- Üretim modellerinde izleme
- Model AzureML işlem ortamıyla aracılığıyla üretime dağıtma [Azure kapsayıcı hizmeti](https://azure.microsoft.com/services/container-service/) ve [Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Docker kapsayıcıları ile modelleri oluşturma ve yerel olarak test etme
- Otomatik modeli yeniden eğitme
- Eyleme dönüştürülebilir Öngörüler için yakalama modeli telemetri. 

Azure Machine Learning modeli yönetim modeli sürümlerinin bir kayıt defteri sağlar. Ayrıca paketleme ve Machine Learning kapsayıcılar REST API'leri olarak dağıtma için otomatik iş akışları sağlar. Modelleri ve çalışma zamanı bağımlılıklarını, Linux tabanlı Docker kapsayıcısı tahmin API ile paketlenmiştir. 

Azure Machine Learning işlem ortamları modelleri barındırmak için ölçeklenebilir kümeleri ayarlamanıza yardımcı olur. Bilgi işlem ortamı Azure kapsayıcı Services'de temel alır. Azure kapsayıcı hizmetlerini Machine Learning API'ları otomatik riskini aşağıdaki özelliklerle REST API uç noktaları olarak sağlar:

- Kimlik Doğrulaması
- Yük dengeleme
- Otomatik ölçeklendirme
- Şifreleme

Azure Machine Learning modeli yönetim CLI, API ve Azure portalı üzerinden bu özellikleri sağlar. 

Azure Machine Learning modeli yönetim aşağıdaki bilgileri kullanır:

 - Model dosyası veya model dosyalarında sahip bir dizin
 - Kullanıcı işlevi Puanlama modeli uygulama Python dosyası oluşturuldu
 - Çalışma zamanı bağımlılıkları listeleme Conda bağımlılık dosyası
 - Çalışma zamanı ortamı seçim ve 
 - API parametreleri için şema dosyası 

Bu bilgiler aşağıdaki eylemleri gerçekleştirirken kullanılır:

- Bir model kaydetme
- Bir bildirim oluşturmak bir kapsayıcı oluşturulurken kullanılır
- Bir Docker kapsayıcısı görüntü oluşturma
- Azure kapsayıcı hizmeti için bir kapsayıcı dağıtma
 
Aşağıdaki şekilde nasıl modelleri kayıtlı ve kümeye dağıtılan özetini gösterir. 

![](media/model-management-overview/modelmanagement.png)

## <a name="create-and-manage-models"></a>Modelleri oluşturmak ve yönetmek 
Model sürümleri üretimde izlemek için Azure Machine Learning modeli yönetimiyle modelleri kaydedebilirsiniz. Yeniden Üretilebilirlik ve idare kolaylaştırmak için hizmet tüm bağımlılıkları ve ilişkili bilgilerini yakalar. Performans, daha ayrıntılı Öngörüler için sağlanan SDK'sını kullanarak modeli telemetri yakalayabilirsiniz. Model telemetri kullanıcı tarafından sağlanan depolama arşivlenir. Model telemetri modeli performansını analiz etme, yeniden eğitme ve işletmeniz için Öngörüler elde daha sonra kullanılabilir.

## <a name="create-and-manage-manifests"></a>Oluşturma ve bildirimleri yönetme 
Modelleri üretime dağıtmak için ek yapıları gerektirir. Sistem modeli, bağımlılıkları, çıkarım komut dosyası (diğer adıyla Puanlama komut dosyası), örnek verileri, şema vb. kapsayan bir bildirimi oluşturma yeteneği sağlar. Bu bildirim bir Docker kapsayıcısı görüntüsü oluşturmak için tarif davranır. Kuruluşların bildirim otomatik olarak oluşturmak, farklı sürümlerini oluşturun ve bunların bildirimleri yönetin. 

## <a name="create-and-manage-docker-container-images"></a>Oluşturun ve Docker kapsayıcısı görüntülerini yönetme 
Önceki adımda bildirimden ilgili ortamlarında Docker tabanlı kapsayıcı görüntüleri oluşturmak için kullanabilirsiniz. Kapsayıcılı, Docker tabanlı görüntüleri kuruluşların bu görüntüler aşağıdaki işlem ortamda çalıştırmak için esneklik sağlar:

- [Azure kapsayıcı hizmeti Kubernetes dayalı](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Şirket içi kapsayıcı hizmetlerini
- Geliştirme ortamları
- IOT cihazları

Bu Docker tabanlı kapsayıcılı görüntüler tahminleri oluşturmak için gereken tüm gerekli bağımlılıkları kendi içinde yer alan. 

## <a name="deploy-docker-container-images"></a>Docker kapsayıcısı görüntüleri dağıtma 
Azure Machine Learning modeli Management'ı tek bir komut Docker tabanlı kapsayıcı görüntülerle ML işlem ortamı tarafından yönetilen Azure kapsayıcı hizmeti dağıtabilirsiniz. Bu dağıtımlar aşağıdaki özellikleri sağlayan bir ön uç sunucusu ile oluşturulur:

- Düşük gecikme süresi tahminleri ölçekte
- Yük dengeleme
- Otomatik ölçeklendirme ML uç noktaları
- API anahtarı yetkilendirme
- API swagger belgesinin

Dağıtım ölçek ve telemetri aşağıdaki yapılandırma ayarları aracılığıyla denetleyebilirsiniz:

- Sistem günlük kaydı ve her web hizmet düzeyi için model telemetri. Etkinleştirilirse, tüm stdout günlükleri için akışı [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Model telemetri sağladığınız depolama arşivlenir. 
- Otomatik ölçek ve eşzamanlılık sınırlar. Bu ayarlar otomatik olarak var olan küme içindeki yük göre dağıtılan kapsayıcıları sayısını artırın. Bunlar ayrıca üretilen iş ve tahmin gecikme tutarlılığını denetler.

## <a name="consumption"></a>Tüketim 
Azure Machine Learning modeli yönetim REST API için swagger belgesinin birlikte dağıtılan modeli oluşturur. API ile REST API anahtarı ve satır iş kolu uygulamaları bir parçası olarak Öngörüler almak için girişleri model arama tarafından dağıtılan modelleri kullanmasını sağlayabilirsiniz. Örnek kod dillerde Java, Github'da kullanılabilir [Python](https://github.com/CortanaAnalyticsGallery-Int/digit-recognition-cnn-tf/blob/master/client.py)ve REST API'leri çağırmak için C#. Azure Machine Learning modeli yönetim CLI bu REST API'leri ile çalışmak için kolay bir yol sağlar. Tek bir CLI komutu, bir swagger özellikli uygulamalar veya curl kullanarak API'leri kullanabilir. 

## <a name="retraining"></a>Yeniden eğitme 
Azure Machine Learning modeli yönetim Modellerinizi yeniden eğitme kullandığınız API'ler sağlar. API'ler, model güncelleştirilmiş sürümlerini güncelleştirme mevcut dağıtımları için de kullanabilirsiniz. Veri bilimi akışı bir parçası olarak, model deneme ortamınızda oluşturun. Ardından, Model yönetimiyle modelini kaydettirmek ve varolan dağıtımları güncelleştirin. Güncelleştirmeleri tek bir güncelleştirme CLI komutu kullanılarak gerçekleştirilir. Güncelleştirme komut API URL veya anahtar değiştirmeden var olan dağıtımlar güncelleştirir. Model kullanan uygulamaları devam herhangi bir kod değişikliği çalışma ve yeni modeli kullanarak daha iyi Öngörüler almaya başlayın.

Bu kavramlar açıklayan tam iş akışı aşağıdaki resimde yakalanır.

![](media/model-management-overview/modelmanagementworkflow.png)

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS) 
- Hangi veri türleri destekleniyor mu? NumPy diziler doğrudan web hizmeti için giriş olarak geçirebilirsiniz?

   Ardından generate_schema SDK kullanılarak oluşturulmuş şema dosyası sağlıyorsanız NumPy ve/veya Pandas DF geçirebilirsiniz. Ayrıca, tüm JSON seri hale getirilebilir girişleri geçirebilirsiniz. İkili Kodlanmış dize da görüntü geçirebilirsiniz.

- Web hizmeti birden çok girişi desteklemek veya farklı girişleri ayrıştırma? 

   Evet, bir JSON istekteki bir sözlük olarak paketlenmiş birden çok girişi alabilir. Her giriş için tek benzersiz sözlük anahtarı karşılık gelir.

- Olduğu web isteğine etkinleştiren aramaya hizmet engelleme çağrısı veya bir zaman uyumsuz çağrı?

   Engelleme/zaman uyumlu bir çağrı yapılır ve ardından hizmet CLI ya da API, bir parçası olarak gerçek zamanlı seçeneği kullanılarak oluşturulduysa. Gerçek zamanlı hızlı olması bekleniyor. İstemci iş parçacığı önlemek için zaman uyumsuz HTTP kitaplığını kullanarak çağırabilirsiniz istemci tarafında engelleme rağmen.

- Kaç tane istekleri web hizmeti aynı anda işleyebilir?

   Küme ve web hizmeti ölçeğini bağlıdır. Çoğaltmaların 100 x hizmetinize ölçeğini ve ardından, birçok istek eşzamanlı olarak işleyebilir. Hizmet verimliliğini artırmak için en fazla eşzamanlı istek başına çoğaltma da yapılandırabilirsiniz.

- Kaç tane istekleri web hizmeti sıraya?

   Yapılandırılabilir. Varsayılan olarak, tek çoğaltma başına 10 ~ ayarlanmış, ancak, artırma/bunu uygulama gereksinimlerinizi azaltma. Genellikle, artan sıraya alınan istek sayısı hizmet performansı artırır ancak yüksek yüzdebirlik değeri gecikmeleri daha da kötüsü yapar. Gecikme tutarlı tutmak için düşük bir değer (1-5) queuing ayarlamak istediğiniz ve üretilen işi işlemek için yineleme sayısı artar. Ayrıca ayarlama çoğaltmaların sayısı yüküne göre otomatik olarak yapmak için otomatik ölçeklendirmeyi etkinleştirebilirsiniz. 

- Aynı makine ya da küme için birden çok web hizmeti uç noktaları kullanılabilir mi?

   Kesinlikle. Aynı kümede Hizmetleri/uç noktaları 100 x çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Model yönetimine Başlarken için bkz: [yapılandırma modeli Yönetim](model-management-configuration.md).
