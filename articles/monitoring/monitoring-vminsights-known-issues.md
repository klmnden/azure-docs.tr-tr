---
title: Azure İzleyici için Vm'leri (Önizleme) bilinen sorunlar | Microsoft Docs
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
ms.date: 11/07/2018
ms.author: magoedte
ms.openlocfilehash: 0f0ef83feac1a40c93910abdbd6abcada1a4d328
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613894"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>VM'ler (Önizleme) için Azure İzleyici ile ilgili bilinen sorunlar

Aşağıda Azure İzleyici sistem durumu özelliği olan VM'ler için bilinen sorunlar verilmiştir:

- Sistem durumu özelliği bile, başlatılan ve tek bir VM'den tamamlandı, Log Analytics çalışma alanınıza bağlı tüm sanal makineler için etkinleştirilir.
- Onboarding yeniden denendiğinde desteklenen yöntemleri kullanarak bir VM için izlemeyi devre dışı bırakma durumunda sonra aynı çalışma alanına yapılmalıdır.  Bu VM için sistem durumunu görüntülerken, yeni bir çalışma alanı kullanılıyorsa, anormal davranış gösterebilir.
- Kaldırıldı veya silindiği için bir Azure VM artık mevcut değilse, üç ila yedi gün boyunca VM liste görünümünde gösterilir. Ayrıca, kaldırıldı veya silinen sanal makinenin durumunu tıklanması **sistem tanılama** için daha sonra bir yükleme döngüye giriyor görünümü. Silinen bir sanal Makinenin adını seçerek bir dikey pencere, VM silinip silinmediğini belirten bir iletiyle başlatır.
- Bu sürümle birlikte, zaman aralığı ve sıklığı durumu ölçütlerini değiştirilemez. 
- Sistem durumu ölçütlerini devre dışı bırakılamaz. 
- Ekledikten sonra Azure İzleyici'de gösterilen veri önce zaman alabilir -> sanal makineler -> sistem durumu veya VM kaynak dikey penceresinden Insights ->
- Görünümler arasında geçiş yaparken bilgi gecikmeler karşılaşabileceğiniz için sistem durumu tanılama güncelleştirmeleri diğer herhangi bir görünüm, daha hızlı deneyimi  
- Azure İzleyici ile sanal makine için dahil edilen Özet yalnızca izlenen Azure Vm'leriyle algılanan sistem durumu sorunları için tetiklenen uyarılar için uyarılar.
- **Uyarılar listesinde** görünümü tek bir VM ve Azure İzleyici gösterir sayfasındaki uyarıları izleme koşulu "30 gün için tetiklenme" olarak ayarlanır.  Bunlar, yapılandırılabilir değildir. Özet, bir kez tıkladıktan sonra ancak **uyarı listesi** görünüm sayfası başlatıldığında, filtre değeri izleme koşulu ve zaman aralığını değiştirebilirsiniz.
- Üzerinde **Uyarılar listesinde** görünüm sayfası öneririz yoksa değiştirme **kaynak türü**, **kaynak**, ve **İzleyicisi hizmeti** olarak filtreleri (Bu liste görünümü bazı ek alanlar için Azure İzleyici karşılaştırıldığında gibi -> Uyarılar liste görünümü gösterir) çözümü belirli yapılandırıldı.    
- Linux vm'lerinde **sistem tanılama** görünümü kullanıcı tanımlı VM adı yerine bir VM'nin tüm etki alanı adı vardır.
- Vm'lerini kapatılıyor sistem durumu ölçütlerini bazıları kritik durum ve diğer kritik bir durumdaki sanal makinenin ağ durumu sağlıklı bir duruma güncelleştirir.
- Sistem durumu uyarı önem derecesi değiştirilemez, bunlar yalnızca devre dışı bırakabilir veya etkinleştirilebilir.  Ayrıca, bazı önem dereceleri güncelleştirme durumu ölçütlerini durumuna bağlı.
- Herhangi bir ayar durumu ölçütü örneğinin değiştirme aynı ayar değişikliği tüm sistem durumu ölçütlerini örneklerinde aynı türde VM'de önünü açacak. Örneğin, eşiği disk alanı durumu ölçütü örneği ücretsiz, karşılık gelen mantıksal diske C: değiştirilir ve ardından bu eşik bulunan ve aynı sanal makine için izlenen tüm diğer mantıksal diskleri uygulanır.   
- Sistem durumlarını zaten ayarlandığı bir Windows VM hedefleyen aşağıdaki durumu ölçütlerini eşikleri değiştirilebilir, olmayan **çalıştıran** veya **kullanılabilir**. Gelen sorgulandığında [iş yükü İzleyicisi API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), sistem durumu gösterir *comparisonOperator* değerini **LessThan** veya **GreaterThan**ile bir *eşiği* değerini **4** hizmet veya varlık varsa:
   - DNS istemcisi hizmet durumu-hizmet çalışmıyor 
   - DHCP istemci hizmeti sistem durumu-hizmet çalışmıyor 
   - RPC hizmet durumu-hizmet çalışmıyor 
   - Windows Güvenlik Duvarı Hizmet durumu-hizmet çalışmıyor
   - Windows olay günlüğü hizmeti sistem durumu – hizmeti çalışmıyor 
   - Sunucu hizmet durumu-hizmet çalışmıyor 
   - Windows Uzaktan Yönetimi hizmetinin sistem durumu – hizmet çalışmıyor 
   - Dosya sistemi hatası veya bozulma – Mantıksal Disk kullanılamıyor

- Eşikleri aşağıdaki Linux durumu ölçütlerini değiştirilebilir, olmayan için sistem durumu beri zaten ayarlanmış **true**.  Sistem durumu gösterir *comparisonOperator* bir değerle **LessThan** ve *eşiği* değerini **1** iş yükünden sorgulandığında Varlık bağlama bağlı olarak için API izleme:
   - Mantıksal Disk durumu – mantıksal disk değil çevrimiçi / kullanılabilir
   - Disk durumu – Disk değil çevrimiçi / kullanılabilir
   - Ağ bağdaştırıcısı durumu - ağ bağdaştırıcısı devre dışı bırakıldı  

- **Toplam CPU kullanımı** Windows sistem durumu ölçütü gösteren bir eşik **4 eşit** portalından ve iş yükü izleme API'den CPU kullanımı % 95'den büyük olduğunda ve sistem sırası uzunluğu sorgulandığında 15'ten daha büyük. Bu sürümde bu sistem durumu ölçütü değiştirilemez.  
- Bir eşiği güncelleştirme gibi yapılandırma değişiklikleri, portalı veya iş yükü İzleyicisi API'yi hemen güncelleştirebilir olsa da etkili olması için en fazla 30 dakika sürer.  
- Tek tek işlemci ve mantıksal işlemci düzeyi durumu ölçütlerini kullanılabilir değil Windows, yalnızca **toplam CPU kullanımı** Windows Vm'leri için kullanılabilir.  
- Her sistem durumu ölçütü için tanımlanan uyarı kuralları, Azure portalında kullanıma sunulmaz. Yalnızca gelen yapılandırılabilir [iş yükü İzleyicisi API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager) etkinleştirme veya bir sistem durumu uyarı kuralı devre dışı.  
- Atama bir [Azure İzleyici'eylem grubu](../monitoring-and-diagnostics/monitoring-action-groups.md) için sistem durumu uyarıları Azure portalından mümkün değildir. Bildirim ayarı API yalnızca her bir sistem durumu uyarısı tetiklendiğinde tetiklenmesi için bir eylem grubu yapılandırmak için de kullanabilirsiniz. Şu anda, bir VM'ye karşı eylem gruplarına atanabilir gibi tüm *sistem durumu uyarılarını* aynı Eylem grupları karşı VM tetikleyici tetiklendi. Ayrı bir eylem grubu için geleneksel Azure uyarıları gibi her durumu uyarı kuralının kavramı yoktur. Ayrıca, sistem durumu uyarılarını tetiklendiğinde bir e-posta veya SMS göndererek bildirmek için yapılandırılmış olan yalnızca eylem grubu desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [sanal makineler için yerleşik Azure İzleyici](monitoring-vminsights-onboard.md) gereksinimleri ve yöntemler, sanal makinelerin izlemeyi etkinleştirmek için anlamak için.