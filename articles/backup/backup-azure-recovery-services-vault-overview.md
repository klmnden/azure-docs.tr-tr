---
title: Kurtarma Hizmetleri kasalarına genel bakış
description: Bir genel bakış ve kurtarma Hizmetleri kasaları ve Azure Backup kasaları karşılaştırması.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 8/10/2018
ms.author: raynew
ms.openlocfilehash: 924b36701ecf21f6bd84938aeefbf25e47fcbaa7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699265"
---
# <a name="recovery-services-vaults-overview"></a>Kurtarma Hizmetleri kasalarına genel bakış

Bu makalede, bir kurtarma Hizmetleri kasası özelliklerini açıklar. Kurtarma Hizmetleri kasası azure'da barındırıldığı veri depolama varlığıdır. Verileri veri ya da sanal makineleri (VM'ler), iş yükleri, sunucular veya iş istasyonları için yapılandırma bilgileri genellikle kopyalanır. Kurtarma Hizmetleri kasaları, Iaas VM'ler (Linux veya Windows) ve Azure SQL veritabanları gibi çeşitli Azure Hizmetleri için yedekleme verileri tutmak için kullanabilirsiniz. Kurtarma Hizmetleri kasaları destek System Center DPM, Windows Server, Azure Backup sunucusu ve daha fazlası. Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir.

Bir Azure aboneliğinde bölge başına abonelik başına en çok 500 kurtarma Hizmetleri kasası oluşturabilirsiniz.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Karşılaştırma kurtarma Hizmetleri kasaları ve yedekleme kasaları

Backup kasaları hala varsa, Kurtarma Hizmetleri kasalarına otomatik olarak yükseltme yükleniyor. Kasım 2017'ye kadar tüm Backup kasaları kurtarma Hizmetleri kasalarına yükseltilmiş oldu.

Kurtarma Hizmetleri kasaları, yedekleme kasaları Azure Service Manager modele dayalı ise Azure, Azure Resource Manager modelinin temel alır. Bir Backup kasasının kurtarma Hizmetleri kasasına yükselttiğinizde, yedekleme verileri sırasında ve sonrasında yükseltme işlemi değişmeden kalır. Kurtarma Hizmetleri kasaları için yedekleme kasalarını, mevcut olmayan özellikler gibi sağlayın:

- **Gelişmiş Özellikler yedek verilerin korunmasına yardımcı olun**: Kurtarma Hizmetleri kasaları ile Azure yedekleme, bulut yedeklemelerini korumak için güvenlik özellikleri sunar. Üretim ve yedekleme sunucuları tehlikede olsa bile güvenlik özelliklerine yedeklerinizi güvenli ve güvenli bir şekilde verileri kurtarmak emin olun. [Daha fazla bilgi](backup-azure-security-feature.md)

- **Karma BT ortamındaki Merkezi İzleme**: Kurtarma Hizmetleri kasaları ile izleyebilirsiniz yalnızca, [Azure Iaas Vm'leri](backup-azure-manage-vms.md) aynı zamanda, [şirket varlıkları](backup-azure-manage-windows-server.md#manage-backup-items) merkezi bir portaldan. [Daha fazla bilgi](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rol tabanlı erişim denetimi (RBAC)**: RBAC, azure'da ayrıntılı erişim yönetimi denetim sağlar. [Azure, çeşitli yerleşik rol sağlar](../role-based-access-control/built-in-roles.md), ve Azure Backup sahip üç [kurtarma noktaları yönetmek için yerleşik roller](backup-rbac-rs-vault.md). Kurtarma Hizmetleri kasaları, yedekleme kısıtlayan RBAC ile uyumludur ve tanımlanan kullanıcı rollerini kümesine erişimi geri yüklemek. [Daha fazla bilgi](backup-rbac-rs-vault.md)

- **Tüm yapılandırmalar Azure sanal makinelerinin korunmasına**: Kurtarma Hizmetleri kasaları Resource Manager tabanlı Vm'leri Premium diskler, yönetilen diskler ve şifrelenmiş VM'ler gibi koruyun. Bir Backup kasasının kurtarma Hizmetleri kasasına yükseltme, Resource Manager tabanlı sanal makinelere Service Manager tabanlı sanal makinelerinize yükseltme olanağı sunar. Kasa yükseltme yapılırken, Service Manager tabanlı VM kurtarma noktalarını Beklet ve yükseltilen (Resource Manager özellikli) VM'ler için korumayı yapılandırın. [Daha fazla bilgi](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Iaas Vm'leri için anında geri yükleme**: Kurtarma Hizmetleri Kasası'nı kullanarak, dosya ve klasörleri bir Iaas VM'den daha hızlı geri yükleme süreleri sağlayan tüm VM'yi geri yüklemeden geri yükleyebilirsiniz. Iaas Vm'leri için anında geri yükleme, hem Windows hem de Linux Vm'leri için kullanılabilir. [Daha fazla bilgi](https://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Portalı'nda, Kurtarma Hizmetleri kasalarını yönetme
Oluşturulmasını ve yönetimini Azure portalında kurtarma Hizmetleri kasaları olduğundan kolay yedekleme hizmeti diğer Azure Hizmetleri ile tümleşir. Bu tümleştirme oluşturabilir veya bir kurtarma Hizmetleri kasası yönetme anlamına gelir *hedef hizmet bağlamında*. Örneğin, bir VM için kurtarma noktalarını görüntülemek için VM'nizi seçin ve tıklayın **yedekleme** işlemler menüsünde.

![Kurtarma Hizmetleri kasasının ayrıntıları VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context-vm.png)

Ardından VM Yedekleme yapılandırıldı yoksa, bu, yedeklemeyi yapılandırmak için ister. Yedekleme yapılandırıldı, yedekleme geri yükleme noktaları listesi dahil olmak üzere, sanal makine bilgilerini görürsünüz.  

![Kurtarma Hizmetleri kasasının ayrıntıları VM](./media/backup-azure-recovery-services-vault-overview/vm-recovery-point-list.png)

Önceki örnekte, **ContosoVM** sanal makinenin adıdır. **ContosoVM-demovault** kurtarma Hizmetleri kasası adıdır. Kurtarma noktalarını depolayan bir kurtarma Hizmetleri kasası adını anımsamak gerekmez, bu bilgileri sanal makineden erişebilirsiniz.  

Bir kurtarma Hizmetleri kasası birden çok sunucu koruyorsa kurtarma Hizmetleri kasası aramak için daha mantıklı olabilir. Abonelikteki tüm kurtarma Hizmetleri kasaları için arama yapın ve listeden birini seçin.

Aşağıdaki bölümler, her etkinlik türü bir kurtarma Hizmetleri kasası kullanmayı açıklayan makalelerin bağlantıları içerir.

> [!NOTE]
> 24 saat içinde silinmiş olması durumunda, Kurtarma Hizmetleri kasası aynı ada sahip oluşturulamıyor. Farklı bir kaynak adı kullanın veya farklı bir kaynak grubu seçin veya 24 saat sonra yeniden deneyin.

### <a name="back-up-data"></a>Verileri yedekleyin
- [Bir Azure VM'yi yedekleme](backup-azure-vms-first-look-arm.md)
- [Windows Server veya Windows iş istasyonu yedekleme](backup-try-azure-backup-in-10-mins.md)
- [DPM iş yüklerini Azure'a yedekleme](backup-azure-dpm-introduction.md)
- [Azure Backup sunucusu kullanarak iş yüklerini yedeklemek hazırlama](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Kurtarma noktalarını yönetin
- [Azure VM yedeklemelerini yönetme](backup-azure-manage-vms.md)
- [Dosyaları ve klasörleri yönetme](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Verileri kasadan geri yükleme
- [Bir Azure VM'den tek tek dosya kurtarma](backup-azure-restore-files-from-vm.md)
- [Azure VM geri yükleme](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Kasa güvenliğini sağlama
- [Bulut yedekleme verileri kurtarma Hizmetleri kasaları, güvenliğini sağlama](backup-azure-security-feature.md)



## <a name="next-steps"></a>Sonraki Adımlar
İçin aşağıdaki makaleleri kullanın:</br>
[Bir Iaas VM'yi yedekleme](backup-azure-arm-vms-prepare.md)</br>
[Bir Azure Backup sunucusu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Bir Windows Server Yedekleme](backup-configure-vault.md)
