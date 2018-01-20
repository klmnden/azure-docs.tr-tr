---
title: "Hizmet Yönetimi API (Python) - özellik kılavuzu kullanma"
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
ms.openlocfilehash: d0fd1063194ecbccb0af1abc0c441aa65b18883b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-service-management-from-python"></a>Hizmet Yönetimi python'dan kullanma
Bu kılavuz program aracılığıyla Python ortak hizmet yönetim görevlerini gerçekleştirmek nasıl gösterir. **ServiceManagementService** sınıfını [Python için Azure SDK](https://github.com/Azure/azure-sdk-for-python) programlı erişim kullanılabilir servis yönetimiyle ilgili işlevlerinin çoğunu destekler [Azure Portal] [ management-portal] (gibi **oluşturma, güncelleştirme ve bulut Hizmetleri, dağıtımları, Veri Yönetimi Hizmetleri ve sanal makineleri silme**). Bu işlev hizmet yönetimi için programlı erişim ihtiyaç duyan uygulamalar oluşturmada faydalı olabilir.

## <a name="WhatIs"></a>Hizmet yönetimi nedir
Hizmet Yönetimi API aracılığıyla kullanılabilir hizmet yönetim işlevlerinin çoğunu için programlı erişim sağlayan [Azure portal][management-portal]. Python için Azure SDK'sı, bulut Hizmetleri ve depolama hesapları yönetmenize olanak sağlar.

Hizmet Yönetimi API'sini kullanmak için yapmanız [bir Azure hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Kavramları
Python için Azure SDK'sı sarmalar [Azure Hizmet Yönetimi API'si][svc-mgmt-rest-api], REST API olduğu. Tüm API işlemleri SSL üzerinden gerçekleştirilir ve X.509 v3 sertifikaları kullanılarak karşılıklı kimlik doğrulaması yapılır. Yönetim hizmetine Azure'da çalışan bir hizmetin içinden veya doğrudan İnternet üzerinden, HTTPS isteği gönderebilen ve HTTPS yanıtı alabilen herhangi bir uygulamadan erişilebilir.

## <a name="Installation"></a>Yükleme
Bu makalede açıklanan tüm özellikler mevcuttur `azure-servicemanagement-legacy` paketi olarak PIP kullanarak yükleyebilirsiniz. Bu makalede (örneğin, Python için yeni olan) yükleme hakkında daha fazla bilgi için bkz: [yükleme Python ve Azure SDK'sı](../python-how-to-install.md)

## <a name="Connect"></a>Nasıl yapılır: Hizmet Yönetimi için Bağlan
Hizmet Yönetimi uç noktasına bağlanmak için Azure abonelik Kimliğinizi ve geçerli bir yönetim sertifikası gerekir. Abonelik Kimliğinizi aracılığıyla elde edebilirsiniz [Azure portal][management-portal].

> [!NOTE]
> Artık Windows üzerinde çalışan OpenSSL ile oluşturulan sertifikaları kullanmak mümkündür.  Python 2.7.4 gerektirir veya sonraki bir sürümü. Kullanıcıların sertifikalar büyük ihtimalle gelecekte kaldırılacaktır .pfx desteği itibaren .pfx yerine OpenSSL kullanmasını öneririz.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Yönetim sertifikaları Windows/Mac/Linux (OpenSSL)
Kullanabileceğiniz [OpenSSL](http://www.openssl.org/) yönetim sertifikası oluşturmak için.  Aslında iki sertifika, sunucu için bir tane oluşturmanız gerekir (bir `.cer` dosyası) ve bir istemci için (bir `.pem` dosyası). Oluşturmak için `.pem` dosya, yürütün:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Oluşturmak için `.cer` sertifika, yürütün:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Azure sertifikaları hakkında daha fazla bilgi için bkz: [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md). OpenSSL parametreler tam bir açıklaması için belgelerine bakın [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Bu dosyalar oluşturduktan sonra karşıya yüklemek gereken `.cer` "Ayarlar" sekmesinde "Karşıya Yükle" eylemini aracılığıyla Azure dosyasına [Azure portal][management-portal], ve kaydettiğiniz yeri not edin gerekir `.pem` dosya.

Abonelik Kimliğinizi aldıktan sonra bir sertifika oluşturulur ve karşıya `.cer` dosyasını Azure'a, abonelik kimliği ve yolunu geçirerek Azure yönetim uç noktasına bağlanabilir `.pem` dosya  **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte `sms` olan bir **ServiceManagementService** nesnesi. **ServiceManagementService** Azure hizmetleri yönetmek için kullanılan birincil sınıf bir sınıftır.

### <a name="management-certificates-on-windows-makecert"></a>Yönetim sertifikaları Windows (MakeCert)
Otomatik imzalı yönetim sertifikası, makine kullanarak oluşturabileceğiniz `makecert.exe`.  Açık bir **Visual Studio komut istemi** olarak bir **yönetici** ve aşağıdaki komut, değiştirme *AzureCertificate* kullanmak istediğiniz sertifika adına sahip.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Komut oluşturur `.cer` dosya ve içinde yükler **kişisel** sertifika deposu. Daha fazla bilgi için bkz: [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md).

Sertifika oluşturduktan sonra karşıya yüklemek gereken `.cer` "Ayarlar" sekmesinde "Karşıya Yükle" eylemini aracılığıyla Azure dosyasına [Azure portal][management-portal].

Abonelik Kimliğinizi aldıktan sonra bir sertifika oluşturulur ve karşıya `.cer` dosyasını Azure'a, abonelik kimliği ve sertifikada konumunu geçirerek Azure yönetim uç noktasına bağlanabilir, **kişisel**  sertifika deposuna **ServiceManagementService** (yeniden Değiştir *AzureCertificate* sertifikanızın adıyla):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Önceki örnekte `sms` olan bir **ServiceManagementService** nesnesi. **ServiceManagementService** Azure hizmetleri yönetmek için kullanılan birincil sınıf bir sınıftır.

## <a name="ListAvailableLocations"></a>Nasıl yapılır: kullanılabilir konumlarını listeleyin
Barındırma hizmetleri için kullanılabilir konumları listelemek için kullanın **listesi\_konumları** yöntemi:

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

## <a name="CreateCloudService"></a>Nasıl yapılır: bir bulut hizmeti oluştur
Bir uygulama oluşturduğunuzda ve Azure'da çalıştırmak, kod ve yapılandırma birlikte Azure adlandırılır [bulut hizmeti] [ cloud service] (olarak bilinen bir *barındırılan hizmetin* önceki Azure serbest bırakır). **Oluşturma\_barındırılan\_hizmet** yöntemi (hangi Azure içinde benzersiz olması gerekir) bir barındırılan hizmet adı, (otomatik olarak base64 kodlanmış) bir etiket, açıklama, sağlayarak yeni bir barındırılan hizmet oluşturmanıza olanak sağlar ve konumu.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Barındırılan tüm hizmetleri aboneliğinizle için listeleyebilirsiniz **listesi\_barındırılan\_Hizmetleri** yöntemi:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Belirli bir barındırılan hizmet hakkında bilgi almak istiyorsanız, bunu barındırılan hizmet adını geçirerek yapabilirsiniz **almak\_barındırılan\_hizmet\_özellikleri** yöntemi:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Bir bulut hizmeti oluşturduktan sonra kodunuzu hizmetiyle dağıtabileceğiniz **oluşturma\_dağıtım** yöntemi.

## <a name="DeleteCloudService"></a>Nasıl yapılır: bir bulut hizmetini silin
Bir bulut hizmeti için hizmet adı geçirerek silebilirsiniz **silmek\_barındırılan\_hizmet** yöntemi:

    sms.delete_hosted_service('myhostedservice')

Bir hizmet silmeden önce tüm dağıtımlar için hizmet silinmesi gerekir. (Bkz [nasıl yapılır: bir dağıtımı silin](#DeleteDeployment) Ayrıntılar için.)

## <a name="DeleteDeployment"></a>Nasıl yapılır: bir dağıtımı silin
Bir dağıtımı silmek için kullanın **silmek\_dağıtım** yöntemi. Aşağıdaki örnekte nasıl adlı bir dağıtım silineceğini gösterir `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Nasıl yapılır: bir depolama birimi hizmeti oluşturma
A [depolama birimi hizmeti](../storage/common/storage-create-storage-account.md) , Azure'a erişmenizi [BLOB'lar](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabloları](../cosmos-db/table-storage-how-to-use-python.md), ve [sıraları](../storage/queues/storage-python-how-to-use-queue-storage.md). Bir depolama birimi hizmeti oluşturmak için bir ad hizmeti (3 ila 24 küçük harf karakter ve Azure içinde benzersiz), bir açıklama, etiket (en fazla 100 karakter otomatik olarak base64 kodlanmış) ve bir konum gerekir. Aşağıdaki örnek, bir konum belirterek bir depolama birimi hizmeti oluşturulacağını gösterir.

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

Önceki örnekte Not, durumunu **oluşturma\_depolama\_hesap** işlemi tarafından döndürülen sonuç geçirerek alınabilir **oluşturma\_depolama\_hesap** için **almak\_işlemi\_durum** yöntemi.  

Depolama hesaplarınızı ve özellikleri ile listeleyebilirsiniz **listesi\_depolama\_hesapları** yöntemi:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Nasıl yapılır: bir depolama birimi hizmeti Sil
Depolama hizmet adına geçirerek bir depolama birimi hizmeti silebilirsiniz **silmek\_depolama\_hesap** yöntemi. Bir depolama birimi hizmeti silme (BLOB'lar, tablolar ve Kuyruklar) hizmetinde depolanan tüm verileri siler.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Nasıl yapılır: listesinde kullanılabilir işletim sistemleri
Barındırma hizmetleri için kullanılabilen işletim sistemlerini listelemek için kullanın **listesi\_işletim\_sistemleri** yöntemi:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternatif olarak, kullanabileceğiniz **listesi\_işletim\_sistem\_aileleri** işletim sistemi ailesi tarafından grupları yöntemi:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Nasıl yapılır: bir işletim sistemi görüntüsü oluşturma
Görüntü deposu için bir işletim sistemi görüntüsü eklemek için kullanın **ekleme\_os\_görüntü** yöntemi:

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

Kullanılabilir işletim sistemi görüntülerini listelemek için kullanın **listesi\_os\_görüntüleri** yöntemi. Tüm platform görüntüleri ve kullanıcı görüntüleri içerir:

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

## <a name="DeleteVMImage"></a>Nasıl yapılır: bir işletim sistemi görüntüsünü silme
Bir kullanıcı görüntüsü silmek için kullanın **silmek\_os\_görüntü** yöntemi:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Nasıl yapılır: bir sanal makine oluşturun
Bir sanal makine oluşturmak için önce oluşturmanız gerekir bir [bulut hizmeti](#CreateCloudService).  Dağıtım kullanarak sanal makineyi oluşturmak **oluşturma\_sanal\_makine\_dağıtım** yöntemi:

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

## <a name="DeleteVM"></a>Nasıl yapılır: bir sanal makineyi silme
Bir sanal makineyi silmek için önce dağıtım kullanarak silmeniz **silmek\_dağıtım** yöntemi:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Bulut hizmeti kullanılarak sonra silinebilir **silmek\_barındırılan\_hizmet** yöntemi:

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Nasıl yapılır: bir sanal makine yakalanan görüntüden sanal makine oluşturma
Bir VM görüntüsü yakalamak için ilk çağrı **yakalama\_vm\_görüntü** yöntemi:

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

Ardından, görüntüyü başarıyla yakaladıktan emin olmak için kullanın **listesi\_vm\_görüntüleri** API'sini ve görüntünüzü sonuçlarında görüntülenen emin olun:

    images = sms.list_vm_images()

Son olarak yakalanan görüntüyü kullanarak sanal makine oluşturmak için kullanın **oluşturma\_sanal\_makine\_dağıtım** önce ancak bu kez vm_image_name yerine geçirirken yöntemi

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

Linux sanal makine yakalama hakkında daha fazla bilgi için bkz: [Linux sanal makine yakalama.](../virtual-machines/linux/classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

Windows sanal makinesi yakalama hakkında daha fazla bilgi için bkz: [Windows sanal makinesi yakalama.](../virtual-machines/windows/classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="What's Next"> </a>Sonraki adımlar
Hizmet Yönetimi öğrendiğinize göre erişebilirsiniz [tam API başvuru belgeleri Azure Python SDK'sı](http://azure-sdk-for-python.readthedocs.org/) ve python uygulamanızı kolayca yönetmek için karmaşık görevleri gerçekleştirin.

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://portal.azure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
