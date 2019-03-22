---
title: Azure Bilişsel hizmetler için Azure Stack dağıtma | Microsoft Docs
description: Azure Bilişsel hizmetler için Azure Stack dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2018
ms.author: mabrigg
ms.reviewer: guanghu
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: 8080355bebf00c9f37c28ae8ed54bba092f8dc17
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58099942"
---
# <a name="deploy-azure-cognitive-services-to-azure-stack"></a>Azure Bilişsel hizmetler için Azure Stack dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Azure Bilişsel hizmetler için Önizleme aşamasındadır.

Azure Bilişsel hizmetler, Azure Stack üzerinde kapsayıcı desteği ile kullanabilirsiniz. Azure Bilişsel hizmetler kapsayıcı desteği, Azure'da kullanılabilen aynı zengin API'lerin kullanmanıza olanak tanır. Kapsayıcıları kullanımınız, dağıtma ve barındırmak sunulan Hizmetleri nerede esneklik sağlar. [Docker kapsayıcıları](https://www.docker.com/what-container). Kapsayıcı desteği şu anda bir alt kümesini Azure Bilişsel bölümlerini dahil olmak üzere hizmetler için önizleme modunda [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview), ve [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview), ve [Language Understanding'i](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) (LUIS).

Kapsayıcı içinde bir uygulama veya hizmet, yapılandırması ve bağımlılıkları da dahil olmak üzere paketlenir bir kapsayıcı görüntüsü bir yazılım dağıtımına bir yaklaşımdır. Çok az kayıpla veya hiç değişiklik yapmadan, görüntüyü bir kapsayıcı konağı için dağıtabilirsiniz. Her kapsayıcı, diğer kapsayıcılardan ve alttaki işletim sisteminden ayrı tutulur. Sistem kendisini, yalnızca görüntünüzü çalıştırmak için gerekli bileşenleri vardır. Bir kapsayıcı konağı bir daha küçük kaplama alanı bir sanal makine daha vardır. Ayrıca, görüntülerinden kısa vadeli görevler için kapsayıcılar oluşturabilirsiniz ve artık gerekmediğinde kaldırıldı.

## <a name="use-containers-with-cognitive-services-on-azure-stack"></a>Azure Stack'te Bilişsel hizmetler ile kapsayıcılar kullanma

- **Veriler üzerinde denetim**  
  Uygulama kullanıcılarınızın, Bilişsel hizmetler kullanarak verileri üzerinde denetime sahip olmasını sağlar. Bilişsel hizmetler, küresel Azure veri veya genel bulut gönderemiyor uygulama kullanıcılar için teslim edebilirsiniz.

- **Model üzerinde denetim güncelleştirir**  
  Uygulama sürümü ve bunların çözümünde dağıtılmış modelleri güncelleştirme kullanıcılara sağlar.

- **Taşınabilir mimarisi**  
  Taşınabilir uygulama mimarisi oluşturulmasını etkinleştirin; böylelikle özel, genel bulut çözümünüzü dağıtabileceğiniz bulut şirket içinde veya uç cihazlarında. Azure Stack içindeki bir Kubernetes kümesi veya Azure Kubernetes hizmeti, Azure Container Instances kapsayıcınızı dağıtabilirsiniz. Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

- **Yüksek aktarım hızına ve düşük gecikme süresi**  
   Uygulama kullanıcılarınızın, ani trafik yüksek aktarım hızı ve düşük gecikme süresi ile ölçeklendirme olanağı sağlar. Azure Kubernetes hizmetinde, uygulama mantığı ve verileri yakın fiziksel olarak çalıştırmak, Bilişsel hizmetler sağlar.

Azure Stack ile bir Kubernetes kümesinde uygulama kapsayıcılarınızı yüksek kullanılabilirlik ve esnek ölçeklendirme yanı sıra Bilişsel hizmetler kapsayıcıları dağıtın. Bilişsel hizmetler uygulama hizmetleri, İşlevler, Blob Depolama veya SQL veya mySQL veritabanları oluşturulan bileşenler ile birleştirerek, uygulama geliştirebilirsiniz. 

Bilişsel hizmetler kapsayıcılar hakkında daha fazla ayrıntı için Git [Azure Bilişsel hizmetler kapsayıcı desteği](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support).

## <a name="deploy-the-azure-face-api"></a>Azure yüz tanıma API'si ile dağıtma

Bu makalede, Azure Stack üzerinde Kubernetes kümesinde Azure yüz tanıma API'si dağıtmayı açıklar. Azure Stack Kubernetes kümesindeki diğer bilişsel hizmetler kapsayıcıları dağıtmak için aynı yaklaşımı kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce yapmanız gerekir:

1.  Azure Bilişsel hizmetler kapsayıcı kayıt defterinden yüz kapsayıcı görüntüleri çekmek için kapsayıcı kayıt defterine erişim isteyin. Ayrıntılar bölümüne gidin [özel kapsayıcı kayıt defterine erişim isteği](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers#request-access-to-the-private-container-registry).

2.  Azure Stack'te bir Kubernetes kümesi hazırlayın. Makale izleyebilirsiniz [Azure Stack dağıtma Kubernetes](azure-stack-solution-template-kubernetes-deploy.md).

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Yüz tanıma, LUIS veya metni tanı kapsayıcılar, sırasıyla önizlemek için Azure üzerinde bir Bilişsel Hizmet kaynağı oluşturun. Bilişsel hizmet kapsayıcı örneği oluşturmak için abonelik anahtarını ve uç nokta URL'si kaynaktan kullanmanız gerekir.

1. Azure portalında bir Azure kaynağı oluşturun. Yüz tanıma kapsayıcıları Önizleme istiyorsanız, öncelikle Azure portalında karşılık gelen bir yüz kaynak oluşturmanız gerekir. Daha fazla bilgi için [hızlı başlangıç: Azure portalında bir Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

   > [!Note]
   >  Yüz tanıma veya görüntü işleme kaynak F0 fiyatlandırma katmanını kullanmanız gerekir.

2. Azure kaynakları için uç nokta URL'si ve abonelik anahtarını alın. Azure kaynak oluşturulduktan sonra Önizleme için karşılık gelen yüz tanıma, LUIS veya metni tanı kapsayıcı örneği oluşturmak için abonelik anahtarını ve uç nokta URL'si bu kaynaktan kullanmanız gerekir.

## <a name="create-a-kubernetes-secret"></a>Bir Kubernetes gizli dizisi oluşturma 

Kubectl Al kullanımını özel kapsayıcı kayıt defterine erişim için gizli komutu oluşturun. Değiştirin **&lt;kullanıcıadı&gt;** kullanıcı adıyla ve **&lt;parola&gt;** Azure'dan aldığınız kimlik bilgisi sağlanan parolayla Bilişsel hizmet takımının.

```bash  
    kubectl create secret docker-registry <secretName> \
        --docker-server='containerpreview.azurecr.io' \
        --docker-username='<username>' \
        --docker-password='<password>' 
```

## <a name="prepare-a-yaml-configure-file"></a>Bir YAML hazırlama dosyasını yapılandırma

YAML kullanacağınız dosya Kubernetes kümesinde bilişsel hizmet dağıtımını basitleştirmek için yapılandırın.

İşte bir örnek YAML yapılandırma dosyası, Azure Stack için yüz hizmeti dağıtmak için:

```Yaml  
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: <deploymentName>
spec:
  replicas: <replicaNumber>
  template:
    metadata:
      labels:
        app: <appName>
    spec:
      containers:
      - name: <containersName>
        image: <ImageLocation>
        env: 
        - name: EULA
          value: accept 
        - name: Billing
          value: <billingURL> 
        - name: apikey
          value: <apiKey>
        tty: true
        stdin: true
        ports:
        - containerPort: 5000 
      imagePullSecrets:
        - name: <secretName>
---
apiVersion: v1
kind: Service
metadata:
  name: <LBName>
spec:
  type: LoadBalancer
  ports:
  - port: 5000
    targetPort : 5000
    name: <PortsName>
  selector:
    app: <appName>
```

Bu YAML içinde yapılandırma dosyası, bilişsel hizmet kapsayıcı görüntülerini Azure Container Registry'den almak için kullanılan gizli diziyi kullanın. Ve kullanım gizli dizi dosyasını kapsayıcının belirli bir çoğaltma dağıtılacak. Kullanıcıların bu hizmet dışarıdan erişebildiğinden emin olmak için bir yük dengeleyici oluşturabilir.

Anahtar alanları hakkındaki ayrıntıları:

| Alan | Notlar |
| --- | --- |
| replicaNumber | İlk çoğaltmaları oluşturmak için örnekleri tanımlar. Elbette dağıtımdan sonra ölçeklendirebilirsiniz. |
| ImageLocation | Belirli bir bilişsel hizmet kapsayıcı görüntüyü ACR konumunu gösterir. Örneğin, yüz tanıma hizmeti: `aicpppe.azurecr.io/microsoft/cognitive-services-face` |
| BillingURL |Uç nokta URL'si. adımda not ettiğiniz [Azure Kaynağı Oluştur](#create-azure-resources) |
| ApiKey | Abonelik anahtarı. adımda not ettiğiniz [Azure Kaynağı Oluştur](#create-azure-resources) |
| secretName | Gizli anahtar adı, yalnızca Create adımda not ettiğiniz secrete özel kapsayıcı kayıt defterine erişim izni |

## <a name="deploy-the-cognitive-service"></a>Bilişsel hizmet dağıtma

Bilişsel hizmet kapsayıcıları dağıtmak için aşağıdaki komutu kullanımı

```bash  
    Kubectl apply -f <yamlFineName>
```
Nasıl dağıttığı izlemek için aşağıdaki komutu kullanın: 
```bash  
    Kubectl get pod – watch
```

## <a name="test-the-cognitive-service"></a>Bilişsel hizmet test

Erişim [Openapı belirtimi](https://swagger.io/docs/specification/about/) (eski adıyla Swagger belirtimi) den örneklenmiş bir kapsayıcı tarafından desteklenen işlemleri açıklayan **/swagger** bu kapsayıcı için göreli URI. Örneğin, aşağıdaki URI, önceki örnekte örneklenmiş yaklaşım analizi kapsayıcı Openapı belirtimi için erişim sağlar:

```HTTP  
http:<External IP>:5000/swagger
```

Aşağıdaki komutu dış IP adresini alabilirsiniz: 

```bash  
    Kubectl get svc <LoadBalancerName>
```

## <a name="try-the-services-with-python"></a>Python ile Hizmetleri deneyin

Bilişsel hizmetler, Azure Stack hakkında bazı basit Python betiklerini çalıştırarak doğrulamak deneyebilirsiniz. Resmi Python hızlı başlangıç örnekleri vardır [görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), [yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview), ve [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview), ve [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto) () LUIS) başvuru amacıyla.

Kapsayıcıları üzerinde çalışan hizmetleri çalıştırmanızı Python uygulaması yapmak için göz önünde iki şey gerek vardır: 
1. Bilişsel hizmetler kapsayıcısında kimlik doğrulaması için alt anahtarları gerek yoktur ancak biz yine de bir SDK'sı karşılamak için bir yer tutucu olarak herhangi bir dize girişi gerekir. 
2. Base_URL gerçek bir hizmet uç noktası IP adresiyle değiştirin 

Örnek Python betiği kullanılan yüz tanıma Hizmetleri algılamak ve bir görüntüdeki yüzleri çerçeve için Python SDK'sı aşağıda verilmiştir. 

```Python  
import cognitive_face as CF

# Cognitive Services in container do not need sub keys for authentication
# Keep the invalid key to satisfy the SDK inputs requirement. 
KEY = '0'  #  (keeping the quotes in place).
CF.Key.set(KEY)

# Get your actual Ip Address of service endpoint of your cognitive service on Azure Stack
BASE_URL = 'http://<svc IP Address>:5000/face/v1.0/'  
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)

```

## <a name="next-steps"></a>Sonraki adımlar

[Yükleme ve görüntü işleme API'si kapsayıcıları çalıştırmak nasıl.](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-how-to-install-containers)

[Yükleme ve yüz tanıma API'si kapsayıcıları çalıştırın](https://docs.microsoft.com/azure/cognitive-services/face/face-how-to-install-containers)

[Yükleme ve metin analizi API'si kapsayıcıları çalıştırın](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)

[Yükleme ve dil anlama (LIUS) kapsayıcıları çalıştırın](https://docs.microsoft.com/azure/cognitive-services/luis/luis-container-howto)
