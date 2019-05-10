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
ms.openlocfilehash: c0bb8d7a5a1ea30b704b44c9337cd28043597ff7
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209508"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Hızlı Başlangıç - azure'da VMware sanal makinelerini kullanma

Azure portalında sanal makine oluşturmak için aboneliğinizin CloudSimple yöneticiniz sanal makine şablonları kullanın. Bu VM şablonları, VMware altyapı üzerinde bulunur.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>Bir VM şablonu azure'da CloudSimple VM oluşturmayı gerektirir

Özel bulutunuzda vcenter kullanıcı Arabirimi bir sanal makine oluşturun. Bir şablon oluşturmak için yönergeleri izleyin. [vSphere Web istemcisi şablonunda, sanal bir makineyi kopyalayıp](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). VM şablonu, özel bulut vCenter Store.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Azure portalında sanal makine oluşturma

1. **Tüm Hizmetler**’i seçin.

2. Arama **CloudSimple sanal makineler**.

3. **Ekle**'yi tıklatın.

    ![CloudSimple sanal makine oluşturma](media/create-cloudsimple-virtual-machine.png)

4. Temel bilgiler çarpıyı **sonraki: boyutu**.

    ![CloudSimple sanal makine - temellerini oluşturma](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Alan | Açıklama |
    | ------------ | ------------- |
    | Abonelik | Özel bulut ile ilişkili azure aboneliği.  |
    | Kaynak Grubu | Kaynak grubu VM'ye atanacak. Varolan bir grubu seçin veya yeni bir tane oluşturun. |
    | Ad | Sanal Makineyi tanımlamak için ad.  |
    | Location | İçinde bu VM'nin barındırıldığı azure bölgesi.  |
    | Özel bulut | Sanal makineyi oluşturmak istediğiniz CloudSimple özel bulut. |
    | Kaynak Havuzu | Sanal makine için eşlenen kaynak havuzu. Kullanılabilir kaynak havuzlarından seçin. |
    | vSphere şablonu | VM şablonu vSphere.  |
    | Kullanıcı adı | VM yöneticinin kullanıcı adını (için Windows Şablonları)|
    | Parola |  Sanal Makine Yöneticisi (Windows Şablonları) parolası. |
    | Parolayı onayla | Parolayı onaylayın |

5. Çekirdek ve bellek kapasitesi tıklatın ve VM sayısını seçin **sonraki: yapılandırmaları**. Konuk işletim sistemi için tam CPU sanallaştırma kullanıma sunmak istiyorsanız onay kutusunu işaretleyin. Donanım sanallaştırma gerektiren uygulamalar, ikili çeviri veya yarı olmadan sanal makinelerinde çalıştırabilirsiniz. Daha fazla bilgi için VMware bkz <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">VMware donanım Yardımlı sanallaştırma kullanıma</a>.

    ![CloudSimple sanal makine - boyutu oluşturun](media/create-cloudsimple-virtual-machine-size.png)

6. Ağ arabirimleri ve diskleri aşağıdaki tabloda açıklandığı gibi yapılandırın ve tıklayın **gözden geçir + Oluştur**.

    ![CloudSimple sanal makine - yapılandırmaları oluşturma](media/create-cloudsimple-virtual-machine-configurations.png)

    Ağ arabirimleri için tıklatın **Ekle ağ arabirimi** ve aşağıdaki ayarları yapılandırın.
    
    | Denetim | Açıklama |
    | ------------ | ------------- |
    | Ad | Arabirim tanımlamak için bir ad girin.  |
    | Ağ | Özel bulut vSphere dağıtılmış yapılandırılan bağlantı noktası grubu listesinden seçin.  |
    | Bağdaştırıcı | Sanal makine için yapılandırılan kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için bkz: VMware Bilgi Bankası makalesi <a href="https://kb.vmware.com/s/article/1001805" target="_blank">sanal makineniz için bir ağ bağdaştırıcısı seçme</a>. |
    | Önyükleme sırasında açma | Sanal makine önyüklendiğinde NIC donanımı etkinleştirmek isteyip istemediğinizi seçin. Varsayılan değer **etkinleştirme**. |

    Diskler için tıklatın **Ekle disk** ve aşağıdaki ayarları yapılandırın.

    | Öğe | Açıklama | 
    | ------------ | ------------- | 
    | Ad | Diski tanımlamak için bir ad girin.  | 
    | Boyutlandır | Kullanılabilir boyutlar birini seçin.  | 
    | SCSI denetleyicisi | Disk bir SCSI denetleyicisi seçin.  |
    | Mod | Disk anlık görüntüleri nasıl katıldığı belirler. Bu seçeneklerden birini seçin: <br> -Bağımsız kalıcı: Diske yazılan tüm veriler kalıcı olarak yazılır.<br> -Kalıcı olmayan bağımsız: Kapatmak veya sanal makineyi Sıfırla diske yazılan değişiklikler atılır.  Bağımsız kalıcı olmayan modu her zaman aynı durumda sanal Makineyi yeniden başlatmanızı sağlar. Daha fazla bilgi için <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware belgeleri</a>.

7. Doğrulama tamamlandıktan sonra ayarları gözden geçirin ve tıklayın **Oluştur**. Değişiklik yapmak için üstteki sekmeleri tıklatın veya tıklayın.

    ![CloudSimple sanal makine oluştur - gözden geçirin](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Sonraki adımlar

* [CloudSimple sanal makinelerin listesini görüntüleyin](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [Azure'dan CloudSimple sanal makineyi yönetin](https://docs.azure.cloudsimple.com/azure-manage-vm/)
