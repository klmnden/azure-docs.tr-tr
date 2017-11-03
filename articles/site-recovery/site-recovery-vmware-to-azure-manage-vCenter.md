---
title: " Azure Site Recovery VMware vCenter Server'da yönetme | Microsoft Docs"
description: "Bu makalede nasıl ekleyin ve Azure Site Recovery VMware Vcenter'da yönetin."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 091f0884417535427c52beee7bcdc5ed1dd83315
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>VMware vCenter Server Azure Site kurtarma yönetme
Bu makalede bir VMware vCenter üzerinde gerçekleştirilen çeşitli Site kurtarma işlemleri açıklanır.

## <a name="prerequisites"></a>Ön koşullar

**VMware vCenter ve VMware vSphere ESX konak desteği** | **Ayrıntılar** |
|--- | --- |
|**Şirket içi VMware sunucuları** | 6.0, 5.5, 5.1 ile en son güncelleştirmeleri çalıştıran bir veya daha fazla VMware vSphere sunucuları. Sunucuları yapılandırma sunucusu (veya ayrı işlem sunucusu) aynı ağda yer.<br/><br/> En son güncelleştirmeleri 6.0 veya 5.5 çalıştıran konak yönetmek için bir vCenter sunucusu öneririz. Sürüm 6.0 dağıttığınızda 5.5 içinde kullanılabilir olan özellikler desteklenir.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama
Site Recovery, sanal makineleri otomatik olarak bulmak üzere işlem sunucusu için ve yük devretme ve sanal makinelerin yeniden çalışma için VMware erişimi olmalıdır.

* **Geçiş**: yalnızca herhangi bir zamanda bunları geri başarısız olmadan Azure için VMware sanal makineleri geçirmek istiyorsanız, bir salt okunur rolüyle VMware hesabı kullanabilirsiniz. Bu tür bir rol, yük devretme çalıştırabilirsiniz ancak korumalı kaynak makineleri kapatmayı olamaz. Bu geçiş için gerekli değildir.
* **Replicate/Recover**: tam çoğaltma (çoğaltılır, yük devretme, yeniden çalışma) dağıtmak istiyorsanız hesap oluşturma ve diskleri, sanal makineyi başlatırken kaldırma gibi işlemleri çalıştırabilir ve olmalıdır.
* **Otomatik bulma**: en az bir salt okunur hesabı gereklidir.


|**Görevler** | **Gerekli hesap/rol** | **İzinler** | **Ayrıntılar**|
|--- | --- | --- | ---|
|**İşlem sunucusu VMware sanal makineleri otomatik olarak bulur.** | En az bir salt okunur kullanıcının gerekiyor | Veri Merkezi Nesne –> Propagate alt nesne için rol = salt okunur | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.|
|**Yük devretme** | En az bir salt okunur kullanıcının gerekiyor | Veri Merkezi Nesne –> Propagate alt nesne için rol = salt okunur | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.<br/><br/> Geçiş amacıyla, ancak değil tam çoğaltma, yük devretme, yeniden çalışma için kullanışlıdır.|
|**Yük devretme ve yeniden çalışma** | Gerekli izinlere sahip bir rol (AzureSiteRecoveryRole) oluşturun ve bir VMware kullanıcı veya grup rol atamak öneririz | Veri Merkezi Nesne –> Propagate alt nesne için rol AzureSiteRecoveryRole =<br/><br/> Veri deposu alanı Ayır ->, veri deposu, alt düzey dosya işlemleri göz atın, dosyayı kaldırmak, sanal makine dosyalarını güncelleştir<br/><br/> Ağ -> Ağ atama<br/><br/> Kaynak VM atamak için kaynak havuzu ->, VM güç beslemeli geçirmek, VM güç beslemeli geçirme<br/><br/> Görevler oluşturma görevi, güncelleştirme görevi -><br/><br/> Sanal Makine Yapılandırma -><br/><br/> Sanal makine -> etkileşimde bulunma yanıt soru, cihaz bağlantısı ->, CD ortamı yapılandırmak, disket ortamı, kapatma, açma, VMware araçları yükleme yapılandırın<br/><br/> Sanal makine -> Stok Oluştur ->, kaydetme, kaydı<br/><br/> Sanal makine sağlama -> izin sanal makine indirme ->, sanal makine dosyalarını karşıya yükleme izin ver<br/><br/> Sanal makine anlık görüntüleri -> Kaldır anlık görüntüleri -> | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.|

## <a name="create-an-account-to-connect-to-vmware-vcenter-server-vmware-vsphere-exsi-host"></a>VMware vCenter sunucusuna bağlanmak için bir hesap oluşturun / VMware vSphere EXSi konağı
1. Yapılandırma sunucusu ve başlatma masaüstüne yerleştirilmiş kısayolunu kullanarak cspsconfigtool.exe oturum açın.
2. Tıklatın **hesabı Ekle** üzerinde **hesabı Yönet** sekmesi.

  ![Hesabı Ekle](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Hesap ayrıntıları sağlayın ve hesabı eklemek için Tamam'ı tıklatın. Hesabın listelenen ayrıcalıklarına sahip olması [otomatik bulma için bir hesap hazırlama](#prepare-an-account-for-automatic-discovery) bölümü.

  >[!NOTE]
  Yaklaşık 15 dakika yukarı Site Recovery hizmeti ile eşitlenmiş hesap bilgilerini alır.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>İlişkilendirme bir VMware vCenter / ESX VMware vSphere ana bilgisayar (Ekle vCenter)
* Azure Portal'da Gözat *YourRecoveryServicesVault* > **Site Recovery altyapısı** > **Configuration Server'lar** > *ConfigurationServer*
* Yapılandırma sunucusunun Ayrıntıları sayfasında tıklatın + vCenter düğmesi.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-to-connect-to-the-vcenter-server-vsphere-esxi-host"></a>VCenter sunucusuna bağlanmak için kullanılan kimlik bilgilerini değiştirmeyi / vSphere ESXi ana bilgisayar

1. Oturum başlatma cspsconfigtool.exe ve yapılandırma sunucusu
2. Tıklatın **hesabı Ekle** üzerinde **hesabı Yönet** sekmesi.

  ![Hesabı Ekle](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Yeni hesap ayrıntılarını sağlayın ve hesabı eklemek için Tamam'ı tıklatın. Hesabın listelenen ayrıcalıklarına sahip olması [otomatik bulma için bir hesap hazırlama](#prepare-an-account-for-automatic-discovery) bölümü.
4. Azure Portal'da Gözat *YourRecoveryServicesVault* > **Site Recovery altyapısı** > **Configuration Server'lar** > *ConfigurationServer*
5. Yapılandırma sunucusunun Ayrıntıları sayfasında tıklatın **sunucusunu Yenile eylemi** düğmesi.
6. Yenileme sunucu işi tamamlandığında, vCenter Server vCenter Özet sayfasını açmak için seçin.
7. Yeni eklenen hesaptaki **vCenter sunucusu/vSphere ana bilgisayar hesabı** alanına gelin ve **kaydetmek** düğmesi.

  ![değiştirme hesabı](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Azure Site Recovery Vcenter'da Sil
1. Azure Portal'da Gözat *YourRecoveryServicesVault* > **Site Recovery altyapısı** > **Configuration Server'lar** > *ConfigurationServer*
2. Yapılandırma sunucusunun Ayrıntıları sayfasında vCenter Server vCenter Özet sayfasını açmak için seçin.
3. Tıklayın **silmek** düğmesini vCenter silmek için

  ![hesabı Sil](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Sonra Vcenter IP adresi/FQDN, bağlantı noktası ayrıntıları değiştirmek gerekiyorsa vCenter Server silme ve yeniden geri eklemeniz gerekir.
