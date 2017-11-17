---
title: "Azure Site Recovery kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımı koruma | Microsoft Docs"
description: "Bu makalede, Azure Site RECOVERY'yi kullanarak SAP NetWeaver uygulama dağıtımları korumak açıklar"
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
ms.openlocfilehash: 5a47acab598e113ef7ed968dd3a6429ac3bc1ec3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>Azure Site Recovery kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımı koruma

En büyük ve orta ölçekli SAP dağıtımları bazı olağanüstü durum kurtarma çözümü sahiptir.  SAP gibi uygulamalar için daha fazla çekirdek iş süreçlerini taşınırken sağlam ve sınanabilir olağanüstü durum kurtarma çözümleri önemini artar.  Azure Site Recovery test edilmiş ve tümleşik SAP uygulamaları olmuştur ve daha düşük maliyetle toplam sahip olma maliyetini (TCO) rakip çözümleri daha çoğu şirket içi olağanüstü durum kurtarma çözümleri yeteneklerini aşıyor.
Azure Site Recovery ile şunları yapabilirsiniz:
* Bileşenleri Azure'a çoğaltarak, şirket içinde çalışan SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin.
* Bileşenleri başka bir Azure veri merkezine çoğaltarak,Azure çalıştıran SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin.
* SAP dağıtımınızı Azure'a geçirmek için Site Recovery'yi kullanarak buluta geçişi basitleştirin.
* SAP uygulamaları test etmek için talep üzerine üretim kopyası oluşturarak SAP proje yükseltmelerini, testlerini ve prototip oluşturma işlemlerini basitleştirin.

Bu makalede kullanarak SAP NetWeaver uygulama dağıtımları korumak nasıl [Azure Site Recovery](site-recovery-overview.md). Bu makalede Azure ile ilgili üç katmanlı SAP NetWeaver dağıtım Azure Site Recovery, desteklenen senaryolar ve yapılandırmaları kullanarak başka bir Azure veri merkezine çoğaltma tarafından korunması için en iyi yöntemler yer almaktadır ve yük devretme, gerçekleştirme hem de yük devretme (olağanüstü durum kurtarma ayrıntılarını) ve gerçek yük devretme testi.


## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce aşağıdakileri bildiğinizden emin olun:

1. [Bir sanal makine Azure'a çoğaltma](azure-to-azure-walkthrough-enable-replication.md)
2. Nasıl yapılır [kurtarma ağını tasarlama](site-recovery-azure-to-azure-networking-guidance.md)
3. [Azure'a yük devretme sınamasını yapmak](azure-to-azure-walkthrough-test-failover.md)
4. [Azure için bir yük devretme işleminden](site-recovery-failover.md)
5. Nasıl yapılır [bir etki alanı denetleyicisi çoğaltma](site-recovery-active-directory.md)
6. Nasıl yapılır [SQL Server çoğaltma](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Azure Site Recovery ile aşağıdaki senaryolar için olağanüstü durum kurtarma çözümü uygulayabilirsiniz:
* Bir Azure veri merkezi başka bir Azure veri merkezine (Azure Azure DR), çoğaltma olarak tasarlanmış olarak çalışan SAP sistemleri [burada](https://aka.ms/asr-a2a-architecture).
* Site tasarlanmış gibi bazı ek bileşenleri gerektiren bir Azure veri merkezinde (VMware Azure DR) SAP sistemleri VMWare (veya fiziksel) sunucuları için DR çoğaltma şirket içi çalışan [burada](https://aka.ms/asr-v2a-architecture).
* Site tasarlanmış gibi bazı ek bileşenleri gerektiren bir Azure veri merkezinde (Hyper-V-Azure'a DR) Hyper-V için DR çoğaltma şirket içi çalışan SAP sistemleri [burada](https://aka.ms/asr-h2a-architecture).

Bu belge, Azure Site Recovery'nin SAP olağanüstü durum kurtarma özellikleri göstermek için ilk harfi - Azure Azure DR - kullanır. Azure Site Recovery çoğaltma uygulama belirsiz olduğundan açıklanan sürecin diğer senaryolar için tutmak için bekleniyor.

### <a name="required-foundation-services"></a>Gerekli foundation Hizmetleri
Tüm bu belgeleri senaryo edilmiş dağıtılan aşağıdaki foundation Hizmetleri ile dağıtılır:
* ExpressRoute veya siteden siteye sanal özel ağ (VPN)
* Azure'da çalışan en az bir Active Directory etki alanı denetleyicisi ve DNS sunucusu

Azure Site Recovery dağıtmadan önce yukarıdaki altyapınızı oluşturduğunuzu önerilir.


## <a name="typical-sap-application-deployment"></a>Tipik SAP uygulama dağıtımı
Büyük SAP müşteriler genellikle 6-20 tek tek SAP uygulamalar arasında dağıtın.  Bu uygulamaların çoğu SAP NetWeaver ABAP veya Java motorlarına dayanır.  Bu çekirdek NetWeaver uygulamaları destekleyen pek çok daha küçük belirli NetWeaver SAP olmayan tek başına motorları ve genellikle bazı SAP olmayan uygulamalar şunlardır.  

Stok tüm SAP uygulamalar, bir yatay ve dağıtım modunu (Katman 2 veya 3 katmanlı), sürüm, düzeltme ekleri belirlemek için çalıştırma boyutları, karmaşıklığı hızları ve disk Kalıcılık gereksinimleri için kritik öneme sahiptir.

![Dağıtım modeli](./media/site-recovery-sap/sap-typical-deployment.png)

SAP veritabanı saklama katmanını, SQL Server AlwaysOn, Oracle DataGuard veya HANA sistem çoğaltma gibi yerel DBMS araçları aracılığıyla korunmalıdır. İstemci katmanı da Azure Site Recovery tarafından korumalı olmayan, ancak bu katman DNS yayılma gecikmesi, güvenlik ve Uzaktan erişim DR veri merkezine gibi etkisi konuları dikkate almak önemlidir.

Azure Site Recovery (A) SCS dahil olmak üzere uygulama katmanı için önerilen çözümdür. NetWeaver SAP uygulamaları ve SAP olmayan uygulamalar gibi diğer uygulamaları form genel SAP dağıtım ortamının bir parçası ve ayrıca Azure Site Recovery ile korunması.

## <a name="replicate-virtual-machines"></a>Çoğaltma sanal makineler
İzleyin [bu kılavuz](azure-to-azure-walkthrough-enable-replication.md) tüm SAP uygulama sanal makineleri Azure DR veri merkezine çoğaltma başlatmak için.

Bir statik IP kullanıyorsanız, sanal makinenin işlem ve ağ ayarlarının ağ arabirimi kartları bölümünde almak istediğiniz IP belirtebilirsiniz.

![Hedef IP](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma
Bir kurtarma planı çeşitli katmanlarda çok katmanlı bir uygulama, bu nedenle, uygulama tutarlılığını sağlamak için yük devretme sıralama sağlar. Açıklanan adımları izleyin [burada](site-recovery-create-recovery-plans.md) çok katmanlı web uygulaması için bir kurtarma planı oluşturma sırasında.

### <a name="adding-scripts-to-the-recovery-plan"></a>Betikleri kurtarma planına ekleniyor
Azure sanal makineleri post yük devretme ve test yük devretme düzgün çalışması, uygulamalarınız için üzerindeki bazı işlemler yapmanız gerekebilir. DNS girişi güncelleştirme ve ilgili betikleri kurtarma planında açıklandığı gibi ekleyerek bağlamalar ve bağlantıları değiştirme gibi post yük devretme işlemi otomatikleştirebilirsiniz [bu makalede](site-recovery-create-recovery-plans.md#add-scripts).

### <a name="dns-update"></a>DNS güncelleştirme
DNS genellikle dinamik DNS güncelleştirmeleri ve ardından sanal makineleri için yapılandırılmışsa, DNS başladıktan sonra yeni IP ile güncelleştirin. Sanal makineleri yeni IP ile DNS güncelleştirme sonra bu eklemek için açık bir adım eklemek istiyorsanız [IP DNS'de güncelleştirmek için komut dosyası](https://aka.ms/asr-dns-update) kurtarma planı grupları sonrası eylemi olarak.  

## <a name="example-azure-to-azure-deployment"></a>Örnek Azure Azure dağıtımı
Azure Site Recovery Azure Azure DR Aşağıdaki diyagramda senaryo belirtilmiştir:
* Birincil veri merkezindeki Singapur (Azure Güney-Doğu Asya) ve DR datacenter Hong Kong (Azure Doğu Asya).  Bu senaryoda, SQL Server AlwaysOn Singapur zaman uyumlu modda çalışan iki VM sağlayarak yerel yüksek kullanılabilirlik sağlanır.
* Dosya Paylaşımı ASCS SAP tek hata noktaları için HA sağlamak için kullanılabilir. Dosya Paylaşımı ASCS bir küme paylaşılan disk gerektirmez ve SIOS gibi uygulamalar gerekli değildir.
* DBMS katmanı için DR koruma zaman uyumsuz çoğaltma kullanılarak gerçekleştirilir.
* Bu senaryoyu üretim tam bir kopyasını bir DR çözüm tanımlamak için kullanılan bir terim "simetrik DR" – gösterir, bu nedenle DR SQL Server çözümünü yerel yüksek kullanılabilirliğe sahip. Simetrik DR kullanımını veritabanı katmanı için zorunlu değildir ve bir yerel yüksek kullanılabilirlik düğümü DR olayından sonra hızlı bir şekilde oluşturmak için bulut dağıtımları esnekliğini birçok müşteri yararlanın.
* Diyagram Azure Site Recovery tarafından çoğaltılan SAP NetWeaver ASCS ve uygulama sunucusu katmanı gösterir.

![Çoğaltma senaryosu](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>Yük devretme sınamasını yapmak
İzleyin [bu kılavuz](azure-to-azure-walkthrough-test-failover.md) yük devretme sınamasını yapmak için.

1.  Azure Portalı'na gidin ve kurtarma Hizmetleri kasanız seçin.
2.  SAP uygulamaları (s) için oluşturulan kurtarma planı tıklayın.
3.  'Test yük devretme üzerinde' tıklayın.
4.  Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.
5.  İkincil ortamı kurma olduğunda, doğrulama gerçekleştirebilirsiniz.
6.  Doğrulama tamamlandıktan sonra 'Temizleme test yük devretme' ve yük devretme ortamında temizlemek için tıklatın.

## <a name="doing-a-failover"></a>Bir yük devretme işleminden
İzleyin [bu kılavuz](site-recovery-failover.md) , yaparken bir yük devretme.

1.  Azure Portalı'na gidin ve kurtarma Hizmetleri kasanız seçin.
2.  SAP uygulamaları için oluşturulan kurtarma planı tıklayın.
3.  'Failover' üzerinde'ı tıklatın.
4.  Yük devretme işlemini başlatmak için kurtarma noktası seçin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Site RECOVERY'yi kullanarak SAP NetWeaver dağıtımları için bir olağanüstü durum kurtarma çözümü oluşturma hakkında daha fazla bilgi [Bu teknik](http://aka.ms/asr-sap). Teknik İnceleme de farklı SAP mimari için öneriler açıklanır, Azure üzerinde SAP için desteklenen uygulamalar ve VM türlerini listeler ve olası test planları, olağanüstü durum kurtarma çözümünüzün anlatılır.

Daha fazla bilgi edinmek [diğer iş yüklerini çoğaltma](site-recovery-workload.md) Site RECOVERY'yi kullanarak.
