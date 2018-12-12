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
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/07/2018
ms.author: magoedte
ms.openlocfilehash: d720a7401b9ed1188a01d3cc2cc9ec7b66b640ce
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091560"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>VM'ler (Önizleme) için Azure İzleyici ile ilgili bilinen sorunlar

Bu makalede, bir çözümde bir sistem durumu ve performans izlemesi Azure VM'nin işletim sistemini bir araya getiren Azure VM'ler için Azure İzleyici ile ilgili bilinen sorunlar ele alınmıştır. 

Aşağıdaki sistem durumu özelliği ile bilinen sorunlar verilmiştir:

- Log Analytics çalışma alanınıza bağlı tüm sanal makineler için sistem durumu özelliği etkin. Bu nedenle bile eylemi ne zaman başlar ve tek bir VM'de sona erer budur.
- Sonra desteklenen yöntemleri kullanarak bir VM için izlemeyi devre dışı bırakın ve yeniden dağıtmayı deneyin, aynı çalışma alanında dağıtmanız gerekir. Bu VM için sistem durumunu görüntülemek için yeni çalışma alanı ve deneyin kullanırsanız, anormal davranış görüntülenebilir.
- Bir Azure VM kaldırılması veya silinmesi durumunda üç ila yedi gün boyunca VM liste görünümünde görüntülenir. Ayrıca, kaldırıldı veya silinmiş bir VM'nin durumunu'ı tıklatarak açılır **sistem tanılama** görüntüleyin ve sonra bir yükleme döngüsü başlatır. Silinen sanal Makinenin adını seçerek, bir VM silinip silinmediğini belirten ileti ile bir bölme açılır.
- Bu sürümle birlikte, zaman aralığı ve sıklığı durumu ölçütlerini değiştirilemez. 
- Sistem durumu ölçütlerini devre dışı bırakılamaz. 
- Dağıtımdan sonra veri görüntülemeden önce zaman alabilir **Azure İzleyici** > **sanal makineler** > **sistem durumu** bölmesinde veya  **VM kaynak** > **Insights** bölmesi.
- Sistem durumu tanılama güncelleştirmeleri daha hızlı herhangi bir görünüm deneyimi. Görünümler arasında geçiş yaptığınızda bilgileri gecikebilir. 
- Azure İzleyici ile sanal makine için dahil edilen uyarılar özeti ile algılanan sistem durumu sorunları sonuçtan Azure Vm'leri izlenen uyarıları görüntüler.
- **Uyarılar listesinde** tek VM ve Azure İzleyici bölmesinde izleme koşulu ayarlandığında uyarılar görüntülenir *harekete* son 30 gün içinde. Uyarıları yapılandırılabilir değildir. Ancak, sonra **uyarı listesi** bölmesi açıldığında, izleme koşulu ve zaman aralığını filtre değerini değiştirmek için Özet'i tıklatın.
- İçinde **Uyarılar listesinde** bölmesinde öneririz değiştirirken **kaynak türü**, **kaynak**, ve **İzleyicisi hizmeti** filtreler. Bunlar belirli çözüm için yapılandırılmış. Daha fazla alan bu liste görünümü görüntüler **Azure İzleyici** > **uyarılar** liste görünümü yok.   
- Linux vm'lerinde **sistem tanılama** görünümü kullanıcı tanımlı VM adı yerine bir VM'nin tüm etki alanı adını görüntüler.
- Vm'lerini kapatılıyor, güncelleştirmeleri durumu ölçütlerini bazıları *kritik* ve başkalarına *sağlıklı*. Net VM durumu olarak görüntülenen *kritik*.
- Sistem durumu uyarı önem derecesi değiştirilemez. Bunu yalnızca devre dışı bırakabilir veya etkinleştirilebilir. Ayrıca, bazı önem dereceleri durumu ölçütlerini durumuna göre güncelleştirilir.
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

- *Toplam CPU kullanımı* Windows sistem durumu ölçütü görüntüler eşiğinin **4 eşit** portalı hem de iş yükü izleme API. Ne zaman Eşiğe ulaşıldığında *toplam CPU kullanımı* yüzde 95 ve sistemi büyük kuyruk uzunluğu, 15'ten büyüktür. Bu sürümde bu sistem durumu ölçütü değiştirilemez. 
- Portalı veya iş yükü İzleyicisi API bunları hemen güncelleştirebilir olsa bile bir eşiği güncelleştirme gibi yapılandırma değişiklikleri, 30 dakika kadar yararlanın. 
- Tek tek işlemci ve mantıksal işlemci düzeyi durumu ölçütlerini Windows içinde kullanılamaz. Toplam CPU kullanımı yalnızca, Windows VM'ler için kullanılabilir. 
- Her sistem durumu ölçütü için tanımlanan uyarı kuralları, Azure portalında görüntülenmez. Etkinleştirebilir veya sistem durumu uyarısı devre dışı yalnızca kural [iş yükü İzleyicisi API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager). 
- Atama yapılamaz bir [Azure İzleyici'eylem grubu](../../monitoring-and-diagnostics/monitoring-action-groups.md) Azure portalındaki sistem durumu uyarıları için. Her sistem durumu uyarısı tetiklendiğinde tetiklenmesi için bir eylem grubu yapılandırmak için yalnızca bildirim ayarı API'yi kullanabilirsiniz. Şu anda, bir VM'ye karşı Eylem grupları atayabilirsiniz böylece tüm *sistem durumu uyarılarını* aynı Eylem grupları karşı VM tetikleyici tetiklendi. Geleneksel Azure uyarıları ayrı bir eylem grubu her sistem durumu uyarı kuralının kavramı yoktur. Ayrıca, sistem durumu uyarı tetiklendiğinde e-posta veya SMS bildirimleri sağlamak için yapılandırılmış olan eylem grupları desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi izlemeyi etkinleştirme yöntemlerini ve gereksinimleri hakkında bilgilere [VM'ler için Azure İzleyici'ı Dağıtma](vminsights-onboard.md).
