---
title: Azure işlevleri KEDA ile Kubernetes üzerinde
description: Bulutta kubernetes Azure işlevleri'ni çalıştırma öğrenin veya şirket içinde KEDA, olay temelli Kubernetes tabanlı otomatik ölçeklendirmeyi kullanma.
services: functions
documentationcenter: na
author: jeffhollan
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme, dinamik işlem, sunucusuz mimari, kubernetes
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: jehollan
ms.openlocfilehash: c82ed7aa841f53f5c81f3281ed1b09926e565e75
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077628"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>Azure işlevleri KEDA ile Kubernetes üzerinde

Azure işlevleri çalışma zamanı barındırma nerede ve nasıl istediğiniz esneklik sağlar.  [KEDA](https://github.com/kedacore/kore) (Kubernetes tabanlı olay tabanlı otomatik ölçeklendirme) çiftlerini sorunsuzca EventDriven ölçeği kubernetes sağlamak için araçları ve Azure işlevleri çalışma zamanı.

## <a name="how-kubernetes-based-functions-work"></a>Kubernetes tabanlı nasıl işlevler çalışma

Azure işlevleri hizmeti iki önemli bileşenden oluşur: bir çalışma zamanı ve ölçek denetleyicisi.  İşlevler çalışma zamanı çalışır ve kodunuzu yürütür.  Çalışma zamanı tetiklemek için oturum ve işlev yürütmelerini yönetme konusunda mantığı içerir.  Başka bir bileşen, bir ölçek denetleyicisidir.  Ölçek denetleyici işlevinizi hedeflediğiniz olaylarının oranını izler ve proaktif olarak, uygulamanızı çalıştıran örnek sayısını ölçeklendirir.  Daha fazla bilgi için bkz. [Azure işlevlerini ölçeklendirme ve barındırma](functions-scale.md).

Kubernetes temel işlevleri sağlayan İşlevler çalışma zamanı'nda bir [Docker kapsayıcısı](functions-create-function-linux-custom-image.md) KEDA ile olay temelli ölçeklendirme.  KEDA ölçeklendirme yapabilen (olay ortaya çıkan olduğunda) 0 örnekleri ve en fazla *n* örnekleri. Bunu Kubernetes otomatik ölçeklendiricinin (yatay Pod otomatik Ölçeklendiricinin) için özel ölçümleri göstererek yapar.  İşlevleri kapsayıcılar ile KEDA kullanarak sunucusuz bir işlev özelliklerinde herhangi bir Kubernetes kümesinde çoğaltma mümkün kılar.  Bu işlevler kullanılarak da dağıtılabilir [Azure Kubernetes Hizmetleri (AKS) sanal düğümü](../aks/virtual-nodes-cli.md) sunucusuz bir altyapı özelliği.

## <a name="managing-keda-and-functions-in-kubernetes"></a>KEDA ve Kubernetes işlevlerde yönetme

Kubernetes kümenizde işlevleri çalıştırmak için KEDA bileşen yüklemeniz gerekir. Bu bileşenini kullanarak yükleyebileceğiniz [Azure işlevleri çekirdek Araçları](functions-run-local.md).

### <a name="installing-with-the-azure-functions-core-tools"></a>İle Azure işlevleri çekirdek araçları yükleme

Varsayılan olarak, temel araçları olay odaklı destek KEDA ve Osiris bileşenleri ve HTTP ölçeklendirme, sırasıyla yükler.  Yükleme kullanan `kubectl` geçerli bağlamda çalışıyor.

KEDA kümenizde aşağıdaki yükleme komutunu çalıştırarak yükleyin:

```cli
func kubernetes install --namespace keda
```

## <a name="deploying-a-function-app-to-kubernetes"></a>Kubernetes için bir işlev uygulaması dağıtma

Herhangi bir işlev uygulaması KEDA çalışan bir Kubernetes kümesine dağıtabilirsiniz.  İşlevlerinizi bir Docker kapsayıcısında çalıştırma olduğundan, projenize gerekli bir `Dockerfile`.  Bunu zaten bir sahip değilse, İşlevler projeniz kökünde aşağıdaki komutu çalıştırarak bir Dockerfile ekleyebilirsiniz:

```cli
func init --docker-only
```

Bir görüntü oluşturun ve Kubernetes için işlevlerinizi dağıtmak için aşağıdaki komutu çalıştırın:

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> Değiştirin `<name-of-function-deployment>` işlev uygulamanızın adıyla.

Bu, bir Kubernetes oluşturur `Deployment` kaynak, bir `ScaledObject` kaynak ve `Secrets`, içeri aktarılan ortam değişkenlerini içeren, `local.settings.json` dosya.

## <a name="removing-a-function-app-from-kubernetes"></a>Bir işlev uygulaması Kubernetes kaldırılıyor

Dağıtım bir işlev ilişkili kaldırarak kaldırabilirsiniz sonra `Deployment`, `ScaledObject`e `Secrets` oluşturulur.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>Kubernetes KEDA kaldırma

Kubernetes küme KEDA kaldırmak için aşağıdaki çekirdek araçları komutu çalıştırabilirsiniz:

```cli
func kubernetes remove --namespace keda
```

## <a name="supported-triggers-in-keda"></a>Desteklenen KEDA Tetikleyicileri

KEDA şu anda beta desteği için aşağıdaki Azure işlevi Tetikleyiciler ile oluşturulur.

* [Azure depolama kuyrukları](functions-bindings-storage-queue.md)
* [Azure Service Bus kuyrukları](functions-bindings-service-bus.md)
* [HTTP](functions-bindings-http-webhook.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Özel bir görüntü kullanarak bir işlev oluşturma](functions-create-function-linux-custom-image.md)
* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-develop-local.md)
* [Azure işlevi tüketim planı nasıl çalışır?](functions-scale.md)