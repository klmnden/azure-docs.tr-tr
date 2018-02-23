---
title: "Azure Site Recovery kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımı koruma | Microsoft Docs"
description: "Bu makalede, Azure Site RECOVERY'yi kullanarak SAP NetWeaver uygulama dağıtımlarını korumaya açıklar."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 1ee472498bdefc4eeb9863670e5480326b5ba6d8
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
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

Bu makalede, Site Recovery SAP olağanüstü durum kurtarma özelliklerini göstermek için bir Azure Azure olağanüstü durum kurtarma senaryosunda kullanın. Site Recovery çoğaltma uygulamaya özgü olmadığından açıklanan işlemi diğer senaryolar için de geçerlidir beklenir.

### <a name="required-foundation-services"></a>Gerekli foundation Hizmetleri
Bu makalede aşağıdakiler ele senaryosunda aşağıdaki foundation Hizmetleri dağıtılır:
* Azure ExpressRoute veya Azure VPN ağ geçidi
* En az bir Active Directory etki alanı denetleyicisi ve DNS sunucusu, Azure'da çalışan

Site Recovery dağıtmadan önce bu altyapı kurmak öneririz.

## <a name="typical-sap-application-deployment"></a>Tipik SAP uygulama dağıtımı
Büyük SAP müşteriler genellikle arasında 6 ile 20 bireysel SAP uygulamaları dağıtın. Bu uygulamaların çoğu SAP NetWeaver ABAP veya Java motorlarına dayanır. Pek çok daha küçük, belirli olmayan NetWeaver SAP tek başına motorları ve bazı SAP olmayan uygulamalar genellikle bu çekirdek NetWeaver uygulamaları destekler.  

Ortamınızda çalışan tüm SAP uygulamaları stok önemlidir. Sonra dağıtım modu (iki katmanlı veya üç katmanlı), sürümler, düzeltme ekleri, boyutları, karmaşası hızları ve disk Kalıcılık gereksinimlerini belirleyin.

![Tipik bir SAP dağıtım Düzen diyagramı](./media/site-recovery-sap/sap-typical-deployment.png)

SQL Server AlwaysOn, Oracle Data Guard veya SAP HANA sistem çoğaltma gibi yerel DBMS araçları kullanarak SAP veritabanı saklama katmanını koruyun. SAP veritabanı katmanı gibi istemci katmanı Site Recovery tarafından korunmuyor. Bu katman etkileyen etkenleri göz önünde bulundurmak önemlidir. Etkenler DNS yayılma gecikmesi, güvenlik ve olağanüstü durum kurtarma veri merkezine uzaktan erişim içerir.

Site kurtarma SAP SCS ve ASCS dahil olmak üzere uygulama katmanı için önerilen çözümdür. NetWeaver SAP uygulamaları ve SAP olmayan uygulamalar gibi diğer uygulamaları genel SAP dağıtım ortamı parçasını oluşturur. Bunları Site Recovery ile korumanız gerekir.

## <a name="replicate-virtual-machines"></a>Çoğaltma sanal makineler
Tüm SAP uygulama sanal makineleri Azure olağanüstü durum kurtarma veri merkezine çoğaltma başlatmak için yönergeleri [bir sanal makine Azure'a](azure-to-azure-walkthrough-enable-replication.md).

Statik bir IP adresi kullanırsanız, sanal makinenin almak için istediğiniz IP adresini belirtebilirsiniz. IP adresi ayarlamak için Git **işlem ve ağ ayarlarını** > **ağ arabirim kartı**.

![Site kurtarma ağ arabirimi kartı Bölmesi'nde özel bir IP adresi ayarlamak gösteren ekran görüntüsü](./media/site-recovery-sap/sap-static-ip.png)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
Bir kurtarma planı yük devretme sırasında çok katmanlı bir uygulama çeşitli katmanları sıralama destekler. Sıralama uygulama tutarlılığı korumaya yardımcı olur. Çok katmanlı web uygulaması için bir kurtarma planı oluşturduğunuzda, tam adımlar açıklanan [Site Recovery kullanarak bir kurtarma planı oluşturmak](site-recovery-create-recovery-plans.md).

### <a name="add-scripts-to-the-recovery-plan"></a>Komut dosyaları kurtarma planına ekleyin
Uygulamalarınızın düzgün çalışması, yük devretme sonrasında veya yük devretme testi sırasında Azure sanal makinelerde bazı işlemler yapmanız gerekebilir. Bazı yük devretme sonrası işlemleri otomatik hale getirebilirsiniz. Örneğin, DNS girişini güncelleştirmek ve kurtarma planına betikleri ekleyerek bağlamalar ve bağlantıları değiştirin.

### <a name="dns-update"></a>DNS güncelleştirme
DNS dinamik DNS güncelleştirme için yapılandırılmışsa, bunlar başlattığınızda sanal makinelerin DNS genellikle yeni IP adresiyle güncelleştirin. Sanal makineleri yeni IP adresleri ile DNS güncelleştirme, ekleme için açık bir adım eklemek istiyorsanız bir [IP adresini DNS'de güncelleştirmek için komut dosyası](https://aka.ms/asr-dns-update) kurtarma planı grupları bir yük devretme sonrasında eylem olarak.  

## <a name="example-azure-to-azure-deployment"></a>Örnek Azure Azure dağıtımı
Aşağıdaki diyagramda, Site Recovery Azure Azure olağanüstü durum kurtarma senaryosunda gösterilmektedir:

![Bir Azure Azure çoğaltma senaryosu diyagramı](./media/site-recovery-sap/sap-replication-scenario.png)

* Birincil veri merkezindeki Singapur (Azure Güney-Doğu Asya) olur. Olağanüstü durum kurtarma datacenter Hong Kong (Azure Doğu Asya) olur. Bu senaryoda, yerel yüksek kullanılabilirlik, SQL Server AlwaysOn Singapur zaman uyumlu modda çalışan iki VM tarafından sağlanır.
* Dosya Paylaşımı SAP ASCS SAP tekli hata noktaları için yüksek kullanılabilirlik sağlar. Dosya Paylaşımı ASCS bir küme paylaşılan disk gerektirmez. SIOS gibi uygulamalar gerekli değildir.
* Olağanüstü durum kurtarma koruması DBMS katman için zaman uyumsuz çoğaltma kullanılarak elde edilir.
* Bu senaryo "simetrik olağanüstü durum kurtarma" gösterir Bu terim üretim tam bir çoğaltması bir olağanüstü durum kurtarma çözümü açıklanmaktadır. Olağanüstü durum kurtarma SQL Server çözümünü yerel yüksek kullanılabilirliğe sahip. Simetrik olağanüstü durum kurtarma için veritabanı katmanı zorunlu değildir. Birçok müşteri yerel yüksek kullanılabilirlik düğümü bir olağanüstü durum kurtarma olayı sonra hızlı bir şekilde oluşturmak için bulut dağıtımları esnekliğini yararlanın.
* Diyagram SAP NetWeaver ASCS ve uygulama sunucusu katmanı Site Recovery tarafından çoğaltılan gösterilmektedir.

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
