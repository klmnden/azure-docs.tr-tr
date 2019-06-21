---
title: SQL Server 2008 ve SQL Server 2008 R2 ile Azure desteği'ni genişletin
description: SQL Server 2008 ve SQL Server 2008 R2 desteği SQL Server Örneğinize Azure'a geçen veya örnekleri şirket içi tutmak için genişletilmiş destek satın genişletmeyi öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/08/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: ecb7030fa3652525a36ce15d66ea6e5daf9c3296
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304217"
---
# <a name="extend-support-for-sql-server-2008-and-sql-server-2008-r2-with-azure"></a>SQL Server 2008 ve SQL Server 2008 R2 ile Azure desteği'ni genişletin

SQL Server 2008 ve SQL Server 2008 R2 hem de yaklaşan [(EOS) destek yaşam döngüsü sonuna](https://www.microsoft.com/sql-server/sql-server-2008). Birçok müşterimizin yine de iki sürüm de kullandığımızdan, destek almaya devam etmek için çeşitli seçenekler sağlıyoruz. Azure sanal makineleri (VM'ler), şirket içi SQL Server örneklerini geçirmek, Azure SQL veritabanı'na geçirme veya şirket içinde kalmasını ve genişletilmiş güvenlik güncelleştirmeleri satın alın.

Farklı bir yönetilen örnek sayesinde, bir Azure sanal makinesi için geçiş uygulamalarınızın recertifying gerektirmez. Ve farklı olarak şirket içinde kaldığını ile ücretsiz genişletilmiş güvenlik düzeltme eklerinin bir Azure sanal makinesi için geçiş yaparak alırsınız.

Bu makalenin geri kalanında Azure VM'deki SQL Server Örneğinize geçiş konuları sağlar.

## <a name="provisioning"></a>Sağlama

Kullandığın kadar Öde yoktur `SQL Server 2008 R2 on Windows Server 2008 R2` Azure Marketi'nde görüntü.

SQL Server 2008'de müşterileri otomatik olarak yüklemeniz veya SQL Server 2008 R2'ye yükseltme ya da gerekir. Benzer şekilde, müşteriler Windows Server 2008 ya da kendi özel bir VHD dağıtmak veya Windows Server 2008 R2'ye yükseltmeniz gerekir.

Market aracılığıyla dağıtılan görüntülerin önceden yüklenmiş bir SQL Iaas uzantısı ile gelir. SQL Iaas uzantısı, esnek Lisanslama ve otomatik düzeltme eki uygulama için bir gereksinimdir. Kendi kendine yüklü Vm'leri dağıtma müşterilerin SQL Iaas uzantısı el ile yüklemeniz gerekir. SQL Iaas uzantısı, Windows 2008'de desteklenmiyor.

  > [!NOTE]
  > SQL Server while `Create` ve `Manage` dikey pencereler, Azure portalında SQL Server 2008R2 görüntüsü ile çalışır, aşağıdaki özellikler _desteklenmiyor_: Otomatik yedeklemeler, Azure Key Vault tümleştirmesi, R Hizmetleri ve depolama yapılandırması.

## <a name="licensing"></a>Lisanslama
Kullandıkça Öde SQL Server 2008R2 dağıtımları için dönüştürebilir [Azure hibrit Avantajı'nı (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/).

Yazılım Güvencesi (SA) tabanlı lisans Kullandıkça Öde aboneliğine dönüştürmek için müşterilerin SQL VM kaydolmalıdır [kaynak sağlayıcısı](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). SQL VM kaynak sağlayıcısına kayıtlı sonra SQL lisans türü AHB ve Kullandıkça Öde arasında birbirinin yerine olacaktır.

Azure VM'de SQL Server 2008 veya SQL Server 2008 R2 Self yüklü örnek SQL kaynak sağlayıcısı ile kaydedebilir veya kullanıcıların lisans türünü Kullandıkça Öde aboneliğine dönüştürün.

## <a name="migration"></a>Geçiş
El ile yedekleme/geri yükleme yöntemleri ile bir Azure VM'ye EOS SQL Server örneklerini geçirebilirsiniz; Bu en yaygın geçiş şirket içinden Azure VM'ye yöntemdir.

### <a name="azure-site-recovery"></a>Azure Site Recovery

Toplu geçişler için öneririz [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) hizmeti. Müşteriler, Azure Site Recovery ile Azure VM'LERİNİ şirket içi SQL Server dahil olmak üzere tüm VM çoğaltabilirsiniz.

SQL Server kurtarma güvence altına almak için uygulamayla tutarlı Azure Site Recovery anlık görüntüleri gerektirir; ve Azure Site Recovery uygulamayla tutarlı anlık görüntülerle en az 1 saatlik zaman aralığını destekler. Azure Site Recovery migrations ile SQL Server için mümkün olan en düşük RPO 1 saat ve RTO yanı sıra SQL Server kurtarma süresi 2 saat.

### <a name="database-migration-service"></a>Veritabanı Geçiş Hizmeti

[Veritabanı geçiş hizmeti](/azure/dms/dms-overview) geçirme şirket içi SQL Server'ı SQL Server 2012'ye yükseltme tarafından Azure VM ve büyük bir seçenek müşterilere yöneliktir.

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Azure sanal makinesinde EOS SQL Server için olağanüstü durum kurtarma çözümleri aşağıdaki gibidir:

- **SQL Server yedeklemeleri**: EOS SQL sunucunuza fidye yazılımı, yanlışlıkla silme ve bozulmalara karşı korumak için Azure Backup'ı kullanın. Çözüm, şu an EOS SQL Server için Önizleme aşamasındadır ve SQL Server 2008 ve 2008 R2 destekleyen Windows 2008 R2 SP1'de çalışıyor. Daha fazla ayrıntı için bu başvuru [makale](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#support-for-sql-server-2008-and-sql-server-2008-r2)
- **Günlük aktarma**: Başka bir günlük aktarma çoğaltma oluşturabilirsiniz bölgesi veya Azure bölgesiyle RTO azaltmak için sürekli geri yükler. Müşterilerin günlük aktarma el ile yapılandırmanız gerekir.
- **Azure Site Recovery**: Sanal makinenizin alanları ve bölgeleri arasında Azure Site Recovery çoğaltma arasında çoğaltabilirsiniz. SQL Server kaybedildiği bir olağanüstü durumda kurtarma sağlamak uygulama tutarlı anlık gerektirir. Azure Site Recovery EOS SQL Server DR için en az 1 saat RPO ve 2 saat + SQL Server kurtarma zamanı RTO sağlar.

## <a name="security-patching"></a>Güvenlik düzeltme eki uygulama
Genişletilmiş güvenlik güncelleştirmeleri SQL Server Vm'leri için SQL ile SQL Server VM kaydedildikten sonra Microsoft Update kanallar aracılığıyla gönderilir [kaynak sağlayıcısı](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). Düzeltme ekleri, ya da el ile veya otomatik olarak indirilebilir.

**Otomatik düzeltme eki uygulama** varsayılan olarak etkindir. Otomatik düzeltme eki uygulama Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını sağlar. SQL Iaas uzantısı yüklü değilse, bir gün haftanın günü, saati ve bir bakım penceresi süresi belirtebilirsiniz. Azure düzeltme eki uygulamayı bu bakım penceresinde gerçekleştirir. Bakım penceresi zamanlaması saat için VM yerel saatini kullanır.  Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-sql-automated-patching.md).


## <a name="next-steps"></a>Sonraki adımlar

SQL Server VM'yi Azure'a geçirme

* [Bir SQL Server veritabanını Azure VM’deki SQL Server’a geçirme](virtual-machines-windows-migrate-sql.md)

Azure sanal makinelerinde SQL Server kullanmaya başlayın:

* [Azure portalında SQL Server VM’i oluşturma](quickstart-sql-vm-create-portal.md)

SQL VM'ler hakkında sık sorulan soruların yanıtlarını alın:

* [Azure Sanal Makineler’de SQL Server ile ilgili SSS](virtual-machines-windows-sql-server-iaas-faq.md)
