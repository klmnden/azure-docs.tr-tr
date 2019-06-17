---
title: Azure NetApp dosyaları hakkında SSS | Microsoft Docs
description: Azure NetApp dosyaları hakkında sık sorulan soruları yanıtlar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: b-juche
ms.openlocfilehash: 6f1ca3398678b59a81e5c22b51b36a1f5505d4c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806394"
---
# <a name="faqs-about-azure-netapp-files"></a>Azure NetApp dosyaları hakkında SSS

Bu makalede, Azure NetApp dosyaları hakkında sık sorulan sorular (SSS) yanıtlarını. 

## <a name="networking-faqs"></a>Ağ iletişimi SSS

### <a name="does-the-nfs-data-path-go-over-the-internet"></a>NFS veri yolu Internet üzerinden gider?  

Hayır. NFS veri yolu, Internet üzerinden geçmez. Azure NetApp dosyaları Azure sanal hizmetin kullanılabildiği ağa (VNet) dağıtıldığı bir Azure yerel hizmetidir. Azure NetApp dosya yetkilendirilmiş bir alt ağ kullanır ve VNet üzerinde doğrudan bir ağ arabirimi sağlar. 

Bkz: [yönergeleri Azure NetApp dosyaları için ağ planlama](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) Ayrıntılar için.  

### <a name="can-i-connect-a-vnet-that-i-already-created-to-the-azure-netapp-files-service"></a>NetApp dosyaları Azure hizmeti için önceden oluşturulmuş bir sanal ağa bağlayabilir miyim?

Evet, oluşturduğunuz sanal ağlar hizmete bağlanabilir. 

Bkz: [yönergeleri Azure NetApp dosyaları için ağ planlama](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) Ayrıntılar için.  

### <a name="can-i-mount-an-nfs-volume-of-azure-netapp-files-using-dns-fqdn-name"></a>Ben Azure NetApp DNS FQDN adı kullanarak dosyalarını bir NFS birimi takabilir miyim?

Gerekli DNS girişleri oluşturmanızı, Evet, kullanabilirsiniz. Azure NetApp dosyaları için sağlanan birimin hizmet IP sağlar. 

> [!NOTE] 
> NetApp dosya Azure hizmeti için ek IP gerektiği şekilde dağıtabilirsiniz.  DNS girişlerini düzenli olarak güncelleştirilmesi gerekebilir.

## <a name="security-faqs"></a>Güvenlik hakkında SSS

### <a name="can-the-network-traffic-between-the-azure-vm-and-the-storage-be-encrypted"></a>Azure VM ve depolama arasındaki ağ trafiğini şifrelenebilir?

Veri trafiği (Azure NetApp dosyaları birimlere NFSv3 veya SMBv3 istemciden gelen trafik) şifrelenmez. Ancak, trafik (bir NFS veya SMB istemcisi çalıştıran) bir Azure VM'den Azure NetApp dosyaları başka bir Azure VM için VM trafik güvenlidir. Bu Azure veri merkezi ağa yerel trafiğidir. 

### <a name="can-the-storage-be-encrypted-at-rest"></a>Depolama, bekleme sırasında şifrelenebilir?

Tüm Azure NetApp dosyaları birimleri, FIPS 140-2 standardı kullanılarak şifrelenir. Tüm anahtarları Azure NetApp dosya hizmeti tarafından yönetilir. 

### <a name="how-are-encryption-keys-managed"></a>Şifreleme anahtarları nasıl yönetilir? 

Azure NetApp dosyaları için anahtar yönetimi hizmeti tarafından gerçekleştirilir.  Şu anda, kullanıcı tarafından yönetilen anahtarlar (Getir bilgisayarınızı kendi anahtarlar) desteklenmez.

### <a name="can-i-configure-the-nfs-export-policy-rules-to-control-access-to-the-azure-netapp-files-service-mount-target"></a>Azure NetApp dosyaları hizmet bağlama hedefi erişimi denetlemek için NFS verme ilkesi kuralları yapılandırabilirim?


Evet, tek bir NFS Verme İlkesi'nde beş adede kadar kural yapılandırabilirsiniz.

### <a name="does-azure-netapp-files-support-network-security-groups"></a>NetApp dosya Azure ağ güvenlik grupları destekliyor mu?

Hayır, şu anda, ağ güvenlik grupları Azure NetApp dosya ya da hizmeti tarafından oluşturulan ağ arabirimleri temsilci alt ağa uygulayamazsınız.

### <a name="can-i-use-azure-iam-with-azure-netapp-files"></a>Azure IAM Azure NetApp dosyaları ile kullanabilir miyim?

Evet, Azure NetApp dosyaları Azure IAM RBAC özellikleriyle destekler.

## <a name="performance-faqs"></a>Performans ile ilgili SSS

### <a name="what-should-i-do-to-optimize-or-tune-azure-netapp-files-performance"></a>En iyi duruma getirme veya Azure NetApp dosyaları performansı ayarlamak için ne yapmalıyım?

Performans gereksinimleri başına aşağıdaki eylemleri gerçekleştirebilirsiniz: 
- Sanal makineyi uygun şekilde boyutlandırıldığından emin olun.
- Sanal makine için hızlandırılmış ağ iletişimi etkinleştirin.
- İstenen hizmet düzeyi ve kapasitesi havuzu boyutu seçin.
- Kapasite ve performans için istenen kotayı boyutuna sahip bir birim oluşturun.

### <a name="how-do-i-convert-throughput-based-service-levels-of-azure-netapp-files-to-iops"></a>Aktarım hızı tabanlı hizmet düzeyleri Azure NetApp dosyalarının IOPS için nasıl dönüştürebilirim?

Aşağıdaki formülü kullanarak IOPS için MB/sn dönüştürebilirsiniz:  

`IOPS = (MBps Throughput / KB per IO) * 1024`

### <a name="how-do-i-change-the-service-level-of-a-volume"></a>Bir birim hizmet düzeyini nasıl değiştirebilirim?

Bir birim hizmet düzeyini değiştirerek şu anda desteklenmiyor.

### <a name="how-do-i-monitor-azure-netapp-files-performance"></a>Azure NetApp dosyaları performansı nasıl izleyebilirim?

Azure NetApp dosyaları toplu performans ölçümleri sağlar. Azure İzleyici, Azure için NetApp dosyaları kullanım ölçümleri izlemek için de kullanabilirsiniz.  Bkz: [Azure NetApp dosyaları için ölçümleri](azure-netapp-files-metrics.md) performans ölçümlerini Azure NetApp dosyaların listesi.

## <a name="nfs-faqs"></a>NFS SSS

### <a name="i-want-to-have-a-volume-mounted-automatically-when-an-azure-vm-is-started-or-rebooted--how-do-i-configure-my-host-for-persistent-nfs-volumes"></a>Bir Azure VM başlatıldığında veya yeniden başlatıldığında otomatik olarak bağlanmış bir birime sahip istiyorsunuz.  My konağını kalıcı NFS birimleri nasıl yapılandırabilirim?

NFS birimi VM başlatma veya yeniden başlatma otomatik olarak bağlamak bir giriş eklemek `/etc/fstab` konaktaki. 

Örneğin, `$ANFIP:/$FILEPATH      /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

- $ANFIP  
    Azure NetApp dosyaları biriminin birim özellikleri dikey penceresinde bulunan IP adresi
- $FILEPATH  
    Azure NetApp dosyaları birim dışarı aktarma yolu
- $MOUNTPOINT  
    NFS dışarı aktarma bağlamak için kullandığınız Linux ana bilgisayarına oluşturulduğu dizini

### <a name="why-does-the-df-command-on-nfs-client-not-show-the-provisioned-volume-size"></a>Neden DF komutunu NFS İstemcisi için sağlanan birim hacminin göstermiyor?

DF içinde bildirilen birim boyutu, Azure NetApp dosyaları birime kadar büyüyebilir en büyük boyutudur. DF komutta Azure NetApp dosyaları birimin boyutu kota veya birimin boyutu yansıtıcı değil.  Azure portalı veya API Azure NetApp dosyaları birim boyutu veya kota alabilirsiniz.

### <a name="what-nfs-version-does-azure-netapp-files-support"></a>Hangi NFS sürüm Azure NetApp dosyaları destekliyor mu?

NetApp Azure dosyaları şu anda NFSv3 destekler.

### <a name="how-do-i-enable-root-squashing"></a>Kök bastırma nasıl etkinleştirebilirim?

Kök bastırma şu anda desteklenmiyor.

## <a name="smb-faqs"></a>SMB SSS

### <a name="does-azure-netapp-files-support-azure-active-directory"></a>Azure NetApp dosyaları, Azure Active Directory destekliyor mu?

Hayır, şu anda desteklenir.  NetApp dosya Azure Active Directory etki alanı (AD, Own Getir) mevcut Active Directory etki alanı denetleyicilerini Azure NetApp dosyaları ile kullanabileceğiniz Hizmetleri destekler. Etki alanı denetleyicileri Azure'da sanal makineler olarak veya ExpressRoute aracılığıyla şirket içi bulunabilir.

### <a name="is-an-active-directory-connection-required-for-smb-access"></a>SMB erişimi için gereken bir Active Directory bağlantı var mı? 

Evet, SMB birim dağıtmadan önce bir Active Directory bağlantısı oluşturmanız gerekir. Belirtilen etki alanı denetleyicileri tarafından temsil edilen alt ağ Azure NetApp dosya başarılı bir bağlantı için erişilebilir olmalıdır.  Bkz: [SMB birim oluşturma](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes#create-an-smb-volume) Ayrıntılar için. 

### <a name="how-many-active-directory-connections-are-supported"></a>Active Directory bağlantı sayısı destekleniyor mu?

Azure NetApp dosyaları, abonelik başına bir Active Directory bağlantısı şu anda destekler. Ayrıca, tek bir NetApp hesap için Active Directory bağlantı özeldir; hesapları arasında paylaşılmaz. 

### <a name="what-versions-of-windows-ad-are-supported"></a>Windows AD'ın hangi sürümleri destekleniyor?

NetApp dosyaları Azure Active Directory etki alanı Hizmetleri Windows Server 2016 2008r2SP1 sürümünü destekler.

## <a name="capacity-management-faqs"></a>Kapasite yönetimi SSS

### <a name="how-do-i-monitor-usage-for-capacity-pool-and-volume-of-azure-netapp-files"></a>Kullanım kapasitesi havuzu ve Azure NetApp dosyaları hacmi için nasıl izleyebilirim? 

Azure NetApp dosyaları kapasitesi havuzu ve birimin kullanım ölçümleri sağlar. Azure İzleyici, Azure için NetApp dosyaları kullanımını izlemek için de kullanabilirsiniz. Bkz: [Azure NetApp dosyaları için ölçümleri](azure-netapp-files-metrics.md) Ayrıntılar için. 

### <a name="can-i-manage-azure-netapp-files-through-azure-storage-explorer"></a>Azure Depolama Gezgini aracılığıyla Azure NetApp dosyaları yönetebilirim?

Hayır. Azure NetApp dosyaları Azure Depolama Gezgini tarafından desteklenmiyor.

## <a name="data-migration-and-protection-faqs"></a>Veri geçişi ve koruma hakkında SSS

### <a name="how-do-i-migrate-data-to-azure-netapp-files"></a>Azure için NetApp dosya verileri nasıl geçirebilirim?
Azure NetApp dosyaları, NFS ve SMB birimleri sağlar.  Veri hizmetine geçiş yapmayı herhangi bir dosya tabanlı kopyalama aracını kullanabilirsiniz. 

NetApp SaaS tabanlı bir çözüm sunar [NetApp bulut eşitleme](https://cloud.netapp.com/cloud-sync-service).  SMB verileri Azure NetApp dosya NFS dışarı aktarma veya SMB paylaşımları ya da çözüm NFS çoğaltmanıza olanak sağlar. 

Verileri kopyalamak için bir çeşit ücretsiz araçları da kullanabilirsiniz. NFS, iş yüklerini araçlarını gibi kullanabileceğiniz [rsync](https://rsync.samba.org/examples.html) kopyalayın ve bir Azure NetApp dosyaları birime veri kaynağını eşitlemek için. SMB için iş yüklerini kullanabileceğiniz [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) aynı şekilde.  Bu araçlar ayrıca dosya veya klasör izinleri çoğaltabilirsiniz. 

Veri geçiş için şirket içi Azure NetApp dosyaları gereksinimleri aşağıdaki gibidir: 

- Azure NetApp dosyaları hedef Azure bölgeniz kullanılabilir olduğundan emin olun.
- Kaynak ve Azure NetApp dosyaları hedef birim IP adresi arasındaki ağ bağlantısını doğrulayın. Şirket içi ve Azure NetApp dosyaları hizmeti arasında veri aktarımını ExpressRoute üzerinden desteklenir.
- Hedef Azure NetApp dosyaları birim oluşturun.
- Kaynak veriler, tercih edilen dosya kopyalama aracını kullanarak hedef birime aktarın.

### <a name="how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region"></a>Başka bir Azure bölgesinde Azure NetApp dosyaları birimin bir kopyasını nasıl oluşturabilirim?
    
Azure NetApp dosyaları, NFS ve SMB birimleri sağlar.  Herhangi bir dosya tabanlı kopyalama aracı, Azure bölgeleri arasında veri çoğaltmak için kullanılabilir. 

NetApp bir SaaS tabanlı çözüm sunar [NetApp bulut eşitleme](https://cloud.netapp.com/cloud-sync-service).  SMB verileri Azure NetApp dosya NFS dışarı aktarma veya SMB paylaşımları ya da çözüm NFS çoğaltmanıza olanak sağlar. 

Verileri kopyalamak için bir çeşit ücretsiz araçları da kullanabilirsiniz. NFS, iş yüklerini araçlarını gibi kullanabileceğiniz [rsync](https://rsync.samba.org/examples.html) kopyalayın ve bir Azure NetApp dosyaları birime veri kaynağını eşitlemek için. SMB için iş yüklerini kullanabileceğiniz [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) aynı şekilde.  Bu araçlar ayrıca dosya veya klasör izinleri çoğaltabilirsiniz. 

Bir Azure NetApp dosyaları birim başka bir Azure bölgesine çoğaltmak için gereksinimleri aşağıdaki gibidir: 
- Azure NetApp dosyaları hedef Azure bölgeniz kullanılabilir olduğundan emin olun.
- Her bölgedeki sanal ağlar arasındaki ağ bağlantısını doğrulayın. Şu anda, genel sanal ağ arasında eşleme desteklenmiyor.  İle ExpressRoute bağlantı hattına bağlama veya S2S VPN bağlantısı kullanarak sanal ağlar arasında bağlantı kurabilirsiniz. 
- Hedef Azure NetApp dosyaları birim oluşturun.
- Kaynak veriler, tercih edilen dosya kopyalama aracını kullanarak hedef birime aktarın.

### <a name="is-migration-with-azure-data-box-supported"></a>Desteklenen Azure Data Box ile geçiş mi?

Hayır. Azure Data Box Azure NetApp dosyaları şu anda desteklemiyor. 

### <a name="is-migration-with-azure-importexport-service-supported"></a>Azure içeri/dışarı aktarma hizmeti ile desteklenen geçiş mi?

Hayır. Azure içeri/dışarı aktarma hizmeti şu anda Azure NetApp dosyaları desteklemez.

## <a name="next-steps"></a>Sonraki adımlar  

- [Microsoft Azure ExpressRoute SSS](https://docs.microsoft.com/azure/expressroute/expressroute-faqs)
- [Microsoft Azure sanal ağ hakkında SSS](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq)
- [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)
- [Azure Data Box](https://docs.microsoft.com/azure/databox-family/)