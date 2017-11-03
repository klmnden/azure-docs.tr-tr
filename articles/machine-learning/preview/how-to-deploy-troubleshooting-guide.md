---
title: "Azure Machine Learning dağıtım sorun giderme kılavuzu | Microsoft Docs"
description: "Dağıtım ve hizmet oluşturmak için sorun giderme kılavuzu"
services: machine-learning
author: raymondl
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: b9287c7151c96aaccbcda81c111cfe36ead5ab38
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Hizmet dağıtımı ve ortam Kurulumu sorunlarını giderme
Aşağıdaki bilgiler model yönetim ortamını ayarlarken hatalarının nedeninin belirlenmesine yardımcı olabilir.

## <a name="model-management-environment"></a>Model yönetim ortamı
### <a name="owner-permission-required"></a>Sahibi izin gerekiyor
Machine Learning işlem kaydetmek için Azure aboneliğine sahip izninizin olması gerekir.

Ayrıca bir küme dağıtımı, web hizmetleri için ayarlamak üzere sahibi izni gerekir.

### <a name="resource-availability"></a>Kaynak kullanılabilirliği
Ortam kaynakları sağlamak için aboneliğinizde kullanılabilir yeterli kaynak olması gerekir.

### <a name="subscription-caps"></a>Abonelik Caps
Aboneliğinizi hangi, ortam kaynakları sağlama önleyebilir faturalama üzerinde bir cap olabilir. Sağlamayı etkinleştirmek için cap Removet.

### <a name="enable-debug-and-verbose-options"></a>Hata ayıklama ve ayrıntılı seçenekleri etkinleştirme
Kullanım `--debug` ve `--verbose` bayrakları ortamı sağlanan olarak hata ayıklama ve izleme bilgilerini görüntülemek için Kurulum komutu.

```
az ml env setup -l <loation> -n <name> -c --debug --verbose 
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
- Varsa `env setup` komutu başarısız, yeterli çekirdek aboneliğinizde kullanılabilir olduğundan emin olun.
- Alt çizgi (_) web hizmeti adını kullanmayın (olarak *my_webservice*).
- Alırsanız yeniden deneme bir **502 hatalı ağ geçidi** web hizmeti çağrılırken hata. Normalde, kapsayıcı kümeye henüz dağıtılan kurmadı anlamına gelir.
- Alırsanız **CrashLoopBackOff** bir hizmet oluşturma sırasında hata günlüklerinizi denetleyin. Genellikle eksik bağımlılıklar sonucu olan **init** işlevi.
