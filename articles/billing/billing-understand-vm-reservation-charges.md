---
title: Azure ayrılmış örnekler indirim anlama | Microsoft Docs
description: Çalışan sanal makinelere Azure ayrılmış VM örnek indirim nasıl uygulandığı hakkında bilgi edinin.
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
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: d1229a49a5bb3bf3198c91a748ed37b7a626c506
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064043"
---
# <a name="understand-how-the-reserved-instance-discount-is-applied"></a>Ayrılmış örnek indirim nasıl uygulandığını anlama
Azure ayrılmış VM örnek satın sonra ayrılmış örnek indirim öznitelikleri ve ayrılmış örnek miktarı ile eşleşen sanal makinelere otomatik olarak uygulanır. Ayrılmış örnek sanal makinelerinizi altyapı maliyetinden kapsar. Ayrılmış örnek satın aldıktan sonra sanal makine maliyetlerini aşağıdaki tabloda gösterilmektedir. Her durumda, depolama ve ağ aygıtları için ücretlendirilirsiniz normal hızlarında.

| Sanal makine türü  | Ayrılmış örnek ile ücretleri |    
|-----------------------|--------------------------------------------|
|Ek yazılım olmadan Linux VM'ler | Ayrılmış örnek VM altyapı maliyetlerini kapsar.|
|Yazılım giderleri (örneğin, Red Hat) ile Linux VM'ler | Ayrılmış örnek altyapı maliyetlerini kapsar. Ek yazılım için sizden ücret kesilir.|
|Windows Vm'lerini ek yazılım olmadan |Ayrılmış örnek altyapı maliyetlerini kapsar. Windows yazılım için sizden ücret kesilir.|
|Ek yazılım (örneğin, SQL server) ile Windows sanal makineleri | Ayrılmış örnek altyapı maliyetlerini kapsar. Windows yazılım ve diğer yazılım için sizden ücret kesilir.|
|Windows VM ile birlikte [Azure karma avantajı](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Ayrılmış örnek altyapı maliyetlerini kapsar. Windows yazılım maliyetleri Azure karma avantajı tarafından ele alınmıştır. Ek yazılım ayrı olarak ücretlendirilir.| 

## <a name="application-of-reserved-instance-discount-to-non-windows-vms"></a>Ayrılmış örnek indirim uygulamasının Windows olmayan VM'ler için
 Saatlik olarak başka bir VM örnekleri çalıştırmak için Azure ayrılmış örnek indirim uygulanır. Satın aldığınız ayrılmış örnekler, ayrılmış örnek indirimi uygulamak için çalışan VM'ler tarafından gösterilen kullanımı eşleştirilir. Tam saat çalışmayabilir VM'ler için ayrılmış örnek Vm'leri eşzamanlı olarak çalışan dahil olmak üzere ayrılmış bir örnek kullanmayan diğer VM'lerin doldurulur. Saat sonunda bir saat içinde ayrılmış örnek uygulama VM'ler için kilitlendi. Bir VM için bir saat çalışmaz veya saat içinde eşzamanlı VM'ler ayrılmış örnek saatlik doldurmayın durumunda, ayrılmış örnek için bu saat gereğinden az. Aşağıdaki grafikte ayrılmış bir örnek uygulama Faturalanabilir VM kullanımını gösterir. Çizimde, bir ayrılmış örnek satın alma ve eşleşen iki VM örneğini dayanır.

![Uygulanan bir ayrılmış örnek ve eşleşen iki VM örneğini ekran görüntüsü](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Ayrılmış örnek satırın üstünde kullanım normal Kullandıkça Öde ücretleri temel alınarak. Ayrılmış örnek satın alma bir parçası olarak önceden ödenen beri ayrılmış örnekler satırın altındaki tüm kullanım için ücret etmediğinizden.
2.  Saat 1, 1 örneği; 0,75 saat boyunca çalışır ve örnek 2 0,5 saattir çalıştırır. Saat 1 için toplam kullanım 1,25 saattir. Kullandıkça Öde oranları kalan 0,25 saatler için ücretlendirilirsiniz.
3.  Saat 2 ve 3 saat için her iki örnek 1 saat boyunca her verdi. Bir örneği tarafından ayrılmış örnek ele ve diğer Kullandıkça Öde ücretlerinden ücretlendirilir.
4.  4 saat, 1 örneği 0,5 saat çalışır ve örnek 2 1 saat boyunca çalışır. Örnek 1 ayrılmış örneği tarafından tam olarak ele ve örnek 2 0,5 saatlik kapsanan. Kullandıkça Öde oranı kalan 0,5 saatler için ücret ödersiniz.

Anlamak ve Azure ayrılmış örneklerinizi uygulama kullanım raporlarını faturalama görüntülemek için bkz: [anlamak ayrılmış örnek kullanım](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reserved-instance-discount-to-windows-vms"></a>Ayrılmış örnek indirim uygulama Windows VM'ler için
Windows VM örnekleri çalıştırırken, ayrılmış örnek altyapı maliyetlerini karşılamak üzere uygulanır. Windows sanal makineleri VM altyapı maliyetlerini ayrılmış örnek uygulamasının Windows olmayan VM'ler için aynıdır. Ayrıca, Windows yazılım vCPU başına temelinde ücretlendirilirsiniz. Bkz: [Windows yazılım maliyetleri ayrılmış örnekler ile](https://go.microsoft.com/fwlink/?linkid=862756). Lisanslama maliyetleri [karma avantajı Windows Server için Azure] ile Windows kapak (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing).

## <a name="next-steps"></a>Sonraki adımlar
Ayrılmış örnekler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayrılmış VM örnekleri nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure'da ayrılmış örnekler yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](billing-understand-reserved-instance-usage.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](billing-understand-reserved-instance-usage-ea.md)
- [Ayrılmış örnek kullanım CSP aboneliklerinin anlama](https://docs.microsoft.com/partner-center/azure-reservations)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)


## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
