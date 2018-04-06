---
title: SAP HANA kullanılabilirlik Azure bölgeler arasında | Microsoft Docs
description: SAP HANA birden çok Azure bölgelerindeki Azure vm'lerinde çalıştırırken kullanılabilirlik konuları genel bakış.
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
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: edbd1885dd529e4ccd38f2012d56865a2147f64d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="sap-hana-availability-across-azure-regions"></a>SAP HANA kullanılabilirlik Azure bölgeler arasında

Bu makalede, SAP HANA kullanılabilirlik farklı Azure bölgeler arasında ilgili senaryolar açıklanmaktadır. Azure bölgeleri arasındaki uzaklığı nedeniyle birden çok Azure bölgelerinde SAP HANA kullanılabilirliğini ayarı özel konular içerir.

## <a name="why-deploy-across-multiple-azure-regions"></a>Neden birçok Azure bölgesinde dağıtma

Azure bölgeleri genellikle büyük uzaklıklar tarafından ayrılır. Coğrafi bölge bağlı olarak Azure bölgeleri arasındaki uzaklığı mil yüzlerce ya da hatta birkaç bin mil, gibi Amerika Birleşik Devletleri'nde olabilir. İki farklı Azure bölgelerinde dağıtılan varlıklar arasındaki ağ trafiğini uzaklığı nedeniyle önemli ölçüde ağ gidiş dönüş gecikme süresi yaşar. Gecikme tipik SAP iş yükleri altında iki SAP HANA örnekleri arasında zaman uyumlu veri değişimi hariç tutmak çok önemlidir. 

Diğer taraftan, kuruluşlar çoğunlukla uzaklığı birincil veri merkezindeki konumu ile ikincil veri merkezine arasında gereksinim. Bir uzaklık gereksinim doğal afet daha geniş bir coğrafi konumda oluşursa kullanılabilirliğini sağlamaya yardımcı olur. Örnek Karayipler ve İzmir'Eylül ve Ekim 2017 isabet kasırgalar verilebilir. Kuruluşunuz, en az bir en az uzaklığı gereksinim olabilir. Çoğu Azure müşteriler için en az uzaklığı tanımı içinde kullanılabilirliği için tasarım gerektirir [Azure bölgeleri](https://azure.microsoft.com/regions/). İki Azure bölgeleri arasındaki uzaklığı HANA zaman uyumlu çoğaltma modu kullanmak için çok büyük olduğundan RTO ve RPO gereksinimleri, bir bölgede kullanılabilirlik yapılandırmaları dağıtmak ve ikinci bir ek dağıtımlarında'nı tamamlamak için zorlayabilir bölge.

Bu senaryoda, dikkate alınması gereken bir diğer unsuru yük devretme olduğundan ve istemci yönlendirin. İki farklı Azure bölgelerinde her zaman SAP HANA örnekleri arasında bir yük devretme el ile bir yük devretme olduğu varsayılır. SAP HANA sistem çoğaltma çoğaltma modu zaman uyumsuz ayarlandığından, birincil HANA örneğinde kaydedilen verileri henüz bu ikincil HANA örneğine yaptığını kurmadı olası yoktur. Bu nedenle, otomatik yük devretme, çoğaltma zaman uyumsuz olduğu yapılandırmaları için bir seçenek değildir. Bir yük devretme alıştırma olduğu gibi el ile Denetlenen yük devretme ile bile başka bir Azure bölgesine el ile taşımanız önce birincil tarafındaki kaydedilmiş tüm veriler bu ikincil örneğine yaptığını sağlamak için ölçümleri gerçekleştirmeniz gerekir.
 
Azure sanal ağı, farklı bir IP adresi aralığı kullanır. IP adreslerini ikinci Azure bölgesinde dağıtılır. Bu nedenle, ya da SAP HANA istemci yapılandırmasını değiştirmek ihtiyacınız veya tercihen, ad çözümlemesi değiştirme adımları oluşturmanız gerekir. Bu şekilde, istemciler yeni ikincil sitenin sunucu IP adresine yönlendirilir. Daha fazla bilgi için SAP makalesine bakın [devralma sonra istemci bağlantısı kurtarma](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html).   

## <a name="simple-availability-between-two-azure-regions"></a>İki Azure bölgeler arasında basit kullanılabilirliği

Değil tek bir bölge içinde herhangi bir kullanılabilirlik yapılandırma yararlanılmasını, ancak hala bir olağanüstü durum gerçekleşirse sunulan iş yükü için isteğe bağlı olması da tercih edebilirsiniz. Bu gibi sistemler için tipik örnekleridir üretim dışı sistemleridir. Yarım gün veya hatta bir gün için aşağı sistem sahip sürdürülebilir olsa da, sistem 48 saat veya daha fazla bilgi için kullanılamaz hale gelmesine izin veremez. Kurulum daha az maliyetli hale getirmek için VM'yi bile daha az önemli olan başka bir sisteme çalıştırın. Başka bir sistemi hedef olarak çalışır. Daha küçük olacak şekilde ikincil bölge ' VM boyutu ve verileri önceden değil seçin. Yük devretme el ile gerçekleştirilir ve yük devretme tam uygulama yığını çok daha fazla adımları kapsar olduğundan VM kapatma, yeniden boyutlandırabilir ve VM'yi yeniden başlatmak için ek süre kabul edilebilir.

> [!NOTE]
> Veri önyükleme HANA sistem çoğaltma hedefi kullanmasanız bile, en az 64 GB bellek gerekir. Yeterli bellek hedef örneği bellekte rowstore verileri tutmak için 64 GB yanı sıra da gerekir.

![İki bölgede üzerinden iki VM diyagramı](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> Bu yapılandırmada, bir RPO sağlayamaz HANA sistem çoğaltma modu zaman uyumsuz olduğundan = 0. Bir RPO sağlamak ihtiyacınız varsa = 0, bu yapılandırma seçeneği yapılandırmasını değil.

Yapılandırmada yapabilen küçük değişiklikler, verileri önceden yükleme olarak yapılandırmak için olabilir. Ancak, yük devretme ve uygulama katmanları de ikinci bölgesine taşımanıza gerek olgu el ile yapısına göz önüne alındığında, bu verileri önceden yüklemek için anlamlı olmayabilir. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Bir bölge içinde ve bölgeler arasında kullanılabilirlik birleştirin 

Kullanılabilirlik içinde ve bölgeler arasında birleşimi, bu unsurlar güdümlü:

- RPO gereksinimi = 0 içinde bir Azure bölgesi.
- Kuruluş istekli veya daha büyük bir bölge etkileyen önemli bir doğal catastrophe tarafından etkilenen genel işlemler mümkün değildir. Bu son birkaç yıl içinde Karayipler isabet bazı kasırgalar durum oluştu.
- İsteğe bağlı açıkça hangi Azure kullanılabilirlik bölgeleri sağlayabilirsiniz birincil ve ikincil siteler arasında uzaklıklar düzenlemelere.

Bu durumlarda, hangi SAP aramaları ayarlayabileceğiniz bir [SAP HANA çok katmanlı sistem çoğaltma yapılandırması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) HANA sistem çoğaltma kullanarak. Mimari şöyle olabilir:

![İki bölgede üzerinden üç VM'ler diyagramı](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Bu yapılandırma, bir RPO sağlar = 0, birincil bölge içinde düşük RTO ile. İkinci bölge için bir taşıma alıyorsa yapılandırma makul RPO de sağlar. İkinci bölgede RTO kez veri önceden yüklenmişse bağımlı olur. Birçok müşteri VM ikincil bölge'de bir test sistemini çalıştırmak için kullanın. Kullanım, veri önceden.

> [!NOTE]
> Kullanmakta olduğunuz çünkü **logreplay** 1 (zaman uyumlu çoğaltma birincil bölge içinde), Katman 2 katmandan Katman 2 ve Katman 3 (ikincil siteye çoğaltma) arasında çoğaltma giderek HANA sistem çoğaltma için işlem modu olamaz **delta_datashipping** işlem modu. SAP makaleyi işlem modları ve bazı sınırlamalar hakkında daha fazla bilgi için bkz [SAP HANA sistem çoğaltma işlemi modlarını](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Sonraki adımlar

Bu yapılandırmalar Azure ayarlama hakkında adım adım yönergeler için bkz:

- [SAP HANA sistem çoğaltma Azure VM'de ayarlama](sap-hana-high-availability.md)
- [Sistem çoğaltma kullanarak SAP HANA için yüksek kullanılabilirlik](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
