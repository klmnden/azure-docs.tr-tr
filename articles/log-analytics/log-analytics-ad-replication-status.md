---
title: Active Directory çoğaltma durumu Azure Log Analytics ile izleme | Microsoft Docs
description: Active Directory çoğaltma durumu çözüm paketi, Active Directory ortamınızı çoğaltma hataları için düzenli olarak izler.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 7c850eee67224d09ea2715a58c3cd8eca4ab07af
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041911"
---
# <a name="monitor-active-directory-replication-status-with-log-analytics"></a>Log Analytics ile Active Directory çoğaltma durumunu izleme

![AD çoğaltma durumu simgesi](./media/log-analytics-ad-replication-status/ad-replication-status-symbol.png)

Active Directory kuruluş BT ortamında önemli bir bileşenidir. Yüksek kullanılabilirlik ve yüksek performans sağlamak için kendi Active Directory veritabanının kopyasını her etki alanı denetleyicisi vardır. Etki alanı denetleyicileri birbirlerini değişiklikleri kuruluş genelinde yaymak için çoğaltma. Bu çoğaltma işlemi hataları kuruluş genelinde çeşitli sorunlara neden olabilir.

AD çoğaltma durumu çözüm paketi, Active Directory ortamınızı çoğaltma hataları için düzenli olarak izler.

## <a name="installing-and-configuring-the-solution"></a>Çözümünü yükleme ve yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

* Değerlendirilecek etki alanının üyesi olan etki alanı denetleyicilerinde aracıları yüklemeniz gerekir. Veya üye sunuculara aracıları yükleyin ve AD çoğaltma verilerini Log Analytics'e göndermek için aracıları yapılandırmanız gerekir. Windows bilgisayarlarını Log Analytics'e bağlama anlamak için bkz. [bağlanmak Windows bilgisayarlarını Log Analytics'e](log-analytics-windows-agent.md). Etki alanı denetleyicinizi Log Analytics'e bağlama, görmek istediğiniz mevcut bir System Center Operations Manager ortamının bir parçası ise [Log Analytics için Operations Manager'ı bağlama](log-analytics-om-agents.md).
* Active Directory çoğaltma durumu çözümü, açıklanan işlemi kullanarak Log Analytics çalışma alanınıza eklemek [Log Analytics çözümleri ekleme çözüm Galerisi'ndeki](log-analytics-add-solutions.md).  Başka bir yapılandırma işlemi gerekmez.

## <a name="ad-replication-status-data-collection-details"></a>AD çoğaltma durumu verileri toplama ayrıntıları
Veri toplama metotlarını ve verileri için AD çoğaltma durumu nasıl toplanır hakkında diğer ayrıntıları aşağıdaki tabloda gösterilmektedir.

| Platform | Doğrudan Aracı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |beş günde bir |

## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-log-analytics"></a>İsteğe bağlı olarak, AD verilerini Log Analytics'e göndermek olmayan etki alanı denetleyicisi etkinleştir
Herhangi bir etki alanı denetleyicilerinizin doğrudan Log Analytics'e bağlanmak istemiyorsanız, herhangi bir bilgisayarda etki alanınızda Log Analytics'e bağlı AD çoğaltma durumu çözüm paketi için veri toplamak ve bu verileri göndermek için kullanabilirsiniz.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-log-analytics"></a>AD verilerini Log Analytics'e göndermek olmayan etki alanı denetleyicisi etkinleştirmek için
1. Bilgisayarı AD çoğaltma durumu çözümünü kullanarak izlemek istediğiniz etki alanının bir üyesi olduğunu doğrulayın.
2. [Windows bilgisayarı Log Analytics'e bağlayın](log-analytics-windows-agent.md) veya [kullanarak mevcut Operations Manager ortamınızı Log analytics'e bağlanma](log-analytics-om-agents.md), zaten bağlı değil.
3. Bu bilgisayarda, aşağıdaki kayıt defteri anahtarını ayarlayın:

   * Anahtar: **hkey_local_machıne\system\currentcontrolset\services\healthservice\parameters\management grupları\<ManagementGroupName > \Solutions\ADReplication**
   * Değer: **IsTarget**
   * Değer verisi: **true**

   > [!NOTE]
   > Bu değişiklikler, yeniden başlatmaya kadar Microsoft Monitoring Agent hizmeti (HealthService.exe) etkili olmaz.
   >
   >

## <a name="understanding-replication-errors"></a>Çoğaltma hatalarını anlama
AD çoğaltma durumu verilerini Log Analytics'e gönderilen oluşturduktan sonra şu anda kaç çoğaltma hataları belirten Log analytics'te aşağıdaki görüntüye benzer bir kutucuk görürsünüz.  
![AD çoğaltma durumu kutucuğu](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritik çoğaltma hataları** veya % 75'ini üzerindeki hatalar [ömründen](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) , Active Directory ormanı için.

Kutucuğa tıkladığınızda, hatalar hakkında daha fazla bilgi görüntüleyebilirsiniz.
![AD çoğaltma durumu Panosu](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Hedef sunucu durumu ve kaynak sunucu durumu
Bu sütunlar, hedef sunucu ve çoğaltma hataları yaşayan kaynak sunucular durumunu gösterir. Her etki alanı denetleyicisi adından sonra sayı, bu etki alanı denetleyicisi çoğaltma hatalarını sayısını gösterir.

Bazı sorunlar kaynak sunucu Perspektif ve diğer kaynaklardan hedef sunucu açısından gidermeyi kolaylaştırması açısından çünkü hedef sunucuları hem de kaynak sunucular için hataları gösterilmektedir.

Bu örnekte, birden çok hedef sunucu aynı sayıda hata kabaca olması, ancak tüm diğerlerinden çok daha fazla hata sahip bir kaynak sunucu (ADDC35) görebilirsiniz. Bazı sorun olduğunu, çoğaltma ortaklarından veri göndermek başarısız olmasına neden olan ADDC35 üzerinde olasıdır. Üzerinde ADDC35 sorunlarını giderme hedef sunucu alanında görünen hatalarda birçoğu çözebilir.

### <a name="replication-error-types"></a>Çoğaltma hatası türleri
Bu alan, kuruluşunuza algılanan hatalar türleri hakkında bilgi sağlar. Her bir benzersiz sayısal kodu ve hatanın nedenini belirlemenize yardımcı olabilecek bir ileti hatalı.

Üst halka hangi hataları daha fazla görünür ve daha az sıklıkta ortamınızda fikir verir.

Birden çok etki alanı denetleyicileri aynı çoğaltma hatası kullanırken gösterilir. Bu durumda, keşfedin veya bir etki alanı denetleyicisinde bir çözüm sonra aynı hatayı tarafından etkilenen diğer etki alanı denetleyicilerinde yineleyin.

### <a name="tombstone-lifetime"></a>Silinmiş Öğe işareti yaşam süresi
Silinmiş Öğe işareti yaşam bir kaldırma anılan ne kadar silinmiş bir nesneye belirler, Active Directory veritabanında tutulur. Silinmiş bir nesneyi silinmiş öğe işareti yaşam süresi geçtiğinde, çöp toplama işlemi Active Directory veritabanından otomatik olarak kaldırır.

Varsayılan silinmiş öğe işareti yaşam 180 gün Windows, en son sürümleri için ancak daha eski sürümlerindeki 60 gün olduğu ve açıkça bir Active Directory Yöneticisi tarafından değiştirilebilir.

Kullanım ömrü yaklaştığı ya da çoğaltma hatalarının yaşıyorsanız, bilmek önemlidir. İki etki alanı denetleyicisi kullanım ömrü devam eden bir çoğaltma hatası karşılaşırsanız, temel çoğaltma hata düzeltilene bile çoğaltma bu iki etki alanı denetleyicileri arasında devre dışı bırakıldı.

Silinmiş Öğe işareti yaşam süresi alan devre dışı çoğaltma gerçekleşmesini tehlikesi olduğu yerde belirlemenize yardımcı olur. Her bir hata **% 100'den TSL** kategorisi için kaynak ve hedef sunucu arasında en az bir orman için silinmiş öğe işareti yaşam çoğaltılmamış bir bölümünü temsil eder.

Bu durumda, yalnızca çoğaltma hatası düzeltme yeterli olmaz. En az çoğaltma yeniden başlatmadan önce kalan nesneleri yukarı temizlemek üzere el ile araştırma gerekir. Hatta bir etki alanı denetleyicisi yetkisini gerekebilir.

Kullanım ömrü kalıcı çoğaltma hataları belirlemeye ek olarak, ayrıca içine denk gelen tüm hataları dikkat edilmesi gereken istediğiniz **50-%75 TSL** veya **75-%100 TSL** kategorileri.

Bunlar büyük olasılıkla ihtiyaç duydukları müdahale çözümlenecek şekilde açıkça kalan, geçici olmayan hatalar var. Güzel bir haberimiz var, bunlar henüz ömründen ulaştığınıza değil ' dir. Varsa, bu sorunları en kısa sürede düzeltmek ve *önce* ömründen ulaşmadan, çoğaltmayı el ile müdahale en az yeniden başlatın.

AD çoğaltma durumu çözümü sayısını gösterir. Pano kutucuğu daha önce belirtildiği gibi *kritik* %75 (hatalar dahil olmak üzere kullanım ömrü'nın üzerinde olan hata olarak tanımlanır, ortamınızda çoğaltma hataları üzerinde TSL %100 olan). Bu sayı 0'da korumak ister.

> [!NOTE]
> Bir özel silinmiş öğe işareti yaşam süresi değeri ayarlanmış olsa bile bu yüzdeleri doğru güvenebileceğiniz şekilde tüm silinmiş öğe işareti yaşam süresi yüzdesi hesaplamalar, Active Directory ormanı için gerçek ömründen temel alır.
>
>

### <a name="ad-replication-status-details"></a>AD çoğaltma durumu ayrıntıları
Bu listelerden birine herhangi bir öğeye tıkladığınızda, günlük arama özelliğini kullanarak hakkındaki ek ayrıntıları bakın. Sonuçları yalnızca bu öğe ile ilgili hataları gösterecek şekilde filtrelenir. Tıklayarak etki alanı denetleyicisine altında listelendiğinden **hedef sunucu durumu (ADDC02)**, arama sonuçları için filtre gördüğünüz show hata etki alanı denetleyicisi ile hedef sunucu olarak listelenen:

![Arama sonuçlarında AD çoğaltma durumu hataları](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Buradan daha fazla filtrelemek, arama sorguyu değiştirin ve benzeri. Günlük araması'nı kullanma hakkında daha fazla bilgi için bkz. [günlük aramaları](log-analytics-log-searches.md).

**HelpLink** alan belirli hata hakkında ek ayrıntılar ile TechNet sayfanın URL'sini gösterir. Kopyalayın ve bu bağlantı sorunlarını giderme ve hata düzeltme hakkında bilgi için tarayıcı pencerenizi yapıştırın.

Ayrıca **dışarı** sonuçları Excel'e. Verileri dışarı aktarma, çoğaltma hatası görselleştirmek istediğiniz herhangi bir şekilde yardımcı olabilir.

![Excel dışarı aktarılan AD çoğaltma durumu hataları](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD çoğaltma durumu ile ilgili SSS
**S: ne sıklıkta AD çoğaltma durumu veriler güncelleştirildi mi?**
Y: bilgileri beş günde bir güncelleştirilir.

**S: Bu veriler sıklıkla güncelleştirilir yapılandırmak için bir yol var mı?**
Y: şu anda değil.

**S: my etki alanı denetleyicilerinin çoğaltma durumunu görmek için Log Analytics çalışma alanıma Ekle gerekiyor?**
C: Hayır, yalnızca bir tek etki alanı denetleyicisi eklenmelidir. Log Analytics çalışma alanınızda birden çok etki alanı denetleyiciniz varsa, bunların tümünün verileri Log Analytics'e gönderilir.

**S: tüm etki alanı denetleyicileri Log Analytics çalışma alanıma Ekle istemiyorum. AD çoğaltma durumu çözümü kullanabilir miyim?**
C: Evet. Etkinleştirmek için bir kayıt defteri anahtarının değerini ayarlayabilirsiniz. Bkz: [AD verilerini Log Analytics'e göndermek olmayan etki alanı denetleyicisi etkinleştirmek için](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**S: veri koleksiyonu yapan işlemin adı nedir?**
Y: AdvisorAssessment.exe

**S: ne kadar toplanacak veri için sürer?**
Y: veri toplama süresi, Active Directory ortamında boyutuna bağlıdır, ancak genellikle 15 dakikadan kısa sürer.

**S: hangi türde veri toplanır?**
Y: çoğaltma bilgilerini LDAP toplanır.

**S: verileri toplandığında yapılandırmak için bir yol var mı?**
Y: şu anda değil.

**S: hangi izinlerin verileri toplamak ihtiyacım var?**
Y: normal kullanıcı izinleri için Active Directory yeterlidir.

## <a name="troubleshoot-data-collection-problems"></a>Veri toplama sorunlarını giderme
Veri toplamak için Log Analytics çalışma alanınıza bağlı için en az bir etki alanı denetleyicisi AD çoğaltma durumu çözüm paketi gerektirir. Bir etki alanı denetleyicisine bağlanmak kadar belirten bir ileti görünür **veri hala toplanmakta olan**.

Etki alanı denetleyicilerinizden biri bağlama konusunda yardıma ihtiyacınız varsa, belgeleri görüntüleyebilirsiniz [bağlanmak Windows bilgisayarlarını Log Analytics'e](log-analytics-windows-agent.md). Alternatif olarak, etki alanı denetleyicinize bir System Center Operations Manager ortamı için zaten bağlıysa, belgeleri görüntüleyebilirsiniz [System Center Operations Manager'a bağlama Log analytics'e](log-analytics-om-agents.md).

Herhangi bir etki alanı denetleyicilerinizin doğrudan Log Analytics veya System Center Operations Manager bağlanmak istemiyorsanız bkz [AD verilerini Log Analytics'e göndermek olmayan etki alanı denetleyicisi etkinleştirmek için](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [Log Analytics'te günlük aramaları](log-analytics-log-searches.md) ayrıntılı Active Directory çoğaltma Durumu verisini görüntülemek için.
