---
title: SSL ile güvenli web Hizmetleri
titleSuffix: Azure Machine Learning service
description: HTTPS'yi etkinleştirerek Azure Machine Learning hizmeti ile dağıtılmış bir web hizmeti güvenli hale getirme hakkında bilgi edinin. HTTPS yerine Güvenli Yuva Katmanı (SSL) için Aktarım Katmanı Güvenliği (TLS) kullanan istemciler tarafından gönderilen verilerin güvenliğini sağlar. Web hizmeti kimliğini doğrulamak için istemciler tarafından da kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 527f16e34e0f21d435fbd166328235566687bc88
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65852016"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile web hizmetlerinin güvenliğini sağlamak için SSL kullan

Bu makalede, Azure Machine Learning hizmeti ile dağıtılmış bir web hizmeti güvenli hale getirmeyi öğreneceksiniz. Web hizmetlerine erişimi kısıtlama ve kullanan istemciler tarafından gönderilen verilerin güvenliğini [Köprü Metni Aktarım Protokolü güvenli (HTTPS)](https://en.wikipedia.org/wiki/HTTPS).

HTTPS, ikisi arasındaki iletişimleri şifrelemek tarafından web hizmetiniz ile bir istemci arasındaki iletişimin güvenliğini sağlamak için kullanılır. Şifreleme kullanarak işlenir [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). Bazen TLS hala olarak Güvenli Yuva Katmanı (TLS öncülü olan SSL), adlandırılır.

> [!TIP]
> Azure Machine Learning SDK özelliklerinin ' SSL güvenli iletişim etkinleştirme ile ilgili' terimini kullanır. Bu gelmez TLS web hizmetiniz tarafından kullanılmaz, yalnızca bir SSL birçok okuyucular için daha fazla tanınabilir bir terimdir.

TLS ve SSL hem de bağımlı __dijital sertifikalar__, şifreleme ve kimlik doğrulama gerçekleştirmek için kullanılır. Dijital sertifikalar çalışma hakkında daha fazla bilgi için üzerinde Wikipedia girişine bakın. [ortak anahtar altyapısı (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!Warning]
> Etmez etkinleştirmek ve web hizmetiniz için HTTPS kullanmak, gönderilen ve alınan hizmet veri İnternette diğer açın görünür olabilir.
>
> HTTPS sunucusunun bağlandığı orjinalliği doğrulamak istemci de sağlar. Bu istemciler karşı korur [adam-de-ADAM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) saldırıları.

Yeni bir web hizmeti veya mevcut bir güvenlik altına almanın genel süreç aşağıdaki gibidir:

1. Bir etki alanı adını alın.

2. Bir dijital sertifika alın.

3. Dağıtmak veya web hizmeti SSL ayar etkin güncelleştirin.

4. Web hizmetine işaret edecek şekilde güncelleştirin.

> [!IMPORTANT]
> Azure Kubernetes Service (AKS) dağıtıyorsanız, kendi sertifikanızı sağlamanız ya da Microsoft tarafından sağlanan bir sertifika kullanın. Belirtilen sertifika Microsoft kullanırsanız, bir etki alanı adı veya SSL sertifikası alın gerekmez. Daha fazla bilgi için [SSL etkinleştirmek ve dağıtmak](#enable) bölümü.

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

## <a id="enable"></a> SSL etkinleştirmek ve dağıtmak

Dağıtın (veya yeniden için) SSL etkin hizmetiyle ayarlayın `ssl_enabled` parametresi `True`, uygun olan her yerde. Ayarlama `ssl_certificate` parametre değerine __sertifika__ dosya ve `ssl_key` değerine __anahtarı__ dosya.

+ **Azure Kubernetes Service'i (AKS) üzerinde dağıtın ve FPGA**

  > [!NOTE]
  > Bu bölümdeki bilgiler, görsel arabirim güvenli web hizmeti dağıtırken de geçerlidir. Python SDK'ın kullanımıyla ilgili bilgi sahibi değilseniz bkz [Azure Machine Learning Python SDK'sı genel bakış.](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

  AKS için dağıtırken, yeni bir AKS kümesi oluşturabilir veya mevcut bir paylaşımın. Kullanan yeni bir küme oluşturma [AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none-) mevcut bir kümeye kullanırken [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none-). Her ikisi de sahip bir yapılandırma nesnesi döndürür bir `enable_ssl` yöntemi.

  `enable_ssl` Yöntemi ya da Microsoft veya sağladığınız biri tarafından sağlanan bir sertifika kullanın.

  * Bir sertifika kullanırken __Microsoft tarafından sağlanan__, kullanmanız gereken `leaf_domain_label` parametresi. Bu parametre kullanarak, Microsoft tarafından sağlanan bir sertifika kullanarak hizmeti oluşturacaksınız. `leaf_domain_label` Hizmetin DNS adı oluşturmak için kullanılır. Örneğin, bir değeri `myservice` bir etki alanı adını oluşturur `myservice<6-random-characters>.<azureregion>.cloudapp.azure.com`burada `<azureregion>` hizmeti içeren bölgedir. İsteğe bağlı olarak kullanabileceğiniz `overwrite_existing_domain` var olan bir yaprak etki alanı etiketi üzerine yazmak için parametre.

    Dağıtın (veya yeniden dağıtmak için) SSL etkin hizmetiyle ayarlayın `ssl_enabled` parametresi `True`, uygun olan her yerde. Ayarlama `ssl_certificate` parametre değerine __sertifika__ dosya ve `ssl_key` değerine __anahtarı__ dosya.

    > [!IMPORTANT]
    > Microsoft tarafından sağlanan bir sertifika kullanırken, kendi sertifika veya etki alanı adı satın almanız gerekmez.

    Aşağıdaki örnek, Microsoft tarafından oluşturulan SSL sertifikasının etkinleştirin yapılandırmalarının nasıl oluşturulacağını gösterir:

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

  * Kullanırken __satın aldığınız bir sertifika__, kullanın `ssl_cert_pem_file`, `ssl_key_pem_file`, ve `ssl_cname` parametreleri. Aşağıdaki örnek, sağladığınız kullanarak SSL sertifikası kullanma yapılandırmalarının nasıl oluşturulacağını gösterir `.pem` dosyaları:

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

  Daha fazla bilgi için `enable_ssl`, bkz: [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) ve [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

+ **Azure Container Instances (ACI) üzerinde dağıtın**

  ACI'ya dağıtma sırasında SSL ile ilgili parametreler için kod parçacığında gösterilen değerleri sağlayın:

    ```python
    from azureml.core.webservice import AciWebservice

    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

  Daha fazla bilgi için [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none-).

## <a name="update-your-dns"></a>Güncelleştirin

Ardından, DNS sunucunuzun web hizmetine işaret edecek şekilde güncelleştirmeniz gerekir.

+ **ACI için**:

  Etki alanı adınız için DNS kaydı güncelleştirmek için etki alanı adı kayıt şirketi tarafından sağlanan araçları kullanın. Kayıt hizmetinin IP adresine işaret etmelidir.

  Bağlı olarak kayıt şirketi ve süresi (TTL) canlı etki alanı adı için yapılandırılmış, istemciler etki alanı adı çözümleyebilir önce birkaç saate kadar birkaç dakika sürebilir.

+ **AKS için**:

  > [!WARNING]
  > Kullandıysanız `leaf_domain_label` Microsoft tarafından sağlanan bir sertifika ile hizmet oluşturmak için el ile küme DNS değeri güncelleştirmez. Değer otomatik olarak ayarlanması gerekir.

  "Genel IP adresi" görüntüde gösterildiği gibi AKS kümesi "Yapılandırma" sekmesi altındaki DNS güncelleştirin. AKS aracı düğümleri ve diğer ağ kaynakları içeren kaynak grubu altında oluşturulan kaynak türlerini biri olarak genel IP adresini bulabilirsiniz.

  ![Azure Machine Learning hizmeti: Web Hizmetleri SSL ile güvenli hale getirme](./media/how-to-secure-web-service/aks-public-ip-address.png)



## <a name="next-steps"></a>Sonraki adımlar
Şunları nasıl yapacağınızı öğrenin:
+ [Bir machine learning web hizmeti olarak modeli kullanma](how-to-consume-web-service.md)
+ [Denemeler ve bir Azure sanal ağ içindeki çıkarım güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)

