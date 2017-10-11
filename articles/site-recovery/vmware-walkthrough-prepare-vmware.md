---
title: "Azure Site Recovery ile Azure'a çoğaltma için şirket içi VMware kaynakları hazırla | Microsoft Docs"
description: "Azure depolama alanına VMware Vm'lerinde çalışan iş yükleri çoğaltmak için gereken adımları özetler"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 3e1c589030210c2eae1ad9c02811775d9d6365d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-to-azure"></a>6. adım: şirket içi VMware çoğaltma Azure için hazırlama

Azure Site Recovery ile etkileşim kurmak için şirket içi VMware sunucuları hazırlamak için bu makaledeki yönergeleri kullanın ve VMWare Vm'lerini mobilite hizmetinin yüklenmesi için hazırlayın. Mobility Hizmeti Aracısı Azure'a çoğaltmak istediğiniz tüm şirket içi sanal makineleri yüklenmelidir.

## <a name="prepare-for-automatic-discovery"></a>Otomatik bulma işlemi için hazırlama

Site Recovery vSphere ESXi konakları (ile veya bir vCenter sunucusu olmadan) üzerinde çalışan sanal makineleri otomatik olarak bulur. Otomatik bulma için Site Recovery ana bilgisayarları ve sunucuları erişmek için bir hesap gerekir:

1. Adanmış bir hesap kullanmak için bir rol (düzeyinde vCenter, aşağıdaki tabloda açıklanan izinlerle. oluşturun Gibi bir ad verin **Azure_Site_Recovery**.
2. Ardından, vSphere ana bilgisayar/vCenter sunucusu üzerinde bir kullanıcı oluşturun ve kullanıcıya rol atayın. Site Recovery dağıtımı sırasında bu kullanıcı hesabı belirtin.


### <a name="vmware-account-permissions"></a>VMware hesabı izinleri

Site Recovery, sanal makineleri otomatik olarak bulmak üzere işlem sunucusu için ve yük devretme ve sanal makineleri geri dönmesi için VMware erişimi olmalıdır.

- **Geçiş**: yalnızca herhangi bir zamanda bunları geri başarısız olmadan VMware Vm'lerini Azure'a geçirmek istiyorsanız, bir salt okunur rolüyle VMware hesabı kullanabilirsiniz. Bu tür bir rol, yük devretme çalıştırabilirsiniz ancak korumalı kaynak makineleri kapatmayı olamaz. Bu geçiş için gerekli değildir.
- **Replicate/Recover**: tam çoğaltma (çoğaltılır, yük devretme, yeniden çalışma) dağıtmak istiyorsanız hesap oluşturma ve diskler, sanal makineleri vb. üzerinde başlatırken kaldırma gibi işlemleri çalıştırabilir ve olmalıdır.
- **Otomatik bulma**: en az bir salt okunur hesabı gereklidir.


**Görev** | **Gerekli hesap/rol** | **İzinler** | **Ayrıntılar**
--- | --- | --- | ---
**İşlem sunucusu VMware sanal makineleri otomatik olarak bulur.** | En az bir salt okunur kullanıcının gerekiyor | Veri Merkezi Nesne –> Propagate alt nesne için rol = salt okunur | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.
**Yük devretme** | En az bir salt okunur kullanıcının gerekiyor | Veri Merkezi Nesne –> Propagate alt nesne için rol = salt okunur | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.<br/><br/> Geçiş amacıyla, ancak değil tam çoğaltma, yük devretme, yeniden çalışma için kullanışlıdır.
**Yük devretme ve yeniden çalışma** | Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturun ve bir VMware kullanıcı veya grup rol atamak öneririz | Veri Merkezi Nesne –> Propagate alt nesne için rol Azure_Site_Recovery =<br/><br/> Veri deposu alanı Ayır ->, veri deposu, alt düzey dosya işlemleri göz atın, dosyayı kaldırmak, sanal makine dosyalarını güncelleştir<br/><br/> Ağ -> Ağ atama<br/><br/> Kaynak VM atamak için kaynak havuzu ->, VM güç beslemeli geçirmek, VM güç beslemeli geçirme<br/><br/> Görevler oluşturma görevi, güncelleştirme görevi -><br/><br/> Sanal Makine Yapılandırma -><br/><br/> Sanal makine -> etkileşimde bulunma yanıt soru, cihaz bağlantısı ->, CD ortamı yapılandırmak, disket ortamı, kapatma, açma, VMware araçları yükleme yapılandırın<br/><br/> Sanal makine -> Stok Oluştur ->, kaydetme, kaydı<br/><br/> Sanal makine sağlama -> izin sanal makine indirme ->, sanal makine dosyalarını karşıya yükleme izin ver<br/><br/> Sanal makine anlık görüntüleri -> Kaldır anlık görüntüleri -> | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.


## <a name="prepare-for-push-installation-of-the-mobility-service"></a>Mobilite hizmetinin göndermeli yüklemesi için hazırlama

Çoğaltmak istediğiniz tüm sanal makinelerin mobilite hizmetinin yüklenmesi gerekir. Bir hizmeti el ile yükleme de dahil olmak üzere, yüklemek, Site Recovery işlem sunucusu ve System Center Configuration Manager gibi yöntemlerle yükleme göndermeli için çeşitli yöntemler vardır.

Anında yükleme kullanmak istiyorsanız, Site Recovery VM erişmek için kullanabileceğiniz bir hesap hazırlamanız gerekir.

- Bir etki alanı veya yerel hesabı kullanın
- Windows, bir etki alanı hesabı kullanmıyorsanız yerel makine üzerinde uzak kullanıcı erişimi denetimini devre dışı bırakmak gerekir. Bu, altında kayıttaki yapmak için **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, DWORD girdisi ekleyin **LocalAccountTokenFilterPolicy**, 1 değerine sahip.
- CLI üzerinden için Windows kayıt defteri girdisini eklemek istiyorsanız, yazın:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux için hesabın kaynak Linux sunucusu kökünde olması gerekir.



## <a name="next-steps"></a>Sonraki adımlar

Git [adım 7: bir kasa oluşturun](vmware-walkthrough-create-vault.md)
