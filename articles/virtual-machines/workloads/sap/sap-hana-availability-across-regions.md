---
title: "SAP HANA kullanılabilirlik Azure bölgeler arasında | Microsoft Docs"
description: "SPA HANA Azure Vm'lerinde çalıştırırken kullanılabilirlik dikkate alınacak noktaları genel bakış açıklanmaktadır"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 052394884f85d527caa88ff6c9464af669f47bb5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-across-azure-regions"></a>SAP HANA kullanılabilirlik Azure bölgeler arasında
Bu makalede SAP HANA kullanılabilirlik farklı Azure bölgeler arasında çevresinde senaryoları açıklanmıştır ve ele alınan. Ayrı Azure bölgeleri aralarındaki büyük uzaklığı sahip olgu göz önüne alındığında, bu makalede listelenen özel noktalar vardır.

## <a name="motivation-to-deploy-across-multiple-azure-regions"></a>Motivasyon birçok Azure bölgesinde dağıtmak için
Farklı Azure bölgeleri daha büyük bir uzaklık tarafından ayrılır. Coğrafi bölge bağımlı, bu mil yüzlerce ya da hatta birkaç bin mil, gibi Amerika Birleşik Devletleri'nde olabilir. Farklı Azure bölgeler arasındaki uzaklığı nedeniyle iki farklı Azure bölgeleri deneyimleri önemli ölçüde ağ gidiş dönüş gecikme dağıtılan varlıklar arasındaki trafiği ağ. Yeterince önemli tipik SAP iş yükü altında iki SAP HANA örnekleri arasında zaman uyumlu veri değişimi dışlanacak. Diğer tarafta, genellikle daha geniş bir alan basarsa doğal afet durumunda kullanılabilirlik sağlamak için tanımlanmış gereksinimi birincil veri merkeziniz ile ikincil veri merkezine arasındaki uzaklığı temel olduğunu olgu ile karşı karşıya kalmaktadır. Eylül ve Ekim 2017 vuruş Karayipler ve İzmir'alanı kasırgalar gibi. Veya en az bir en az uzaklığı gereksinimi. Çoğu müşteri örneklerinin bu en az uzaklığı tanımı içinde kullanılabilirliği için tasarım gerektirir [Azure bölgeleri](https://azure.microsoft.com/regions/). Uzaklığı arasında çok büyük olduğundan HANA zaman uyumlu çoğaltma modu, RTO ve RPO gereksinimlere kullanmak için iki Azure bölgeleri kullanılabilirlik yapılandırmaları bir bölge içinde dağıtmanıza ve ikinci bir ek dağıtımlarında ek niteliğindedir zorlayabilir bölge.

Bu yapılandırmalar olarak kabul edilmesi için bir diğer unsuru yük devretme ve istemci yeniden yönlendirme ' dir. İki farklı Azure bölgelerinde her zaman SAP HANA örnekleri arasında bir yük devretme el ile bir yük devretme olduğu varsayılır. SAP HANA sistem çoğaltma çoğaltma modu zaman uyumsuz ayarlanmış olduğundan, birincil HANA örneğinde kaydedilen veri, henüz ikincil HANA örneğine yaptığını olmayan olasılığına yoktur. Bu nedenle otomatik yük devretme çoğaltması zaman uyumsuz olduğu yapılandırmaları için kabul edilemez. Bir yük devretme alıştırma olduğu gibi el ile Denetlenen yük devretme ile bile birincil tarafındaki kaydedilmiş tüm verileri ikincil örneğine etmeden el ile başka bir Azure bölgesine yapılan olduğundan emin olmak için önlemleri gerekir.
 
Bu yana ikinci Azure bölgesi, SAP HANA istemcileri ya da yapılandırmalarında değiştirilmesi gereken veya adımları ad çözümlemesi değiştirmek için yerleştirdiniz gereken şekilde daha iyi dağıtılan Azure vnet'lerdeki farklı bir IP adres aralığı ile çalışır. Bu nedenle, istemciler yeni yönlendirildiğini sunucusu IP adresi ikincil. SAP makaleyi [devralma sonra istemci bağlantısı kurtarma](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html) daha fazla ayrıntı gider.   

## <a name="simple-availability-between-two-azure-regions"></a>İki Azure bölgeler arasında basit kullanılabilirliği
Bu senaryoda, herhangi bir kullanılabilirlik yapılandırma tek bir bölge içinde yararlanılmasını değil karar vermiştir. Ancak, bir olağanüstü durumda sunulan iş yükü için isteğe bağlı olacaktır. Tipik örnekleridir sistemler için üretim dışı sistemleridir ister. Yarım gün veya hatta bir gün için aşağı sistem olacak şekilde karşılayabilir olsa, 48 saat veya daha fazla kullanılabilir olmaması sisteme izin veremez. Daha az kurulumunu yapmak için maliyetli, hatta daha az önemli nleri başka bir sistemi hedef olarak işlevleri VM çalıştırdığınız veya ikincil bölge küçük VM'nin boyutunu ve verileri önceden yüklememeye seçin. Yük devretme el ile olacağını ve yük devretme de tam uygulama yığınını pek çok daha fazla adımları kapsar karşıladığından VM getirmek, yeniden boyutlandırabilir ve VM yeniden başlatmak için ek süre kabul edilebilir.

> [!NOTE]
> Bile veri ön yük HANA sistem çoğaltma hedefi, en az 64 GB bellek gerekir ve hedef örneği bellekte rowstore verileri tutmak için yeterli belleğin dışında.

![İki bölgede üzerinden iki VM](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> Bu yapılandırmada, bir RPO sağlayamaz HANA sistem çoğaltma modu zaman uyumsuz olduğundan = 0. Bir RPO sağlamak ihtiyacınız varsa = 0, bu yapılandırma seçeneği yapılandırması değil.

Küçük değişiklik yapılandırmasında veri ön yükleme yapılandırmak için olabilir. Ancak el ile niteliği yük devretme ve uygulama katmanları taşımak için de bu ikinci bölgeye değil kolaylaştırır algılama verilerini önceden yüklemek için gereken olgu verilir. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Bir bölge içinde ve bölgeler arasında kullanılabilirlik birleştirin 
Kullanılabilirlik içinde ve bölgeler arasında birleşimi tarafından yönlendirilen:

- RPO gereksinimini = 0 içinde bir Azure bölgesi.
- İstekli ya da etkilenmesini şirket genel işlemlerini olması için önemli bir doğal catastrophe tarafından daha büyük bir bölge etkiler. Son birkaç yıl içinde Karayipler isabet bazı kasırgalar tarafından durumda olduğu gibi.
- İsteğe bağlı açıkça hangi Azure kullanılabilirlik bölgeleri olan birincil ve ikincil site arasında uzaklıklar düzenlemelere sağlayabilir.

 
Böyle durumlarda, hangi SAP çağrıları yapılandırmanız gerekir. bir [SAP HANA Multitier sistem çoğaltma yapılandırması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) HANA sistem çoğaltma ile. Mimarisi gibi görünür:

![iki bölgede üzerinden üç VM'ler](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Bu yapılandırma, bir RPO sağlar = 0 küçük RTO süreler birincil bölge içinde ve ayrıca sağlar RPO Düzen ikinci bölgesine taşıma durumunda üzerinden. İkinci bölgede RTO kez olup veri ön yükleme veya yapılandırdığınız bağımlı olur. Çoğu müşteri durumda ikincil bölge VM'yi test sistemini çalıştırmak için kullanılır. Bu kullanım sonucunda verileri önceden yüklenemiyor.

> [!NOTE]
> Logreplay çalışma modu HANA sistem Katman 2 (zaman uyumlu çoğaltma birincil bölgede) Katman 1'den giden çoğaltma için kullandığınız olduğundan, Katman 2 ve Katman 3 (ikincil siteye çoğaltma) arasında çoğaltma delta_datashipping işleminde olamaz modu. İşlem modları ve bazı kısıtlamalar ayrıntılarını SAP içinde tarafından belgelenmiştir [SAP HANA sistem çoğaltma işlemi modlarını](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Sonraki adımlar
Bu tür bir yapılandırma Azure ayarlama konusunda adım adım yönergeler gerekiyorsa, makaleleri okuyun:

- [SAP HANA sistem Azure VM'ler çoğaltmasında Kurulumu](sap-hana-high-availability.md)
- [SAP HANA sistemi çoğaltması kullanmak için Azure – bölümü 4 – yüksek kullanılabilirliğine, SAP](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
