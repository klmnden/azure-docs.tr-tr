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
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: c55dcf0736642690f245f680db5cb1620c2175e7
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44390971"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>Azure stack'teki Python ile API Sürüm profillerini kullanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="python-and-api-version-profiles"></a>Python ve API sürümü profillerini

Python SDK'sı, Azure yığını ve genel Azure gibi farklı bulut platformları hedeflemek için API sürümü profillerini destekler. Hibrit bulut çözümleri oluşturma API profillerini kullanabilirsiniz. Python SDK'sı aşağıdaki API profillerini destekler:

1. **en son**  
    Profil, tüm hizmet sağlayıcıları Azure platformundaki için en son API sürümlerini hedefler.
2.  **2017-03-09-profile**  
    **2017-03-09-profile**  
    Profili Azure yığını tarafından desteklenen kaynak sağlayıcıları API sürümlerini hedefler.

    API profilleri ve Azure Stack hakkında daha fazla bilgi için bkz. [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

## <a name="install-azure-python-sdk"></a>Azure Python SDK’yı yükleme

1.  Git'ten yükleme [resmi sitesi](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2.  Python SDK'sını yüklemek yönergeler için bkz: [Python geliştiricileri için Azure](https://docs.microsoft.com/python/azure/python-sdk-azure-install?view=azure-python).
3.  Yoksa, bir abonelik oluşturur ve daha sonra kullanılmak üzere abonelik Kimliğini kaydedin. Bir abonelik oluşturmak yönergeler için bkz: [Azure Stack'te teklifleri abonelikleri oluşturma](../azure-stack-subscribe-plan-provision-vm.md). 
4.  Hizmet sorumlusu oluşturma ve onun Kimliğini ve parolasını kaydedin. Azure Stack için hizmet sorumlusu oluşturma yönergeleri için bkz. [uygulamalar erişim sağlamak için Azure Stack](../azure-stack-create-service-principals.md). 
5.  Hizmet sorumlusu, aboneliğinizde katkıda bulunan/sahip rolü olduğundan emin olun. Hizmet sorumlusu için rol atama hakkında yönergeler için bkz: [uygulamalar erişim sağlamak için Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Önkoşullar

Python Azure SDK'sı, Azure Stack ile kullanmak için aşağıdaki değerleri girin ve ardından ortam değişkenleriyle değerleri ayarlayın. Tablodan sonra sağlanan işletim sistemi ortam değişkenlerini ayarlama konusunda yönergelere bakın. 

| Değer | Ortam değişkenleri | Açıklama |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| Kiracı Kimliği | AZURE_TENANT_ID | Azure Stack değerini [Kiracı kimliği](../azure-stack-identity-overview.md). |
| İstemci Kimliği | AZURE_CLIENT_ID | Hizmet sorumlusu uygulama kimliği bu belgenin önceki bölümde üzerinde hizmet sorumlusu oluşturulurken kaydedilen. |
| Abonelik Kimliği | AZURE_SUBSCRIPTION_ID | [Abonelik kimliği](../azure-stack-plan-offer-quota-overview.md#subscriptions) nasıl, teklifler eriştiği Azure Stack'te. |
| İstemci Gizli Anahtarı | AZURE_CLIENT_SECRET | Hizmet sorumlusu oluştururken hizmet sorumlusu uygulama gizli anahtarı kaydedildi. |
| Resource Manager uç noktası | ARM_ENDPOINT | Bkz: [Azure Stack Kaynak Yöneticisi uç noktası](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |


## <a name="python-samples-for-azure-stack"></a>Azure Stack için Python örnekleri 

Aşağıdaki kod örnekleri, genel yönetim görevleri için sanal makineler, Azure Stack'te gerçekleştirmek için kullanabilirsiniz.

Kod örnekleri için Göster:

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

Bu işlemler gerçekleştirmeye yönelik kod gözden geçirmek için kullanıma **run_example()** Python betiğini işlevinde **Hybrid/unmanaged-disks/example.py** GitHub deposunda [ sanal makineler-python-yönetiminde](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88).

Her işlem, açıkça bir açıklama ve yazdırma işlevi ile etiketlenir.
Örnekler mutlaka yukarıdaki listede gösterilen sırada değildir.


## <a name="run-the-python-sample"></a>Python örneği çalıştırma

1.  Bu, zaten yoksa, [Python yükleme](https://www.python.org/downloads/).

    Bu örnek (ve SDK'sı), 3.4, 3.5 ve 3.6 gibi Python 2.7 ile uyumludur.

2.  Python geliştirme için genel bir öneri, bir sanal ortam kullanmaktır. 
    Daha fazla bilgi için bkz. https://docs.python.org/3/tutorial/venv.html
    
    Yükleme ve Python 3'te "venv" modülü ile sanal ortamı başlatın (yüklemelisiniz [virtualenv](https://pypi.python.org/pypi/virtualenv) Python 2.7 için):

    ````bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ````

3.  Depoyu kopyalayın.

    ````bash
    git clone https://github.com/Azure-Samples/virtual-machines-python-manage.git
    ````

4.  PIP kullanarak bağımlılıkları yükler.

    ````bash
    cd virtual-machines-python-manage\Hybrid
    pip install -r requirements.txt
    ````

5.  Oluşturma bir [hizmet sorumlusu](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) Azure Stack ile çalışmak için. Hizmet sorumlunuzu sahip olduğundan emin olun [katkıda bulunan/sahip rolü](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) aboneliğinizde.

6.  Aşağıdaki değişkenleri ayarlamak ve bu ortam değişkenlerini geçerli kabuğunuzun içinde dışarı aktarın. 

    ```bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  Bu örneği çalıştırmak için Ubuntu 16.04 LTS ve Windows Server 2012 R2 Datacenter görüntüleri Azure Stack Pazar yerinde mevcut olması gerekir. Bunlar olabilir [Azure'dan indirilen](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) veya [Platform görüntü deposuna eklendi](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image).

8. Örnek uygulamayı çalıştırın.

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>Notlar

Kullanarak bir sanal makinenin işletim sistemi diski almayı denemek için fikri size cazip gelebilir `virtual_machine.storage_profile.os_disk`.
Bazı durumlarda, bu, ancak size verir dikkat istediğinizi yapabilirsiniz bir `OSDisk` nesne.
İşletim sistemi Disk boyutu olarak güncelleştirmek için `example.py` , gerek yoktur bir `OSDisk` nesne ancak `Disk` nesne.
`example.py` alır `Disk` aşağıdaki nesnesi:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Python Geliştirme Merkezi](https://azure.microsoft.com/develop/python/)
- [Azure sanal makineler belgeleri](https://azure.microsoft.com/services/virtual-machines/)
- [Sanal makineler için öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- Bir Microsoft Azure aboneliğiniz yoksa, ücretsiz bir deneme hesabı alabilirsiniz [burada](http://go.microsoft.com/fwlink/?LinkId=330212).
