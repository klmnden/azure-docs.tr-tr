---
title: API sürümü profillerini Azure yığınında Python ile kullanma | Microsoft Docs
description: API sürümü profilleri Azure yığınında Python ile kullanma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: mabrigg
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: a4fe62ba8c0732745326831b977e8975e1210436
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>API sürümü profillerini Azure yığınında Python ile kullanma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="python-samples-for-azure-stack"></a>Azure yığınının Python örnekleri 

Aşağıdaki kod örnekleri, Azure yığınında sanal makineler için genel yönetim görevlerini gerçekleştirmek için kullanabilirsiniz.

Kod örnekleri için Göster:

- Sanal makineler oluşturun:
    - Linux sanal makinesi oluşturma
    - Windows sanal makinesi oluşturma
- Bir sanal makine güncelleştirmesi:
    - Bir sürücüyü genişletin
    - Bir sanal makine etiketi
    - Veri diskleri ekleme
    - Veri diskini çıkarma
- Bir sanal makine çalışır:
    - Bir sanal makineyi Başlat
    - Bir sanal makineyi durdurma
    - Bir sanal makineyi yeniden başlatın
- Liste sanal makineler
- Sanal makineyi silme

Bu işlemleri gerçekleştirmek için kodu gözden geçirmek için kullanıma **run_example()** Python komut dosyası işlevinde **Hybrid/unmanaged-disks/example.py** GitHub depodaki [ sanal makineler-python-yönetilmesi](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88).

Her bir işlemin açıkça bir açıklama ve yazdırma işlevi ile etiketlenir.
Örnekler mutlaka yukarıdaki listede gösterilen sırada olup olmadığı.


## <a name="run-the-python-sample"></a>Python örneği çalıştırma

1.  Zaten yoksa, [Python yüklemek](https://www.python.org/downloads/).

    Bu örnek (ve SDK) 3.4, 3.5 ve 3.6 gibi Python 2.7 ile uyumludur.

2.  Genel Python geliştirme için sanal bir ortam kullanmanız önerilir. 
    Daha fazla bilgi için bkz: https://docs.python.org/3/tutorial/venv.html
    
    Yükleme ve Python 3'te "venv" modülüyle sanal ortam başlatma (yüklemelisiniz [virtualenv](https://pypi.python.org/pypi/virtualenv) Python 2.7 için):

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

5.  Oluşturma bir [hizmet sorumlusu](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals) Azure yığın ile çalışmak için. Hizmet sorumlusu sahip olduğundan emin olun [katılımcı/sahibi rolü](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) aboneliğinizi üzerinde.

6.  Aşağıdaki değişkenleri ayarlamak ve bu ortam değişkenleri geçerli kabuğundan dışa aktarın. 

`Note: provide an explanation of where these variables come from?`

    ````bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  Bu örneği çalıştırmak için Ubuntu 16.04-LTS ve Windows Server 2012 R2 Datacenter görüntüleri Azure yığın Pazar yerinde mevcut olması gerektiğini unutmayın. Bunlar ya da olabilir [Azure'dan indirilen](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-download-azure-marketplace-item) veya [Platform görüntü deposu için eklenen](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-add-vm-image).


8. Örnek uygulamayı çalıştırın.

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>Notlar

Kullanarak bir sanal makinenin işletim sistemi diski almak için isteği olabilir `virtual_machine.storage_profile.os_disk`.
Bazı durumlarda bu, ancak, sağlar unutmayın istediğinizi yapabilirsiniz bir `OSDisk` nesnesi.
İşletim sistemi Disk boyutu olarak güncelleştirmek için `example.py` , gerekmez bir `OSDisk` nesne ancak bir `Disk` nesnesi.
`example.py` alır `Disk` aşağıdaki nesnesiyle:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Python Geliştirme Merkezi](https://azure.microsoft.com/develop/python/)
- [Azure Virtual Machines belgeleri](https://azure.microsoft.com/services/virtual-machines/)
- [Sanal makineler için öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- Microsoft Azure aboneliğiniz yoksa, ücretsiz bir deneme hesabı alabilirsiniz [burada](http://go.microsoft.com/fwlink/?LinkId=330212).
