---
title: "Hizmet Yönetimi API'si (Python) - özellik kılavuzu kullanın"
description: "Program aracılığıyla Python ortak hizmet yönetim görevlerini gerçekleştirmek öğrenin."
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: b89f1aad46621d35728934ea068a5893ba674094
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="use-service-management-from-python"></a>Python'dan Hizmet Yönetimi'ni kullanın
Bu kılavuz program aracılığıyla Python ortak hizmet yönetim görevlerini gerçekleştirmek nasıl gösterir. **ServiceManagementService** sınıfını [Python için Azure SDK](https://github.com/Azure/azure-sdk-for-python) programlı erişim kullanılabilir servis yönetimiyle ilgili işlevlerinin çoğunu destekler [Azure Portal][management-portal]. Oluştur, Güncelleştir ve bulut Hizmetleri, dağıtımları, Veri Yönetimi Hizmetleri ve sanal makineleri silmek için bu işlevi kullanabilirsiniz. Bu işlev hizmet yönetimi için programlı erişim ihtiyaç duyan uygulamalar oluşturmada faydalı olabilir.

## <a name="WhatIs"></a>Hizmet yönetimi nedir?
Azure Hizmet Yönetimi API aracılığıyla kullanılabilir hizmet yönetim işlevlerinin çoğunu için programlı erişim sağlayan [Azure portal][management-portal]. Bulut Hizmetleri ve depolama hesaplarını yönetecek Python için Azure SDK'sını kullanabilirsiniz.

Hizmet Yönetimi API'sini kullanmak için yapmanız [bir Azure hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Kavramları
Python için Azure SDK'sı sarmalar [Hizmet Yönetimi API'si][svc-mgmt-rest-api], REST API olduğu. Tüm API işlemleri SSL üzerinden gerçekleştirilen ve birbirini X.509 v3 sertifikaları kullanarak kimlik doğrulaması gerçekleştirir. Yönetim hizmetini Azure'da çalışan bir hizmet kapsamındaki erişilebilir. Ayrıca, doğrudan Internet'ten bir HTTPS isteği gönderebilir ve bir HTTPS yanıt herhangi bir uygulama üzerinden de erişilebilir.

## <a name="Installation"></a>Yükleme
Bu makalede açıklanan tüm özellikler mevcuttur `azure-servicemanagement-legacy` paketi olarak PIP kullanarak yükleyebilirsiniz. (Örneğin, Python için yeni) yükleme hakkında daha fazla bilgi için bkz: [Python yüklemek ve Azure SDK'sı](../python-how-to-install.md).

## <a name="Connect"></a>Hizmet Yönetimi Bağlan
Hizmet Yönetim uç noktasına bağlanmak için Azure abonelik Kimliğinizi ve geçerli bir yönetim sertifikası gerekir. Abonelik Kimliğinizi aracılığıyla elde edebilirsiniz [Azure portal][management-portal].

> [!NOTE]
> Artık Windows üzerinde çalışan OpenSSL ile oluşturulan sertifikalar kullanabilirsiniz. Python 2.7.4 veya sonraki sürümü gereklidir. .Pfx yerine OpenSSL kullanmak için öneririz gelecekte kaldırılacak olası .pfx sertifikaları için destek.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Yönetim sertifikaları Windows/Mac/Linux (OpenSSL)
Kullanabileceğiniz [OpenSSL](http://www.openssl.org/) yönetim sertifikası oluşturmak için. Sunucusu için iki sertifika oluşturmanız gerekir (bir `.cer` dosyası) ve bir istemci için (bir `.pem` dosyası). Oluşturmak için `.pem` dosya, yürütün:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Oluşturmak için `.cer` sertifika, yürütün:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Azure sertifikaları hakkında daha fazla bilgi için bkz: [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md). OpenSSL parametreler tam bir açıklaması için belgelerine bakın [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Bu dosyalar oluşturduktan sonra karşıya yükleme `.cer` Azure dosyasına. İçinde [Azure portal][management-portal], **ayarları** sekmesine **karşıya**. Kaydettiğiniz Not `.pem` dosya.

Abonelik Kimliğinizi edindikten sonra bir sertifika oluşturmalı ve karşıya `.cer` dosya için Azure, Azure yönetim uç noktasına bağlanın. Abonelik kimliği ve yolunu geçirerek bağlanmak `.pem` dosya **ServiceManagementService**.

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte `sms` olan bir **ServiceManagementService** nesnesi. **ServiceManagementService** Azure hizmetleri yönetmek için kullanılan birincil sınıf bir sınıftır.

### <a name="management-certificates-on-windows-makecert"></a>Yönetim sertifikaları Windows (MakeCert)
Otomatik imzalı yönetim sertifikası makinenizde kullanarak oluşturabileceğiniz `makecert.exe`. Açık bir **Visual Studio komut istemi** olarak bir **yönetici** ve aşağıdaki komut, değiştirme *AzureCertificate* kullanmak istediğiniz sertifika adına sahip:

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Komut oluşturur `.cer` dosya ve içinde yükler **kişisel** sertifika deposu. Daha fazla bilgi için bkz: [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md).

Sertifika oluşturduktan sonra karşıya yükleme `.cer` Azure dosyasına. İçinde [Azure portal][management-portal], **ayarları** sekmesine **karşıya**.

Abonelik Kimliğinizi edindikten sonra bir sertifika oluşturmalı ve karşıya `.cer` dosya için Azure, Azure yönetim uç noktasına bağlanın. Abonelik kimliği ve sertifikada konumunu geçirerek bağlanmak, **kişisel** sertifika deposuna **ServiceManagementService** (yeniden Değiştir *AzureCertificate* sertifikanızın adıyla).

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte `sms` olan bir **ServiceManagementService** nesnesi. **ServiceManagementService** Azure hizmetleri yönetmek için kullanılan birincil sınıf bir sınıftır.

## <a name="ListAvailableLocations"></a>Listesinde kullanılabilir konumları
Barındırma hizmetleri için kullanılabilir konumları listelemek için kullanın **listesi\_konumları** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Bir bulut hizmeti veya depolama hizmeti oluşturduğunuzda, geçerli bir konum sağlamanız gerekir. **Listesi\_konumları** yöntem her zaman şu anda kullanılabilir konumların güncel bir listesini döndürür. Bu makalenin yazıldığı sırada kullanılabilir konumlarını şunlardır:

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

## <a name="CreateCloudService"></a>Bir bulut hizmeti oluştur
Bir uygulama oluşturduğunuzda ve Azure'da çalıştırmak, kod ve yapılandırma birlikte Azure adlandırılır [bulut hizmeti][cloud service]. (Olarak biliniyordu bir *barındırılan hizmetin* Azure önceki sürümlerde.) Kullanabileceğiniz **oluşturma\_barındırılan\_hizmet** yöntemi yeni bir barındırılan hizmet. Azure içinde benzersiz olması gerekir) bir barındırılan hizmet adı (, (otomatik olarak base64 kodlanmış) bir etiket, açıklama ve bir konum sağlayarak hizmeti oluşturun.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Barındırılan tüm hizmetleri aboneliğinizle için listeleyebilirsiniz **listesi\_barındırılan\_Hizmetleri** yöntemi.

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Belirli bir barındırılan hizmet hakkında bilgi almak için barındırılan hizmet adını geçirmek **almak\_barındırılan\_hizmet\_özellikleri** yöntemi.

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Bir bulut hizmeti oluşturduktan sonra kodunuzu hizmetle dağıtmak **oluşturma\_dağıtım** yöntemi.

## <a name="DeleteCloudService"></a>Bir bulut hizmetini silin
Bir bulut hizmeti için hizmet adı geçirerek silebilirsiniz **silmek\_barındırılan\_hizmet** yöntemi.

    sms.delete_hosted_service('myhostedservice')

Bir hizmet silmeden önce tüm dağıtımlar için hizmet silinmesi gerekir. Daha fazla bilgi için bkz: [bir dağıtımı silin](#DeleteDeployment).

## <a name="DeleteDeployment"></a>Bir dağıtımı silin
Bir dağıtımı silmek için kullanın **silmek\_dağıtım** yöntemi. Aşağıdaki örnekte nasıl adlı bir dağıtım silineceğini gösterir `v1`:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Depolama hizmeti oluşturma
A [depolama birimi hizmeti](../storage/common/storage-create-storage-account.md) , Azure'a erişmenizi [BLOB'lar](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabloları](../cosmos-db/table-storage-how-to-use-python.md), ve [sıraları](../storage/queues/storage-python-how-to-use-queue-storage.md). Depolama hizmet oluşturmak için (3 ila 24 küçük harf karakter ve Azure içinde benzersiz) hizmeti için bir ad gerekir. Ayrıca, bir etiket (en fazla 100 karakter otomatik olarak base64 kodlanmış) ve bir konum ve bir açıklama gerekir. Aşağıdaki örnek, bir konum belirterek bir depolama birimi hizmeti oluşturulacağını gösterir:

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

Önceki örnekte, durumunu **oluşturma\_depolama\_hesap** işlemi tarafından döndürülen sonuç geçirerek alınabilir **oluşturma\_depolama\_ Hesap** için **almak\_işlemi\_durum** yöntemi. 

Depolama hesaplarınızı ve özellikleri ile listeleyebilirsiniz **listesi\_depolama\_hesapları** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Depolama hizmeti silin
Bir depolama birimi hizmeti silmek için depolama hizmet adına geçirmek **silmek\_depolama\_hesap** yöntemi. Bir depolama birimi hizmeti silme (BLOB'lar, tablolar ve Kuyruklar) hizmetinde depolanan tüm verileri siler.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Listesinde kullanılabilir işletim sistemleri
Barındırma hizmetleri için kullanılabilen işletim sistemlerini listelemek için kullanın **listesi\_işletim\_sistemleri** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternatif olarak, kullanabileceğiniz **listesi\_işletim\_sistem\_aileleri** işletim sistemi ailesi tarafından grupları yöntemi.

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Bir işletim sistemi görüntüsü oluşturma
Görüntü deposu için bir işletim sistemi görüntüsü eklemek için kullanın **ekleme\_os\_görüntü** yöntemi.

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

Kullanılabilir işletim sistemi görüntülerini listelemek için kullanın **listesi\_os\_görüntüleri** yöntemi. Tüm platform görüntüleri ve kullanıcı görüntüleri içerir.

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

## <a name="DeleteVMImage"></a>Bir işletim sistemi görüntüsünü silme
Bir kullanıcı görüntüsü silmek için kullanın **silmek\_os\_görüntü** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Bir sanal makine oluşturun
Bir sanal makine oluşturmak için önce oluşturmanız gerekir bir [bulut hizmeti](#CreateCloudService). Sanal makine dağıtımı kullanarak oluşturursunuz **oluşturma\_sanal\_makine\_dağıtım** yöntemi.

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

## <a name="DeleteVM"></a>Sanal makineyi silme
Bir sanal makineyi silmek için önce dağıtım kullanarak silmeniz **silmek\_dağıtım** yöntemi.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Bulut hizmeti kullanarak daha sonra silinebilir **silmek\_barındırılan\_hizmet** yöntemi.

    sms.delete_hosted_service(service_name='myvm')

## <a name="create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Bir sanal makine yakalanan görüntüden sanal makine oluşturma
Bir VM görüntüsü yakalamak için ilk çağrı **yakalama\_vm\_görüntü** yöntemi.

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

Görüntüsü başarıyla yakalandı emin olmak için **listesi\_vm\_görüntüleri** API. Görüntünüzü sonuçlarında görüntülendiğinden emin olun.

    images = sms.list_vm_images()

Son olarak yakalanan görüntüyü kullanarak sanal makine oluşturmak için kullanın **oluşturma\_sanal\_makine\_dağıtım** önce ancak bu kez vm_image_name yerine geçirirken yöntemi.

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

Klasik dağıtım modelindeki Linux sanal makine yakalama hakkında daha fazla bilgi için bkz: [Linux sanal makine yakalama](../virtual-machines/linux/classic/capture-image-classic.md).

Klasik dağıtım modelinde Windows sanal makinesi yakalama hakkında daha fazla bilgi için bkz: [Windows sanal makinesi yakalama](../virtual-machines/windows/classic/capture-image-classic.md).

## <a name="What's Next"></a>Sonraki adımlar
Hizmet Yönetimi öğrendiğinize göre erişebilirsiniz [tam API başvuru belgeleri Azure Python SDK'sı](http://azure-sdk-for-python.readthedocs.org/) ve Python uygulamanızı kolayca yönetmek için karmaşık görevleri gerçekleştirin.

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](/develop/python/).

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
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
