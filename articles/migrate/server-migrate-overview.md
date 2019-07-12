---
title: Bir Azure geçişi sunucusu geçişi ile VMware geçiş seçeneğini | Microsoft Docs
description: Azure geçişi sunucusu geçişi ile azure'a geçirme VMware Vm'leri için seçeneklerine genel bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: f8bfbe26dc4c6ddbcf622f91938ba060de00b2ec
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811576"
---
# <a name="select-a-vmware-migration-option"></a>Bir VMware geçiş seçeneğini belirleyin

VMware Vm'lerini Azure geçişi Server Geçiş Aracı kullanarak Azure'a geçirebilirsiniz. Bu araç, birkaç VMware VM geçişi için seçenek sunar:

- Aracısız çoğaltma kullanarak geçiş. Herhangi bir şey yükler gerek kalmadan sanal makineleri geçirin.
- Çoğaltma için bir aracı geçişle. VM çoğaltma için bir aracı yükleyin.


Aracısız çoğaltma dağıtım açısından daha kolay olsa da, şu anda bir dizi sınırlandırması vardır.

## <a name="agentless-migration-limitations"></a>Aracısız geçiş sınırlamaları

Sınırlamalar aşağıdaki gibidir:

- **Eşzamanlı çoğaltma**: En çok 50 sanal makineleri aynı anda bir vCenter Server çoğaltılabilir.<br/> Geçiş için 50'den fazla VM varsa, VM'lerin birden çok toplu iş oluşturun.<br/> Daha fazla tek seferde çoğaltma performansını etkiler.
- **VM diskleri**: Geçirmek istediğiniz bir VM 60 veya daha az disk olması gerekir.
- **VM işletim sistemleri**: Genel olarak, Azure geçişi herhangi bir Windows Server veya Linux işletim sistemi geçirebilirsiniz, ancak Azure'da çalıştırabilmeniz için VM'ler üzerinde değişiklikler gerektirebilir. Azure geçişi, bu işletim sistemleri için otomatik olarak değişiklikleri yapar:
    - Red Hat Enterprise Linux 6.5+, 7.0+
    - CentOS 6.5+, 7.0+
    - SUSE Linux Enterprise Server 12 SP1+
    - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS
    - Debian 7,8
    - Diğer işletim sistemleri için el ile geçiş işleminden önce ayarlamalar yapmanız gerekir. [Öğretici geçirme](tutorial-migrate-vmware.md) bunun nasıl yapılacağı açıklanır.
- **Linux önyükleme**: Makinesiyse adanmış bir bölüme ise, işletim sistemi diskinde bulunması gereken ve birden çok diske yayılma değil.<br/> Makinesiyse (/) kök bölümünde parçası ise, ardından '/' bölüm işletim sistemi diskinde olması ve diğer disklerin span değil.
- **UEFI boot'a**: UEFI boot'a sahip sanal makineleri geçiş için desteklenmiyor.
- **Şifrelenmiş diskler/birim (BitLocker, cryptfs)** : Şifrelenmiş diskler/birimleri olan sanal makineleri geçiş için desteklenmiyor.
- **RDM/geçiş diskleri**: VM, RDM veya doğrudan geçiş diskleri varsa, bu diskleri Azure'a çoğaltılması olmaz
- **NFS**: NFS birimleri Vm'lerde birimleri olarak bağlanabilir çoğaltılması gerekmez.
- **Hedef depolama alanını**: VMware Vm'lerini Azure Vm'leri yönetilen disklerle (HDD standart, Premium SSD) yalnızca geçirebilirsiniz.



## <a name="deployment-steps-comparison"></a>Dağıtım adımları karşılaştırma

Sınırlamaları gözden geçirdikten sonra her çözüm dağıtımında yer alan adımların anlama seçmek için hangi seçeneği karar vermenize yardımcı.

**Görev** | **Ayrıntılar** |**Aracısız** | **Aracı tabanlı**
--- | --- | --- | ---
**VMware sunucuları ve sanal makineleri geçiş için hazırlama** | Ayar, VMware sunucuları ve Vm'leri üzerinde yapılandırın. | Gerekli | Gerekli
**Sunucu geçiş aracı ekleme** | Azure geçişi Server Geçiş Aracı, Azure geçişi projesinde ekleyin. | Gerekli | Gerekli
**Azure geçişi Gereci dağıtma** | VM bulma ve değerlendirme için bir VMware VM'de basit bir gereç ayarlayın. | Gerekli | Gerekli değildir.
**Vm'lerinde Mobility hizmetini yükleme** | Çoğaltmak istediğiniz her sanal makinede Mobility hizmetini yükleme | Gerekli değil | Gerekli
**Azure geçişi sunucusu geçişi çoğaltma Gereci dağıtma** | VM'ler ve Azure geçişi sunucusu geçişi üzerinde çalışmakta olan mobilite hizmet arasında köprü oluşturması ve bir VMware VM'de bir gereç Vm'leri bulmak için ayarlayın | Gerekli değil | Gerekli
**Vm'lerini çoğaltma**. VM çoğaltmayı etkinleştirin. | Çoğaltma ayarlarını yapılandırma ve çoğaltma sanal makineleri seçin | Gerekli | Gerekli
**Geçiş testi çalıştırma** | Her şeyin beklendiği gibi çalıştığından emin olmak için geçiş testi çalıştırma. | Gerekli | Gerekli
**Tam bir geçiş çalıştırın** | Vm'leri geçirme. | Gerekli | Gerekli




## <a name="next-steps"></a>Sonraki adımlar

[VMware Vm'lerini geçirme](tutorial-migrate-vmware.md) aracısız geçiş kullanarak.



