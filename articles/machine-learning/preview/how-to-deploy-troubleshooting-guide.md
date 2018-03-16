---
title: "Azure Machine Learning dağıtım sorun giderme kılavuzu | Microsoft Docs"
description: "Dağıtım ve hizmet oluşturmak için sorun giderme kılavuzu"
services: machine-learning
author: AashishB
ms.author: AashishB
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 614767840f8781c3c30c358dcf3ffc366aa3c0d6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Hizmet dağıtımı ve ortam Kurulumu sorunlarını giderme
Aşağıdaki bilgiler model yönetim ortamını ayarlarken hatalarının nedeninin belirlenmesine yardımcı olabilir.

## <a name="model-management-environment"></a>Model yönetim ortamı
### <a name="contributor-permission-required"></a>Katkıda bulunan izin gerekiyor
Abonelik veya kaynak grubu, bir küme dağıtımı, web hizmetleri için ayarlamak üzere katkıda bulunan erişmeniz gerekir.

### <a name="resource-availability"></a>Kaynak kullanılabilirliği
Ortam kaynakları sağlamak için aboneliğinizde kullanılabilir yeterli kaynak olması gerekir.

### <a name="subscription-caps"></a>Abonelik Caps
Aboneliğinizi hangi, ortam kaynakları sağlama önleyebilir faturalama üzerinde bir cap olabilir. Sağlamayı etkinleştirmek için bu CAP'ye kaldırın.

### <a name="enable-debug-and-verbose-options"></a>Hata ayıklama ve ayrıntılı seçenekleri etkinleştirme
Kullanım `--debug` ve `--verbose` bayrakları ortamı sağlanan olarak hata ayıklama ve izleme bilgilerini görüntülemek için Kurulum komutu.

```
az ml env setup -l <location> -n <name> -c --debug --verbose 
```

## <a name="service-deployment"></a>Hizmet dağıtımı
Aşağıdaki bilgiler, hataları dağıtımı sırasında veya web hizmeti çağrılırken nedenini yardımcı olabilir.

### <a name="service-logs"></a>Hizmet Günlükleri
`logs` CLI hizmet seçeneği Docker ve Kubernetes günlük verilerini sağlar.

```
az ml service logs realtime -i <web service id>
```

Ek günlük ayarlarını kullanmak `--help` (veya `-h`) seçeneği.

```
az ml service logs realtime -h
```

### <a name="debug-and-verbose-options"></a>Hata ayıklama ve ayrıntılı seçenekleri
Kullanım `--debug` hizmeti dağıtılmış olarak hata ayıklama günlüklerini göster bayrağı.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Kullanım `--verbose` hizmeti dağıtılmış gibi ek ayrıntıları görmek için bayrak.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

### <a name="enable-request-logging-in-app-insights"></a>Uygulama öngörü günlüğü isteği etkinleştir
Ayarlama `-l` istek düzeyi günlük kaydını etkinleştirmek için bir web hizmeti oluştururken true olarak bayrağı. İstek günlüklerini Azure ortamınızda App Insights örneğine yazılır. Arama kullanılırken kullanılan ortam adı kullanarak bu örnek için `az ml env setup` komutu.

- Ayarlama `-l` hizmet oluşturulurken true.
- App Insights Azure portalında açın. App Insights örneği bulunamadı, ortam adı kullanın.
- Bir kez App Insights ' arama sonuçlarını görüntülemek için üst menüde tıklatın.
- Veya gitmek `Analytics`  >  `Exceptions`  >  `exceptions take | 10`.


### <a name="add-error-handling-in-scoring-script"></a>Hata betik Puanlama işleme ekleme
Özel durum olarak işleme kullanın, `scoring.py` komut dosyanızın **çalıştırmak** işlevi, web hizmeti çıkış bir parçası olarak hata iletisi döndürecek şekilde.

Python örnek:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="other-common-problems"></a>Diğer yaygın sorunlar
- Azure CLI ml PIP yüklemesi şu hata ile başarısız olursa `cannot find the path specified` bir Windows makinesinde uzun yol desteği etkinleştirmeniz gerekir. Bkz: https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/. 
- Varsa `env setup` komutu başarısız ile `LocationNotAvailableForResourceType`, öğrenme kaynakları makine için büyük olasılıkla yanlış konum (bölge) kullanıyorsanız. İle belirtilen konumunuz emin olun `-l` parametresi `eastus2`, `westcentralus`, veya `australiaeast`.
- Varsa `env setup` komutu başarısız ile `Resource quota limit exceeded`, aboneliğinizde kullanılabilir yeterli çekirdek varsa ve kaynaklarınızı kullanılmayan yukarı diğer işlemleri emin olun.
- Varsa `env setup` komutu başarısız ile `Invalid environment name. Name must only contain lowercase alphanumeric characters`, hizmet adı büyük harfler, simgeler veya alt çizgi (_) içermediğinden emin olun (olarak *my_environment*).
- Varsa `service create` komutu başarısız ile `Service Name: [service_name] is invalid. The name of a service must consist of lower case alphanumeric characters (etc.)`, hizmet adı uzunluğu 3 ile 32 karakter arasında olan; başlatır ve küçük harf alfasayısal karakterlerle; sona erer ve büyük harfler, semboller tire (-) ve süre dışında içermediğinden emin olun ( . ), veya alt çizgi (_) (olarak *my_webservice*).
- Alırsanız yeniden deneme bir `502 Bad Gateway` web hizmeti çağrılırken hata. Normalde, kapsayıcı kümeye henüz dağıtılan kurmadı anlamına gelir.
- Alırsanız `CrashLoopBackOff` bir hizmet oluşturma sırasında hata günlüklerinizi denetleyin. Genellikle eksik bağımlılıklar sonucu olan **init** işlevi.
