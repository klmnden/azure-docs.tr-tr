---
title: Azure'da VMware Vm'lerini CloudSimple hızlı başlangıç - Azure VMware çözümüyle kullanma
description: Yapılandırma ve Azure portalından Azure VMware CloudSimple çözümüyle kullanarak VMware sanal makinelerini kullanma hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 58edadb553730b646f23f4981d6cbf1bdbfe76d5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577728"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Hızlı Başlangıç - azure'da VMware sanal makinelerini kullanma

Azure portalında sanal makine oluşturmak için aboneliğinizin CloudSimple yöneticiniz sanal makine şablonları kullanın. Bu VM şablonları, VMware altyapı üzerinde bulunur.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>Bir VM şablonu azure'da CloudSimple VM oluşturmayı gerektirir

Özel bulutunuzda vcenter kullanıcı Arabirimi bir sanal makine oluşturun. Bir şablon oluşturmak için yönergeleri izleyin. [vSphere Web istemcisi şablonunda, sanal bir makineyi kopyalayıp](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). VM şablonu, özel bulut vCenter Store.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Azure portalında sanal makine oluşturma

1. Sol menüsünde **+** veya **kaynak Oluştur**.

2. Sol menüsünde **işlem**ve ardından **CloudSimple sanal makine**.

3. Tıklayın **Onayla** yeni bir VM oluşturmak istediğinizi doğrulayın.

4. Aşağıdaki tabloda açıklandığı gibi temel yapılandırmayı ayarlayın ve ardından **sonraki: Boyutu**.

    | Alan | Açıklama |
    | ------------ | ------------- |
    | Abonelik | Özel bulut dağıtımınız ile ilişkili azure aboneliği.  |
    | Kaynak Grubu | Dağıtım grubu VM'ye atanacak. Varolan bir grubu seçin veya yeni bir tane oluşturun. |
    | Ad | Sanal Makineyi tanımlamak için ad.  |
    | Location | İçinde bu VM'nin barındırıldığı azure bölgesi.  |
    | Kaynak Havuzu | VM için fiziksel kaynakları. Kullanılabilir kaynak havuzlarından seçin. |
    | vSphere şablonu | VM için işletim sistemi şablon türü.  |
    | Kullanıcı adı | Sanal makine yönetici kullanıcı adı. |
    | Parola parolayı onayla | Sanal makine yönetici parolası.  |

5. Çekirdek ve bellek kapasitesi VM sayısını seçin.

6. (İsteğe bağlı) Konuk işletim sistemi için tam CPU sanallaştırma kullanıma sunmak istiyorsanız belirleyin **konuk işletim sistemine kullanıma** onay kutusu.
Bu seçimi, donanım sanallaştırma ikili çeviri veya yarı olmayan sanal makineleri çalıştırmak için gerektiren uygulamalar sağlar. Daha fazla bilgi için VMware bkz [VMware donanım Yardımlı sanallaştırma kullanıma](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html).

7. Tıklayın **sonraki: Yapılandırma**.

8. Ağ arabirimleri ve diskleri aşağıdaki tabloda açıklandığı gibi yapılandırın.

    Ağ arabirimleri için tıklatın **Ekle ağ arabirimi** ve aşağıdaki ayarları yapılandırın.

    | Denetim | Açıklama |
    | ------------ | ------------- |
    | Ad | Arabirim tanımlamak için bir ad girin.  |
    | Ağ | Özel bulut vSphere yapılandırılmış ağlar listesinden seçin.  |
    | Bağdaştırıcı | Sanal makine için yapılandırılan kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için bkz: VMware Bilgi Bankası makalesi [sanal makineniz için bir ağ bağdaştırıcısı seçme](https://kb.vmware.com/s/article/1001805). |
    | Önyükleme sırasında açma | Sanal makine önyüklendiğinde NIC donanımı etkinleştirmek isteyip istemediğinizi seçin. Varsayılan değer **etkinleştirme**. |

    Diskler için tıklatın **Ekle disk** ve aşağıdaki ayarları yapılandırın.

    | Öğe | Açıklama |
    | ------------ | ------------- |
    | Ad | Diski tanımlamak için bir ad girin.  |
    | Boyut | Kullanılabilir boyutlar birini seçin.  |
    | SCSI denetleyicisi | SCSI denetleyicisi seçin. Kullanılabilir denetleyicilerini farklı desteklenen işletim sistemleri için farklılık gösterir.  |
    | Mod | Disk anlık görüntüleri nasıl katıldığı belirler. Bu seçeneklerden birini seçin: <br> -Bağımsız kalıcı: Diske yazılan tüm veriler kalıcı olarak yazılır.<br> -Kalıcı olmayan bağımsız: Kapatmak veya sanal makineyi Sıfırla diske yazılan değişiklikler atılır.  Bağımsız kalıcı olmayan modu her zaman aynı durumda sanal Makineyi yeniden başlatmanızı sağlar. Daha fazla bilgi için [VMware belgeleri.](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html)

9. Ayarları gözden geçirin. Değişiklik yapmak için üstteki sekmeleri tıklatın.

10. Tıklayın **Oluştur** ayarları kaydedin ve VM oluşturma.

## <a name="next-steps"></a>Sonraki adımlar

* [CloudSimple sanal makinelerin listesini görüntüleyin](https://docs.azure.cloudsimple.com/azurelistvms/)
* [Azure'dan CloudSimple sanal makineyi yönetin](https://docs.azure.cloudsimple.com/azureoverviewpage/)