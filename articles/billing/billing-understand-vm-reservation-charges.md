---
title: Azure ayrılmış VM örnekleri indirim anlama | Microsoft Docs
description: Azure ayrılmış VM örneği indirim çalışan sanal makinelere nasıl uygulanacağını öğrenin.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: banders
ms.openlocfilehash: c8994041d525186dd55bb4f4475f7e68036454f6
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649838"
---
# <a name="understand-how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Azure ayırma indirimi sanal makinelerine nasıl uygulanacağını anlama

Azure ayrılmış sanal makine örneği satın aldığınız sonra ayırma indirimini öznitelikleri ve ayırma miktarı ile eşleşen sanal makinelere otomatik olarak uygulanır. Rezervasyon sanal makinelerinizi işlem maliyetini kapsar.

SQL veritabanı ayrılmış kapasite için bkz: [Azure ayrılmış örnekler anlamak indirim](billing-understand-reservation-charges.md).

Ayrılmış VM örneği satın aldıktan sonra sanal makineniz için maliyetleri aşağıdaki tabloda gösterilmektedir. Her durumda, depolama ve ağ için ücretlendirildiğiniz normal fiyatlar.

| Sanal makine türü  | Ayrılmış VM örneği ile ücretleri |
|-----------------------|--------------------------------------------|
|Ek yazılım olmadan Linux Vm'leri | VM altyapı maliyetlerinizi ayırma kapsar.|
|Yazılım ücretleri (örneğin, Red Hat) ile Linux Vm'leri | Ayırma, altyapı maliyetlerini kapsar. Ek yazılım için ücret ödersiniz.|
|Ek yazılım olmadan Windows Vm'leri |Ayırma, altyapı maliyetlerini kapsar. Windows yazılım için ücret ödersiniz.|
|Ek yazılım (örneğin, SQL server) ile Windows Vm'leri | Ayırma, altyapı maliyetlerini kapsar. Windows yazılım ve ek yazılım için ücret ödersiniz.|
|İle Windows Vm'leri [Azure hibrit avantajı](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Ayırma, altyapı maliyetlerini kapsar. Windows yazılım maliyetleri, Azure hibrit avantajı tarafından ele alınmaktadır. Herhangi bir ek yazılım ayrı olarak ücretlendirilir.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Ayırma indirimi uygulamasının Windows olmayan Vm'leri için

 Azure ayırma indirimi, saatlik olarak VM örnekleri çalıştırmak için uygulanır. Rezervasyon satın aldığınız ayırma indirimini uygulamak için çalışan VM'ler tarafından yayılan kullanım eşleştirilir. Tam bir saat çalışmayabilir VM'ler için Vm'leri eşzamanlı olarak çalışan dahil olmak üzere, bir ayırma kullanmayan diğer Vm'lerden ayırma doldurulur. Saatin sonunda, VM'ler için ayırma uygulama bir saat içinde kilitli. Bir VM için bir saat çalışmaz veya saat içinde eşzamanlı VM ayırma saatlik doldurmayın durumunda, ayırma için o saat potansiyelinden az kullanılmasına neden. Aşağıdaki grafikte, Faturalanabilir VM kullanımı için ayırma uygulamasını gösterir. Çizim, bir rezervasyon satın alma ve iki eşleşen VM örnekleri üzerinde temel alır.

![Ekran görüntüsü uygulanan bir ayırma ve iki sanal makine örnekleri eşleştirme](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Ayırma satırın üzerinde tüm kullanımlar, normal Kullandıkça Öde fiyatları üzerinden ücret. Rezervasyon satın alma işleminin bir parçası olarak önceden ödenen sonra ayırmalar satırın altına herhangi bir kullanım ücreti olmaz.
2. 1 saat, örneği 1 saatlerini 0.75 çalışır ve örneği 2 0,5 saat boyunca çalışır. 1 saat için toplam kullanım 1,25 saattir. Kalan 0,25 saatliğine Kullandıkça Öde fiyatları üzerinden ücret ödersiniz.
3. Saat 2 ve 3 saat için her iki örnek 1 saat boyunca her çalıştı. Bir örnek tarafından ayırma kapsamındadır ve diğer Kullandıkça Öde fiyatları üzerinden ücretlendirilir.
4. 4 saat, örnek 1 0,5 saat çalışır ve örnek 2, 1 saat boyunca çalıştırılır. Örnek 1 ayırma tarafından tam olarak ele alınmıştır ve örnek 2 0,5 saat ele alınmaktadır. Kullandıkça Öde fiyatı kalan 0,5 saat için ücret ödersiniz.

Anlamak ve kullanım raporları faturalama Azure Ayırmalarınızın uygulamayı görüntülemek için bkz: [ayırma kullanımını anlamak](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Ayırma indirimi, bir uygulama için Windows Vm'leri

Windows VM örneklerini çalıştırırken ayırma altyapı maliyetlerini karşılamak için uygulanır. VM altyapı maliyetleri için Windows Vm'leri için ayırma uygulamasının Windows olmayan Vm'leri için aynıdır. Windows yazılım vCPU başına temelinde için ayrı olarak ücret ödersiniz. Bkz: [ayırmaları Windows yazılım maliyetleri](https://go.microsoft.com/fwlink/?linkid=862756). Lisans maliyetlerini ile Windows ele [Windows Server için Azure hibrit avantajı](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing).

## <a name="discount-applies-to-different-sizes-with-instance-size-flexibility"></a>İndirim için farklı boyutlarda örneği boyutu esnekliği ile geçerlidir.

Ne zaman satın aldığınız ayrılmış VM örneği, seçerseniz **için en iyi duruma getirilmiş**: **örnek boyutu esneklik**, indirim kapsamı seçtiğiniz VM boyutuna bağlıdır. Ayırma aynı boyutu seri grubu içindeki sanal makineler (VM) boyutlarına uygulayabilirsiniz. Daha fazla bilgi için [ayrılmış VM örnekleri ile sanal makine boyutu esneklik](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure için ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure için ayırmaları yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için ayırma kullanımını anlama](https://docs.microsoft.com/partner-center/azure-reservations)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
