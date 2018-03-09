---
title: " Azure Site kurtarma VMware vCenter sunucuları yönetme | Microsoft Docs"
description: "Bu makalede nasıl ekleyin ve Azure Site Recovery VMware Vcenter'da yönetin."
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 21e1a7bee7bba2f410a2841505cb6a6f8673bac3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="manage-vmware-vcenter-servers"></a>VMware vCenter sunucuları yönetme 

Bu makalede bir VMware vCenter üzerinde gerçekleştirilen çeşitli Site kurtarma işlemleri açıklanır. Doğrulama [Önkoşullar](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)başlamadan önce s.


## <a name="set-up-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap ayarlama

Site Recovery, sanal makineleri otomatik olarak bulmak üzere işlem sunucusu için ve yük devretme ve sanal makinelerin yeniden çalışma için VMware erişimi olmalıdır. Erişim için bir hesap gibi oluşturun:

1. Yapılandırma sunucusu makine oturum açın.
2. Başlatma masaüstü kısayolunu kullanarak cspsconfigtool.exe açın.
3. Tıklatın **hesabı Ekle** üzerinde **hesabı Yönet** sekmesi.

  ![Hesabı Ekle](./media/vmware-azure-manage-vcenter/addaccount.png)
1. Hesap ayrıntılarını belirtin ve tıklayın **Tamam** ekleyin.  Hesabı aşağıdaki tabloda özetlenen ayrıcalıkları olması gerekir. 

Yaklaşık 15 dakika yukarı Site Recovery hizmeti ile eşitlenmiş hesap bilgilerini alır.

### <a name="account-permissions"></a>Hesabı izinleri

|**Görev** | **Hesap** | **İzinler** | **Ayrıntılar**|
|--- | --- | --- | ---|
|**Otomatik bulma/geçirme (olmadan yeniden çalışma)** | En az bir salt okunur kullanıcının gerekiyor | Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.|
|**Çoğaltma/yük devretme** | En az bir salt okunur kullanıcının gerekiyor| Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.<br/><br/> Geçiş amacıyla, ancak değil tam çoğaltma, yük devretme, yeniden çalışma için kullanışlıdır.|
|**Çoğaltma/yük devretme/yeniden çalışma** | Gerekli izinlere sahip bir rol (AzureSiteRecoveryRole) oluşturun ve bir VMware kullanıcı veya grup rol atamak öneririz | Veri Merkezi Nesne –> Propagate alt nesne için rol AzureSiteRecoveryRole =<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.|


## <a name="add-vmware-server-to-the-vault"></a>VMware server kasaya ekleme

1. Azure Portal'da kasanızı açmak > **Site Recovery altyapısı** > **Configuration Server'lar**ve yapılandırma sunucusu açın.
2. Üzerinde **ayrıntıları** sayfasında, **+ vCenter**.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials"></a>Kimlik bilgilerini değiştirme

VCenter sunucusu veya ESXi ana şu şekilde bağlanmak için kullanılan kimlik bilgilerini değiştirin:

1. Yapılandırma sunucuya oturum açın ve masaüstünden cspsconfigtool.exe başlatın.
2. Tıklatın **hesabı Ekle** üzerinde **hesabı Yönet** sekmesi.

  ![Hesabı Ekle](./media/vmware-azure-manage-vcenter/addaccount.png)
3. Yeni hesap ayrıntılarını belirtin ve tıklayın **Tamam** ekleyin. Hesabın listelenen ayrıcalıklarına sahip olması [yukarıda](#account-permissions).
4. Azure Portal'da kasası açmak > **Site Recovery altyapısı** > **Configuration Server'lar**ve yapılandırma sunucusu açın.
5. İçinde **ayrıntıları** sayfasında, **sunucusunu Yenile eylemi**.
6. VCenter açmak için sunucu, vCenter Server yenileme işi tamamlandıktan sonra seçin **Özet** sayfası.
7. Yeni eklenen hesaptaki **vCenter sunucusu/vSphere ana bilgisayar hesabı** alan öğesini tıklatıp **kaydetmek**.

    ![değiştirme hesabı](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>VCenter server silme

1. Azure portalında kasanızı açmak > **Site Recovery altyapısı** > **Configuration Server'lar**ve yapılandırma sunucusu açın.
2. Üzerinde **ayrıntıları** sayfasında, vCenter sunucusu seçin.
3. Tıklayın **silmek** düğmesi.

  ![delete-account](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Ardından vCenter IP adresi, FQDN veya bağlantı noktası değiştirmeniz gerekiyorsa, vCenter server silme ve portala geri eklemeniz gerekir.
