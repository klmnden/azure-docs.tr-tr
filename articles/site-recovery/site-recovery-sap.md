---
title: Çok katmanlı SAP NetWeaver uygulama dağıtımı için Azure Site Recovery ile olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Bu makalede, Azure Site RECOVERY'yi kullanarak SAP NetWeaver uygulama dağıtımları için olağanüstü durum kurtarma ayarlamayı açıklar.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: 0848738b71a605d8baf049847daa3ae2428a7abe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65793682"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-sap-netweaver-app-deployment"></a>Çok katmanlı SAP NetWeaver uygulama dağıtımı için olağanüstü durum kurtarmayı ayarlama

En büyük boyut ve orta ölçekli SAP dağıtımları çeşit olağanüstü durum kurtarma çözümü kullanın. Güçlü ve test edilebilir olağanüstü durum kurtarma çözümlerinin önemi, SAP gibi uygulamalar için daha fazla çekirdek iş işlemleri geçildiği arttı. Azure Site Recovery, test edilmiş ve SAP uygulamalarıyla tümleştirilmiş. Site kurtarma özellikleri çoğu şirket içi olağanüstü durum kurtarma çözümlerinin ve daha düşük bir maliyetle toplam sahip olma maliyetinizi (TCO) rakip çözümlerine kıyasla aşıyor.

Site Recovery ile şunları yapabilirsiniz:
* **Şirket içi SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin** bileşenleri Azure'a çoğaltma tarafından.
* **Azure üzerinde çalışan SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin** bileşenleri başka bir Azure veri merkezine çoğaltma tarafından.
* **Buluta geçişi basitleştirin** kullanarak SAP dağıtımınızı Azure'a geçirmek için Site Recovery kullanarak.
* **SAP proje yükseltmelerini, testlerini ve prototip basitleştirmek** SAP uygulamaları test etmek için isteğe bağlı bir üretim oluşturarak kopyalayın.

Bu makalede kullanarak SAP NetWeaver uygulama dağıtımlarının korunmasına nasıl [Azure Site Recovery](site-recovery-overview.md). Makale, bir üç katmanlı Azure üzerinde SAP NetWeaver dağıtım Site Recovery kullanarak başka bir Azure veri merkezine çoğaltarak korumak için en iyi uygulamaları kapsar. Desteklenen senaryolar ve yapılandırmaları ve yük devretme testi (olağanüstü durum kurtarma tatbikatlarını) gerçekleştirmeyi açıklar ve gerçek bir yük devretme işlemleri.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdaki görevleri gerçekleştirmek nasıl bildiğinizden emin olun:

* [Bir sanal makineyi Azure'a çoğaltın](azure-to-azure-walkthrough-enable-replication.md)
* [Bir kurtarma ağı tasarlama](site-recovery-azure-to-azure-networking-guidance.md)
* [Azure'a yük devretme testi yapın](azure-to-azure-walkthrough-test-failover.md)
* [Azure'a yük devretme yapın](site-recovery-failover.md)
* [Bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
* [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Aşağıdaki senaryolarda bir olağanüstü durum kurtarma çözümü uygulamak için Site RECOVERY'yi kullanabilirsiniz:
* Başka bir Azure veri merkezine (Azure'dan Azure'a olağanüstü durum kurtarma) çoğaltma bir Azure veri merkezinde çalışan SAP sistemler. Daha fazla bilgi için [Azure'dan Azure'a çoğaltma mimarisi](https://aka.ms/asr-a2a-architecture).
* VMware (veya fiziksel) çalışan SAP sistemlerini sunucuları şirket içi olağanüstü durum kurtarma sitesine Azure veri merkezindeki (Vmware'den Azure'a olağanüstü durum kurtarma) Bu çoğaltma. Bu senaryo, bazı ek bileşenler gerektirir. Daha fazla bilgi için [Vmware'den Azure'a çoğaltma mimarisi](https://aka.ms/asr-v2a-architecture).
* Hyper-V (Hyper-V Azure'a olağanüstü durum kurtarma) Azure veri merkezindeki bir olağanüstü durum kurtarma siteye çoğaltılan şirket içinde çalışan SAP sistemlerini. Bu senaryo, bazı ek bileşenler gerektirir. Daha fazla bilgi için [Hyper-V Azure'a çoğaltma mimarisi](https://aka.ms/asr-h2a-architecture).

Bu makalede kullandığımız bir **Azure'dan Azure'a** Site Kurtarma'nın SAP olağanüstü durum kurtarma özellikleri göstermek için olağanüstü durum kurtarma senaryosuna. Site Recovery çoğaltma uygulamaya özgü olmadığından, açıklanan işlemi diğer senaryolar için de geçerli beklenir.

### <a name="required-foundation-services"></a>Gerekli foundation Hizmetleri
Bu makalede ele senaryoda aşağıdaki foundation Hizmetleri dağıtılır:
* Azure ExpressRoute ya da Azure VPN ağ geçidi
* En az bir Active Directory etki alanı denetleyicisi ve DNS sunucusu, Azure'da çalışan

Site Recovery dağıtmadan önce bu altyapı oluşturmanızı öneririz.

## <a name="reference-sap-application-deployment"></a>Başvuru SAP uygulama dağıtımı

Bu başvuru mimarisi, SAP NetWeaver Windows ortamında yüksek kullanılabilirlik ile Azure üzerinde çalışan gösterir.  Bu mimari, kuruluşunuzun gereksinimlerini karşılayacak şekilde belirli bir sanal makine (VM) boyutları ile dağıtılır.

![Tipik bir SAP dağıtım modeli diyagramı](./media/site-recovery-sap/sap-netweaver_latest.png)

## <a name="disaster-recovery-considerations"></a>Olağanüstü durum kurtarma değerlendirmeleri

Olağanüstü Durum Kurtarma (DR), ikincil bir bölgeye yük devretme mümkün olması gerekir. Olağanüstü durum kurtarma (DR) koruması sağlamak için her katman farklı bir strateji kullanır.

#### <a name="vms-running-sap-web-dispatcher-pool"></a>SAP Web Dispatcher havuzu çalıştıran VM'ler 
Web Dispatcher bileşen SAP trafiği SAP uygulama sunucuları arasında bir yük dengeleyici olarak kullanılır. Web Dispatcher bileşeni için yüksek kullanılabilirlik elde etmek için Azure Load Balancer dengeleyici havuzunda kullanılabilir Web dağıtıcıları arasında HTTP (S) trafiğinin dağıtım için hepsini bir kez deneme yapılandırmasında paralel Web dağıtıcısı Kurulum uygulamak için kullanılır. Bu Azure Site kasadır kullanarak çoğaltılır ve yük dengeleyici üzerinde olağanüstü durum kurtarma bölgesindeki yapılandırmak için Otomasyon betikleri kullanılır. 

#### <a name="vms-running-application-servers-pool"></a>Uygulama sunucuları havuzu çalıştıran VM'ler
SMLG işlem ABAP uygulama sunucuları için oturum açma grupları yönetmek için kullanılır. Yük Dengeleme ileti sunucusu merkezi Hizmetleri işlevindeki SAPGUIs ve RFC için SAP uygulama sunucuları havuzu arasındaki iş yükünü dağıtmak için kullandığı trafiği. Bu Azure Site Recovery kullanılarak çoğaltılır 

#### <a name="vms-running-sap-central-services-cluster"></a>SAP Central Services'in kümesi çalıştıran VM'ler
Bu başvuru mimarisi, uygulama katmanında Vm'lerde merkezi hizmetleri çalıştırır. Merkezi Hizmetleri için tek bir VM dağıtılırken hata (SPOF) bir olası tek noktası olan — yüksek kullanılabilirlik gereksinimi olmadığı durumlarda tipik dağıtım.<br>

Bir yüksek kullanılabilirlik çözümü uygulamak için bir paylaşılan disk kümesi veya dosya paylaşımı kümesi kullanılabilir. Paylaşılan disk kümesi için Vm'leri yapılandırmak için Windows Server Yük devretme kümesi kullanın. Bulut Tanığını bir çekirdek tanığı olarak önerilir. 
 > [!NOTE]
 > Azure Site Recovery bulut tanığı yinelenmez bu nedenle olağanüstü durum kurtarma bölgesindeki bulut tanığı dağıtma için önerilir.

Yük devretme küme ortamında desteklemek için [SIOS DataKeeper Cluster Edition](https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8) küme düğümlerinin sahip olduğu bağımsız diskleri çoğaltarak Küme Paylaşılan Birimi işlevi gerçekleştirir. Azure Paylaşılan diskleri yerel olarak desteklemez ve bu nedenle SIOS tarafından sağlanan çözümleri gerektirir. 

Kümeleme işlemek için başka bir yol, bir dosya paylaşımı kümesi uygulamaktır. [SAP](https://blogs.sap.com/2018/03/19/migration-from-a-shared-disk-cluster-to-a-file-share-cluster) /sapmnt genel dizinleri UNC yolu üzerinden erişmek için Yönetim Hizmetleri dağıtım modeli yakın zamanda değiştirilmiş. Bununla birlikte, yine de /sapmnt UNC paylaşımı yüksek oranda kullanılabilir olmasını sağlamak için önerilir. Bu merkezi Hizmetleri örneğinde ölçek kullanıma arama dosya sunucusu (SOFS) ve Windows Server 2016 depolama alanları doğrudan (S2D) özelliği ile Windows Server Yük devretme kümesi kullanılarak yapılabilir. 
 > [!NOTE]
 > Şu anda Azure Site Recovery destek yalnızca kilitlenme tutarlı noktası çoğaltma SIOS Datakeeper, depolama alanları doğrudan ve Pasif düğümü kullanan sanal makinelerin


## <a name="disaster-recovery-considerations"></a>Olağanüstü durum kurtarmayla konusunda dikkat edilmesi gerekenler

Azure Site Recovery, yük boyunca tüm SAP dağıtımının Azure bölgeleri arasında düzenlemek için kullanabilirsiniz.
Olağanüstü durum kurtarma ayarlama adımları aşağıda verilmiştir 

1. Çoğaltma sanal makineler 
2. Bir kurtarma ağı tasarlama
3.  Bir etki alanı denetleyicisi çoğaltma
4.  Temel katman verileri çoğaltma 
5.  Yük devretme testi gerçekleştirin 
6.  Yük devretme gerçekleştirin 

Aşağıda bu örnekte kullanılan her bir katman olağanüstü durum kurtarma için önerilir. 

 **SAP katmanları** | **Öneri**
 --- | ---
**SAP Web Dispatcher havuzu** |  Site recovery kullanarak çoğaltma 
**SAP uygulama sunucusu havuzu** |  Site recovery kullanarak çoğaltma 
**SAP Central Services'in küme** |  Site recovery kullanarak çoğaltma 
**Active directory sanal makineler** |  Active directory çoğaltma 
**SQL veritabanı sunucuları** |  SQL çoğaltma her zaman açık

## <a name="replicate-virtual-machines"></a>Çoğaltma sanal makineler

Tüm SAP uygulama sanal makineleri Azure'a olağanüstü durum kurtarma veri merkezine çoğaltma başlatmak için sunulan yönergeleri [bir sanal makine, Azure'a](azure-to-azure-walkthrough-enable-replication.md).


* Active Directory ve DNS'yi koruma ile ilgili yönergeler için bkz [korumak Active Directory ve DNS](site-recovery-active-directory.md) belge.

* Veritabanı katmanı SQL sunucusunda çalışan koruma ile ilgili yönergeler için bkz [SQL Server koruma](site-recovery-active-directory.md) belge.

## <a name="networking-configuration"></a>Ağ yapılandırması

Statik bir IP adresi kullanırsanız, sanal makinenin almak için istediğiniz IP adresi belirtebilirsiniz. IP adresi ayarlamak için şuraya gidin: **işlem ve ağ ayarları** > **ağ arabirim kartı**.

![Site Recovery ağ arabirimi kartı Bölmesi'nde özel bir IP adresi ayarlamak gösteren ekran görüntüsü](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma
Bir kurtarma planı yük devretme sırasında çok katmanlı bir uygulama çeşitli katmanları sıralama destekler. Sıralama, özel olarak uygulama tutarlılığı sürdürmeye yardımcı olur. Çok katmanlı web uygulaması için bir kurtarma planı oluşturduğunuzda, tam adımlar açıklanan [Site Recovery kullanarak bir kurtarma planı oluşturma](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler için yük devretme grupları ekleme

1.  Uygulama sunucusu, web dispatcher ve SAP Central Services'in Vm'leri ekleyerek bir kurtarma planı oluşturun.
2.  'Sanal makineleri gruplandırmak için Özelleştir' tıklayın. Varsayılan olarak, tüm VM'ler 'Grubu 1' bir parçasıdır.



### <a name="add-scripts-to-the-recovery-plan"></a>Betikleri kurtarma planına ekleyin
Uygulamalarınızın düzgün çalışması, yük devretme işleminden sonra veya bir yük devretme testi sırasında Azure sanal makinelerinde bazı işlemler yapmanız gerekebilir. Bazı yük devretme sonrası işlemleri otomatik hale getirebilirsiniz. Örneğin, DNS girişini güncelleştirmek ve kurtarma planına betikleri ekleyerek bağlamaları ve bağlantıları değiştirin.


En yaygın olarak kullanılan Azure Site Recovery betikleri için 'Azure'a Dağıt' düğmesini Otomasyon hesabınıza dağıtabilirsiniz. Yayımlanan herhangi bir komut dosyası kullanırken, komut dosyasındaki yönergeleri emin olun.

[![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Bir ön eylem betiği 'Grubu 1' için yük devretme SQL kullanılabilirlik grubuna ekleyin. Örnek betiklerde yayımlanan 'ASR-SQL-FailoverAG' betiğini kullanın. Komut dosyasındaki yönergeleri izleyin ve komut dosyasında gerekli değişiklikleri yapın, uygun şekilde emin olun.
2. Yük dengeleyicide başarısız eklemek için son eylem betik ekleme yükü devredilen sanal makineler, Web Katmanı (Grup 1). Örnek betiklerde yayımlanan 'AddSingleLoadBalancer ASR' betiğini kullanın. Komut dosyasındaki yönergeleri izleyin ve komut dosyasında gerekli değişiklikleri yapın, uygun şekilde emin olun.

![SAP kurtarma planı](./media/site-recovery-sap/sap_recovery_plan.png)


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1.  Azure portalında kurtarma Hizmetleri kasanızı seçin.
2.  SAP uygulamaları için oluşturduğunuz kurtarma planı seçin.
3.  **Yük Devretme Testi**'ni seçin.
4.  Test yük devretme işlemini başlatmak için kurtarma noktası ve Azure sanal ağı'nı seçin.
5.  İkincil bir ortamı yukarı olduğunda doğrulamaları gerçekleştirin.
6.  Yük devretme ortamı temizlemek için doğrulamaları tamamlandığı zaman seçin **yük devretme testini Temizle**.

Daha fazla bilgi için [Azure Site recovery'de yük devretme testi](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1.  Azure portalında kurtarma Hizmetleri kasanızı seçin.
2.  SAP uygulamaları için oluşturduğunuz kurtarma planı seçin.
3.  **Yük devretme**'yi seçin.
4.  Yük devretme işlemini başlatmak için kurtarma noktasını seçin.

Daha fazla bilgi için [Site recovery'de yük devretme](site-recovery-failover.md).

## <a name="next-steps"></a>Sonraki adımlar
* Site RECOVERY'yi kullanarak SAP NetWeaver dağıtımlar için bir olağanüstü durum kurtarma çözümü oluşturma hakkında daha fazla bilgi edinmek için indirilebilir teknik incelemesine bakın [SAP NetWeaver: Azure Site Recovery ile bir olağanüstü durum kurtarma çözümü oluşturma](https://aka.ms/asr_sap). Beyaz Kağıt, çeşitli SAP mimarileri için öneriler ele alınmaktadır, Azure üzerinde SAP için desteklenen uygulamalar ve VM türleri listeler ve olağanüstü durum kurtarma çözümünüzü test planı seçenekleri açıklar.
* Daha fazla bilgi edinin [diğer iş yükleri çoğaltma](site-recovery-workload.md) Site Recovery kullanarak.
