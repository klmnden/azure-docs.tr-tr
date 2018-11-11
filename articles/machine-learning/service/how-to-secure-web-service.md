---
title: Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme
description: Azure Machine Learning hizmeti ile dağıtılmış bir web hizmeti güvenli hale getirme hakkında bilgi edinin. Web hizmetlerine erişimi kısıtlama ve Güvenli Yuva Katmanı (SSL) kullanan istemciler tarafından gönderilen verilerin güvenliğini sağlamak ve anahtar tabanlı kimlik doğrulaması.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 10/02/2018
ms.openlocfilehash: ec7b956f080837b297bac56e6237ac0672601ce7
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344493"
---
# <a name="secure-azure-machine-learning-web-services-with-ssl"></a>Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme

Bu makalede, Azure Machine Learning hizmeti ile dağıtılmış bir web hizmeti güvenli hale getirmeyi öğreneceksiniz. Web hizmetlerine erişimi kısıtlama ve Güvenli Yuva Katmanı (SSL) kullanan istemciler tarafından gönderilen verilerin güvenliğini sağlamak ve anahtar tabanlı kimlik doğrulaması.

> [!Warning]
> SSL etkinleştirmezseniz, internet üzerindeki herhangi bir kullanıcı web hizmetine çağrı yapmak mümkün olacaktır.

SSL istemci ve web hizmeti arasında gönderilen verileri şifreler. Bu da sunucunun kimliğini doğrulamak için istemci tarafından kullanılmış. Kimlik doğrulaması, yalnızca bir SSL sertifikası ve anahtar sağladığınız Hizmetleri için etkindir.  SSL'yi etkinleştirirseniz, bir kimlik doğrulama anahtarı web hizmetine erişim gereklidir.

SSL ile etkin bir web hizmetini dağıtma veya mevcut dağıtılan web hizmeti için SSL'yi aynı adımlar şunlardır:

1. Bir etki alanı adını alın.

2. Bir SSL sertifikası alın.

3. Dağıtmak veya web hizmeti SSL ayar etkin güncelleştirin.

4. Web hizmetine işaret edecek şekilde güncelleştirin.

Web hizmetleri arasında güvenli hale getirirken davranışlarına olan [dağıtım hedefleri](how-to-deploy-and-where.md). 

## <a name="get-a-domain-name"></a>Bir etki alanı adı

Zaten bir etki alanı adına sahip olmadığınız, birinden, satın alabileceğiniz bir __etki alanı adı kayıt şirketi__. Maliyeti gibi işlemi kaydedicilerin arasında farklılık gösterir. Kayıt şirketi, araçlar da ile etki alanı adı yönetmek için sağlar. Bu araçlar, web hizmetini barındıran bir IP adresi için bir tam etki alanı adı (örneğin, www.contoso.com) eşlemek için kullanılır.

## <a name="get-an-ssl-certificate"></a>Bir SSL sertifikası alma

Bir SSL sertifikası almak için birçok yolu vardır. En sık karşılaşılan bir satın almaktır bir __sertifika yetkilisi__ (CA). Sertifika burada elde bağımsız olarak, aşağıdaki dosyaları gerekir:

* A __sertifika__. Sertifikayı tüm sertifika zincirine içermelidir ve PEM kodlu olmalıdır.
* A __anahtar__. Bu anahtar, PEM kodlu olmalıdır.

Sertifika isterken, web hizmeti için kullanmayı planladığınız adresinin tam etki alanı adı (FQDN) sağlamalısınız. Örneğin, www.contoso.com. Sertifikayı damgalı adresi ve istemciler tarafından kullanılan adres, web hizmeti kimliğini doğrularken karşılaştırılır. Adresleri eşleşmiyorsa, istemcilerin bir hata alırsınız. 

> [!TIP]
> Sertifika yetkilisi sertifika ve anahtarı sağlayamazsa ve PEM kodlu dosyaları olarak gibi bir yardımcı programını kullanın [OpenSSL](https://www.openssl.org/) biçimini değiştirmek için.

> [!WARNING]
> Otomatik olarak imzalanan sertifikalar yalnızca geliştirme için kullanılması gerekir. Bunlar üretim ortamında kullanılmamalıdır. Otomatik olarak imzalanan sertifikaları, uygulamaları istemcinizde sorunlara neden olabilir. Daha fazla bilgi için istemci uygulamanızda kullanılan ağ kitaplıkları için belgelere bakın.

## <a name="enable-ssl-and-deploy"></a>SSL etkinleştirmek ve dağıtmak

Dağıtın (veya yeniden dağıtmak için) SSL etkin hizmetiyle ayarlayın `ssl_enabled` parametresi `True`, uygun olan her yerde. Ayarlama `ssl_certificate` parametre değerine __sertifika__ dosya ve `ssl_key` değerine __anahtarı__ dosya. 

+ **Azure Kubernetes Service'i (AKS) üzerinde dağıtın**
  
  AKS kümesi sağlanırken, SSL ile ilgili parametreler için kod parçacığında gösterilen değerleri sağlayın:

    ```python
    from azureml.core.compute import AksCompute
    
    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Azure Container Instances (ACI) üzerinde dağıtın**
 
  ACI'ya dağıtma sırasında SSL ile ilgili parametreler için kod parçacığında gösterilen değerleri sağlayın:

    ```python
    from azureml.core.webservice import AciWebservice
    
    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Alanda programlanabilir kapı dizileri (FPGA) üzerinde dağıtın**

  Yanıtın `create_service` işlem hizmetinin IP adresini içeriyor. Hizmetin IP adresine DNS adı eşleme IP adresi kullanılır. Yanıtını da içeren bir __birincil anahtar__ ve __ikincil anahtar__ hizmeti kullanmak için kullanılır. SSL ile ilgili parametreler için kod parçacığında gösterilen değerleri sağlayın:

    ```python
    from amlrealtimeai import DeploymentClient
    
    subscription_id = "<Your Azure Subscription ID>"
    resource_group = "<Your Azure Resource Group Name>"
    model_management_account = "<Your AzureML Model Management Account Name>"
    location = "eastus2"
    
    model_name = "resnet50-model"
    service_name = "quickstart-service"
    
    deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)
    
    with open('cert.pem','r') as cert_file:
        with open('key.pem','r') as key_file:
            cert = cert_file.read()
            key = key_file.read()
            service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
    ```

## <a name="update-your-dns"></a>Güncelleştirin

Ardından, DNS sunucunuzun web hizmetine işaret edecek şekilde güncelleştirmeniz gerekir.

+ **ACI ve FPGA**:  

  Etki alanı adınız için DNS kaydı güncelleştirmek için etki alanı adı kayıt şirketi tarafından sağlanan araçları kullanın. Kayıt hizmetinin IP adresine işaret etmelidir.  

  Bağlı olarak kayıt şirketi ve süresi (TTL) canlı etki alanı adı için yapılandırılmış, istemciler etki alanı adı çözümleyebilir önce birkaç saate kadar birkaç dakika sürebilir.

+ **AKS için**: 

  "Genel IP adresi" görüntüde gösterildiği gibi AKS kümesi "Yapılandırma" sekmesi altındaki DNS güncelleştirin. AKS aracı düğümleri ve diğer ağ kaynakları içeren kaynak grubu altında oluşturulan kaynak türlerini biri olarak genel IP adresini bulabilirsiniz.

  ![Azure Machine Learning hizmeti: web hizmetleri SSL ile güvenli hale getirme](./media/how-to-secure-web-service/aks-public-ip-address.png)Self-

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [ML Model dağıtılan web hizmeti olarak Tüket](how-to-consume-web-service.md).