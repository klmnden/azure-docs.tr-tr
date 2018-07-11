---
title: " Azure Site recovery'de VMware vCenter sunucularını yönetme | Microsoft Docs"
description: Bu makalede nasıl ekleme ve Azure Site recovery'de VMware vCenter'ı yönetme.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: ramamill
ms.openlocfilehash: 6f3edf8e5d7a6fda1795991ac0a21cc316c29414
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950453"
---
# <a name="manage-vmware-vcenter-servers"></a>VMware vCenter sunucularını yönetme 

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

1. Kasanız Azure portalında açın > **Site Recovery altyapısı** > **yapılandırma sunucularından**, yapılandırma sunucusunu açın.
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

> [!NOTE]
VCenter IP adresi, FQDN veya bağlantı noktasını değiştirmeniz gerekiyorsa, vCenter sunucusu silme ve portala geri ekleme gerekir.
