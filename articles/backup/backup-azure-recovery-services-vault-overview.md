---
title: "Genel Bakış kurtarma Hizmetleri kasaları | Microsoft Docs"
description: "Genel bir bakış ve kurtarma Hizmetleri kasalarının ve Azure yedekleme kasaları karşılaştırması."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/15/2017
ms.author: markgal;arunak;sogup
ms.openlocfilehash: ad685744595ab86def8d226d7c2fb5455af98da4
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="recovery-services-vaults-overview"></a>Kurtarma Hizmetleri kasaları genel bakış

Bu makalede, bir kurtarma Hizmetleri kasası özelliklerini açıklar. Kurtarma Hizmetleri kasası veri barındırıldığı Azure depolama varlıktır. Genellikle veri ya da sanal makineleri (VM'ler), iş yükleri, sunucular veya iş istasyonları için yapılandırma bilgilerini kopyalarını veridir. Kurtarma Hizmetleri kasalarının Iaas VM'ler (Linux veya Windows) ve Azure SQL veritabanları gibi çeşitli Azure Hizmetleri için yedek verileri saklamak için kullanabilirsiniz. Kurtarma Hizmetleri kasaları destek System Center DPM, Windows Server, Azure yedekleme sunucusu ve daha fazlası. Kurtarma Hizmetleri kasaları, yedekleme verilerinizi düzenlemeyi kolaylaştırırken yönetim zorluklarını da en aza indirir. 

Bir Azure aboneliği bölge başına en fazla 25 kurtarma Hizmetleri kasalarının oluşturabilirsiniz.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Karşılaştırma kurtarma Hizmetleri kasaları ve yedekleme kasaları

Yedekleme kasaları hala varsa, bunlar otomatik-kurtarma Hizmetleri kasalarının yükseltiliyor. Kasım 2017 tüm yedekleme kasaları kurtarma Hizmetleri kasalarının yükseltildi. 

Kurtarma Hizmetleri kasaları, yedekleme kasaları Azure Service Manager modeline bağlı olan ancak Azure, Azure Resource Manager modelini temel alır. Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltme yaparken, yedekleme verilerini sırasında ve yükseltme işlemi sonrasında değişmeden kalır. Kurtarma Hizmetleri kasaları gibi yedekleme kasaları için kullanılamayan özellikleri sağlar:

- **Gelişmiş Güvenlik yedekleme verileri yardımcı olmak için özellikleri**: ile kurtarma Hizmetleri kasaları, Azure Backup bulut yedekleme korumak için güvenlik özellikleri sağlar. Üretim ve yedekleme sunucuları tehlikeye olsa bile güvenlik özellikleri Yedeklemelerinizin güvenli ve güvenli bir şekilde verileri kurtarmak emin olun. [Daha fazla bilgi](backup-azure-security-feature.md)

- **Karma BT ortamında Merkezi İzleme**: ile kurtarma Hizmetleri kasaları, izleyebilirsiniz yalnızca, [Azure Iaas Vm'leri](backup-azure-manage-vms.md) aynı zamanda, [içi varlıklar](backup-azure-manage-windows-server.md#manage-backup-items) merkezi bir gelen Portal. [Daha fazla bilgi](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rol tabanlı erişim denetimi (RBAC)**: RBAC Azure ayrıntılı erişim yönetimi denetim sağlar. [Azure sağlayan çeşitli yerleşik roller](../active-directory/role-based-access-built-in-roles.md), ve Azure Backup sahip üç [kurtarma noktaları yönetmek için yerleşik roller](backup-rbac-rs-vault.md). Kurtarma Hizmetleri kasaları yedekleme kısıtlayan RBAC ile uyumludur ve kullanıcı rolleri tanımlı kümesine erişim geri yükleyin. [Daha fazla bilgi](backup-rbac-rs-vault.md)

- **Tüm yapılandırmaları Azure sanal makinelerin korunmasına**: Kurtarma Hizmetleri kasaları şunları korur Resource Manager tabanlı VM'ler Premium diskler, yönetilen diskleri ve şifrelenmiş VM'ler dahil olmak üzere. Bir Backup kasasının kurtarma Hizmetleri Kasası'na yükseltme Resource Manager tabanlı VM'ler için Service Manager tabanlı VM'ler yükseltme olanağı sağlar. Kasa yükseltirken, Service Manager tabanlı VM kurtarma noktalarını korumak ve yükseltilen (Resource Manager etkin) VM'ler için korumayı yapılandırın. [Daha fazla bilgi](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Iaas VM'ler için anlık geri**: kullanarak kurtarma Hizmetleri kasaları, dosya ve klasörlerin bir Iaas sanal makineden daha hızlı geri yükleme süreleri sağlar tüm VM geri yüklemeden geri yükleyebilirsiniz. Iaas VM'ler için anlık geri yükleme, Windows ve Linux VM'ler için kullanılabilir. [Daha fazla bilgi](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Portalı'nda, Kurtarma Hizmetleri kasalarını yönetme
Oluşturulmasını ve yönetimini kurtarma Hizmetleri kasalarını Azure portalındaki kolay Backup hizmeti Azure ayarları menüsüne tümleşik olduğundan. Bu tümleştirme oluşturmak veya bir kurtarma Hizmetleri kasası yönetmek anlamına gelir *hedef hizmet bağlamında*. Örneğin, bir VM için kurtarma noktalarını görüntülemek için onu seçin ve tıklatın **yedekleme** Ayarlar menüsünden. Bu VM özel yedekleme bilgileri görüntülenir. Aşağıdaki örnekte, **ContosoVM** sanal makinenin adıdır. **ContosoVM demovault** kurtarma Hizmetleri kasası adıdır. Kurtarma noktalarını depolayan kurtarma Hizmetleri kasası adını anımsamak gerekmez, bu bilgiler sanal makineden erişebilir.  

![Kurtarma Hizmetleri kasası ayrıntıları VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context.png)

Birden çok sunucu, aynı kurtarma Hizmetleri kasası kullanarak koruduysanız kurtarma Hizmetleri kasası aramak için daha fazla mantıksal olabilir. Abonelikteki tüm kurtarma Hizmetleri kasalarının aramak ve listeden birini seçin.

Aşağıdaki bölümler, her etkinlik türü bir kurtarma Hizmetleri kasası kullanmayı açıklayan makalelerinin bağlantıları içerir.

### <a name="back-up-data"></a>Verileri yedekleyin
- [Bir Azure VM'yi yedeklemek](backup-azure-vms-first-look-arm.md)
- [Windows Server veya Windows iş istasyonu yedekleyin](backup-try-azure-backup-in-10-mins.md)
- [DPM iş yüklerini Azure'a yedekleme](backup-azure-dpm-introduction.md)
- [Azure yedekleme sunucusu kullanarak iş yüklerini Yedeklemeye hazırlanma](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Kurtarma noktalarını yönetin
- [Azure VM yedeklemeleri yönetme](backup-azure-manage-vms.md)
- [Dosyaları ve klasörleri yönetme](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Kasadan veri geri yükleme
- [Tek tek dosyaların bir Azure sanal makineden kurtarma](backup-azure-restore-files-from-vm.md)
- [Bir Azure VM geri yükleme](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Kasa güvenli
- [Kurtarma Hizmetleri kasalarının bulut yedekleme verileri güvenli hale getirme](backup-azure-security-feature.md)



## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki makaleler için kullanın:</br>
[Bir Iaas VM'yi yedeklemek](backup-azure-arm-vms-prepare.md)</br>
[Bir Azure yedekleme sunucuyu yedekleme](backup-azure-microsoft-azure-backup.md)</br>
[Bir Windows Server'ı Yedekle](backup-configure-vault.md)
