---
title: SSL kullanarak güvenli web Hizmetleri
titleSuffix: Azure Machine Learning service
description: HTTPS'yi etkinleştirerek Azure Machine Learning hizmeti aracılığıyla dağıtılan bir web hizmeti güvenli hale getirme hakkında bilgi edinin. HTTPS, Aktarım Katmanı Güvenliği (TLS) yerine Güvenli Yuva Katmanı (SSL) kullanarak verileri istemciler tarafından güvenliğini sağlar. İstemciler HTTPS web hizmetinin kimliğini doğrulamak için de kullanır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: c176458cfc404a9d55d7fb71a36ea63110b3a6d6
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657962"
---
# <a name="use-ssl-to-secure-a-web-service-through-azure-machine-learning"></a>Azure Machine Learning aracılığıyla bir web hizmeti güvenli hale getirmek için SSL kullan

Bu makalede, Azure Machine Learning hizmeti ile dağıtılmış bir web hizmeti güvenli hale getirme işlemini göstermektedir.

Kullandığınız [HTTPS](https://en.wikipedia.org/wiki/HTTPS) web hizmetlerine erişimi kısıtlayabilir ve istemcilerin gönderme verilerin güvenliğini sağlamak için. HTTPS, ikisi arasındaki iletişimleri şifrelemek tarafından bir istemci ve web hizmeti arasında güvenli iletişim yardımcı olur. Şifreleme kullanan [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS hala bazen olarak adlandırılır *Güvenli Yuva Katmanı* (SSL) olduğu TLS öncülü.

> [!TIP]
> Azure Machine Learning SDK'sı, iletişimlerin güvenliğini sağlamak için ilgili özellikleri için "SSL" terimini kullanır. Bu web hizmetini kullanmaz gelmez *TLS*. SSL daha yaygın olarak tanınan bir terim kullanılır.

TLS ve SSL hem de bağımlı *dijital sertifikalar*, şifreleme ve kimlik doğrulama ile yardımcı olur. Dijital sertifikalar çalışma hakkında daha fazla bilgi için Wikipedia konusuna [ortak anahtar altyapısı](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!WARNING]
> Web hizmetiniz için HTTPS kullanmazsanız, gönderilen ve hizmetten veri internet'te başkalarına görünür olabilir.
>
> HTTPS sunucusunun bağlandığı orjinalliği doğrulamak istemci de sağlar. Bu özelliği istemcilerin karşı koruyan [adam-de-ADAM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) saldırıları.

Bu güvenli bir web hizmeti için genel süreç.

1. Bir etki alanı adını alın.

2. Bir dijital sertifika alın.

3. Dağıtmak veya web hizmeti SSL etkin ile güncelleştirin.

4. Web hizmetine işaret edecek şekilde güncelleştirin.

> [!IMPORTANT]
> Azure Kubernetes Service (AKS) dağıtıyorsanız, kendi sertifika satın alabilir veya Microsoft tarafından sağlanan bir sertifika kullanın. Microsoft gelen bir sertifika kullanıyorsanız, bir etki alanı adı veya SSL sertifikası alın gerekmez. Daha fazla bilgi için [SSL etkinleştirmek ve dağıtmak](#enable) bu makalenin.

Web hizmetleri arasında güvenli hale getirdiğinizde davranışlarına olan [dağıtım hedefleri](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Bir etki alanı adı

Bir etki alanı adı zaten sahibi siz değilseniz, bir satın bir *etki alanı adı kayıt şirketi*. İşlem ve fiyat her Kaydedici farklılık gösterir. Kayıt şirketi, etki alanı adı yönetmek için araçlar sağlar. Bir tam etki alanı adı (FQDN) eşlemek için bu araçları kullanın (www gibi\.contoso.com) web hizmetini barındıran IP adresi.

## <a name="get-an-ssl-certificate"></a>Bir SSL sertifikası alma

Bir SSL sertifikası (dijital sertifika) almak için birçok yolu vardır. En sık karşılaşılan bir satın almaktır bir *sertifika yetkilisi* (CA). Sertifika nereden bağımsız olarak aşağıdaki dosyaları gerekir:

* A **sertifika**. Sertifikayı tüm sertifika zincirine içermelidir ve "PEM kodlu." olmalıdır
* A **anahtar**. Anahtar de PEM kodlu olmalıdır.

Bir sertifika talep ettiğinizde, web hizmeti için kullanmayı planladığınız adresinin FQDN sağlamanız gerekir (örneğin, www\.contoso.com). Web hizmeti kimliğini doğrulamak için sertifikayı damgalandı adresini ve istemcilerin kullandığı adresini karşılaştırılır. Bu adresler eşleşmiyorsa, istemci bir hata iletisi alır.

> [!TIP]
> Sertifika yetkilisi sertifika ve anahtarı sağlayamazsa ve PEM kodlu dosyaları olarak gibi bir yardımcı programını kullanın [OpenSSL](https://www.openssl.org/) biçimini değiştirmek için.

> [!WARNING]
> Kullanım *otomatik olarak imzalanan* sertifikalar yalnızca geliştirme için. Bunları üretim ortamında kullanmayın. Otomatik olarak imzalanan sertifikaları, uygulamaları istemcinizde sorunlara neden olabilir. Daha fazla bilgi için İstemci uygulamanızın kullandığı ağ kitaplıkları için belgelere bakın.

## <a id="enable"></a> SSL etkinleştirmek ve dağıtmak

Dağıtın (veya yeniden için) SSL etkin hizmetiyle ayarlayın *ssl_enabled* parametresi için "True" uygulanabilir olduğunda. Ayarlama *ssl_certificate* parametre değerine *sertifika* dosya. Ayarlama *ssl_key* değerine *anahtarı* dosya.

### <a name="deploy-on-aks-and-field-programmable-gate-array-fpga"></a>AKS ve alanda programlanabilen geçit dizileri (FPGA) dağıtma

  > [!NOTE]
  > Bu bölümdeki bilgiler, ayrıca, görsel arabirim güvenli web hizmeti dağıttığınızda geçerlidir. Python SDK'sını kullanmaya alışık değilseniz, bkz. [Python için Azure Machine Learning SDK'sı nedir?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

AKS için dağıttığınızda, yeni bir AKS kümesi oluşturma veya mevcut bir paylaşımın.
  
-  Yeni bir küme oluşturursanız, kullandığınız  **[AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none--cluster-purpose-none-)** .
- Mevcut bir kümeye ekleme, kullandığınız  **[AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)** . Her ikisi de sahip bir yapılandırma nesnesi döndürür bir **enable_ssl** yöntemi.

**Enable_ssl** yöntemi, Microsoft tarafından sağlanan bir sertifika veya satın aldığınız bir sertifika kullanabilirsiniz.

  * Microsoft gelen bir sertifika kullandığınızda, kullanmalısınız *leaf_domain_label* parametresi. Bu parametre, hizmetin DNS adı oluşturur. Örneğin, bir etki alanı adı "myservice" değerini oluşturur "myservice\<altı rastgele-karakterler >.\< azureregion >. cloudapp.azure.com "burada \<azureregion > hizmeti içeren bölgedir. İsteğe bağlı olarak kullanabileceğiniz *overwrite_existing_domain* parametresi var olanın üzerine yaz *leaf_domain_label*.

    Dağıtın (veya yeniden için) SSL etkin hizmetiyle ayarlayın *ssl_enabled* parametresi için "True" uygulanabilir olduğunda. Ayarlama *ssl_certificate* parametre değerine *sertifika* dosya. Ayarlama *ssl_key* değerine *anahtarı* dosya.

    > [!IMPORTANT]
    > Microsoft gelen bir sertifika kullandığınızda, kendi sertifika veya etki alanı adı satın almanız gerekmez.

    Aşağıdaki örnek bir SSL sertifikası Microsoft gelen sağlayan bir yapılandırmasının nasıl oluşturulacağını gösterir:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(leaf_domain_label = "myservice")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                          cluster_name = cluster_name)
    attach_config.enable_ssl(leaf_domain_label = "myservice")
    ```

  * Kullanırken *satın aldığınız bir sertifika*, kullandığınız *ssl_cert_pem_file*, *ssl_key_pem_file*, ve *ssl_cname* Parametreler. Aşağıdaki örnek nasıl kullanılacağını gösterir *.pem* satın aldığınız SSL sertifikası kullanan yapılandırması oluşturacak şekilde dosyaları:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
    attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

Hakkında daha fazla bilgi için *enable_ssl*, bkz: [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) ve [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

### <a name="deploy-on-azure-container-instances"></a>Azure Container Instances üzerinde dağıtın

Azure Container Instances'a dağıttığınızda, değerler aşağıdaki kod parçacığı gösterildiği gibi SSL ile ilgili parametreler için sağlar:

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
```

Daha fazla bilgi için [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-).

## <a name="update-your-dns"></a>Güncelleştirin

Ardından, DNS sunucunuzun web hizmetine işaret edecek şekilde güncelleştirmeniz gerekir.

+ **Container Instances için:**

  Etki alanı adınız için DNS kaydı güncelleştirmek için etki alanı adı kayıt araçları kullanın. Kayıt hizmetinin IP adresine işaret etmelidir.

  Dakika veya saat istemciler etki alanı adı için yapılandırılmış kayıt şirketi ve "yaşam süresi" bağlı olarak etki alanı adı (TTL) çözümleyebilir önce bir gecikme olabilir.

+ **AKS için:**

  > [!WARNING]
  > Kullandıysanız *leaf_domain_label* Microsoft gelen bir sertifika kullanarak bir hizmet oluşturmak için el ile küme DNS değeri güncelleştirmez. Değer otomatik olarak ayarlanması gerekir.

  DNS güncelleştirme **yapılandırma** AKS kümesi genel IP adresini sekmesi. (Aşağıdaki resme bakın). Genel IP adresini AKS aracı düğümleri ve diğer ağ kaynakları içeren kaynak grubu altında oluşturulan bir kaynak türüdür.

  ![Azure Machine Learning hizmeti: Web Hizmetleri SSL ile güvenli hale getirme](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Sonraki adımlar
Şunları nasıl yapacağınızı öğrenin:
+ [Bir machine learning web hizmeti olarak modeli kullanma](how-to-consume-web-service.md)
+ [Denemeler ve bir Azure sanal ağ içindeki çıkarım güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
