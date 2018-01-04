---
title: "Azure ayrılmış sanal makine örnekleri indirim uygulama anlama | Microsoft Docs"
description: "Sanal makineleri çalıştıran için Azure ayrılmış VM örnek indirim nasıl uygulandığı hakkında bilgi edinin."
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: vikdesai
<<<<<<< HEAD
ms.openlocfilehash: 3fd12bd3c51eeef57c896da030a83e447dc3e8ff
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
=======
ms.openlocfilehash: d476380fa841617f7eb914167ebd7d5b8aa611c2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="understand-how-the-reserved-virtual-machine-instance-discount-is-applied"></a>Ayrılmış sanal makine örnek indirim nasıl uygulandığını anlama
Ayrılmış bir VM örnek satın sonra ayırma indirim öznitelikleri ve ayırma miktarı ile eşleşen sanal makinelere otomatik olarak uygulanır. Bir ayırma sanal makinelerinizi altyapı maliyetinden kapsar. Bir ayırma satın aldıktan sonra sanal makine maliyetlerini aşağıdaki tabloda gösterilmektedir. Her durumda, depolama ve ağ aygıtları için ücretlendirilirsiniz normal hızlarında.

| Sanal makine türü  | Ayırma ile ücretleri |    
|-----------------------|--------------------------------------------| 
|Ek yazılım olmadan Linux VM'ler | Ayırma VM altyapı maliyetlerini kapsar.|
|Yazılım giderleri (örneğin, Red Hat) ile Linux VM'ler | Ayırma altyapı maliyetlerini kapsar. Ek yazılım için sizden ücret kesilir.|
|Windows Vm'lerini ek yazılım olmadan |Ayırma altyapı maliyetlerini kapsar. Windows yazılım için sizden ücret kesilir.|
|Ek yazılım (örneğin, SQL server) ile Windows sanal makineleri | Ayırma altyapı maliyetlerini kapsar. Windows yazılım ve diğer yazılım için sizden ücret kesilir.|
|Windows VM ile birlikte [Azure karma avantajı](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | Ayırma altyapı maliyetlerini kapsar. Windows yazılım maliyetleri Azure karma avantajı tarafından ele alınmıştır. Ek yazılım ayrı olarak ücretlendirilir.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Uygulama Windows olmayan VM'ler için ayırma indirim
 Saatlik olarak başka bir VM örnekleri çalıştırmak için ayırma indirim uygulanır. Satın aldığınız ayırmaları ayırma indirimi uygulamak için çalışan VM'ler tarafından gösterilen kullanımı eşleştirilir. Aşağıdaki grafikte Faturalanabilir VM kullanımı için bir ayırma uygulama gösterilmektedir. Çizimde, bir ayırma satın alma ve eşleşen iki VM örneğini dayanır.

![Ayrılmış VM örnek uygulaması](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Ayrılmış VM örnek çizgidir kullanım normal Kullandıkça Öde ücretleri temel alınarak. Ayırma satın alma bir parçası olarak önceden ödenen beri ayrılmış VM örnekleri satırın altındaki tüm kullanım için ücret etmediğinizden.
2.  Saat 1, 1 örneği; 0,75 saat boyunca çalışır ve örnek 2 0,5 saattir çalıştırır. Saat 1 için toplam kullanım 1,25 saattir. Kullandıkça Öde oranları kalan 0,25 saatler için ücretlendirilirsiniz.
3.  Saat 2 ve 3 saat için her iki örnek 1 saat boyunca her verdi. Bir örnek tarafından ayırmasını ele ve diğer Kullandıkça Öde ücretlerinden ücretlendirilir.
4.  4 saat, 1 örneği 0,5 saat çalışır ve örnek 2 1 saat boyunca çalışır. Örnek 1 ayırma tarafından tam olarak ele ve örnek 2 0,5 saatlik kapsanan. Kullandıkça Öde oranı kalan 0,5 saatler için ücret ödersiniz.

Anlamak ve kullanım raporları faturalama, rezervasyonları uygulama görüntülemek için bkz: [anlamak ayrılmış VM örnek kullanım](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Windows VM ayırma indirim uygulamaya
Windows VM örnekleri çalışırken ayırma altyapı maliyetlerini karşılamak üzere uygulanır. VM altyapı maliyetlerini Windows VM'ler için ayırma uygulamasının Windows olmayan VM'ler için aynıdır. Ayrıca, Windows yazılım vCPU başına temelinde ücretlendirilirsiniz. Bkz: [Windows yazılım maliyetleri ayırmaları ile](https://go.microsoft.com/fwlink/?linkid=862756). Windows lisans maliyetleriniz [karma avantajı Windows Server için Azure] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) ile ele.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
