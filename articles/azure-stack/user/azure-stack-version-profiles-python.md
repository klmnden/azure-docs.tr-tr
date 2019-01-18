---
title: Azure stack'teki Python API sürümü profillerini kullanarak | Microsoft Docs
description: Azure stack'teki Python ile API Sürüm profillerini kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: 8049db848e34b0aa9bc23f08169a8c63f765791a
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389744"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>Azure stack'teki Python ile API Sürüm profillerini kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="python-and-api-version-profiles"></a>Python ve API sürümü profillerini

Python SDK'sı, Azure yığını ve genel Azure gibi farklı bulut platformları hedeflemek için API sürümü profillerini destekler. Hibrit bulut çözümleri oluşturma API profillerini kullanabilirsiniz. Python SDK'sı aşağıdaki API profillerini destekler:

- **en son**  
    Bu profil, tüm hizmet sağlayıcıları Azure platformundaki için en son API sürümlerini hedefler.
- **2018-03-01-karma**     
    Bu profil, Azure Stack platformda tüm kaynak sağlayıcıları için en son API sürümlerini hedefler.
- **2017-03-09-profile**  
    Bu profil, Azure Stack tarafından desteklenen kaynak sağlayıcıları en uyumlu API sürümlerini hedefler.

   API profilleri ve Azure Stack hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

## <a name="install-the-azure-python-sdk"></a>Azure Python SDK'sını yükleme

1. Git'ten yükleme [resmi sitesi](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2. Yönergeler Python SDK'sını yüklemek için bkz: [Python geliştiricileri için Azure](/python/azure/python-sdk-azure-install?view=azure-python).
3. Yoksa, bir abonelik oluşturur ve daha sonra kullanmak üzere abonelik Kimliğini kaydedin. Abonelik oluşturma ile ilgili yönergeler için bkz: [Azure Stack'te teklifleri abonelikleri oluşturma](../azure-stack-subscribe-plan-provision-vm.md).
4. Hizmet sorumlusu oluşturma ve onun Kimliğini ve parolasını kaydedin. Azure Stack için hizmet sorumlusu oluşturma hakkında yönergeler için bkz: [uygulamalar erişim sağlamak için Azure Stack](../azure-stack-create-service-principals.md).
5. Hizmet sorumlunuzu aboneliğinizde katkıda bulunan/sahip rolü olduğundan emin olun. Hizmet sorumlusu için rol atama hakkında yönergeler için bkz: [uygulamalar erişim sağlamak için Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Önkoşullar

Azure Python SDK'sı, Azure Stack ile kullanmak için aşağıdaki değerleri girin ve ardından ortam değişkenleriyle değerleri ayarlayın. Tablodan sonra sağlanan işletim sistemi ortam değişkenlerini ayarlama konusunda yönergelere bakın.

| Değer | Ortam değişkenleri | Açıklama |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| Kiracı Kimliği | AZURE_TENANT_ID | Azure Stack değerini [Kiracı kimliği](../azure-stack-identity-overview.md). |
| İstemci Kimliği | AZURE_CLIENT_ID | Hizmet sorumlusu uygulama kimliği bu makalenin önceki bölümde hizmet sorumlusu oluşturulurken kaydedilen. |
| Abonelik Kimliği | AZURE_SUBSCRIPTION_ID | [Abonelik kimliği](../azure-stack-plan-offer-quota-overview.md#subscriptions) nasıl, teklifler eriştiği Azure Stack'te. |
| İstemci Gizli Anahtarı | AZURE_CLIENT_SECRET | Hizmet sorumlusu oluşturulurken kaydedilen hizmet sorumlusu uygulama gizli anahtarı. |
| Resource Manager uç noktası | ARM_ENDPOINT | Bkz: [Azure Stack Kaynak Yöneticisi uç noktası](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |
| Kaynak Konumu | AZURE_RESOURCE_LOCATION | Azure Stack ortamınıza kaynak konumu.

## <a name="python-samples-for-azure-stack"></a>Azure Stack için Python örnekleri

Python SDK'sını kullanarak Azure Stack için kullanılabilir kod örnekleri bazıları şunlardır:

- [Kaynakları ve kaynak gruplarını yönetme](https://azure.microsoft.com/resources/samples/hybrid-resourcemanager-python-manage-resources/).
- [Depolama hesabı yönetme](https://azure.microsoft.com/resources/samples/hybrid-storage-python-manage-storage-account/).
- [Sanal makineleri yönetme](https://azure.microsoft.com/resources/samples/hybrid-compute-python-manage-vm/).

## <a name="python-manage-virtual-machine-sample"></a>Sanal makine örnek Python'ı yönetme

Aşağıdaki kod örneği, genel yönetim görevleri için sanal makineler, Azure Stack'te gerçekleştirmek için kullanabilirsiniz. Kod örneği gösterilir:

- Sanal makineler oluşturun:
  - Linux sanal makinesi oluşturma
  - Windows sanal makinesi oluşturma
- Bir sanal makineyi güncelleştir:
  - Bir sürücüyü genişletin
  - Bir sanal makine etiketi
  - Veri diski ekleme
  - Veri diskini çıkarma
- Bir sanal makine çalışır:
  - Bir sanal makineyi Başlat
  - Sanal makineyi durdurma
  - Bir sanal makineyi yeniden başlatın
- Sanal makineler listesi
- Sanal makineyi silme

Bu işlemler gerçekleştiren kodu gözden geçirmek için bkz. **run_example()** Python betiğini işlevinde **example.py** GitHub deposunda [karma-Compute-Python-yönetme-VM](https://github.com/Azure-Samples/Hybrid-Compute-Python-Manage-VM).

Her işlem, açıkça bir açıklama ve yazdırma işlevi ile etiketlenir. Örnekler mutlaka bu listede gösterilen sırada değildir.

## <a name="run-the-python-sample"></a>Python örneği çalıştırma

1. Önceden, varsa [Python yükleme](https://www.python.org/downloads/). Bu örnek (ve SDK'sı), 3.4, 3.5 ve 3.6 gibi Python 2.7 ile uyumludur.

2. Python geliştirme için genel bir öneri, bir sanal ortam kullanmaktır. Daha fazla bilgi için [Python belgeleri](https://docs.python.org/3/tutorial/venv.html).

3. Yükleme ve Python 3'te "venv" modülü ile sanal ortamı başlatın (yüklemelisiniz [virtualenv](https://pypi.python.org/pypi/virtualenv) Python 2.7 için):

    ```bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ```

4. Deposunu kopyalayın:

    ```bash
    git clone https://github.com/Azure-Samples/Hybrid-Compute-Python-Manage-VM.git
    ```

5. PIP kullanarak bağımlılıkları yükler:

    ```bash
    cd Hybrid-Compute-Python-Manage-VM
    pip install -r requirements.txt
    ```

6. Oluşturma bir [hizmet sorumlusu](../azure-stack-create-service-principals.md) Azure Stack ile çalışmak için. Hizmet sorumlunuzu sahip olduğundan emin olun [katkıda bulunan/sahip rolü](../azure-stack-create-service-principals.md#assign-a-role) aboneliğinizde.

7. Aşağıdaki değişkenleri ayarlamak ve bu ortam değişkenlerini geçerli kabuğunuzun içinde dışarı aktarın:

    ```bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    export AZURE_RESOURCE_LOCATION={your AzureStack Resource location}
    ```

8. Bu örneği çalıştırmak için Ubuntu 16.04 LTS ve Windows Server 2012 R2 Datacenter görüntüleri Azure Stack marketini mevcut olması gerekir. Bunlar olabilir [Azure'dan indirilen](../azure-stack-download-azure-marketplace-item.md), ya da eklenen [Platform görüntü deposuna](../azure-stack-add-vm-image.md).

9. Örneği çalıştırın:

    ```python
    python example.py
    ```


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Python Geliştirme Merkezi](https://azure.microsoft.com/develop/python/)
- [Azure sanal makineler belgeleri](https://azure.microsoft.com/services/virtual-machines/)
- [Sanal makineler için öğrenme yolu](/learn/paths/deploy-a-website-with-azure-virtual-machines/)
- Bir Microsoft Azure aboneliğiniz yoksa, ücretsiz bir deneme hesabı alabilirsiniz [burada](https://go.microsoft.com/fwlink/?LinkId=330212).
