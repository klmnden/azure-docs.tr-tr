---
title: Azure Machine Learning dağıtım sorunlarını giderme kılavuzu | Microsoft Docs
description: Dağıtım ve hizmet oluşturma için sorun giderme kılavuzu
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 4cf86466d5fca4f5095c67a8400643bff29bb56c
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651227"
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Hizmet dağıtımı ve ortam Kurulumu sorunlarını giderme
Aşağıdaki bilgiler, model yönetim ortamını ayarlarken hatalarının nedenini belirlemeye yardımcı olabilir.

## <a name="model-management-environment"></a>Model yönetim ortamı
### <a name="contributor-permission-required"></a>Katkıda bulunan izni gerekli
Abonelik veya kaynak grubu, web hizmetlerinin dağıtımı için bir küme ayarlamak için katkıda bulunan erişimine ihtiyacınız var.

### <a name="resource-availability"></a>Kaynak kullanılabilirliği
Ortam kaynaklarına sağlayabilirsiniz. Bu nedenle, aboneliğinizde yeterli kaynak olması gerekir.

### <a name="subscription-caps"></a>Abonelik Caps
Aboneliğiniz, size ortam kaynaklarına sağlamasından engelleyebilir faturalandırma hakkında bir sınır olabilir. Sağlamayı etkinleştirmek için bu sınır kaldırın.

### <a name="enable-debug-and-verbose-options"></a>Hata ayıklama ve ayrıntılı seçenekleri etkinleştir
Kullanım `--debug` ve `--verbose` ortamı sağlanıyor olarak hata ayıklama ve izleme bilgilerini göstermek için Kurulum komut bayrakları.

```
az ml env setup -l <location> -n <name> -c --debug --verbose 
```

## <a name="service-deployment"></a>Hizmet dağıtımı
Aşağıdaki bilgiler, web hizmetini çağırırken hatalarının nedenini dağıtımı sırasında veya belirlemeye yardımcı olabilir.

### <a name="service-logs"></a>Hizmeti günlükleri
`logs` CLI hizmet seçeneği, Docker ve Kubernetes günlük verilerini sağlar.

```
az ml service logs realtime -i <web service id>
```

Ek günlük ayarlarını kullanmak `--help` (veya `-h`) seçeneği.

```
az ml service logs realtime -h
```

### <a name="debug-and-verbose-options"></a>Hata ayıklama ve ayrıntılı seçenekleri
Kullanım `--debug` hizmet dağıtıldığında, hata ayıklama günlüklerini göster bayrağı.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Kullanım `--verbose` hizmet dağıtıldıktan gibi ek ayrıntıları görmek için bayrak.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

### <a name="enable-request-logging-in-app-insights"></a>Uygulama anlayışları'nda günlüğe kaydetme isteği'ı etkinleştir
Ayarlama `-l` istek düzeyi günlük kaydını etkinleştirmek için bir web hizmeti oluştururken true olarak bayrak. İstek günlükleri, Azure ortamınızda için App Insights örneğine yazılır. Arama kullanılırken kullandığınız ortam adının kullanarak bu örneğin `az ml env setup` komutu.

- Ayarlama `-l` hizmet oluştururken true.
- App Insights, Azure portalında açın. App Insights örneği bulunamadı, ortam adı kullanın.
- Bir kez uygulama anlayışları'nda, arama sonuçlarını görüntülemek için üst menüden tıklayın.
- Veya Git `Analytics`  >  `Exceptions`  >  `exceptions take | 10`.


### <a name="add-error-handling-in-scoring-script"></a>Hata Puanlama betik içinde işleme ekleme
Özel durum işleme kullanmak, `scoring.py` betiğin **çalıştırma** hata iletisi, web hizmeti çıkış bir parçası olarak döndürülecek işlev.

Python örnek:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="other-common-problems"></a>Diğer yaygın sorunlar
- Azure-cli-ml pip yüklemesi şu hata ile başarısız olursa `cannot find the path specified` uzun yol desteğini etkinleştirmek gereken bir Windows makinede. Bkz: https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/. 
- Varsa `env setup` komutu başarısız `LocationNotAvailableForResourceType`, büyük olasılıkla yanlış konum (bölge) makine öğrenme kaynakları için kullanıyorsunuz. Konumunuz ile belirtilen emin `-l` parametresi `eastus2`, `westcentralus`, veya `australiaeast`.
- Varsa `env setup` komutu başarısız `Resource quota limit exceeded`, yeterli çekirdek aboneliğinizde mevcut olması ve kaynaklarınızı kullanılmayan yukarı diğer işlemlerde emin olun.
- Varsa `env setup` komutu başarısız `Invalid environment name. Name must only contain lowercase alphanumeric characters`, hizmet adı büyük harfler, simgeler veya alt çizgi (_) içermediğinden emin olun (olarak *my_environment*).
- Varsa `service create` komutu başarısız `Service Name: [service_name] is invalid. The name of a service must consist of lower case alphanumeric characters (etc.)`, hizmet adı 3 ile 32 karakter uzunluğunda olan; başlar ve sona erer; küçük harf alfasayısal karakterler ile ve büyük harfler, semboller tire (-) ve süresi dışında içermediğinden emin olun ( . ), veya alt çizgi (_) (olarak *my_webservice*).
- Alırsanız yeniden bir `502 Bad Gateway` web hizmeti çağrılırken hata. Normalde, kapsayıcı kümeye henüz dağıtılan taşınmadığından anlamına gelir.
- Alırsanız `CrashLoopBackOff` bir hizmet oluşturulurken bir hata günlüklerinizi denetleyin. Bu genellikle eksik bağımlılıklar sonucudur **init** işlevi.
