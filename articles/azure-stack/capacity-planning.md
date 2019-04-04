---
title: Kapasite planlaması için Azure Stack | Microsoft Docs
description: Kapasite planlaması için Azure Stack dağıtımları hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2019
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 03/29/2019
ms.openlocfilehash: e4678b445dce5b337fb7d51e1b938adb944b4440
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648733"
---
# <a name="azure-stack-capacity-planning"></a>Azure Stack kapasite planlaması
Azure Stack çözümünü değerlendirirken, genel ve Azure Stack bulut kapasite üzerinde doğrudan etkisi donanım yapılandırma seçeneğiniz vardır. Bunlar, CPU, bellek yoğunluğu, depolama yapılandırması ve genel çözüm ölçek veya sunucu sayısını Klasik seçimlerdir. Geleneksel bir sanallaştırma çözümü, kullanılabilir kapasitesini belirlemek için bu bileşenlerin basit aritmetik geçerli değildir. Azure Stack altyapısını veya yönetim bileşenleri çözüm içinde barındırmak için geliştirilmiştir ilk neden olmasıdır. Çözümün kapasite bazıları ayrılır, dayanıklılık desteklemek üzere ikinci sebebi; Kiracı iş yüklerini bir kesintiyi en aza indirmek için bir yol çözümün yazılım güncelleştiriliyor.

> [!IMPORTANT]
> Sağlanan kapasite planlama bilgileri ve eşlik eden elektronik, yalnızca Azure Stack planlama yapmanıza yardımcı olacak bir kılavuz ve yapılandırma kararları tasarlanmıştır. Araştırma ve analiz için bir alternatif olarak hizmet verecek amaçlanmamıştır. 

## <a name="compute-and-storage-capacity-planning"></a>İşlem ve depolama kapasitesi planlama
Azure Stack çözümünü hiper yakınsanmış küme bilgi işlem, ağ ve depolama olarak oluşturulmuştur. Bu etkin kullanımına izin verir veya kümedeki tüm donanım kapasitesi paylaşımı bir ölçek birimi olarak Azure Stack için kullanılabilirlik ve ölçeklenebilirlik sağlayan bir şekilde göstermektedir. Tüm altyapı yazılım, bir dizi sanal makineleri (VM'ler) içinde barındırıldığı ve Kiracı Vm'leri olarak aynı fiziksel sunucuların paylaşır. Tüm VM'ler, daha sonra ölçek biriminin Windows Server küme teknolojileri ve bağımsız Hyper-V örnekleri tarafından yönetilir. Bu yaklaşım, alma ve Azure Stack çözümünün yönetimini basitleştirir ve tüm hizmetleri (Kiracı ve altyapı) ölçeklenebilirliğini ve taşıma için ölçek birimi tamamen ortaya koymaktadır.

Azure Stack çözümünü aşırı sağlanmış yalnızca fiziksel kaynak sunucu bellektir. Diğer kaynakları, CPU çekirdekleri, ağ bant genişliği ve depolama kapasitesi, kullanılabilir kaynakları en iyi kullanılmasını sağlamak için overprovisioned. Bir çözüm için mevcut kapasiteyi hesaplama sırasında fiziksel sunucu belleği ana katkıda bulunur. Diğer kaynakların kullanımını, açıdan oranı mümkündür'ı Anlama ve hangi hedeflenen iş yükü için kabul edilebilir olacağını ' dir.

Yaklaşık 30 VM'ler, Azure yığını'nın altyapı barındırmak ve toplam olarak, 230 GB bellek ve 140 sanal çekirdek yaklaşık kullanmak için kullanılır. Bu VM sayısı için gerekçe, güvenlik, ölçeklenebilirlik, Bakım ve düzeltme eki gereksinimlerini karşılamak için gerekli hizmet ayrımı karşılamak sağlamaktır. Bunlar geliştirilen gibi bu iç hizmet yapısı yeni altyapı hizmetleri için gelecekteki giriş sağlar.

Otomatik güncelleştirmeyi tüm fiziksel sunucu ve altyapı yazılım bileşenlerini veya düzeltme eki ve güncelleştirme, altyapı ve kullanıcı VM'i desteklemek için yerleşimi ölçek birimi, tüm bellek kaynaklarını tüketir değil. Toplam miktarı azure'daki tüm sunucularda bir ölçek birimi, çözümün dayanıklılık gereksinimlerini desteklemek üzere ayrılmamış olacaktır. Fiziksel sunucu Windows Server görüntüsü güncelleştirildiğinde, sunucunun Windows Server görüntülerini güncelleştirildiği sırada Örneğin, sunucu üzerinde barındırılan sanal makineler başka bir yerde içinde ölçek birimi taşınır. Güncelleştirme tamamlandığında, sunucu yeniden ve iş yüklerini hizmet vermek için döndürdü. Hedef Azure Stack çözümünün güncelleştirme ve düzeltme eki için barındırılan sanal makineleri durdurmak için gereken kaçınmaktır. Bu hedefe ulaşmak da en az bir sunucunun bellek kapasitesi izin vermek için sanal makineler ölçek birimi içinde hareketini ayrılmamış. Bu yerleştirme ve taşıma altyapı Vm'lere hem kullanıcı ya da Kiracı Azure Stack çözümün adına oluşturulan sanal makineler için geçerlidir. Değişen bellek gereksinimleri sanal makineniz olduğunda gereken VM taşıma desteklemek için ayrılmış bellek miktarı nedeniyle yerleştirme zorlukları tek bir sunucunun kapasitesinden daha fazla olacak şekilde son uygulama sonuçları olur. Bellek kullanımı kategorisinde ek yük Windows Server örneğinin olmasıdır. Her sunucu için temel işletim sistemi örneği, işletim sistemini ve Hyper-V tarafından barındırılan sanal makinelerin her yönetmede kullanılan belleğin yanı sıra kendi sanal belleği tabloları için bellek tüketir.

VM kapasitesi kullanılabilir belleğe göre belirlenir. Sanal çekirdek fiziksel çekirdek oranları için sürece çözümü daha fazla sayıda fiziksel çekirdek (başka bir CPU seçilir) ile oluşturulmuş olan VM yoğunluğunu kullanılabilir CPU kapasitesi nasıl değiştireceğini exemplify. Aynı depolama kapasitesi ve depolama önbellek kapasitesi geçerlidir.

VM yoğunluğunu yukarıdaki örnekleri yalnızca açıklama amaçlıdır. Destek hesaplamalarda başka karmaşıklık yoktur. Daha fazla kapasite planlaması seçimleri ve elde edilen ve kullanılabilir kapasiteyi anlamak için Microsoft ile iletişim veya çözüm iş ortağı gereklidir.

Bu bölümün geri kalanında, işlem ve depolama için Azure Stack dağıtım gereksinimleri açıklanmaktadır. Hangi bileşenleri gereklidir anlama temel ve en düşük yapılandırma değerlerini sağlamak için bu bilgileri kullanın. Ek bilgi kullanılabilir kapasiteyi ve olası sınırları Kiracı kapasite ve performans yeteneği onaylamaz sistemin bakımından çözümü nasıl yapılandırıldığını tanımlamak için de sağlanır.

> [!NOTE]
> Yalnızca genel VIP boyutu yapılandırılabilir olduğu gibi ağ iletişimi için kapasite planlama gereksinimleri düşüktür. Azure Stack için daha fazla genel IP adresleri ekleme hakkında daha fazla bilgi için bkz. [genel IP adresleri ekleme](azure-stack-add-ips.md).


## <a name="next-steps"></a>Sonraki adımlar
[İşlem kapasitesini planlama](capacity-planning-compute.md)
