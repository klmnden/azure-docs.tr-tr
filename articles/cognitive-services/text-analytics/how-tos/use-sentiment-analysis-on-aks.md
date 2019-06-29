---
title: Azure Kubernetes hizmeti çalıştırın
titleSuffix: Text Analytics - Azure Cognitive Services
description: Azure Kubernetes hizmetlere ve yaklaşım analizi görüntüsüyle metin analizi kapsayıcıları dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 4d5e1da01be531550915a38bed17dd8e57be907a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454955"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-services-aks"></a>Azure Kubernetes Hizmetleri (AKS) için bir yaklaşım analizi kapsayıcısı dağıtma

Bilişsel hizmetler dağıtmayı öğrenirsiniz [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) yaklaşım analizi görüntüyü Azure Kubernetes Hizmetleri (AKS) ile kapsayıcı. Bu yordam, metin analizi kaynak oluşturmayı, ilgili yaklaşım analizi görüntü ve bu ikisinin bir tarayıcıdan düzenleme çalışma olanağı oluşturulmasını exemplifies. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu yordam, yüklü ve yerel olarak çalıştırma çeşitli araçlar gerektirir. Azure Cloud Shell'i kullanmayın.

* Bir Azure aboneliği kullanın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Metin düzenleyici, örneğin: [Visual Studio Code](https://code.visualstudio.com/download).
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)’yi yükleyin.
* Yükleme [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
* Doğru fiyatlandırma katmanı ile bir Azure kaynağı. Tüm fiyatlandırma katmanları bu kapsayıcısı ile çalışır:
    * **Metin analizi** kaynak F0 veya standart fiyatlandırma katmanlarını yalnızca.
    * **Bilişsel Hizmetler** kaynak fiyatlandırma katmanı S0 ile.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Kubernetes Services (AKS)](../../containers/includes/create-aks-resource.md)]

## <a name="deploy-text-analytics-container-to-an-aks-cluster"></a>Metin analizi kapsayıcı için AKS kümesi dağıtma

1. Oturum açma ve Azure CLI, Azure'da oturum açın

    ```azurecli
    az login
    ```

1. AKS kümesi için oturum açın (Değiştir `your-cluster-name` ve `your-resource-group` uygun değerlerle)

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Bu komut yürütüldükten sonra aşağıdakine benzer bir ileti raporları:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Azure hesabınızda kullanılabilen birden fazla aboneliğiniz varsa ve `az aks get-credentials` komutu ile bir hata döndürür, yanlış aboneliğe kullandığınız ortak bir sorunu. Yalnızca kaynaklarla oluşturulan aynı aboneliği kullanmak için Azure CLI oturumunuzu bağlamını ayarlayın ve yeniden deneyin.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Tercih ettiğiniz metin düzenleyiciyi açın (Bu örnekte __Visual Studio Code__):

    ```azurecli
    code .
    ```

1. Metin Düzenleyici içinde adlı yeni bir dosya oluşturmak _sentiment.yaml_ aşağıdaki YAML yapıştırın:

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: "https://westus2.api.cognitive.microsoft.com/"
            - name: apikey
              value: "16c12e3419f54ba49a3222177cef781d"
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. Dosyayı kaydedin ve Metin Düzenleyicisi'ni kapatın.
1. Kubernetes yürütme `apply` komutunu _sentiment.yaml_ hedef olarak:

    ```console
    kuberctl apply -f sentiment.yaml
    ```

    Komut dağıtımı yapılandırması başarıyla uygulanmış sonra aşağıdakine benzer bir ileti çıktı.

    ```
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. POD dağıtıldığını doğrulayın:

    ```console
    kubectl get pods
    ```

    Bu POD çalışan durumunu çıkarır:

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Hizmetin kullanılabilir olduğundan emin olun ve IP adresini alın:

    ```console
    kubectl get services
    ```

    Bu çalışma durumunu çıkarır _yaklaşım_ POD hizmeti:

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../../cognitive-services-container-support.md)
* Kullanım [metin analizi bağlı hizmeti](../vs-text-connected-service.md)