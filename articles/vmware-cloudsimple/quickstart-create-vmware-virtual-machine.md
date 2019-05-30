---
title: Azure VMware çözümüyle CloudSimple hızlı başlangıç - azure'da VMware sanal makinelerini kullanma
description: Bu hızlı başlangıçta, yapılandırma ve VMware Vm'lerini Azure VMware CloudSimple çözümüyle kullanarak Azure portalından kullanma hakkında bilgi edineceksiniz
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: quickstart
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b430efbc931d72de4b095a7eac4c1e7ca496b1b9
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393507"
---
# <a name="quickstart-consume-vmware-vms-on-azure"></a>Hızlı Başlangıç: Azure'da VMware sanal makinelerini kullanma

Azure portalında sanal makine oluşturmak için kullanılabilir sanal makine şablonları, özel bulut vCenter kullanabilirsiniz. Bir vCenter Yöneticisi üzerine özel buluta ek şablonlar oluşturabilirsiniz.

## <a name="create-a-vm-template"></a>Bir VM şablonu oluşturma

İlk olarak bir sanal makine, kullanıcı Arabirimi vCenter'ı kullanarak özel bulutunuzda oluşturun. Bir şablon oluşturmak için VMware makaledeki yönergeleri [vSphere Web istemcisi şablonunda, sanal bir makineyi kopyalayıp](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). VM şablonu, özel bulut vCenter Store.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Azure portalında sanal makine oluşturma

1. **Tüm Hizmetler**’i seçin.

2. Arama **CloudSimple sanal makineler**.

3. **Add (Ekle)** seçeneğini belirleyin.

    ![Ekle'yi seçin](media/create-cloudsimple-virtual-machine.png)

4. Sanal makine hakkında aşağıdaki bilgileri girin ve ardından **sonraki: Boyutu**.

    ![Sanal makine CloudSimple - temelleri oluşturma](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Alan | Açıklama |
    | ------------ | ------------- |
    | **Abonelik** | Özel bulut ile ilişkili Azure aboneliği.  |
    | **Kaynak grubu** | Kaynak grubu VM'ye atanacak. Varolan bir grubu seçin veya yeni bir tane oluşturun. |
    | **Ad** | Sanal Makineyi tanımlamak için bir ad.  |
    | **Konum** | Hangi VM'nin barındırıldığı Azure bölgesi.  |
    | **Özel bulut** | VM'yi oluşturmak istediğiniz CloudSimple özel bulut. |
    | **ResourcePool** | Sanal makine için eşlenen kaynak havuzu. Kullanılabilir kaynak havuzlarından seçin. |
    | **vSphere şablonu** | VM için vSphere şablonu.  |
    | **Kullanıcı adı** | VM Yöneticisi kullanıcı adı (için Windows Şablonları).|
    | **Parola** |  VM (Windows şablonları için) yönetici parolası. |
    | **Parolayı onayla** | Önceki alanında sağlanan parola. |

5. Çekirdek ve bellek kapasitesi için VM sayısını seçin. Seçin **konuk işletim sistemine kullanıma** tam CPU sanallaştırma konuk işletim sisteminde kullanıma sunmak istiyorsanız. Donanım sanallaştırma gerektiren uygulamalar, ikili çeviri veya yarı olmadan sanal makinelerinde çalıştırabilirsiniz. Daha fazla bilgi için VMware bkz <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">VMware donanım Yardımlı sanallaştırma kullanıma</a>. İşiniz bittiğinde **sonraki: Yapılandırmaları**.

    ![Sanal makine CloudSimple - boyutu oluşturun](media/create-cloudsimple-virtual-machine-size.png)

6. Ağ arabirimleri ve diskleri aşağıdaki tabloda açıklandığı gibi yapılandırın ve ardından **gözden geçir + Oluştur**.

    ![Sanal makine CloudSimple - yapılandırmaları oluşturma](media/create-cloudsimple-virtual-machine-configurations.png)

    Ağ arabirimleri için seçin **Ekle ağ arabirimi** ve sonra aşağıdaki ayarları yapılandırın:
    
    | Ayar | Açıklama |
    | ------------ | ------------- |
    | **Ad** | Arabirim tanımlamak için bir ad girin.  |
    | **Ağ** | İçinde özel bulut vSphere dağıtılmış yapılandırılan bağlantı noktası gruplarının listesini seçin.  |
    | **Bağdaştırıcı** | Sanal makine için yapılandırılan kullanılabilir türler listesinden bir vSphere bağdaştırıcıyı seçin. Daha fazla bilgi için VMware bkz <a href="https://kb.vmware.com/s/article/1001805" target="_blank">sanal makineniz için bir ağ bağdaştırıcısı seçme</a>. |
    | **Önyükleme sırasında açma** | Sanal makine önyüklendiğinde NIC donanımı etkinleştirmek isteyip istemediğinizi seçin. Varsayılan değer **etkinleştirme**. |

    Diskler için seçtiğiniz **Ekle disk** ve sonra aşağıdaki ayarları yapılandırın:

    | Ayar | Açıklama |
    | ------------ | ------------- |
    | **Ad** | Diski tanımlamak için bir ad girin.  |
    | **Boyut** | Kullanılabilir boyutlar birini seçin.  |
    | **SCSI denetleyicisi** | Disk bir SCSI denetleyicisi seçin.  |
    | **Modu** | Disk anlık görüntüleri nasıl katıldığı modunu belirtir. Bu seçeneklerden birini seçin: <br> **Bağımsız kalıcı**: Diske yazılan tüm değişiklikler kalıcı olarak yazılır.<br> **Kalıcı olmayan bağımsız**: Kapatmak veya sanal makineyi Sıfırla diske yazılan değişiklikler atılır. Bağımsız kalıcı olmayan modu her zaman aynı durumda sanal Makineyi yeniden başlatmanızı sağlar. Daha fazla bilgi için <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware belgeleri</a>.

7. Doğrulama tamamlandıktan sonra ayarları gözden geçirin ve seçin **Oluştur**. Değişiklik yapmak için seçin ve üst sekmeleri seçin **önceki: Yapılandırmaları**.

    ![Sanal makine CloudSimple - gözden geçirmesi oluşturma + oluştur](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Sonraki adımlar

* [CloudSimple sanal makinelerin listesini görüntüleyin](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [Azure'dan CloudSimple sanal makineleri yönetme](https://docs.azure.cloudsimple.com/azure-manage-vm/)
