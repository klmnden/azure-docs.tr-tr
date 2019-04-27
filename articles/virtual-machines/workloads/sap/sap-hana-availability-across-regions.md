---
title: Azure bölgeleri arasında SAP HANA kullanılabilirliği | Microsoft Docs
description: Birden fazla Azure bölgesinde Azure sanal makinelerinde SAP HANA çalıştırırken kullanılabilirlik değerlendirmelerine genel bakış.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/12/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 95ada2cb146bdbc972afee883a1d174c95aa67d7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60650309"
---
# <a name="sap-hana-availability-across-azure-regions"></a>Azure bölgeleri arasında SAP HANA kullanılabilirliği

Bu makalede, SAP HANA kullanılabilirliği için farklı Azure bölgelerindeki ilgili senaryolar açıklanmaktadır. Azure bölgeleri arasındaki uzaklığı nedeniyle, birden çok Azure bölgesinde SAP HANA kullanılabilirliği dökümünü ayarlama özel konular içerir.

## <a name="why-deploy-across-multiple-azure-regions"></a>Neden Azure bölgelerinde dağıtma

Azure bölgeleri, genellikle büyük mesafeler tarafından ayrılır. Jeopolitik bölge bağlı olarak, Azure bölgeleri arasındaki uzaklığı mil yüzlerce veya hatta birkaç bin mil, gibi Amerika Birleşik Devletleri'nde olabilir. Önemli ağ gidiş dönüş gecikmesi nedeniyle uzaklığı, iki farklı Azure bölgelerinde dağıtılan varlıklar arasındaki ağ trafiğini karşılaşırsınız. Gecikme süresi, tipik SAP iş yüklerini altında iki SAP HANA örnekleri arasında zaman uyumlu veri değişimi hariç tutmak çok önemlidir. 

Öte yandan, kuruluşlar genellikle bir uzaklık birincil veri merkezi konumu ile ikincil veri merkezine arasında gereksinim. Uzaklık gereksinimi daha geniş bir coğrafi konumda bir doğal felaket ortaya çıkarsa kullanılabilirlik sağlar. Florida ve Karayipler portala Eylül-Ekim 2017 ' tıklama kasırgalar örneklerindendir. Kuruluşunuz, en az bir en az uzaklığı gereksinim olabilir. Çoğu Azure müşterileri için en az uzaklığı tanımı kullanılabilirlik için tasarlama gerektirir [Azure bölgeleri](https://azure.microsoft.com/regions/). İki Azure bölgesi arasındaki uzaklığı HANA zaman uyumlu çoğaltma modu kullanmak için çok büyük olduğundan, RTO ve RPO gereksinimleri tek bir bölge içinde kullanılabilirlik yapılandırmaları dağıtma ve sonra bir saniye içinde başka dağıtımlar ile ek zorlayabilir bölge.

Bu senaryoda dikkate alınması gereken başka bir yük devretme parçasıdır ve istemci yönlendirin. İki farklı Azure bölgelerindeki her zaman SAP HANA örnekleri arasında bir yük devretmeyi el ile bir yük devretme olduğu varsayılır. SAP HANA sistem çoğaltması çoğaltma modunu zaman uyumsuz ayarlandığından, birincil HANA örneğinde kaydedilmiş veri henüz bu ikincil HANA örneğinde yapılan taşınmadığından emin olası yoktur. Bu nedenle, otomatik yük devretme çoğaltması zaman uyumsuz olduğu yapılandırmaları için bir seçenek değildir. Bir yük devretme alıştırma olduğu gibi el ile Denetlenen yük devretme olsa da, diğer Azure bölgesine el ile taşıyabiliyor önce birincil tarafında kaydedilen tüm verilerin ikincil örneğine yapılan emin olmak için önlemler gerekir.
 
Azure sanal ağı, farklı bir IP adresi aralığını kullanır. IP adresleri, ikinci bir Azure bölgesinde dağıtılır. Bu nedenle, SAP HANA istemci yapılandırmasını değiştirmek sahip olmaları veya tercihen, ad çözümlemesi değiştirme adımları oluşturmanız gerekir. Bu şekilde, istemcilerin yeni ikincil sitenin sunucu IP adresine yönlendirilirsiniz. Daha fazla bilgi için SAP bkz [devralma sonra istemci bağlantı kurtarma](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html).   

## <a name="simple-availability-between-two-azure-regions"></a>İki Azure bölgeleri arasında basit kullanılabilirlik

Değil tek bir bölgedeki herhangi bir kullanılabilirlik yapılandırma yararlanılmasını, ancak bir olağanüstü durum oluşursa sunulan iş yükü için isteğe bağlı çözümlenmedi seçebilirsiniz. Bu tür senaryolar için tipik durumlarda, üretim dışı sistemleridir. Yarım gün veya bile bir gün için aşağı sistem sorun sürdürülebilir olsa da, 48 saat veya daha fazla bilgi için kullanılamaz olarak sistem izin veremez. Kurulumu daha az masraflı bir hale getirmek için VM'yi bile daha az önemli olan başka bir sistem çalıştırın. Sistemden bir hedef olarak çalışır. Daha küçük olacak şekilde ikincil bölgedeki VM boyutunu ve veri dağıtılacak değil seçin. Yük devretmeyi el ile ve uygulamanın yığın üzerinde başarısız olan birçok daha fazla adım gerektirir, VM'yi kapatın, yeniden boyutlandırabilir ve VM yeniden için ek süre kabul edilebilir olmasıdır.

Bir sanal makinede bir QA sistemiyle DR hedef paylaşımı, senaryosu kullanıyorsanız, bu konuları dikkate almak gerekir:

- İki [işlem modları](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html) delta_datashipping ve logreplay ile olduğu gibi bir senaryo için kullanılabilir
- Her iki işlem modları veri önceden olmadan farklı bellek gereksinimlerin
- Delta_datashipping logreplay gerektirebilir daha önemli ölçüde daha az bellek önyükleme seçeneği olmadan gerektirebilir. SAP belgenin bölüm 4.3 bakın [nasıl gerçekleştirmek sistem çoğaltmanın için SAP HANA](https://archive.sap.com/kmuuid2/9049e009-b717-3110-ccbd-e14c277d84a3/How%20to%20Perform%20System%20Replication%20for%20SAP%20HANA.pdf)
- Preload olmadan logreplay işlem modu bellek gereksinimi belirleyici değildir ve columnstore yapıları yüklenen bağlıdır. Aşırı durumlarda birincil örneğinin bellek yüzdesi 50 gerektirebilir. Bellek logreplay çalışma modu için verilerin önceden yüklenmiş veya ayarlamak seçtiğiniz bağımsızdır.


![İki bölgeleri üzerinden iki sanal makine diyagramı](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> Bu yapılandırmada, bir RPO sağlayamaz = 0, HANA sistem çoğaltma modunu zaman uyumsuz olduğundan. Bir RPO sağlamanız gerekiyorsa, = 0, bu yapılandırma tercih ettiğiniz bir yapılandırma değildir.

Yapılandırmada yaptığınız küçük bir değişiklik olarak önceden yükleme verilerini yapılandırmak için olabilir. Ancak, el ile yük devretme ve uygulama katmanları da ikinci bölgeye taşımanıza gerek gerçeği göz önünde bulundurulduğunda, bu verileri önceden yüklemek için anlamlı olmayabilir. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Bir bölge içinde ve bölgeler arası kullanılabilirlik birleştirin 

Kullanılabilirlik bölgeler içinde ve arasında bir birleşimi tarafından bu faktörlerin yönlendirebilir:

- RPO gereksinimi = 0 bir Azure bölgesi içinde.
- Kuruluş işleyemeyeceği ya da daha büyük bir bölgeyi etkileyen bir ana doğal felaketler tarafından etkilenen genel işlemler mümkün değildir. Bu, geçtiğimiz birkaç yılda Karayipler'deki isabet bazı kasırgalar durum oluştu.
- İsteğe bağlı açıkça hangi Azure kullanılabilirlik alanları sağlayabilir birincil ve ikincil siteler arasında uzaklıkları düzenlemelerine.

Bu durumlarda, hangi SAP aramaları ayarlayabileceğiniz bir [SAP HANA çok katmanlı sistem çoğaltma yapılandırması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) HANA sistem çoğaltması kullanarak. Mimarisi gibi görünür:

![Üç sanal makineye sahip iki bölgeleri diyagramı](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Sunulan SAP [birden çok hedef sistem çoğaltma](https://help.sap.com/viewer/42668af650f84f9384a3337bcd373692/2.0.03/en-US/0b2c70836865414a8c65463180d18fec.html) HANA 2.0 SPS3 ile. Birden çok hedef sistem çoğaltma güncelleştirme senaryolarda bazı avantajları getirir. Örneğin, Bakım veya güncelleştirmeler için aşağı HA ikincil olduğunda, DR sitesi (bölge 2) etkilenmez. HANA'ya çok hedef sistem çoğaltma hakkında daha fazla bilgi bulabilirsiniz [burada](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/ba457510958241889a459e606bbcf3d3.html).
Birden çok hedef çoğaltma ile olası mimari gibi görünür:

![Üç sanal makineye sahip iki bölgeleri milti-hedef diyagramı](./media/sap-hana-availability-two-region/saphanaavailability_hana_system_2region_HA_and_DR_multitarget_3VMs.PNG)

Kuruluşta second(DR) Azure bölgesi içinde yüksek kullanılabilirlik hazırlık gereksinimleri varsa, mimarisi gibi görünür:

![Üç sanal makineye sahip iki bölgeleri milti-hedef diyagramı](./media/sap-hana-availability-two-region/saphanaavailability_hana_system_2region_HA_and_DR_multitarget_4VMs.PNG)


Logreplay işlem modu kullanarak, bu yapılandırma bir RPO sağlar = 0, birincil bölge içinde düşük RTO ile. İkinci bölgeye taşıma söz konusuysa yapılandırma de iyi bir RPO sağlar. Verileri önceden yüklenmişse ikinci bölgedeki RTO zamanları bağımlıdır. Birçok müşteri, bir test sistemini çalıştırmak için ikincil bölge'de VM kullanır. Kullanım örneği, veri kaynağı.

> [!IMPORTANT]
> İşlem modları farklı Katmanlar arasındaki homojen olması gerekir. **Olamaz** logreply işlem modunu Katman 1 ve Katman 2 ve sağlamak için delta_datashipping 3 katmanı olarak kullanın. Yalnızca bir ya da tüm katmanları için tutarlı olması gereken bir işlem modu da seçebilirsiniz. Delta_datashipping bir RPO vermek uygun olmadığından bu tür bir çok katmanlı yapılandırma logreplay kalır tarafından = 0, yalnızca uygun işlem modu. İşlem modları ve bazı kısıtlamalar hakkında daha fazla ayrıntı için SAP bkz [işlem modları için SAP HANA sistem çoğaltması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Sonraki adımlar

Bu yapılandırmalar azure'da ayarlama hakkında adım adım yönergeler için bkz:

- [Azure vm'lerde SAP HANA sistem çoğaltması ayarlama](sap-hana-high-availability.md)
- [SAP hana sistem çoğaltması kullanarak yüksek kullanılabilirlik](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
