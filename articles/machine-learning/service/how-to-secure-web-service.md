---
title: SSL ile web hizmetlerinin güvenliğini sağlama
titleSuffix: Azure Machine Learning service
description: HTTPS'yi etkinleştirerek Azure Machine Learning hizmeti ile dağıtılmış bir web hizmeti güvenli hale getirme hakkında bilgi edinin. HTTPS yerine Güvenli Yuva Katmanı (SSL) için Aktarım Katmanı Güvenliği (TLS) kullanan istemciler tarafından gönderilen verilerin güvenliğini sağlar. Web hizmeti kimliğini doğrulamak için istemciler tarafından da kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 1a6aa75f3d25cd88cd1edb9b2cdcfabc3b4ec8f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60818523"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile web hizmetlerinin güvenliğini sağlamak için SSL kullan

Bu makalede, Azure Machine Learning hizmeti ile dağıtılmış bir web hizmeti güvenli hale getirmeyi öğreneceksiniz. Web hizmetlerine erişimi kısıtlama ve kullanan istemciler tarafından gönderilen verilerin güvenliğini [Köprü Metni Aktarım Protokolü güvenli (HTTPS)](https://en.wikipedia.org/wiki/HTTPS).

HTTPS, ikisi arasındaki iletişimleri şifrelemek tarafından web hizmetiniz ile bir istemci arasındaki iletişimin güvenliğini sağlamak için kullanılır. Şifreleme kullanarak işlenir [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). Bazen bu hala olarak Güvenli Yuva Katmanı (TLS öncülü olan SSL), adlandırılır.

> [!TIP]
> Azure Machine Learning SDK özelliklerinin ' SSL güvenli iletişim etkinleştirme ile ilgili' terimini kullanır. Bu gelmez TLS web hizmetiniz tarafından kullanılmaz, yalnızca bir SSL birçok okuyucular için daha fazla tanınabilir bir terimdir.

TLS ve SSL hem de bağımlı __dijital sertifikalar__, şifreleme ve kimlik doğrulama gerçekleştirmek için kullanılır. Dijital sertifikalar çalışma hakkında daha fazla bilgi için üzerinde Wikipedia girişine bakın. [ortak anahtar altyapısı (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!Warning]
> Etmez etkinleştirmek ve web hizmetiniz için HTTPS kullanmak, gönderilen ve alınan hizmet veri İnternette diğer açın görünür olabilir.
>
> HTTPS sunucusunun bağlandığı orjinalliği doğrulamak istemci de sağlar. Bu istemciler karşı korur [adam-de-ADAM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) saldırıları.

Yeni bir web hizmeti veya var olan bir güvenli hale getirme işlemi aşağıdaki gibidir:

1. Bir etki alanı adını alın.

2. Bir dijital sertifika alın.

3. Dağıtmak veya web hizmeti SSL ayar etkin güncelleştirin.

4. Web hizmetine işaret edecek şekilde güncelleştirin.

Web hizmetleri arasında güvenli hale getirirken davranışlarına olan [dağıtım hedefleri](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Bir etki alanı adı

Zaten bir etki alanı adına sahip olmadığınız, birinden, satın alabileceğiniz bir __etki alanı adı kayıt şirketi__. Maliyeti gibi işlemi kaydedicilerin arasında farklılık gösterir. Kayıt şirketi, araçlar da ile etki alanı adı yönetmek için sağlar. Bu araçlar, tam etki alanı adını eşleştirmek için kullanılır (www gibi\.contoso.com), web hizmetini barındıran IP adresi.

## <a name="get-an-ssl-certificate"></a>Bir SSL sertifikası alma

Bir SSL sertifikası (dijital sertifika) almak için birçok yolu vardır. En sık karşılaşılan bir satın almaktır bir __sertifika yetkilisi__ (CA). Sertifika burada elde bağımsız olarak, aşağıdaki dosyaları gerekir:

* A __sertifika__. Sertifikayı tüm sertifika zincirine içermelidir ve PEM kodlu olmalıdır.
* A __anahtar__. Bu anahtar, PEM kodlu olmalıdır.

Sertifika isterken, web hizmeti için kullanmayı planladığınız adresinin tam etki alanı adı (FQDN) sağlamalısınız. Örneğin, www\.contoso.com. Sertifikayı damgalı adresi ve istemciler tarafından kullanılan adres, web hizmeti kimliğini doğrularken karşılaştırılır. Adresleri eşleşmiyorsa, istemcilerin bir hata alırsınız.

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

+ **Alanda programlanabilir kapı dizileri (FPGA) dağıtma**

  FPGA için dağıtımı sırasında SSL ile ilgili parametreler için kod parçacığında gösterilen değerleri sağlayın:

    ```python
    from azureml.contrib.brainwave import BrainwaveWebservice

    deployment_config = BrainwaveWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem")
    ```

## <a name="update-your-dns"></a>Güncelleştirin

Ardından, DNS sunucunuzun web hizmetine işaret edecek şekilde güncelleştirmeniz gerekir.

+ **ACI için**:

  Etki alanı adınız için DNS kaydı güncelleştirmek için etki alanı adı kayıt şirketi tarafından sağlanan araçları kullanın. Kayıt hizmetinin IP adresine işaret etmelidir.

  Bağlı olarak kayıt şirketi ve süresi (TTL) canlı etki alanı adı için yapılandırılmış, istemciler etki alanı adı çözümleyebilir önce birkaç saate kadar birkaç dakika sürebilir.

+ **AKS için**:

  "Genel IP adresi" görüntüde gösterildiği gibi AKS kümesi "Yapılandırma" sekmesi altındaki DNS güncelleştirin. AKS aracı düğümleri ve diğer ağ kaynakları içeren kaynak grubu altında oluşturulan kaynak türlerini biri olarak genel IP adresini bulabilirsiniz.

  ![Azure Machine Learning hizmeti: Web Hizmetleri SSL ile güvenli hale getirme](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Sonraki adımlar
Şunları nasıl yapacağınızı öğrenin:
+ [Bir machine learning web hizmeti olarak modeli kullanma](how-to-consume-web-service.md)
+ [Denemeler ve bir Azure sanal ağ içindeki çıkarım güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
