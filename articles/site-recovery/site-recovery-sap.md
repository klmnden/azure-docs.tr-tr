---
title: Azure Site Recovery kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımı koruma | Microsoft Docs
description: Bu makalede, Azure Site RECOVERY'yi kullanarak SAP NetWeaver uygulama dağıtımlarını korumaya açıklar.
services: site-recovery
documentationcenter: ''
author: asgang
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2018
ms.author: asgang
ms.openlocfilehash: 27dfdec4e833a2f30963157ba2f4d95232e21270
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267341"
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-by-using-site-recovery"></a>Site Recovery kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımı koruyun

En büyük boyut ve orta ölçekli SAP dağıtımları olağanüstü durum kurtarma çözümü çeşit kullanın. SAP gibi uygulamalar için daha fazla çekirdek iş süreçlerini taşınırken sağlam ve sınanabilir olağanüstü durum kurtarma çözümleri önemini artar. Azure Site Recovery, test ve SAP uygulamaları ile tümleşiktir. Site kurtarma özellikleri çoğu şirket içi olağanüstü durum kurtarma çözümleri ve daha düşük maliyetle toplam sahip olma maliyetini (TCO) rakip çözümleri daha aşıyor.

Site Recovery ile şunları yapabilirsiniz:
* **Şirket içi SAP NetWeaver ve NetWeaver olmayan üretim uygulamaları korumayı etkinleştir** bileşenleri Azure'a çoğaltma tarafından.
* **Azure üzerinde çalışan SAP NetWeaver ve NetWeaver olmayan üretim uygulamaları korumayı** bileşenlerinin başka bir Azure veri merkezine çoğaltma tarafından.
* **Bulut geçiş işlemini basitleştirebilir** SAP dağıtımınızı Azure'a geçirmek için Site Recovery kullanarak.
* **SAP proje yükseltmelerinde, test ve prototipi oluşturulurken basitleştirmek** bir üretim oluşturarak SAP uygulamaları test etmek için isteğe bağlı klonlayın.

Bu makalede kullanarak SAP NetWeaver uygulama dağıtımları korumak nasıl [Azure Site Recovery](site-recovery-overview.md). Makalede, bir üç katmanlı SAP NetWeaver dağıtımı Azure Site Recovery kullanarak başka bir Azure veri merkezine çoğaltma tarafından korunması için en iyi yöntemler yer almaktadır. Desteklenen senaryolar ve yapılandırmaları ve yük devretme sınaması işlemlerini (olağanüstü durum kurtarma ayrıntılarını) gerçekleştirmek nasıl açıklar ve gerçek yük devretmeleri.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdaki görevlerin nasıl yapılacağını bildiğinizden emin olun:

* [Bir sanal makine azure'a](azure-to-azure-walkthrough-enable-replication.md)
* [Kurtarma ağını tasarlama](site-recovery-azure-to-azure-networking-guidance.md)
* [Azure'a yük devretme testi yapın](azure-to-azure-walkthrough-test-failover.md)
* [Bir yük devretme gerçekleştirmek](site-recovery-failover.md)
* [Bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
* [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Aşağıdaki senaryolarda bir olağanüstü durum kurtarma çözümü uygulamak için Site RECOVERY'yi kullanabilirsiniz:
* Başka bir Azure veri merkezine (Azure Azure olağanüstü durum kurtarma) çoğaltma bir Azure veri merkezinde çalışan SAP sistemler. Daha fazla bilgi için bkz: [Azure Azure çoğaltma mimarisi](https://aka.ms/asr-a2a-architecture).
* SAP sistemleri VMware (veya fiziksel) çalıştıran sunucuları şirket içi bir olağanüstü durum kurtarma sitesine bir Azure veri merkezi (VMware Azure olağanüstü durum kurtarma), çoğaltılması. Bu senaryoda, bazı ek bileşenleri gerekir. Daha fazla bilgi için bkz: [VMware Azure çoğaltma mimarisi](https://aka.ms/asr-v2a-architecture).
* Hyper-V (Azure için Hyper V olağanüstü durum kurtarma) Azure veri merkezinde bir olağanüstü durum kurtarma sitesine çoğaltılan içi çalıştırılan SAP sistemlerine. Bu senaryoda, bazı ek bileşenleri gerekir. Daha fazla bilgi için bkz: [Hyper-V-Azure'a çoğaltma mimarisi](https://aka.ms/asr-h2a-architecture).

Bu makalede, kullandığımız bir **Azure Azure** Site Recovery SAP olağanüstü durum kurtarma özelliklerini göstermek için olağanüstü durum kurtarma senaryosunda. Site Recovery çoğaltma uygulamaya özgü olmadığından açıklanan işlemi diğer senaryolar için de geçerlidir beklenir.

### <a name="required-foundation-services"></a>Gerekli foundation Hizmetleri
Bu makalede aşağıdakiler ele senaryosunda aşağıdaki foundation Hizmetleri dağıtılır:
* Azure ExpressRoute veya Azure VPN ağ geçidi
* En az bir Active Directory etki alanı denetleyicisi ve DNS sunucusu, Azure'da çalışan

Site Recovery dağıtmadan önce bu altyapı kurmak öneririz.

## <a name="reference-sap-application-deployment"></a>Başvuru SAP uygulama dağıtımı

SAP NetWeaver Windows ortamında yüksek kullanılabilirlik ile Azure üzerinde çalışan bu başvuru mimarisinin gösterir.  Bu mimari, kuruluşunuzun ihtiyaçlarını karşılamak için değiştirilebilir belirli bir sanal makine (VM) boyutları ile dağıtılır.

![Tipik bir SAP dağıtım Düzen diyagramı](./media/site-recovery-sap/reference_sap.png)

## <a name="disaster-recovery-considerations"></a>Olağanüstü durum kurtarma değerlendirmeleri

Olağanüstü Durum Kurtarma (DR) için bir ikincil bölge'ye yük devri mümkün olması gerekir. Olağanüstü durum kurtarma (DR) koruması sağlamak için her katman farklı bir strateji kullanır.

#### <a name="vms-running-sap-web-dispatcher-pool"></a>SAP Web dağıtıcı havuzundaki çalışan sanal makineler 
Web gönderen bileşeni, SAP uygulama sunucuları arasında SAP trafiği için yük dengeleyici olarak kullanılır. Web gönderen bileşeni için yüksek kullanılabilirlik elde etmek için Azure yük dengeleyici dengeleyici havuzunda kullanılabilir Web dağıtıcıları arasında HTTP (S) trafiğinin dağıtılmak hepsini yapılandırmasında paralel Web dağıtıcısı Kurulum uygulamak için kullanılır. Bu Azure Site kasadır kullanarak çoğaltılır ve Otomasyon betikleri olağanüstü durum kurtarma bölgesi üzerinde yük dengeleyici yapılandırmak için kullanılır. 

####<a name="vms-running-application-servers-pool"></a>Uygulama sunucuları havuzu çalışan sanal makineler
SMLG işlem ABAP uygulama sunucuları için oturum açma grupları yönetmek için kullanılır. Yük Dengeleme ileti sunucusu merkezi Hizmetleri işlevindeki SAPGUIs ve RFC için SAP uygulama sunucuları havuzu arasında iş yükünü dağıtmak için kullandığı trafiği. Bu Azure Site RECOVERY'yi kullanarak çoğaltılır 

####<a name="vms-running-sap-central-services-cluster"></a>SAP merkezi hizmetlerinin küme çalışan sanal makineler
Bu başvuru mimarisinin merkezi Hizmetleri uygulama katmanında Vm'lerinde çalıştırır. Merkezi Hizmetleri olası tek bir hata (SPOF) için tek bir VM'ye dağıtıldığında noktasıdır — yüksek oranda kullanılabilirlik gereksinimi olmadığında tipik dağıtım.<br>

Yüksek kullanılabilirlik çözümü uygulamak için bir paylaşılan disk kümesi veya bir dosya paylaşımı kümesi kullanılabilir. Bir paylaşılan disk kümesi için sanal makineleri yapılandırmak için Windows Server Yük devretme kullanın. Bulut tanığı çekirdek tanığı önerilir. 
 > [!NOTE]
 > Azure Site Recovery bulut Tanık çoğaltılmaz bu nedenle olağanüstü durum kurtarma bölgedeki bulut Tanık dağıtmak için önerilir.

Yük devretme küme ortamını desteklemek üzere [SIOS DataKeeper Cluster Edition](https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8) küme düğümleri tarafından sahip olunan bağımsız diskler çoğaltarak Küme Paylaşılan Birimi işlevi gerçekleştirir. Azure yerel paylaşılan diskleri desteklemez ve bu nedenle SIOS tarafından sağlanan çözüm gerektirir. 

Kümeleme işlemek için başka bir yolu, bir dosya paylaşımı kümesi uygulamaktır. [SAP](https://blogs.sap.com/2018/03/19/migration-from-a-shared-disk-cluster-to-a-file-share-cluster) /sapmnt genel dizin bir UNC yolu üzerinden erişmek için Yönetim Hizmetleri dağıtım modeli yakın zamanda. Bu değişiklik SIOS gereksinimini veya diğer paylaşılan disk çözümleri merkezi Hizmetleri VM'ler kaldırır. Hala /sapmnt UNC paylaşımı yüksek oranda kullanılabilir olduğundan emin olmak için önerilir. Bu merkezi Hizmetleri örneğinde ölçek genişletme arama dosya sunucusu (SOFS) ve Windows Server 2016 depolama alanları doğrudan (S2D) özelliği ile Windows Server Yük devretme kullanarak yapılabilir. 
 > [!NOTE]
 > Şu anda Azure Site Recovery desteği yalnızca kilitlenme tutarlı noktası çoğaltma sanal makinelerin doğrudan depolama alanları kullanma 


## <a name="disaster-recovery-considerations"></a>Olağanüstü durum kurtarmayla konusunda dikkat edilmesi gerekenler

Azure Site Recovery başarısız üzerinden tam SAP dağıtımını Azure bölgeler arasında düzenlemek için kullanabilirsiniz.
Olağanüstü durum kurtarma adımları aşağıda verilmiştir 

1. Çoğaltma sanal makineler 
2. Kurtarma ağını tasarlama
3.  Bir etki alanı denetleyicisi çoğaltma
4.  Veri temel katmanı Çoğalt 
5.  Yük devretme testi yapın 
6.  Bir yük devretme işlemi gerçekleştirin 

Aşağıda bu örnekte kullanılan her katman olağanüstü durum kurtarması için önerilir. 

 **SAP katmanları** | **Öneri**
 --- | ---
**SAP Web dağıtıcı havuzundaki** |  Site RECOVERY'yi kullanarak çoğaltma 
**SAP uygulama sunucusu havuzu** |  Site RECOVERY'yi kullanarak çoğaltma 
**SAP merkezi Hizmetleri küme** |  Site RECOVERY'yi kullanarak çoğaltma 
**Active directory sanal makineler** |  Active directory çoğaltma 
**SQL veritabanı sunucuları** |  SQL çoğaltma her zaman açık

##<a name="replicate-virtual-machines"></a>Çoğaltma sanal makineler

Tüm SAP uygulama sanal makineleri Azure olağanüstü durum kurtarma veri merkezine çoğaltma başlatmak için yönergeleri [bir sanal makine Azure'a](azure-to-azure-walkthrough-enable-replication.md).


* Active Directory ve DNS'yi koruma ile ilgili yönergeler için bkz [korumak Active Directory ve DNS](site-recovery-active-directory.md) belge.

* Veritabanı katmanı SQL sunucusunda çalışan koruma ile ilgili yönergeler için bkz [SQL Server koruma](site-recovery-active-directory.md) belge.

## <a name="networking-configuration"></a>Ağ yapılandırması

Statik bir IP adresi kullanırsanız, sanal makinenin almak için istediğiniz IP adresini belirtebilirsiniz. IP adresi ayarlamak için Git **işlem ve ağ ayarlarını** > **ağ arabirim kartı**.

![Site kurtarma ağ arabirimi kartı Bölmesi'nde özel bir IP adresi ayarlamak gösteren ekran görüntüsü](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma
Bir kurtarma planı yük devretme sırasında çok katmanlı bir uygulama çeşitli katmanları sıralama destekler. Sıralama uygulama tutarlılığı korumaya yardımcı olur. Çok katmanlı web uygulaması için bir kurtarma planı oluşturduğunuzda, tam adımlar açıklanan [Site Recovery kullanarak bir kurtarma planı oluşturmak](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler yük devretme gruplara ekleme

1.  Uygulama sunucusu, web dağıtıcısı ve SAP merkezi Hizmetleri VM'ler ekleyerek bir kurtarma planı oluşturun.
2.  'VM'ler gruplandırmak için Özelleştir' üzerinde'i tıklatın. Varsayılan olarak, tüm VM'ler 'Grubu 1' bir parçasıdır.



### <a name="add-scripts-to-the-recovery-plan"></a>Komut dosyaları kurtarma planına ekleyin
Uygulamalarınızın düzgün çalışması, yük devretme sonrasında veya yük devretme testi sırasında Azure sanal makinelerde bazı işlemler yapmanız gerekebilir. Bazı yük devretme sonrası işlemleri otomatik hale getirebilirsiniz. Örneğin, DNS girişini güncelleştirmek ve kurtarma planına betikleri ekleyerek bağlamalar ve bağlantıları değiştirin.


En yaygın kullanılan Azure Site Recovery betikleri 'Azure'a dağıtma' düğmesini tıklatarak Otomasyon hesabınızda dağıtabilirsiniz. Yayımlanmış herhangi komut dosyası kullanırken, komut dosyası yer alan yönergeleri izleyin emin olun.

[![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Bir ön eylem betiği 'Grubu 1' için yük devretme SQL kullanılabilirlik grubuna ekleyin. Örnek komut dosyalarını yayımlanan 'ASR-SQL-FailoverAG' komut dosyası kullanın. Komut dosyası yer alan yönergeleri izleyin ve uygun şekilde komut dosyasında gerekli değişiklikleri yapın emin olun.
2. Başarısız bir yük dengeleyicideki eklemek için bir post eylem komut dosyası ekleme Web Katmanı (Grup 1) sanal makinelerin üzerinden. Örnek komut dosyalarını yayımlanan 'ASR-AddSingleLoadBalancer' komut dosyası kullanın. Komut dosyası yer alan yönergeleri izleyin ve uygun şekilde komut dosyasında gerekli değişiklikleri yapın emin olun.

![SAP kurtarma planı](./media/site-recovery-sap/sap_recovery_plan.png)


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1.  Azure portalında, Kurtarma Hizmetleri kasası seçin.
2.  SAP uygulamaları için oluşturulan kurtarma planı seçin.
3.  Seçin **yük devretme testi**.
4.  Test yük devretme işlemini başlatmak için kurtarma noktası ve Azure sanal ağı seçin.
5.  İkincil ortam yukarı olduğunda doğrulama gerçekleştirin.
6.  Doğrulama tamamlandığı zaman yük devretme ortamı temizlenecekse seçin **temizleme yük devretme testi**.

Daha fazla bilgi için bkz: [Azure Site kurtarma için yük devretme testi](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1.  Azure portalında, Kurtarma Hizmetleri kasası seçin.
2.  SAP uygulamaları için oluşturulan kurtarma planı seçin.
3.  Seçin **yük devretme**.
4.  Yük devretme işlemini başlatmak için kurtarma noktası seçin.

Daha fazla bilgi için bkz: [Site Recovery yük](site-recovery-failover.md).

## <a name="next-steps"></a>Sonraki adımlar
* Site RECOVERY'yi kullanarak SAP NetWeaver dağıtımları için bir olağanüstü durum kurtarma çözümü oluşturma hakkında daha fazla bilgi için indirilebilir teknik incelemesine bakın [SAP NetWeaver: Azure Site Recovery ile olağanüstü durum kurtarma çözümü oluşturma](http://aka.ms/asr-sap). Teknik incelemesinde çeşitli SAP mimarileri için öneriler ele alınmıştır, Azure üzerinde SAP için desteklenen uygulamalar ve VM türlerini listeler ve olağanüstü durum kurtarma çözümünüzün test planı seçenekleri açıklar.
* Daha fazla bilgi edinmek [diğer iş yüklerini çoğaltma](site-recovery-workload.md) Site Recovery kullanarak.
