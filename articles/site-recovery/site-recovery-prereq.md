---
title: "Azure Site Recovery kullanarak çoğaltma Azure için Önkoşullar | Microsoft Docs"
description: "Azure Site Recovery hizmetini kullanarak Azure'a sanal makineleri ve fiziksel makineleri çoğaltma önkoşulları hakkında bilgi edinin."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: raynew
ms.openlocfilehash: cb7c3892dc0acbffdec636e5e7b3ce063660153c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
#  <a name="prerequisites-for-replication-from-on-premises-to-azure-by-using-site-recovery"></a>Site Recovery kullanarak şirket içi azure'a çoğaltma için Önkoşullar

> [!div class="op_single_selector"]
> * [Azure'dan azure'a](site-recovery-azure-to-azure-prereq.md)
> * [Şirket içinden azure'a](site-recovery-prereq.md)

Azure Site Recovery, bir Azure sanal makinesini (VM) çoğaltılmasını düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize destekleyen başka bir Azure bölgesine yardımcı olabilir. Site kurtarma da şirket içi fiziksel sunucuların ve sanal makineleri buluta (Azure'a) veya ikincil veri merkezine çoğaltır. Kesinti birincil konumunuzda meydana gelirse, üzerinden uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil bir konuma başarısız olabilir. Normal çalışma birincil konuma geri döndüğünde birincil konumunuz başarısız olabilir. Site Recovery hakkında daha fazla bilgi için bkz: [Site Recovery nedir?](site-recovery-overview.md).

Bu makalede, Azure Site Recovery çoğaltma bir şirket içi makineden başlayan önkoşulları özetler.

Açıklamaları makalenin sonundaki nakledebilirsiniz. Ayrıca, teknik sorular sorabilirsiniz [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="azure-requirements"></a>Azure gereksinimleri

**Gereksinim** | **Ayrıntılar**
--- | ---
**Azure hesabı** | A [Microsoft Azure hesabı](http://azure.microsoft.com/). [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
**Site Recovery hizmeti** | Azure Site Recovery hizmeti için fiyatlandırma hakkında daha fazla bilgi için bkz: [Site Recovery fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Depolama** | Çoğaltılan verileri depolamak için bir Azure Storage hesabı gerekir. Depolama hesabının, Azure Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Yük devretme durumunda azure Vm'leri oluşturulur.<br/><br/> Azure VM yük devretme için kullanmak istediğiniz kaynak modeline bağlı olarak, bir hesap kullanarak ayarlayabilirsiniz [Azure Resource Manager dağıtım modeli](../storage/common/storage-create-storage-account.md) veya [Klasik dağıtım modeli](../storage/common/storage-create-storage-account.md).<br/><br/>[Coğrafi olarak yedekli depolama](../storage/common/storage-redundancy.md#geo-redundant-storage) veya yerel olarak yedekli depolama kullanabilirsiniz. Coğrafi olarak yedekli depolama kullanmanızı öneririz. Coğrafi olarak yedekli depolama ile, bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda veriler korunur.<br/><br/> Bir standart Azure depolama hesabı kullanabilir veya Azure Premium Storage kullanabilirsiniz. [Premium depolama](https://docs.microsoft.com/azure/storage/storage-premium-storage) genellikle sürekli olarak yüksek g/ç performans ve düşük gecikme gerektiren VM'ler için kullanılır. Premium depolama ile bir VM g/Ç kullanımı yoğun iş yükleri barındırabilir. Çoğaltılan veriler için premium depolama kullanırsanız, ayrı bir standart depolama hesabı gerekir. Standart depolama hesabı şirket içi verilerde gerçekleşen değişiklikleri yakalamak çoğaltma günlüklerinde depolar.<br/><br/>
**Depolama sınırlamaları** | Farklı bir kaynak grubu, Site RECOVERY'yi kullanın veya taşımak veya başka bir aboneliğe kullanan depolama hesapları taşınamıyor.<br/><br/> Şu anda, Orta Hindistan ve Güney Hindistan premium depolama hesaplarına çoğaltma kullanılamıyor.
**Azure ağı** | İçin yük devretme sonrasında Azure Vm'lerinin bağlanmak bir Azure ağı gerekir. Azure ağının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.<br/><br/> Azure portalında bir Azure ağı kullanarak oluşturabileceğiniz [Resource Manager dağıtım modeli](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) veya [Klasik dağıtım modeli](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> System Center Virtual Machine Manager (VMM) Azure'a çoğaltma, VMM VM ağları ve Azure ağları arasında ağ eşlemesi ayarlamanız gerekir. Bu, Azure Vm'lerinin yük devretme sonrasında uygun ağlara bağlanmak sağlar.
**Ağ sınırlamaları** | Farklı bir kaynak grubu, Site RECOVERY'yi kullanın veya taşımak veya başka bir aboneliğe kullanan ağ hesapları taşınamıyor.
**Ağ eşlemesi** | VMM bulutlarındaki Microsoft Hyper-V Vm'lerini çoğaltma, ağ eşlemesi ayarlamanız gerekir. Bu, yük devretme sonrasında oluşturulduğunda Azure sanal makinelerinin uygun ağlara bağlanmak sağlar.

> [!NOTE]
> Aşağıdaki bölümlerde müşteri ortamının çeşitli bileşenleri için Önkoşullar açıklanmaktadır. Belirli yapılandırmalar için desteği hakkında daha fazla bilgi için bkz: [destek matrisi](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-to-azure"></a>VMware Vm'leri veya Windows veya Linux fiziksel sunucuları azure'a olağanüstü durum kurtarma
VMware Vm'lerini veya fiziksel Windows veya Linux sunucularının olağanüstü durum kurtarma için aşağıdaki bileşenler gereklidir. Açıklanan olanları yanı sıra bunlar [Azure gereksinimleri](#azure-requirements).


### <a name="configuration-server-or-additional-process-server"></a>Yapılandırma sunucusu veya ek işlem sunucusu

Bir şirket içi makineyi yapılandırma sunucusu olarak şirket içi site ile Azure arasındaki iletişimi düzenlemek için ayarlayın. Bir yapılandırma sunucusu olarak yapılandırılmış bir sanal makinenin sistem ve yazılım gereksinimleri hakkında tablo konuşmaları aşağıda.

> [!IMPORTANT]
> VMware sanal makineleri korumak için bir yapılandırma sunucusu dağıtımı sırasında olarak dağıtmanızı öneririz bir **yüksek oranda kullanılabilir (HA)** sanal makine.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

### <a name="vmware-vcenter-or-vsphere-host-prerequisites"></a>VMware vCenter veya vSphere ana bilgisayar önkoşulları

| **Bileşen** | **Gereksinimleri** |
| --- | --- |
| **vSphere** | Bir veya daha fazla VMware vSphere hiper olması gerekir.<br/><br/>Hiper en son güncelleştirmeleri vSphere sürüm 6.0, 5.5 veya 5.1 çalıştırması gerekir.<br/><br/>Bu vSphere öneririz ana bilgisayarları ve vCenter sunucuları işlem sunucusu olarak aynı ağdaki her ikisi de olmalıdır. Adanmış işlem sunucusu ayarlamadıysanız ayarladınız sürece, bu yapılandırma sunucusunun bulunduğu ağdır. |
| **vCenter** | VSphere ana bilgisayarları yönetmek için bir VMware vCenter sunucusu dağıtmanızı öneririz. VCenter sürüm 6.0 veya 5.5, en son güncelleştirmeleri ile çalışmalıdır.<br/><br/>**Sınırlama**: Site Recovery vCenter VMotion'ı örnekleri arasında çoğaltmayı desteklemez. Ayrıca bir ana hedef VM, depolama DRS ve depolama VMotion'ı, sonra yeniden koruma işlemi desteklenmez.|

### <a name="replicated-machine-prerequisites"></a>Çoğaltılmış makine önkoşulları

| **Bileşen** | **Gereksinimleri** |
| --- | --- |
| **Şirket içi makineler** (VMware VM) | Çoğaltılan VM'ler VMware araçları yüklü ve çalışıyor olması gerekir.<br/><br/> Sanal makineleri karşılamalıdır [Azure önkoşulları](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) bir Azure VM oluşturmak için.<br/><br/>Disk kapasitesi, korunan her makinede 1,023 GB'den fazla olamaz. <br/><br/>Bir en az 2 GB kullanılabilir alan yükleme sürücüsünde bileşen yüklemesi için gereklidir.<br/><br/>Çoklu VM tutarlılığını etkinleştirmek istiyorsanız, bağlantı noktası 20004 VM yerel güvenlik duvarını açık olması gerekir.<br/><br/>Makine adlarının (harf, rakam ve kısa çizgi kullanabilirsiniz) 1 ile 63 karakter uzunluğunda olması gerekir. Ad bir harf veya sayı ile başlamalı ve bir harf veya sayı ile bitmelidir. <br/><br/>Makine için çoğaltmayı etkinleştirdikten sonra Azure ad değiştirebilirsiniz.<br/><br/> |
| **Windows makine** (fiziksel veya VMware) | Makine aşağıdaki desteklenen 64-bit işletim sistemlerinden birini çalıştırmalıdır: <br/>-Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi olan sunucu)<br/>-Windows Server 2012 R2<br/>-Windows Server 2012<br/>-Windows Server 2008 R2 SP1 veya sonraki bir sürümü<br/><br/> İşletim sistem c sürücüsünde bulunan yüklenmelidir İşletim sistemi diskinin bir Windows temel disk olması gerekir ve dinamik değil. Veri diski dinamik olabilir.<br/><br/>|
| **Linux makineler** (fiziksel veya VMware) | Makine aşağıdaki desteklenen 64-bit işletim sistemlerinden birini çalıştırmalıdır: <br/>-Red Hat Enterprise Linux 5.2 için 5.11 6.1 için 6.9, 7.0 için 7.3<br/>-CentOS 5.2 için 5.11 6.1 için 6.9, 7.0 için 7.3<br/>-Ubuntu 14.04 LTS sunucu (Ubuntu için desteklenen çekirdek sürümlerinin listesi için bkz: [desteklenen işletim sistemleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Ubuntu 16.04 LTS sunucu (Ubuntu için desteklenen çekirdek sürümlerinin listesi için bkz: [desteklenen işletim sistemleri](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Debian 7 veya Debian 8<br/>-Red Hat uyumlu çekirdek ya da kesilemeyen kurumsal çekirdek sürüm 3 (UEK3) çalıştıran oracle Enterprise Linux 6.5 veya 6.4,<br/>-SUSE Linux Enterprise Server 11 SP4 veya SUSE Linux Enterprise Server 11 SP3<br/><br/>/ Etc/hosts dosyalarınızı korumalı makinelerdeki tüm ağ bağdaştırıcıları ile ilişkili IP adreslerini yerel ana bilgisayar adı Eşle girişleri olması gerekir.<br/><br/>Bir güvenli Kabuk (SSH) istemcisi kullanarak Linux çalıştıran bir Azure VM bağlanmak istiyorsanız, yük devretme sonrasında korunan makinedeki SSH hizmeti sistem başlangıç otomatik olarak başlayacak şekilde ayarlandığından emin olun. Ayrıca güvenlik duvarı kuralları bir SSH bağlantısı korunan makineye izin verdiğinden emin olun.<br/><br/>Ana bilgisayar adı, bağlama noktaları, aygıt adları ve Linux sistem yolu ve dosya adları (örneğin, /etc/ ve/usr) İngilizce yalnızca olmalıdır.<br/><br/>Aşağıdaki dizinleri (varsa ayrı bölüm olarak ayarlayın veya dosya sistemleri) tüm kaynak sunucudaki aynı diskte (işletim sistemi diski) olması gerekir:<br/>- / (root)<br/>- / önyükleme<br/>-/ usr<br/>-/ usr/yerel<br/>-/var<br/>- / vb.<br/><br/>Şu anda, meta veri sağlama gibi XFS v5 özellikleri XFS dosya sistemlerinde Site Recovery tarafından desteklenmez. XFS dosya sistemleri v5 özellikleri kullanmadığınız emin olun. Süper blok XFS kullanarak bölümün denetlemek için xfs_info yardımcı programını kullanabilirsiniz. Varsa **ftype** ayarlanır **1**, XFS v5 özellikleri kullanılmaktadır.<br/><br/>Red Hat Enterprise Linux 7 ve CentOS 7 sunucularda lsof yardımcı programı yüklü ve kullanılabilir olmalıdır.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-to-azure-no-vmm"></a>Hyper-V sanal makineleri olağanüstü durum kurtarma Azure (VMM yok)

VMM bulutlarındaki Hyper-V sanal makineleri olağanüstü durum kurtarması için aşağıdaki bileşenler gereklidir. Açıklanan olanları yanı sıra bunlar [Azure gereksinimleri](#azure-requirements).

| **Önkoşul** | **Ayrıntılar** |
| --- | --- |
| **Hyper-V konağı** |Bir veya daha fazla şirket içi sunucular ve Hyper-V rolü etkin en son güncelleştirmeleri ile Windows Server 2012 R2 veya Microsoft Hyper-V Server 2012 R2 çalıştırması gerekir.<br/><br/>Hyper-V sunucuları, bir veya daha fazla VM olması gerekir.<br/><br/>Hyper-V sunucuları Internet'e doğrudan veya bir proxy üzerinden bağlı olmalıdır.<br/><br/>Hyper-V sunucuları, Bilgi Bankası makalesinde açıklanan düzeltmeler olmalıdır [2961977](https://support.microsoft.com/kb/2961977) yüklü.
|**Sağlayıcı ve aracı**| Site Recovery dağıtımı sırasında Azure Site Recovery Sağlayıcısı'nı yükleyin. Sağlayıcı yükleme de korumak istediğiniz sanal makineleri çalıştıran her Hyper-V sunucusuna Azure kurtarma Hizmetleri Aracısı'nı yükler. <br/><br/>Bir Site kurtarma kasasına tüm Hyper-V sunucularında aynı sağlayıcı ve Aracı sürümleri olması gerekir.<br/><br/>Sağlayıcı, Internet üzerinden Site Recovery hizmetine bağlanmak gerekir. Trafik, doğrudan veya bir proxy üzerinden gönderilebilir. HTTPS tabanlı proxy desteklenmiyor. Proxy sunucunun aşağıdaki URL'lere erişim izin vermesi gerekir:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Sunucuda IP adresi tabanlı güvenlik duvarı kuralları varsa, kuralların Azure ile iletişim kurmaya izin verdiğinden emin olun.<br/><br/> İzin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)hem de HTTPS (443 numaralı bağlantı noktası).<br/><br/> IP adres aralıklarını aboneliğinizin Azure bölgesi ve Batı ABD (erişim denetimi ve kimlik yönetimi için kullanılan) izin verir.


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure"></a>VMM bulutlarındaki Hyper-V Vm'lerini azure'a olağanüstü durum kurtarma

VMM bulutlarındaki Hyper-V sanal makineleri olağanüstü durum kurtarması için aşağıdaki bileşenler gereklidir. Açıklanan olanları yanı sıra bunlar [Azure gereksinimleri](#azure-requirements).

| **Önkoşul** | **Ayrıntılar** |
| --- | --- |
| **Sanal Makine Yöneticisi** |System Center 2012 R2 veya sonraki bir sürümünü çalıştıran bir veya daha fazla VMM sunucusu olmalıdır. Her bir VMM sunucusunun bir veya daha fazla bulut yapılandırılmış olması gerekir. <br/><br/>Bir buluta sahip olmanız gerekir:<br/>-Bir veya daha fazla VMM ana bilgisayar grubu.<br/>-Bir veya daha fazla Hyper-V ana bilgisayar sunucuları veya kümeleri her konak grubundaki.<br/><br/>VMM bulutlarını ayarlama hakkında daha fazla bilgi için bkz: [Sanal Makine Yöneticisi 2012'de bir bulut oluşturmak nasıl](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |Hyper-V konağı sunucularının, en az Microsoft Hyper-V Server 2012 R2 veya Hyper-V rolü etkin Windows Server 2012 R2 çalıştırması gerekir. En son güncelleştirmeler yüklü olmalıdır.<br/><br/> Bir Hyper-V sunucusu bir veya daha fazla VM olması gerekir.<br/><br/> Bir Hyper-V konak sunucusu veya çoğaltmak istediğiniz Vm'leri içeren küme bir VMM bulutunda yönetilmesi gerekir.<br/><br/>Hyper-V sunucuları Internet'e doğrudan veya bir proxy üzerinden bağlı olmalıdır.<br/><br/>Hyper-V sunucuları, Bilgi Bankası makalesinde açıklanan düzeltmeler olmalıdır [2961977](https://support.microsoft.com/kb/2961977) yüklü.<br/><br/>Hyper-V ana bilgisayar sunucularının azure'a veri çoğaltma için Internet erişimi gerekir. |
| **Sağlayıcı ve aracı** |Azure Site Recovery dağıtımı sırasında VMM sunucusuna Azure Site Recovery sağlayıcısı yükleyin. Kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarlarına yükleyin. Sağlayıcı ve aracının doğrudan Internet üzerinden veya bir proxy üzerinden Azure'a bağlanmanız gerekir. HTTPS tabanlı ara sunucular desteklenmez. Hyper-V konakları ve VMM sunucusundaki proxy sunucusunda erişmesine izin vermek gerekir: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>VMM sunucusunda IP adresi tabanlı güvenlik duvarı kuralları varsa, kuralların Azure ile iletişim kurmaya izin verdiğinden emin olun.<br/><br/> İzin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653) ve HTTPS (443 numaralı bağlantı noktası).<br/><br/>IP adresi aralıkları için Azure bölgesi ve Batı ABD (erişim denetimi ve kimlik yönetimi için kullanılan) aboneliğiniz için izin verir.<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Çoğaltılmış makine önkoşulları

| **Bileşen** | **Ayrıntılar** |
| --- | --- |
| **Korumalı VM'ler** | Site Recovery tarafından desteklenen tüm işletim sistemlerini destekliyor [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Sanal makineleri karşılamalıdır [Azure önkoşulları](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) bir Azure VM oluşturmak için. Makine adlarının (harf, rakam ve kısa çizgi kullanabilirsiniz) 1 ile 63 karakter uzunluğunda olması gerekir. Ad bir harf veya sayı ile başlamalı ve bir harf veya sayı ile bitmelidir. <br/><br/>Sanal makine için çoğaltmayı etkinleştirdikten sonra VM adını değiştirebilirsiniz. <br/><br/> Korunan her makine için disk kapasiteleri 1,023 GB'den fazla olamaz. Bir VM en fazla 16 disk barındırabilir (16 TB'ye kadar).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-a-customer-owned-site"></a>VMM bulutlarındaki Hyper-V sanal makineleri olağanüstü durum kurtarma müşteriye ait bir siteye

VMM bulutlarındaki Hyper-V sanal makineleri olağanüstü durum kurtarma müşteriye ait bir site için aşağıdaki bileşenler gereklidir. Açıklanan olanları yanı sıra bunlar [Azure gereksinimleri](#azure-requirements).

| **Bileşen** | **Ayrıntılar** |
| --- | --- |
| **Sanal Makine Yöneticisi** |  Birincil site ile ikincil sitenin VMM sunucusunda dağıtmanızı öneririz.<br/><br/> Tek bir VMM sunucusundaki Bulutlar arasında çoğaltmak için VMM sunucusunda yapılandırılmış en az iki bulut gerekir.<br/><br/> VMM sunucusu en az çalıştırmalıdır son güncelleştirmeleri içeren System Center 2012 SP1.<br/><br/> Her bir VMM sunucusunun bir veya daha fazla bulut olması gerekir. Tüm bulutlar Hyper-V Kapasite profili kümesi olması gerekir. <br/><br/>Bulut bir veya daha fazla VMM konak grubu olması gerekir. VMM bulutlarını ayarlama hakkında daha fazla bilgi için bkz: [hazırlamak için Azure Site Recovery dağıtımı](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Hyper-V sunucuları çalışıyor olmalıdır en az Windows Server 2012 Hyper-V rolü etkin ve en son güncelleştirmelerin yüklü olması.<br/><br/> Bir Hyper-V sunucusu bir veya daha fazla VM olması gerekir.<br/><br/>  Hyper-V ana bilgisayar sunucularının birincil ve ikincil VMM bulutlarında konak gruplarındaki bulunması gerekir.<br/><br/> Windows Server 2012 R2'de bir kümede Hyper-V çalıştırırsanız, Bilgi Bankası makalesinde açıklanan güncelleştirmeyi yüklemeniz önerilir [2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Windows Server 2012'de bir kümede Hyper-V çalıştıran ve bir statik IP adresi tabanlı küme varsa, küme Aracısı otomatik olarak oluşturulmaz. Küme Aracısı el ile yapılandırmanız gerekir. Küme Aracısı hakkında daha fazla bilgi için bkz: [kümeden kümeye çoğaltma için çoğaltma aracısı yapılandırın](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Sağlayıcı** | Site Recovery dağıtımı sırasında VMM sunucularına Azure Site Recovery Sağlayıcısı'nı yükleyin. Sağlayıcı, çoğaltmayı düzenlemek için HTTPS üzerinden (443 numaralı bağlantı noktası) Site Recovery ile iletişim kurar. LAN üzerinden veya bir VPN bağlantısı üzerinden birincil ve ikincil Hyper-V sunucuları arasında veri çoğaltma oluşur.<br/><br/> VMM sunucusunda çalışan sağlayıcı aşağıdaki URL'lere erişim gerekir:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Site kurtarma sağlayıcısı VMM sunucularından güvenlik duvarı iletişimine izin vermelidir [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (bağlantı noktası 443) protokolüne izin verin. |


## <a name="url-access"></a>URL erişimi
Bu URL'leri VMware, VMM ve Hyper-V ana bilgisayar sunucuları kullanılabilir olmalıdır:

|**URL** | **VMM VMM** | **VMM'den Azure'a** | **Hyper-V'den Azure'a** | **Vmware’den Azure’a** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | İzin Ver | İzin Ver | İzin Ver | İzin Ver |
|``*.backup.windowsazure.com`` | Gerekli değil | İzin Ver | İzin Ver | İzin Ver |
|``*.hypervrecoverymanager.windowsazure.com`` | İzin Ver | İzin Ver | İzin Ver | İzin Ver |
|``*.store.core.windows.net`` | İzin Ver | İzin Ver | İzin Ver | İzin Ver |
|``*.blob.core.windows.net`` | Gerekli değil | İzin Ver | İzin Ver | İzin Ver |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Gerekli değil | Gerekli değil | Gerekli değil | SQL indirme için izin ver |
|``time.windows.com`` | İzin Ver | İzin Ver | İzin Ver | İzin Ver|
|``time.nist.gov`` | İzin Ver | İzin Ver | İzin Ver | İzin Ver |
