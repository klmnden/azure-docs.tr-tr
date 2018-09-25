---
title: Bilinen sorunlar VM'ler için Azure İzleyici | Microsoft Docs
description: VM'ler için Azure İzleyici sistem durumunu ve uygulama bileşenleri ve diğer kaynaklarla ilgili bağımlılıkları otomatik olarak keşfetme yanı sıra Azure VM'nin işletim sistemi izleme performansını birleştirir ve arasındaki iletişimi eşleyen bir Azure çözümüdür bunları. Bu makalede, bilinen sorunlar ele alınmıştır.
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
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: 819c3e74355cf80c7a998abb8b02b10c9e077059
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47062777"
---
# <a name="known-issues-with-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici ile ilgili bilinen sorunlar

Aşağıda Azure İzleyici sistem durumu özelliği olan VM'ler için bilinen sorunlar verilmiştir:

- Bu sürümle birlikte, zaman aralığı ve sıklığı durumu ölçütlerini değiştirilemez. 
- Sistem durumu ölçütlerini devre dışı bırakılamaz. 
- Ekledikten sonra Azure İzleyici'de gösterilen veri önce zaman alabilir -> sanal makineler -> sistem durumu veya VM kaynak dikey penceresinden Insights ->
- Görünümler arasında geçiş yaparken bilgi gecikmeler karşılaşabileceğiniz için sistem durumu tanılama güncelleştirmeleri diğer herhangi bir görünüm, daha hızlı deneyimi  
- Azure İzleyici'de sağlanan VM sistem durumu için Özet yalnızca izlenen Azure Vm'leriyle algılanan sistem durumu sorunları için tetiklenen uyarılar için uyarılar.
- **Uyarılar listesinde** görünümü tek bir VM ve Azure İzleyici gösterir sayfasındaki uyarıları izleme koşulu "30 gün için tetiklenme" olarak ayarlanır.  Bunlar, yapılandırılabilir değildir. Özet, bir kez tıkladıktan sonra ancak **uyarı listesi** görünüm sayfası başlatıldığında, filtre değeri izleme koşulu ve zaman aralığını değiştirebilirsiniz.
- Üzerinde **Uyarılar listesinde** görünüm sayfası öneririz yoksa değiştirme **kaynak türü**, **kaynak**, ve **İzleyicisi hizmeti** olarak filtreleri (Bu liste görünümü bazı ek alanlar için Azure İzleyici karşılaştırıldığında gibi -> Uyarılar liste görünümü gösterir) çözümü belirli yapılandırıldı.    
- Linux vm'lerinde **sistem tanılama** görünümü kullanıcı tanımlı VM adı yerine bir VM'nin tüm etki alanı adı vardır.
- Vm'lerini kapatılıyor sistem durumu ölçütlerini bazıları kritik durum ve diğer kritik bir durumdaki sanal makinenin ağ durumu sağlıklı bir duruma güncelleştirir.
- Sistem durumu uyarı önem derecesi değiştirilemez, bunlar yalnızca devre dışı bırakabilir veya etkinleştirilebilir.  Ayrıca, bazı önem dereceleri güncelleştirme durumu ölçütlerini durumuna bağlı.
- Herhangi bir ayar durumu ölçütü örneğinin değiştirme aynı ayar değişikliği tüm sistem durumu ölçütlerini örneklerinde aynı türde VM'de önünü açacak. Örneğin, eşiği disk alanı durumu ölçütü örneği ücretsiz, karşılık gelen mantıksal diske C: değiştirilir ve ardından bu eşik bulunan ve aynı sanal makine için izlenen tüm diğer mantıksal diskleri uygulanır.   
- Sağlıklı durum zaten kilitli olduğu için DNS İstemcisi hizmeti sistem durumu gibi bazı Windows durumu ölçütlerini eşikler değiştirilebilir, olmayan **çalıştıran**, **kullanılabilir** hizmet veya varlık durumu bağlam bağlı olarak.  Bunun yerine değeri sayı 4'tarafından temsil edilen, gelecek sürümlerden birinde gerçek görüntü dizeye dönüştürülür.  
- Zaten sağlıksız bir duruma tetiklemek için ayarlanırlar gibi bazı Linux durumu ölçütlerini eşikleri Mantıksal Disk sistem durumu gibi değiştirilebilir değildir.  Bunlar, bir şey çevrimiçi/çevrimdışı veya açma veya kapatma ve temsil edilir ve aynı değeri 1 veya 0'ı göstererek belirtir olup olmadığını gösterir.
- Herhangi bir kaynak grubunda kaynak grubu Filtresi kullanırken güncelleştirme uygun ölçekte Azure İzleyici, sanal makineler -> Sistem -> göstermek liste görünümüne neden olur, herhangi bir liste görünümü seçilmiş abonelik ve kaynak grubu -> **sonuç**.  Azure İzleyici geri gidin -> sanal makineler -> sistem durumu sekmesinde istediğiniz abonelik ve kaynak grubunu seçin ve ardından liste görünümüne gidin.

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [sanal makineler için yerleşik Azure İzleyici](monitoring-vminsights-onboard.md) gereksinimleri ve yöntemler, sanal makinelerin izlemeyi etkinleştirmek için anlamak için.