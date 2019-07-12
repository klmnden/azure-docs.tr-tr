---
title: Azure geçişi sunucusu geçişi ile azure'a geçiş için Linux çalıştıran VMware sanal makinelerini hazırlama | Microsoft Docs
description: Bir Linux VM, Azure geçişi sunucusu geçişi ile Azure'a geçiş için hazırlanma işlemi açıklanmaktadır
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/14/2018
ms.author: raynew
ms.openlocfilehash: efc410699ef6f4722857c812b38473c8f54b911b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811797"
---
# <a name="prepare-linux-vmware-vms-for-migration-to-azure"></a>VMware sanal makineleri, Azure'a geçiş için hazırlama 

Bu makalede, bunları azure'a geçirmek istediğinizde Linux çalıştıran VMware sanal makinelerini nasıl hazırlama [Azure geçişi](migrate-overview.md). 

> [!NOTE]
> Azure geçişi sunucusu geçişi şu anda genel Önizleme aşamasındadır. Bulma ve sanal makineleri geçiş için değerlendirme için mevcut GA sürümünü Azure geçişi kullanabilirsiniz, ancak gerçek geçiş var olan genel kullanım sürümünde desteklenmiyor.

Linux makineleri aşağıdaki gibi hazırlayın:

1. Hyper-V Linux Tümleştirme hizmetlerini yükleyin. Linux dağıtımları daha yeni sürümlerinde bu varsayılan olarak yüklü olabilir).
2. Linux init görüntüyü yeniden derleme gerekli Hyper-V sürücüleri içeren ve böylece (bazı dağıtımları için gerekli) azure'da sanal makine önyükleme yapmaz.
3. Sorun giderme için seri konsol günlüklerini etkinleştirin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console).
4. Cihaz eşleme dosyası ile kalıcı cihaz tanımlayıcıları kullanılacak birim ilişkilendirmeleri, cihaz adı güncelleştirin.
5. Kalıcı hacim tanımlayıcıları kullanılacak fstab girişleri güncelleştirin.
6. MAC adresini vb. temel alarak arabirimi adlarını udev kuralları kaldırın.
7. DHCP IP adreslerini almak için ağ arabirimlerini güncelleştirme.
8. SSH olun etkinleştirilir. Sshd hizmet yeniden başlatıldığında otomatik olarak başlatılacak şekilde ayarlandığından emin olun.
9. Gelen ssh bağlantı istekleri işletim sistemi güvenlik duvarı veya IP tablo kuralları tarafından engellenmez emin olun.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) en popüler Linux dağıtımlarında bu değişiklik hakkında.

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası (hazırlama-için-azure.sh) Linux makineleri hazırlamak için bir örnek sağlar. Betik, tüm dağıtımları ve ortamları için çalışmayabilir, ancak bu yararlı bir başlangıç noktası.

Betik gösterir nasıl yapılır: 

- Gerekirse gerekli sürücüleri Linux init görüntüsüyle yeniden oluşturun.
- Kalıcı hacim tanımlayıcıları kullanılacak fstab girişleri güncelleştirin.
- Konsol günlükleri seri bağlantı noktasına yönlendirir.
- Azure seri erişim konsol etkinleştir
- Udev net kuralları kaldırın
- Bir sanal makine önyüklendiğinde çalıştırılan önyükleme betik çalıştırma yerleştirir. Bu makine Azure'da çalışıp çalışmadığını denetler. İse, VM'deki ağ yapılandırması güncelleştirmeleri ve bir IP adresi almak için DHCP kullanmak üzere ilk ethernet arabirimi ayarlar.

### <a name="before-you-start"></a>Başlamadan önce

- Örnek adımlar örnek betiği içerir. Bu, üretim sistemleri üzerinde çalışan olmamalıdır. Zarar verecek veya üzerinde çalıştığı sanal Makinenin bozulmasına neden.
- Bir VM test çalıştırmanızı öneririz. Başlamadan önce gerektiğinde VM'nin geri VM yedeklemesi veya anlık görüntüsünü alın. 
- Bu Linux dağıtımları, bir VM çalışırken betik çalışır:
    - Red Hat Enterprise Linux 6.5+, 7.1+
    - Cent OS 6.5+, 7.1+
    - SUSE Linux Enterprise Server 12 SP1, SP2 SP3
    - Ubuntu 14.04, 16.04, 18.04
    - Debian 7, 8

### <a name="run-the-script"></a>Betiği çalıştırın

1. Kopyalama ve Linux için komut dosyası, sftp veya FileZilla veya WinScp gibi bir scp İstemcisi'ni kullanarak VM test edin.
2. Bir yönetici hesabı kullanarak bir Linux makineye SSH.
3. Betik dizine gidin.
4. Betiği yürütülebilir bir dosyada hale getirmek için çalıştırma **sudo chmod 777 hazırlama-için-azure.sh**.
5. Komut dosyasını Çalıştır **./prepare-for-azure.sh**.

Komut dosyasının nasıl çalıştığını şu şekildedir:

![Linux betik](./media/how-to-prepare-linux-for-migration/script1.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script2.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script3.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script4.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script5.png)
![Linux betiği ](./media/how-to-prepare-linux-for-migration/script6.png)
 ![Linux betik](./media/how-to-prepare-linux-for-migration/script7.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script8.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script9.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script10.png)
![Linux betiği ](./media/how-to-prepare-linux-for-migration/script11.png)
 ![Linux betik](./media/how-to-prepare-linux-for-migration/script12.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script13.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script14.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script15.png)
![Linux betiği ](./media/how-to-prepare-linux-for-migration/script16.png)
 ![Linux betik](./media/how-to-prepare-linux-for-migration/script17.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script18.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script19.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script20.png)
![Linux betiği ](./media/how-to-prepare-linux-for-migration/script21.png)
 ![Linux betik](./media/how-to-prepare-linux-for-migration/script22.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script23.png)
![Linux betik](./media/how-to-prepare-linux-for-migration/script24.png)
![Linux betiği](./media/how-to-prepare-linux-for-migration/script25.png)



## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin [makine bağımlılık eşlemesi](how-to-create-group-machine-dependencies.md) yüksek güvenilirliğe sahip gruplar oluşturmak için.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
