---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/03/2018
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: dac04ed9a43e19d022720979c8f83aa2b4132f78
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
**Son Güncelleştirme'ı belge**: 3 Nisan, 15:00 PST.

Son açıklanması bir [CPU güvenlik açıklarının yeni sınıf](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) kurgusal yürütme yan kanal saldırıları olarak bilinen daha fazla netlik aramayı müşterilerden soruları sonuçlandı.  

Microsoft, tüm bulut hizmetlerimizle arasında Azaltıcı Etkenler dağıtıldığını. Azure çalışan ve müşteri iş yüklerinin birbirinden ayırır altyapı korunur.  Başka bir deyişle, Azure üzerinde çalışan diğer müşteriler güvenlik açıklarından kullanarak uygulamanızı saldırı olamaz.

Azure kullanımını Ayrıca, genişletme [bakım koruma bellek](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#memory-preserving-maintenance) VM konak güncelleştirilirken en çok 30 saniye veya VM duraklatma mümkün olduğunda, zaten güncelleştirilmiş bir ana bilgisayara taşınır.  Daha fazla bakım koruma bellek müşteri etkisini en aza indirir ve yeniden başlatma gereksinimini ortadan kaldırır.  Azure, sistem genelinde güncelleştirmeleri konağa yaparken bu yöntemleri yararlanacak olan.

> [!NOTE] 
> Geç Şubat 2018 içinde Intel Corporation güncelleştirilmiş yayımlanan [mikro kodları gözden geçirme Kılavuzu](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/03/microcode-update-guidance.pdf) kararlılığını geliştirmek ve tarafındanbildirilensongüvenlikaçıklarınakarşıazaltmakkendimikrokodlarısürümlerindurumuileilgili[Google proje sıfır](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). Azure tarafından yerinde Azaltıcı yerleştirin [3 Ocak 2018](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) Intel mikro kod güncelleştirmesini tarafından etkilenmez. Microsoft, güçlü Azaltıcı Etkenler zaten Azure diğer sanal makinelerden Azure müşterilerin korunmasına yerinde yerleştirin.  
>
> Intel mikro kodları değişken 2 giderir Spectre ([CVE 2017 5715](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2017-5715) veya şube hedef ekleme), yalnızca paylaşılan veya güvenilmeyen iş yükleri içinde sanal makineleri Azure üzerinde çalıştırdığınız geçerli olacak saldırılarına karşı korumak için. Bizim mühendisleri Azure müşterilerine yapmadan önce mikro kodları, performans etkileri en aza indirmek için kararlılık test ediyorsunuz.  Çok az müşteriler Vm'leri güvenilmeyen iş yükleri çalıştırırken, müşterilerin çoğu kez yayımlanan bu özelliği etkinleştirmek gerekmez. 
>
> Daha fazla bilgi kullanılabilir olduğu gibi bu sayfa güncelleştirilir.  






## <a name="keeping-your-operating-systems-up-to-date"></a>İşletim sistemlerinin güncel tutma

Bir işletim sistemi güncelleştirme uygulamalarınızı Azure üzerinde çalışan diğer müşterilerden Azure üzerinde çalışan yalıtmak için gerekli olmasa da, bu her zaman işletim sistemi sürümü güncel tutmak üzere en iyi uygulamadır. 2018 ve üzeri Ocak [toplamaları için Windows Güvenlik](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) açıklarından bunları azaltmanın yollarını içerir.

Aşağıdaki teklifleri işletim sisteminizi güncelleştirmeniz için bizim önerilen eylemler şunlardır: 

<table>
<tr>
<th>Teklifi</th> <th>Önerilen Eylem </th>
</tr>
<tr>
<td>Azure Cloud Services </td>  <td>Otomatik güncelleştirmeyi etkinleştirme veya yeni konuk işletim sistemi çalıştırdığınızdan emin olun.</td>
</tr>
<tr>
<td>Azure Linux sanal makineleri</td> <td>Güncelleştirmeler kullanılabilir olduğunda işletim sistemi sağlayıcınızdan yükleyin. </td>
</tr>
<tr>
<td>Azure Windows sanal makineler </td> <td>İşletim sistemi güncelleştirmelerini yüklemeden önce desteklenen bir virüsten koruma uygulaması çalıştığını doğrulayın. Uyumluluk bilgileri için virüsten koruma yazılımı satıcınıza başvurun.<p> Yükleme [Ocak güvenlik dökümü](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Diğer Azure PaaS Hizmetleri</td> <td>Bu hizmetleri kullanan müşteriler için gereken eylemi yok. Azure, işletim sistemi sürümlerini otomatik olarak güncel tutar. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Güvenilmeyen kod çalıştırıyorsanız, ek yönergeler 

Güvenilmeyen kod çalıştırmadığınız sürece ek müşteri Eylem gerekmiyor. Güvenmediğiniz kodu izin verirseniz (örneğin, ikili veya uygulamanızdaki sonra bulutta yürütülen kod parçacığını karşıya müşterilerinize birini izin), sonra da aşağıdaki ek adımları yapılması gerekir.  


### <a name="windows"></a>Windows 
Windows kullanıyorsanız ve güvenilmeyen kod barındırma (özellikle için kurgusal yürütme yan kanal güvenlik açıklarına karşı ek koruma sağlayan çekirdek sanal adres (KVA) gölgeleme adlı Windows özelliği de etkinleştirmeniz gerekir değişken 3 Meltdown, [CVE 2017 5754](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2017-5754), ya da yanlış veri önbelleği yüklemesi). Bu özellik varsayılan olarak kapalıdır ve etkinleştirilirse performansı etkileyebilir. İzleyin [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) sunucu üzerindeki etkinleştirme korumaları için yönergeler. Azure Cloud Services çalıştırıyorsanız, WA çalıştırdığınızı doğrulayın-KONUK-OS-5.15_201801-01 veya WA-GUEST-OS-4.50_201801-01 (kullanılabilir 10 Ocak 2018 başlayarak) ve etkin kayıt defteri anahtarı bir başlangıç görevi.


### <a name="linux"></a>Linux
Linux kullanıyorsanız ve güvenilmeyen kod barındırma kullanıcı alanına ait olanlar çekirdekten tarafından kullanılan belleği tabloları ayıran çekirdek sayfa tablosu yalıtım (KPTI) uygulayan daha yeni bir sürüme ayrıca Linux güncelleştirmeniz gerekir. Bu Azaltıcı Etkenler Linux işletim sistemi güncelleştirme gerektirir ve kullanılabilir olduğunda dağıtım sağlayıcınızdan elde edilebilir. İşletim sistemi sağlayıcınız korumaları etkin veya varsayılan olarak devre dışı olup olmadığını söyleyebilir.



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [CPU güvenlik açığı güvenliğini sağlama Azure müşterilerden](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
