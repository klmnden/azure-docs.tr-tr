---
title: Hizmet Yönetim API'sini (Python) - özelliği Kılavuzu
description: Python'dan programlama yoluyla hizmeti genel yönetim görevleri gerçekleştirmeyi öğreneceksiniz.
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: ''
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: 573c6d3ded8fea58e0c9ba1afa7da2d8dd0fce91
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525539"
---
# <a name="use-service-management-from-python"></a>Hizmet Yönetimi'nden Python kullanma
Bu kılavuz, yaygın hizmet yönetimi görevlerini Python'dan programlama yoluyla gerçekleştirmek nasıl gösterir. **ServiceManagementService** sınıfını [Python için Azure SDK'sı](https://github.com/Azure/azure-sdk-for-python) kullanılabilir Hizmet Yönetimi ile ilgili işlevselliğinin programlı erişim destekler [Azure Portal][management-portal]. Bu işlev, oluşturmak, güncelleştirmek ve bulut Hizmetleri, dağıtımlar, Veri Yönetimi Hizmetleri ve sanal makineleri silmek için kullanabilirsiniz. Bu işlev Hizmet Yönetimi programlı erişim gerekir. uygulamaları oluşturmada faydalı olabilir.

## <a name="WhatIs"> </a>Hizmet Yönetimi nedir?
Azure Hizmet Yönetim API'si aracılığıyla hizmet yönetim işlevselliğinin programlı erişim sağlar [Azure portalında][management-portal]. Python için Azure SDK'sı, bulut Hizmetleri ve depolama hesapları yönetmek için kullanabilirsiniz.

Hizmet Yönetimi API'sini kullanmak için yapmanız [Azure hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Kavramları
Python için Azure SDK'sı sarmalar [Hizmet Yönetimi API'si][svc-mgmt-rest-api], REST API olduğu. Tüm API işlemleri SSL üzerinden gerçekleştirilir ve X.509 v3 sertifikaları kullanılarak karşılıklı kimliği. Yönetim hizmetine Azure'da çalışan bir hizmetin içinden erişilebilir. Ayrıca, doğrudan bir HTTPS isteği gönderebilen ve HTTPS yanıtı alabilen herhangi bir uygulamadan Internet üzerinden de erişilebilir.

## <a name="Installation"> </a>Yükleme
Bu makalede açıklanan tüm özellikleri kullanılabilir `azure-servicemanagement-legacy` PIP kullanarak yükleyebileceğiniz bir paket. (Örneğin, Python için yeni) yükleme hakkında daha fazla bilgi için bkz: [yüklemeniz Python ve Azure SDK'sı](../python-how-to-install.md).

## <a name="Connect"> </a>Bağlanmak için Hizmet Yönetimi
Hizmet Yönetim uç noktasına bağlanmak için Azure abonelik Kimliğinizi ve geçerli bir yönetim sertifikası gerekir. Abonelik Kimliğinizi edinebilirsiniz [Azure portalında][management-portal].

> [!NOTE]
> Artık Windows üzerinde çalışırken OpenSSL ile oluşturulan sertifikalar kullanabilirsiniz. Python 2.7.4 veya üstü gereklidir. OpenSSL .pfx yerine kullanmak için öneririz gelecekte kaldırılması olasılığı .pfx sertifikaları için destek.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Windows/Mac/Linux (OpenSSL) üzerinde yönetim sertifikaları
Kullanabileceğiniz [OpenSSL](https://www.openssl.org/) , yönetim sertifikası oluşturma. Sunucu için iki sertifika oluşturmanız gerekir (bir `.cer` dosyası) ve bir istemci için (bir `.pem` dosyası). Oluşturulacak `.pem` dosyası, yürütme:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Oluşturulacak `.cer` sertifika, yürütün:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Azure sertifikaları hakkında daha fazla bilgi için bkz. [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md). OpenSSL parametrelerinin eksiksiz bir açıklaması için belgelerine bakın [ https://www.openssl.org/docs/apps/openssl.html ](https://www.openssl.org/docs/apps/openssl.html).

Bu dosyaları oluşturduktan sonra karşıya yükleme `.cer` azure'a dosya. İçinde [Azure portalında][management-portal], **ayarları** sekmesinde **karşıya**. Kaydettiğiniz Not `.pem` dosya.

Abonelik Kimliğinizi edindikten sonra bir sertifika oluşturma ve karşıya yükleme `.cer` Azure'da dosya, Azure yönetim uç noktasına bağlanın. Abonelik kimliği ve yolunu geçirerek bağlanma `.pem` dosyasını **ServiceManagementService**.

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte `sms` olduğu bir **ServiceManagementService** nesne. **ServiceManagementService** Azure hizmetlerini yönetmek için kullanılan birincil sınıfı.

### <a name="management-certificates-on-windows-makecert"></a>Windows (MakeCert) üzerinde yönetim sertifikaları
Makinenizde bir otomatik olarak imzalanan bir yönetim sertifikası kullanarak oluşturabilirsiniz `makecert.exe`. Açık bir **Visual Studio komut istemi** olarak bir **yönetici** ve aşağıdaki komut, değiştirme *AzureCertificate* ile kullanmak istediğiniz sertifika adı:

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Komut oluşturur `.cer` içinde yükler ve dosya **kişisel** sertifika deposu. Daha fazla bilgi için [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md).

Sertifikayı oluşturduktan sonra karşıya yükleme `.cer` azure'a dosya. İçinde [Azure portalında][management-portal], **ayarları** sekmesinde **karşıya**.

Abonelik Kimliğinizi edindikten sonra bir sertifika oluşturma ve karşıya yükleme `.cer` Azure'da dosya, Azure yönetim uç noktasına bağlanın. Abonelik kimliği ve sertifikanın konumunu geçirerek bağlanmak, **kişisel** sertifika deposuna **ServiceManagementService** (yeniden değiştirin *AzureCertificate* sertifikanızın adıyla).

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte `sms` olduğu bir **ServiceManagementService** nesne. **ServiceManagementService** Azure hizmetlerini yönetmek için kullanılan birincil sınıfı.

## <a name="ListAvailableLocations"> </a>Kullanılabilir konumların listesi
Barındırma hizmetleri için kullanılabilir konumları listelemek için kullanın **listesi\_konumları** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Bir bulut hizmeti veya depolama hizmeti oluşturduğunuzda, geçerli bir konum vermeniz gerekir. **Listesi\_konumları** yöntemi her zaman güncel şu anda kullanılabilir konumların listesini döndürür. Bu makalenin yazıldığı tarih itibarıyla konumlar şunlardır:

* Batı Avrupa
* Kuzey Avrupa
* Güneydoğu Asya
* Doğu Asya
* Orta ABD
* Orta Kuzey ABD
* Orta Güney ABD
* Batı ABD
* Doğu ABD
* Japonya Doğu
* Japonya Batı
* Güney Brezilya
* Avustralya Doğu
* Avustralya Güneydoğu

## <a name="CreateCloudService"> </a>Bulut hizmeti oluşturma
Bir uygulama oluşturduğunuzda ve Azure'da çalıştırmak, kod ve yapılandırma birlikte Azure verilir [bulut hizmeti][cloud service]. (Olarak biliniyordu bir *barındırılan hizmet* Azure önceki sürümlerde.) Kullanabileceğiniz **oluşturma\_barındırılan\_hizmet** yöntemi yeni bir barındırılan hizmet. (Azure'da benzersiz olması gerekir) barındırılan hizmet adı, (otomatik olarak base64 kodlanmış) bir etiket, açıklama ve bir konum sağlayarak hizmeti oluşturun.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Aboneliğiniz için barındırılan tüm hizmetler listeleyebilirsiniz **listesi\_barındırılan\_Hizmetleri** yöntemi.

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Belirli bir barındırılan hizmet hakkında bilgi almak için barındırılan hizmet adı için geçirmek **alma\_barındırılan\_hizmet\_özellikleri** yöntemi.

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Bir bulut hizmeti oluşturduktan sonra hizmeti ile kodunuzu dağıtmak **oluşturma\_dağıtım** yöntemi.

## <a name="DeleteCloudService"> </a>Bulut hizmetini silme
Bir bulut hizmeti için hizmet adını geçirerek silebilirsiniz **Sil\_barındırılan\_hizmet** yöntemi.

    sms.delete_hosted_service('myhostedservice')

Bir hizmet silebilmeniz için önce hizmet için tüm dağıtımların silinmesi gerekir. Daha fazla bilgi için [bir dağıtımını silme](#DeleteDeployment).

## <a name="DeleteDeployment"> </a>Dağıtımı Sil
Bir dağıtımı silmek için kullanın **Sil\_dağıtım** yöntemi. Aşağıdaki örnek adlı bir dağıtım silme gösterir `v1`:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Depolama hizmeti oluşturma
A [depolama hizmeti](../storage/common/storage-create-storage-account.md) Azure'a erişmenizi [blobları](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabloları](../cosmos-db/table-storage-how-to-use-python.md), ve [kuyrukları](../storage/queues/storage-python-how-to-use-queue-storage.md). Depolama hizmeti oluşturmak için (küçük harf uzunluğu 3 ile 24 karakter arasında ve Azure içinde benzersiz) hizmeti için bir ad gerekiyor. Ayrıca bir açıklama ve etiket (en fazla 100 karakter otomatik olarak base64 kodlanmış) bir konum gerekir. Aşağıdaki örnek, bir konum belirterek bir depolama hizmeti oluşturma işlemi gösterilmektedir:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Yukarıdaki örnekte, durumunu **oluşturma\_depolama\_hesabı** işlemi tarafından döndürülen sonuç geçirilerek alınabilir **oluşturma\_depolama\_ Hesap** için **alma\_işlemi\_durumu** yöntemi. 

Depolama hesaplarınızı ve özellikleri ile listeleyebilirsiniz **listesi\_depolama\_hesapları** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Bir depolama hizmetini Sil
Depolama hizmeti silmek için depolama hizmeti adına geçirmek **Sil\_depolama\_hesabı** yöntemi. Depolama hizmet silme (BLOB'lar, tablolar ve Kuyruklar) hizmete depolanmış tüm verileri siler.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Kullanılabilir işletim sistemlerini Listele
Barındırma hizmetleri için kullanılabilir işletim sistemlerini listelemek için kullanın **listesi\_işletim\_sistemleri** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternatif olarak, **listesi\_işletim\_sistem\_aileleri** işletim sistemi ailesi tarafından gruplayan yöntem.

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Bir işletim sistemi görüntüsü oluşturma
Görüntü deposu için bir işletim sistemi görüntüsü eklemek için **ekleme\_işletim sistemi\_görüntü** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Kullanılabilir işletim sistemi görüntülerini listelemek için kullanın **listesi\_işletim sistemi\_görüntüleri** yöntemi. Bu, tüm platform görüntüleri ve kullanıcı görüntüleri içerir.

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Bir işletim sistemi görüntüsünü Sil
Kullanıcı görüntüsü silmek için kullanın **Sil\_işletim sistemi\_görüntü** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Sanal makine oluşturma
Bir sanal makine oluşturmak için öncelikle oluşturmak için ihtiyacınız bir [bulut hizmeti](#CreateCloudService). Sanal makine dağıtımını kullanarak oluşturup **oluşturma\_sanal\_makine\_dağıtım** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Sanal makineyi silme
Bir sanal makineyi silmek için önce dağıtım kullanarak silmeniz **Sil\_dağıtım** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Bulut hizmeti kullanarak daha sonra silinebilir **Sil\_barındırılan\_hizmet** yöntemi.

    sms.delete_hosted_service(service_name='myvm')

## <a name="create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Yakalanan sanal makine görüntüsünden sanal makine oluşturma
Bir VM görüntüsü yakalamak için önce çağırmanızı **yakalama\_vm\_görüntü** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Görüntü başarıyla yakalandı emin olmak için **listesi\_vm\_görüntüleri** API. Görüntünüzü sonuçlarda görüntülendiğinden emin olun.

    images = sms.list_vm_images()

Son olarak, yakalanan görüntüyü kullanarak sanal makine oluşturmak için kullanın **oluşturma\_sanal\_makine\_dağıtım** önceden ancak bu süre içinde vm_image_name yerine geçirirken yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Klasik dağıtım modelinde bir Linux sanal makinesini yakalama hakkında daha fazla bilgi için bkz: [bir Linux sanal makinesini yakalama](../virtual-machines/linux/classic/capture-image-classic.md).

Klasik dağıtım modelinde bir Windows sanal makinesini yakalama hakkında daha fazla bilgi için bkz: [bir Windows sanal makinesini yakalama](../virtual-machines/windows/classic/capture-image-classic.md).

## <a name="What's Next"> </a>Sonraki adımlar
Hizmet Yönetimi hakkındaki temel bilgileri öğrendiniz, erişebileceğiniz [tam API başvuru belgeleri için Azure Python SDK'sı](https://azure-sdk-for-python.readthedocs.org/) ve Python uygulamanızı kolayca yönetmek için karmaşık görevleri gerçekleştirin.

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/).

[What is service management?]: #WhatIs
[Concepts]: #Concepts
[Connect to service management]: #Connect
[List available locations]: #ListAvailableLocations
[Create a cloud service]: #CreateCloudService
[Delete a cloud service]: #DeleteCloudService
[Create a deployment]: #CreateDeployment
[Update a deployment]: #UpdateDeployment
[Move deployments between staging and production]: #MoveDeployments
[Delete a deployment]: #DeleteDeployment
[Create a storage service]: #CreateStorageService
[Delete a storage service]: #DeleteStorageService
[List available operating systems]: #ListOperatingSystems
[Create an operating system image]: #CreateVMImage
[Delete an operating system image]: #DeleteVMImage
[Create a virtual machine]: #CreateVM
[Delete a virtual machine]: #DeleteVM
[Next steps]: #NextSteps
[management-portal]: https://portal.azure.com/
[svc-mgmt-rest-api]: https://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/azure/cloud-services/
