---
title: Azure Stack için planlama işlem kapasitesi | Microsoft Docs
description: Azure Stack dağıtımlar için planlama işlem kapasitesi hakkında bilgi edinin.
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
ms.date: 04/03/2019
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 04/03/2019
ms.custom: ''
ms.openlocfilehash: 437e55b1a2907418fe47f418245431fa1c882b80
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58915700"
---
# <a name="azure-stack-compute-capacity-planning"></a>Azure Stack işlem kapasitesi planlama
[Azure Stack üzerinde desteklenen VM boyutları](./user/azure-stack-vm-sizes.md) Azure'da desteklediği bir alt kümesidir. Azure kaynak sınırları boyunca operasyonda ekstra tüketimi kaynakların (yerel ve hizmet düzeyi sunucusu) önlemek için birçok vektörleri uygular. Diğer kiracıların kaynakları overconsume, Kiracı kullanımı için bazı limitler izlenmesi olmadan Kiracı deneyimleri düşer. Sanal makineden ağ çıkışı için Azure sınırlamaları eşleşen bant genişliği sınırlaması Azure Stack'te yerinde vardır. Depolama kaynakları için depolama IOPS limitlerine depolama erişimi için kiracılar tarafından temel operasyonda ekstra tüketimi kaynak önlemek için Azure Stack üzerinde uygulanmıştır.  

## <a name="vm-placement-and-virtual-to-physical-core-overprovisioning"></a>VM yerleştirme ve sanal-fiziksel çekirdek açıdan
Azure Stack'te VM yerleştirme için kullanılacak belirli bir sunucu belirtmek bir kiracı için hiçbir yolu yoktur. VM yerleştirme sırasında yalnızca olup yeterli bellek bu VM türü için konakta noktadır. Azure Stack bellek fazla kullanma değil; Ancak, bir overcommit çekirdek sayısı, izin verilir. Sanal-fiziksel çekirdek açıdan oranı bir faktör olarak var olan konumunda yerleştirme algoritmaları arama olduğundan, her konak farklı bir oran olabilir. 

Azure'da, bir kullanılabilirlik kümesindeki birden çok hata etki alanlarına için sanal makineleri bir çoklu VM üretim sisteminin yüksek kullanılabilirlik elde etmek için yerleştirilir. Azure Stack'te hata etki alanı bir kullanılabilirlik kümesinde tek bir düğüm ölçek birimi olarak tanımlanır.

Azure Stack altyapısını hatalara karşı dayanıklı olsa da, (Yük Devretme Kümelemesi) temel alınan teknoloji hala bazı kapalı kalma süresi VM'ler için etkilenen bir fiziksel sunucuda bir donanım arızası olması durumunda artmasına neden olur. Şu anda Azure Stack Azure ile tutarlı olacak şekilde en fazla üç hata etki alanı ile bir kullanılabilirlik sahip destekler. Vm'leri bir kullanılabilirlik kümesine yerleştirilir bunları mümkün olduğunca eşit olarak birden çok hata etki alanları üzerinde (Azure Stack düğüm) yayarak birbirinden fiziksel olarak izole edilmiş olur. Bir donanım hatası varsa, başarısız hata etki alanı Vm'lerden diğer düğümleri yeniden, ancak, mümkün olduğunda, aynı kullanılabilirlik kümesindeki diğer vm'lerden ayrı hata etki alanlarında tutulur. Donanım tekrar çevrimiçi olduğunda, yüksek kullanılabilirliği sürdürmek için Vm'leri yeniden Dengelenecek.

Yüksek kullanılabilirlik sağlamak için Azure tarafından kullanılan başka bir güncelleme etki alanlarındaki kullanılabilirlik kümeleri şeklinde kavramdır. Güncelleme etki alanı, bakımdan geçirilebilen ya da aynı anda yeniden başlatılabilen bir temel alınan donanım mantıksal grubudur. Azure Stack'te kendi temel konak güncelleştirilmeden önce kümedeki çevrimiçi diğer konaklar arasında geçişi, Vm'leri Canlı. Bir konak güncelleştirme sırasında kapalı kalma süresi olmadan Kiracı olduğundan, Azure Stack'te güncelleştirme etki alanı özelliği yalnızca şablon Azure ile uyumluluk için bulunmaktadır.

## <a name="azure-stack-resiliency-resources"></a>Azure Stack dayanıklılık kaynakları
Düzeltme eki ve güncelleştirme bir Azure Stack için izin vermek için Integrated system, ve fiziksel donanım hatalarına dayanıklı olacak şekilde toplam sunucu belleğinin bir parçasını ayrılmış ve Kiracı sanal makine (VM) yerleştirme için kullanılamaz.

Bir sunucu başarısız olursa, başarısız sunucu üzerinde bulunan VM'ler için VM kullanılabilirlik sağlamak için kalan, kullanılabilir sunuculara yeniden başlatılır. Benzer şekilde, düzeltme eki ve güncelleştirme işlemi sırasında bir sunucu üzerinde çalışan tüm sanal makineler olması Canlı sunulan diğer sunucuya geçirilmiş. Bu sanal makine Yönetimi veya taşıma için yeniden başlatma veya geçiş gerçekleşmesine izin vermek için ayrılmış kapasite ise yalnızca gerçekleştirilebilir.

Kiracı VM yerleştirme için kullanılabilir toplam ve kullanılabilir bellek hesaplama aşağıdaki sonuçlanır. Azure Stack ölçek biriminin tamamı için bu bellek kapasitesidir.

  VM yerleştirme için kullanılabilir bellek VM - Azure Stack altyapısını yükü çalıştırarak kullanılan toplam sunucu belleği – dayanıklılık ayırma – bellek = <sup>1</sup>

  Dayanıklılık ayırma H + R = * ((N-1) * Y) + V * (N-2)

> Konumlar:
> - H = tek sunucu bellek boyutu
> - N = ölçek birimi, boyutu (Sunucuları sayısı)
> - R yükü işletim sistemi için işletim sistemi ayrılmış =<sup>2</sup>
> - V ölçek birimindeki en büyük VM =

  <sup>1</sup> azure Stack altyapısının yükü 230 GB =

  <sup>2</sup> ek yükü için işletim sistemi ayrılmış = %15 düğüm bellek. İşletim sistemi ayrılmış değeri, tahmini bir değerdir ve genel işletim sistemi ek yükü ve sunucu üzerinde fiziksel bellek kapasitesi göre değişiklik gösterir.

Değer V, Ölçek birimindeki en büyük VM dinamik olarak en büyük Kiracı VM bellek boyutunu temel alır. Örneğin, en büyük VM değeri, 7 GB veya 112 GB veya tüm diğer desteklenen sanal makine bellek boyutu Azure Stack çözümde olabilir.

Yukarıdaki hesaplama tahminidir ve Azure Stack geçerli sürüme değiştirmek için konu göre. Dağıtılan çözümün ayrıntılarına bağlı Kiracı sanal makineleri ve Hizmetleri dağıtma özelliğine bağlıdır. Bu örnek bir kılavuz ve Vm'leri dağıtma yeteneği olmayan mutlak sorusunun hesaplamadır.



## <a name="next-steps"></a>Sonraki adımlar
[Depolama kapasitesi planlama](capacity-planning-storage.md)
