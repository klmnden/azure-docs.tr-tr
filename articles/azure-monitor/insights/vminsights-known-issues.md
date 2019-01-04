---
title: VM'ler (Önizleme) bilinen sorunlar için Azure İzleyici | Microsoft Docs
description: Bu makalede, bir çözümde bir sistem durumu ve performans izlemesi Azure VM'nin işletim sistemini bir araya getiren Azure VM'ler için Azure İzleyici ile ilgili bilinen sorunlar ele alınmıştır. VM'ler için Azure İzleyici, ayrıca otomatik olarak uygulama bileşenleri ve bağımlılıkları diğer kaynaklarla bulur ve bunların arasındaki iletişimi eşler.
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
ms.date: 12/26/2018
ms.author: magoedte
ms.openlocfilehash: c329e1fa80c6647bb78b11917ecd012461e62ea4
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790512"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>VM'ler (Önizleme) için Azure İzleyici ile ilgili bilinen sorunlar

Bu makalede, bir çözümde bir sistem durumu ve performans izlemesi Azure VM'nin işletim sistemini bir araya getiren Azure VM'ler için Azure İzleyici ile ilgili bilinen sorunlar ele alınmıştır. 

Aşağıdaki sistem durumu özelliği ile bilinen sorunlar verilmiştir:

- Log Analytics çalışma alanınıza bağlı tüm sanal makineler için sistem durumu özelliği etkin. Bu nedenle bile eylemi ne zaman başlar ve tek bir VM'de sona erer budur.
- Linux VM'ler için tek bir VM görünüm durumu ölçütlerini listeleme sayfanın başlığını kullanıcı tanımlı VM adı yerine bir VM'nin tüm etki alanı adı vardır.  
- Sonra desteklenen yöntemleri kullanarak bir VM için izlemeyi devre dışı bırakın ve yeniden dağıtmayı deneyin, aynı çalışma alanında dağıtmanız gerekir. Bu VM için sistem durumunu görüntülemek için yeni çalışma alanı ve deneyin kullanırsanız, anormal davranış gösterebilir.
- Bir Azure VM kaldırılması veya silinmesi durumunda süre için VM liste görünümünde görüntülenir. Ayrıca, kaldırıldı veya silinmiş bir VM'nin durumunu'ı tıklatarak açılır **sistem tanılama** görüntüleyin ve sonra bir yükleme döngüsü başlatır. Silinen sanal Makinenin adını seçerek, bir VM silinip silinmediğini belirten ileti ile bir bölme açılır.
- Bu sürümle birlikte, zaman aralığı ve sıklığı durumu ölçütlerini değiştirilemez. 
- Sistem durumu ölçütlerini devre dışı bırakılamaz. 
- Dağıtımdan sonra veri görüntülemeden önce zaman alabilir **Azure İzleyici** > **sanal makineler** > **sistem durumu** bölmesinde veya  **VM kaynak** > **Insights** bölmesi.
- Sistem durumu tanılama güncelleştirmeleri daha hızlı herhangi bir görünüm deneyimi. Görünümler arasında geçiş yaptığınızda bilgileri gecikebilir. 
- Azure İzleyici ile sanal makine için dahil edilen uyarılar özeti ile algılanan sistem durumu sorunları sonuçtan Azure Vm'leri izlenen uyarıları görüntüler.
- Vm'lerini kapatılıyor, güncelleştirmeleri durumu ölçütlerini bazıları *kritik* ve başkalarına *sağlıklı*. Net VM durumu olarak görüntülenen *kritik*.
- Sistem durumu uyarı önem derecesi değiştirilemez, bunlar yalnızca devre dışı bırakabilir veya etkinleştirilebilir. Ayrıca, bazı önem dereceleri güncelleştirme durumu ölçütlerini durumuna bağlı.   
- Bir sistem durumu ölçütü örneğinin herhangi bir ayarı değiştirirseniz, VM üzerinde aynı türdeki tüm sistem durumu ölçütlerini örnekleri değiştirilir. Örneğin, mantıksal disk C: karşılık gelen disk boş alan sistem durumu ölçütü örneğinin eşiği değiştirilirse Bu eşik bulunan ve aynı sanal makine için izlenen tüm diğer mantıksal diskleri geçerlidir.  
- Sistem durumlarına ayarlanır çünkü hedef bir Windows VM durumu ölçütlerini eşikleri değiştirilebilir, olmayan *çalıştıran* veya *kullanılabilir*. Sistem sağlığı durumunu sorgulanırken [iş yükü İzleyicisi API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), görüntülediği *comparisonOperator* değerini **LessThan** veya **GreaterThan** ile bir *eşiği* değerini **4** hizmet veya varlık varsa:
   - DNS istemcisi hizmet durumu-hizmet çalışmıyor. 
   - DHCP istemci hizmeti sistem durumu-hizmet çalışmıyor. 
   - RPC hizmet durumu-hizmet çalışmıyor. 
   - Windows Güvenlik Duvarı Hizmet durumu-hizmet çalışmıyor.
   - Windows olay günlüğü hizmet durumu-hizmet çalışmıyor. 
   - Sunucu hizmet durumu-hizmet çalışmıyor. 
   - Windows Uzaktan Yönetimi hizmetinin sistem durumu – hizmet çalışmıyor. 
   - Mantıksal Disk, dosya sistemi hatası veya bozulma – kullanılamıyor.

- Sistem durumu zaten ayarlandığından aşağıdaki Linux durumu ölçütlerini eşikleri değiştirilebilir, olmayan *true*. Sistem durumunu görüntüler *comparisonOperator* bir değerle **LessThan** ve *eşiği* değerini **1** gelen sorgulandığında İş yükü izleme API bağlama bağlı olarak varlığı için:
   - Mantıksal Disk durumu – mantıksal disk değil çevrimiçi / kullanılabilir
   - Disk durumu – Disk değil çevrimiçi / kullanılabilir
   - Ağ bağdaştırıcısı durumu - ağ bağdaştırıcısı devre dışı bırakıldı  

- Portalı veya iş yükü İzleyicisi API bunları hemen güncelleştirebilir olsa bile bir eşiği güncelleştirme gibi yapılandırma değişiklikleri, 30 dakika kadar yararlanın. 
- Tek tek işlemci ve mantıksal işlemci düzeyi durumu ölçütlerini Windows içinde kullanılamaz. Toplam CPU kullanımı yalnızca, Windows VM'ler için kullanılabilir. 
- Her sistem durumu ölçütü için tanımlanan uyarı kuralları, Azure portalında görüntülenmez. Etkinleştirebilir veya sistem durumu uyarısı devre dışı yalnızca kural [iş yükü İzleyicisi API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components). 
- Atama yapılamaz bir [Azure İzleyici'eylem grubu](../../azure-monitor/platform/action-groups.md) Azure portalındaki sistem durumu uyarıları için. Her sistem durumu uyarısı tetiklendiğinde tetiklenmesi için bir eylem grubu yapılandırmak için yalnızca bildirim ayarı API'yi kullanabilirsiniz. Şu anda, bir VM'ye karşı Eylem grupları atayabilirsiniz böylece tüm *sistem durumu uyarılarını* aynı Eylem grupları karşı VM tetikleyici tetiklendi. Geleneksel Azure uyarıları ayrı bir eylem grubu her sistem durumu uyarı kuralının kavramı yoktur. Ayrıca, sistem durumu uyarı tetiklendiğinde e-posta veya SMS bildirimleri sağlamak için yapılandırılmış olan eylem grupları desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi izlemeyi etkinleştirme yöntemlerini ve gereksinimleri hakkında bilgilere [VM'ler için Azure İzleyici'ı Dağıtma](vminsights-onboard.md).
