---
title: Azure'da Kubernetes IOT Edge yükleme | Microsoft Docs
description: IOT Edge yerel geliştirme kümesi ortamı ile Kubernetes üzerinde yükleme hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: veyalla
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 66aca7be9a2df93d846d7e78bc64c93279afc2d1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160703"
---
# <a name="how-to-install-iot-edge-on-kubernetes-preview"></a>IOT Edge azure'da Kubernetes (Önizleme) yükleme

IOT Edge, dayanıklı, yüksek kullanılabilirliğe sahip altyapısı katmanı olarak kullanarak Kubernetes ile tümleştirebilirsiniz. IOT Edge kaydeder *özel kaynak tanımı* (Element TokenService BYL) ile Kubernetes API sunucusu. Ayrıca, sağladığı bir *işleci* (IOT Edge Aracısı), istenen durum bulutta yönetilen yerel küme durumu ile mutabık kılar. 

Modül ömrü modülü kullanılabilirlik tutar ve bunların yerleştirme seçer Kubernetes Zamanlayıcı tarafından yönetilir. IOT Edge çalıştıran en üstte edge uygulama platformu sürekli olarak edge kümede durumu ile birlikte IOT Hub'ında belirtilen istenen duruma mutabık kılma yönetir. Edge uygulama modeli hala IOT Edge modülleri ve yollar göre tanıdık modelidir. IOT Edge Aracısı işleç gerçekleştirir *otomatik* Kubernetes natives çeviri gibi pod'ları, dağıtımları, hizmet vb. öğeleri oluşturur.

Bir üst düzey mimari diyagramı şöyledir:

![kubernetes arch](./media/how-to-install-iot-edge-kubernetes/k8s-arch.png)

Edge dağıtımı her bileşeninin birden çok uç cihazlarında ve dağıtımları arasında aynı küme kaynaklarında paylaşılmasını olanaklı hale getirme, cihaz için belirli bir Kubernetes ad alanındadır.

>[!NOTE]
>IOT Edge üzerinde Kubernetes konusu [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="install-locally-for-a-quick-test-environment"></a>Hızlı bir sınama ortamı için yerel olarak yükle

### <a name="prerequisites"></a>Önkoşullar

* Kubernetes 1.10 ya da daha yeni. Var olan bir Küme kurulumu yoksa, kullanabileceğiniz [Minikube](https://kubernetes.io/docs/setup/minikube/) yerel küme ortamı için. 

* [Helm](https://helm.sh/docs/using_helm/#quickstart-guide), Kubernetes Paket Yöneticisi.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) görüntülemek ve kümeyle etkileşimde.

### <a name="setup-steps"></a>Kurulum adımları

1. Başlangıç **Minikube**

    ``` shell
    minikube start
    ```

1. Başlatma **Helm** sunucu bileşeni (*tiller*) kümenizdeki

    ``` shell
    helm init
    ```

1. IOT Edge deponuzu ekleyin ve helm güncelleştirmesi

    ``` shell
    helm repo add edgek8s https://edgek8s.blob.core.windows.net/helm/
    helm repo update
    ```

1. [IOT Hub oluşturma](../iot-hub/iot-hub-create-through-portal.md), [IOT Edge cihazı kaydetme](how-to-register-device-portal.md), bağlantı dizesini not edin.

1. Kümenizle iotedged ve IOT Edge Aracısı yükleme

    ```shell
    helm install \
    --name k8s-edge1 \
    --set "deviceConnectionString=replace-with-device-connection-string" \
    edgek8s/edge-kubernetes
    ```
1. Kubernetes panosunu tarayıcıda açın.

    ```shell
    minikube dashboard
    ```

    Küme ad alanları altında kuralının IOT Edge cihazı için bir tane görürsünüz *msiot -\<iothub-adı >-\<edgedevice-name >*. IOT Edge aracısı ve iotedged pod, bu ad alanında ve çalışıyor olması gerekir.

1. İçindeki adımları kullanarak bir sanal sıcaklık algılayıcısı Modül Ekle [modül dağıtma](quickstart-linux.md#deploy-a-module) hızlı başlangıç bölümünde. IOT Edge modülü yönetimi gibi başka bir IOT Edge cihazı IOT hub'ı portalından gerçekleştirilir. Üzerine olarak Kubernetes araçları aracılığıyla modül yapılandırması yerel değişiklik önerilmez.

1. Birkaç saniye cinsinden yenileme **pod'ların** panosunda edge cihazı ad alanı altında sayfası IOT Edge hub'ı listeler ve IOT Edge hub ile çalışan olarak sanal algılayıcı pod'ları, IOT Hub'ına veri almak pod.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Edge dağıtımı tarafından oluşturulan tüm kaynakları kaldırmak için önceki bölümün 5 adımda kullanılan adı ile aşağıdaki komutu kullanın.

``` shell
helm delete --purge k8s-edge1
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="deploy-as-a-highly-available-edge-gateway"></a>Yüksek oranda kullanılabilir edge ağ geçidi dağıtma 

Sınır cihazı bir Kubernetes kümesinde bir IOT ağ geçidi olarak aşağı akış cihazlar için kullanılabilir. Bu nedenle edge dağıtımları için yüksek düzeyde kullanılabilirlik sağladığınızdan düğüm hatalarına dayanıklı olacak şekilde yapılandırılabilir. Bkz. Bu [ayrıntılı kılavuz](https://github.com/Azure-Samples/iotedge-gateway-on-kubernetes) Bu senaryoda IOT Edge'i kullanma.