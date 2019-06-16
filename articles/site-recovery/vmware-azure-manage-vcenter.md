---
title: Azure Site Recovery kullanılarak Azure'da VMware vm'lerinin olağanüstü durum kurtarma için VMware vCenter sunucularını yönetme | Microsoft Docs
description: Bu makalede nasıl ekleme ve Azure'da Azure Site Recovery ile VMware vm'lerinin olağanüstü durum kurtarma için VMware vCenter'ı yönetme.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: ramamill
ms.openlocfilehash: 9694c682f171ab715812b05fed2064c9bbcd36b3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60600362"
---
# <a name="manage-vmware-vcenter-server"></a>VMware vCenter server'ı yönetme

Bu makalede, bir VMware vCenter üzerinde gerçekleştirilen çeşitli Site Recovery işlemleri açıklanır. Doğrulama [önkoşulları](vmware-physical-azure-support-matrix.md#replicated-machines) başlamadan önce.


## <a name="set-up-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesabı ayarlayın

Site Recovery, yük devretme ve yeniden çalışma sanal makinelerin ve işlem sunucusu sanal makineleri otomatik olarak bulmak üzere VMware erişmesi gerekir. Erişim için bir hesap şu şekilde oluşturun:

1. Yapılandırma sunucusu makinede oturum açın.
2. Başlatma masaüstü kısayolunu kullanarak cspsconfigtool.exe açın.
3. Tıklayın **hesabı Ekle** üzerinde **hesabı Yönet** sekmesi.

   ![Hesap Ekle](./media/vmware-azure-manage-vcenter/addaccount.png)
1. Hesabı ayrıntılarını girin ve tıklatın **Tamam** ekleyin.  Hesabınız aşağıdaki tabloda özetlenen ayrıcalıkları olmalıdır. 

Yedekleme Site Recovery hizmeti ile eşitlenmesi hesap bilgileri için yaklaşık 15 dakika sürer.

### <a name="account-permissions"></a>Hesap izinleri

|**Görev** | **Hesap** | **İzinler** | **Ayrıntılar**|
|--- | --- | --- | ---|
|**Otomatik bulma/geçirme (olmadan yeniden çalışma)** | En az bir salt okunur kullanıcı gerekir | Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için Ata **erişemez** rolüyle **alt yay** nesnesine alt nesnelere (vSphere konakları, veri depoları, sanal makineler ve ağlar).|
|**Çoğaltma/yük devretme** | En az bir salt okunur kullanıcı gerekir| Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için Ata **erişemez** rolüyle **alt yay** nesne alt nesnelere (vSphere konakları, veri depoları, sanal makineler ve ağlar).<br/><br/> Geçiş amacıyla, ancak tam çoğaltma, yük devretme, yeniden çalışma için kullanışlıdır.|
|**Çoğaltma/yük devretme/yeniden çalışma** | Gerekli izinlere sahip bir rol (AzureSiteRecoveryRole) oluşturup rolü VMware kullanıcısı veya grubu atayabilirsiniz öneririz. | Veri Merkezi nesnesi –> alt nesneye yay, rol AzureSiteRecoveryRole =<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için Ata **erişemez** rolüyle **alt yay** nesnesine alt nesnelere (vSphere konakları, veri depoları, sanal makineler ve ağlar).|


## <a name="add-vmware-server-to-the-vault"></a>Kasaya VMware sunucusunu ekleme

1. Kasanızın Azure portalında açın > **Site Recovery altyapısı** > **yapılandırma sunucularından**, yapılandırma sunucusunu açın.
2. Üzerinde **ayrıntıları** sayfasında **+ vCenter**.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials"></a>Kimlik bilgilerini değiştirme

Aşağıdaki gibi vCenter sunucusu veya ESXi ana bilgisayarına bağlanmak için kullanılan kimlik bilgilerini değiştirin:

1. Yapılandırma sunucuya oturum açın ve masaüstünden cspsconfigtool.exe'yi başlatın.
2. Tıklayın **hesabı Ekle** üzerinde **hesabı Yönet** sekmesi.

   ![Hesap Ekle](./media/vmware-azure-manage-vcenter/addaccount.png)
3. Yeni hesap ayrıntılarını belirtin ve tıklayın **Tamam** ekleyin. Hesabınız listelenen ayrıcalıkları olmalıdır [yukarıda](#account-permissions).
4. Kasa Azure portalında açın > **Site Recovery altyapısı** > **yapılandırma sunucularından**, yapılandırma sunucusunu açın.
5. İçinde **ayrıntıları** sayfasında **sunucusunu Yenile eylemi**.
6. Sunucuyu Yenile işi tamamlandıktan sonra ' % s'vCenter sunucusu ile vCenter'ı açmak için seçin **özeti** sayfası.
7. Yeni eklenen hesaptaki **vCenter sunucusu/vSphere ana bilgisayar hesabı** alan ve tıklayın **Kaydet**.

   ![Hesap Değiştir](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>VCenter sunucusu silme

1. Kasanızın Azure portalında açın > **Site Recovery altyapısı** > **yapılandırma sunucularından**, yapılandırma sunucusunu açın.
2. Üzerinde **ayrıntıları** sayfasında, vCenter server'ı seçin.
3. Tıklayarak **Sil** düğmesi.

   ![hesabı Sil](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

## <a name="modify-the-vcenter-ip-address-and-port"></a>VCenter IP adresi ve bağlantı noktası değiştirme

1. Azure Portal’da oturum açın.
2. Gidin **kurtarma Hizmetleri kasası** > **Site Recovery altyapısı** > **Configuration Servers**.
3. Atanan yapılandırma sunucusundaki vCenter'ı tıklatın.
4. İçinde **vCenter sunucuları** bölümünde, değiştirmek istediğiniz vCenter'ı tıklatın.
5. VCenter Özet sayfasında, ilgili alanları vCenter'ın bağlantı noktası ve IP adresi güncelleştirin ve ardından değişikliklerinizi kaydedin.

   ![add_ip_new_vcenter](media/vmware-azure-manage-vcenter/add-ip.png)

6. Değişikliklerinin etkinleşmesi için 15 dakika bekleyin ya da [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="migrate-all-protected-virtual-machines-to-a-new-vcenter"></a>Yeni bir vCenter tüm korumalı sanal makineleri geçirme

Yeni vCenter tüm sanal makineleri geçirmek için başka bir vCenter hesabı eklemeyin. Bu, yinelenen girişler için açabilir. Yalnızca yeni vCenter'ın IP adresini güncelleştirin:

1. Azure Portal’da oturum açın.
2. Gidin **kurtarma Hizmetleri kasası** > **Site Recovery altyapısı** > **Configuration Servers**.
3. Atanan yapılandırma sunucusundaki eski vCenter'ı tıklatın.
4. İçinde **vCenter sunucuları** bölümünde, geçiş planlıyorsanız vcenter'ı tıklatın.
5. IP adresi alanına yeni vCenter'ın vCenter Özet sayfasında, güncelleştirme **vCenter sunucusu/vSphere konak adı veya IP adresi**. Yaptığınız değişiklikleri kaydedin.

IP adresi güncelleştirildikten hemen sonra Site Recovery bileşenlerini yeni vCenter sanal makinelerin bulma bilgileri almaya başlar. Bu, devam eden çoğaltma etkinlikleri etkilemez.

## <a name="migrate-few-protected-virtual-machines-to-a-new-vcenter"></a>Yeni bir vCenter birkaç korumalı sanal makineleri geçirme

> [!NOTE]
> Bu bölüm, yalnızca zaman birkaç korumalı sanal makineleriniz için yeni bir vCenter geçirdiğiniz geçerlidir. Yeni bir vCenter Server'dan yeni bir sanal makine kümesi korumak istiyorsanız [yapılandırma sunucusuna yeni vCenter ayrıntılarını Ekle](#add-vmware-server-to-the-vault) ve başlayın  **[korumayı etkinleştir](vmware-azure-tutorial.md#enable-replication)** .

Yeni bir vCenter birkaç sanal makineleri taşımak için:

1. [Yapılandırma sunucusuna yeni vCenter ayrıntılarını ekleme](#add-vmware-server-to-the-vault).
2. [Sanal makinelerin çoğaltmasını devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) geçirilmesi için planlama.
3. Yeni vcenter seçili sanal makinelerin geçişini tamamlayın.
4. Şimdi, geçirilen sanal makineler tarafından korumak [korumayı etkinleştirdiğinizde yeni vCenter'ı seçerek](vmware-azure-tutorial.md#enable-replication).

> [!TIP]
> Geçirilmekte olan sanal makinelerin sayı **yüksek** burada verilen yönergeleri kullanarak yeni vCenter'ın IP adresi güncelleştirme eski vCenter korunan sanal makine sayısı. Eski vCenter korunur birkaç sanal makineler için [çoğaltmayı devre dışı bırak](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure); [yapılandırma sunucusuna yeni vCenter ayrıntılarını Ekle](#add-vmware-server-to-the-vault)ve başlangıç  **[korumayı etkinleştir](vmware-azure-tutorial.md#enable-replication)** .

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

1. Korunan sanal makinelerin bir ESXi konağından diğerine taşınırsa, çoğaltma etkileyecek?

    Hayır, bu devam eden çoğaltma etkilemez. Ancak, [yeterli ayrıcalıklara sahip ana hedef sunucusu dağıttığınızdan emin olun](vmware-azure-reprotect.md#deploy-a-separate-master-target-server)

2. Bağlantı noktası numaralarıdır için vCenter ve diğer Site Recovery arasındaki iletişim için bileşenleri kullanılıyor?

    Varsayılan bağlantı noktası 443 ' dir. Yapılandırma Sunucusu'na vCenter/vSphere konak bilgilerini Bu bağlantı noktası üzerinden erişir. Bu bilgileri güncelleştirmek isterseniz, tıklayın [burada](#modify-the-vcenter-ip-address-and-port).
