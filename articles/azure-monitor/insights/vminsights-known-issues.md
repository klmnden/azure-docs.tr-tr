---
title: VM'ler (Önizleme) bilinen sorunlar için Azure İzleyici | Microsoft Docs
description: Bu makalede, bir çözümde bir sistem durumu, uygulama bağımlılık bulma ve Azure VM işletim sistemi performans izleme bir araya getiren Azure VM'ler için Azure İzleyici ile ilgili bilinen sorunlar ele alınmıştır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2019
ms.author: magoedte
ms.openlocfilehash: 677fec21b7491398da5e4958441e5405e0c10e0e
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745682"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>VM'ler (Önizleme) için Azure İzleyici ile ilgili bilinen sorunlar

Bu makalede, bir çözümde bir sistem durumu, uygulama bileşenleri bulma ve Azure VM işletim sistemi performans izleme bir araya getiren Azure VM'ler için Azure İzleyici ile ilgili bilinen sorunlar ele alınmıştır. 

## <a name="health"></a>Durum 
Aşağıda sistem durumu özelliğinin geçerli sürümle bilinen sorunlar verilmiştir:

- VM özelliği panel Windows Server 2019 işletim sistemi Windows Server 2016 görüntüler. Bu, gelecek sürümlerden birinde düzeltilecektir.
- Bir Azure VM kaldırılması veya silinmesi durumunda süre için VM liste görünümünde görüntülenir. Ayrıca, kaldırıldı veya silinmiş bir VM'nin durumunu'ı tıklatarak açılır **sistem tanılama** görüntüleyin ve sonra bir yükleme döngüsü başlatır. Silinen sanal Makinenin adını seçerek, bir VM silinip silinmediğini belirten ileti ile bir bölme açılır.
- Portalı veya iş yükü İzleyicisi API bunları hemen güncelleştirebilir olsa bile bir eşiği güncelleştirme gibi yapılandırma değişiklikleri, 30 dakika kadar yararlanın. 
- Sistem durumu tanılama güncelleştirmeleri diğer görünümlerle daha hızlı karşılaşırsınız. Bunlar arasında geçiş yaptığınızda bilgileri gecikebilir. 
- Vm'lerini kapatılıyor, güncelleştirmeleri durumu ölçütlerini bazıları *kritik* ve başkalarına *sağlıklı*. Net VM durumu olarak görüntülenen *kritik*.
- Linux VM'ler için tek bir VM görünüm durumu ölçütlerini listeleme sayfanın başlığını kullanıcı tanımlı VM adı yerine bir VM'nin tüm etki alanı adı vardır. 
- Desteklenen yöntemlerden birini kullanarak bir VM için izlemeyi devre dışı bırakın ve yeniden dağıtmayı deneyin sonra aynı çalışma alanında dağıtmanız gerekir. Farklı bir çalışma alanı ve bu VM için sistem durumunu görüntülemek için try seçerseniz, tutarsız davranış gösterebilir.
- Windows için toplam CPU kullanımı durumu ölçütü gösterir bir eşik *eşit değil* **4**anlamı CPU kullanımı % 95'den büyük ve sistem sırası uzunluğu 15'ten büyükse. Bu sistem durumu ölçütü Bu önizleme sürümünde yapılandırılabilir değildir.  
- Çözüm bileşenleri çalışma alanınızdan kaldırdıktan sonra Azure sanal makinelerinize sistem durumunu görmek devam edebilirsiniz; portalında ya da görünümüne gidin, özellikle performans ve harita verileri. Veri, performans ve harita görünümü süre sonra görünen sonunda durur; ancak durum görünümün Vm'leriniz için sistem durumu göstermeye devam eder. **Şimdi deneyin** seçeneği sunulacaktır re ekleme yalnızca performans ve harita görünümleri.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi izlemeyi etkinleştirme yöntemlerini ve gereksinimleri hakkında bilgilere [VM'ler için Azure İzleyici'ı Dağıtma](vminsights-onboard.md).
