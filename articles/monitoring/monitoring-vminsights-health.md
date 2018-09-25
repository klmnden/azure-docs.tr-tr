---
title: İzleme sanal makine durumu Azure İzleyici ile VM'ler için | Microsoft Docs
description: Bu makalede, VM'ler için Azure İzleyici ile temel işletim sistemi ve sanal makine durumunu anlamak nasıl açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: c8a8598640e31f59476b5b3351fdb2eab7b66a6c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952928"
---
# <a name="understand-the-health-of-your-azure-virtual-machines-with-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici ile Azure sanal makinelerinizin durumunu anlama
Azure İzleme alanı ayrı ayrı bir spesifik rol ya da görev gerçekleştiren birden çok hizmet içerir, ancak bir Azure sanal makinelerinde barındırılan işletim sistemi ayrıntılı sistem durumu açısından sağlama kullanılabilir değildi.  Log Analytics veya Azure İzleyicisi'ni kullanarak için farklı koşullar izleyebilir olsa da bunlar model ve sistem durumunu temel bileşenler veya genel sanal makine durumunu temsil eden üzere tasarlanmamıştır.  VM sistem durumu özelliği için Azure İzleyici ile proaktif olarak Windows veya Linux konuk işletim sistemi ile anahtar bileşenleri ve bu durumunu ölçmek nasıl belirten ölçütleri ilişkilerini temsil eden bir model performansını ve kullanılabilirliğini izler bileşenleri ve iyi durumda olmayan bir koşul algılandığında sizi uyarır.  

Azure VM genel sistem durumunu görüntüleme ve işletim sistemi temel Vm'leri sistem durumu, sanal makineden doğrudan veya Azure İzleyici kaynak grubunun içindeki tüm sanal makineleri için Azure İzleyici ile iki yönlerden gösterilebilir.

Bu makalede hızlı bir şekilde değerlendirmek, araştırmanıza ve algılanan sistem durumu sorunları gidermek nasıl anlamanıza yardımcı olur.

VM'ler için Azure İzleyici yapılandırma hakkında daha fazla bilgi için bkz: [VM'ler için Azure İzleyici'ı etkinleştirme](monitoring-vminsights-onboard.md).

## <a name="monitoring-configuration-details"></a>İzleme Yapılandırma Ayrıntıları
Bu bölümde, Azure Windows ve Linux sanal makinelerini izlemek için tanımlanan varsayılan sistem durumu ölçütlerini özetlenmektedir.

### <a name="windows-vms"></a>Windows VM'leri

- Kullanılabilir megabayt belleği 
- Ortalama Disk yazma (Mantıksal Disk) saniyede
- Ortalama Disk yazma (Disk) saniyede
- Okuma başına ortalama Mantıksal Disk saniye
- Aktarım başına ortalama Mantıksal Disk saniye
- Okuma başına ortalama Disk saniye
- Aktarım başına ortalama Disk saniye
- Geçerli Disk Sırası Uzunluğu (Mantıksal Disk)
- Geçerli Disk Sırası Uzunluğu (Disk)
- Disk yüzde boşta kalma süresi
- Dosya sistemi hatası veya Bozulması
- Mantıksal Disk boş alan (%) Düşük
- Mantıksal Disk boş alan (MB) düşük
- Mantıksal Disk boş kalma süresi yüzdesi
- Saniye başına bellek sayfaları
- Yüzde bant kullanılan okuma
- Toplam yüzde bant kullanılan
- Yüzde bant kullanılan yazma
- Kullanımdaki kaydedilmiş bellek yüzdesi
- Fiziksel Disk yüzde boşta kalma süresi
- DHCP istemci hizmeti durumu
- DNS İstemcisi hizmeti durumu
- RPC hizmeti durumu
- Sunucu hizmeti durumu
- Toplam CPU kullanım yüzdesi
- Windows olay günlüğü hizmeti durumu
- Windows Güvenlik Duvarı hizmeti durumu
- Windows Uzaktan Yönetimi hizmetinin sistem durumu

### <a name="linux-vms"></a>Linux VM'leri
- Disk ortalaması Disk sn/Aktarım 
- Disk ortalaması Disk sn/Okuma 
- Disk ortalaması Disk sn/yazma 
- Disk durumu
- Mantıksal Disk boş alan
- Mantıksal Disk % boş alan
- Mantıksal Disk % boş Inode'ları
- Ağ bağdaştırıcısı durumu
- İşlemci yüzde DPC Zamanı
- İşlemci yüzde işlemci zamanı
- Toplam yüzde işlemci zamanı
- Toplam yüzde DPC Zamanı
- İşletim sistemi kullanılabilir megabayt belleği

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="introduction-to-health-experience"></a>Sistem durumu deneyimi giriş
Tek sanal makine veya VM için sistem durumu özelliğini kullanarak girmeden önce kısa bir giriş sağladığımız bilgileri nasıl görüntülenir ve görselleştirmeler ne temsil anlamak için önemlidir.  

## <a name="view-health-directly-from-a-virtual-machine"></a>Bir sanal makineden doğrudan durumunu görüntüle 
Bir Azure VM durumunu görüntülemek için seçin **Insights (Önizleme)** sol bölmesinde sanal makinenin. VM içgörüler sayfasında **sistem durumu** varsayılan olarak açıktır ve VM sistem durumu görünümünü gösterir.  

![Vm'leri sistem durumuna genel bakış seçili Azure sanal makine için Azure İzleyici](./media/monitoring-vminsights-health/vminsights-directvm-health.png)

Üzerinde **sistem durumu** bölümünde sekmesinde **Konuk VM sistem durumu**, tablo, sanal makinenizin geçerli sistem durumunu gösterir ve VM sistem durumu uyarılarını toplam sayısı, iyi durumda olmayan bir bileşen tarafından oluşturuldu. Başvurmak [uyarı verme ve bir uyarı Yönetimi](#alerting-and-alert-management) daha fazla ayrıntı için.  

Bir VM için tanımlanan sistem durumları şunlardır: 

* **Sağlıklı** – sanal makine için herhangi bir sorun algılandı ve gerektiği gibi çalıştığını.  
* **Kritik** – bir veya daha fazla kritik sorunlar algılandığında, beklendiği gibi normal işlevselliğini geri yüklemek için ele alınması gerekiyor. 
* **Uyarı** -bir veya daha fazla sorun algılandığında, ilgilenilmesi gereken ya da sağlık durumu kritik duruma gelebilir.  
* **Bilinmeyen** – hizmet durumu değişiklikleri bilinmeyen bir duruma VM ile bağlantı kurmak tamamlayamadı.  

Seçme **görüntüleme durumu tanılama** ilişkili durumu ölçütlerini, durum değişikliklerini ve diğer önemli sorunları VM'le ilgili bileşenler izlenerek karşılaşıldı sanal makinenin tüm bileşenleri gösteren bir sayfa açar. Başvurmak [sistem tanılama](#health-diagnostics) daha fazla ayrıntı için. 

Altında **bileşen sistem durumu** bölümü, tablo gösterir sistem durumu toplaması durumunu özellikle bu alanların sistem durumu ölçütlerini tarafından izlenen birincil performans kategorileri **CPU**,  **Bellek**, **Disk**, ve **ağ**.  Bileşenleri herhangi birini seçerek, tüm yönlerini söz konusu bileşen ve her birinin kendi sistem durumu izleme tek tek sistem durumu ölçütü listeleyen bir sayfa açılır.  

Sistem durumu Windows işletim sistemini çalıştıran bir Azure VM'den erişirken, en çok kullanılan 5 çekirdek Windows Hizmetleri sistem durumunu gösterilen bölümü altında **çekirdek sistem durumu Hizmetleri**.  Hizmetlerden herhangi birini seçtiğinizde, söz konusu bileşen ve sistem durumunu izleme durumu ölçütlerini listelendiği bir sayfa açılır.  Sistem durumu ölçütlerini adına tıklayarak özellik bölmesi açılır ve burada tanımlanan karşılık gelen bir Azure İzleyici uyarı durumu ölçütlerini varsa dahil olmak üzere yapılandırma ayrıntılarını gözden geçirebilirsiniz. Bunun hakkında daha fazla bilgi edinmek için [sistem durumu tanılama ve çalışma durumu ölçütlerini](#health-diagnostics).  

## <a name="aggregate-virtual-machine-perspective"></a>Toplam sanal makine perspektifi
Portalı Gezinti listeden bir kaynak grubundaki tüm sanal makinelerinizin için sistem durumu toplama görüntülemek için seçin **Azure İzleyici** seçip **sanal makineler (Önizleme)**.  

![Azure İzleyici görünümünden izleme VM öngörüleri](./media/monitoring-vminsights-health/vminsights-aggregate-health.png)

Gelen **abonelik** ve **kaynak grubu** açılan listeler, bunların sistem durumu görüntülemek için hedef Vm'leri eklenen içeren uygun olanı seçin. 

Üzerinde **sistem durumu** sekmesinde tarafından aşağıdaki öğrenin:

* Kaç tane Vm'niz ne kadar sağlam veya değil gönderen veri (Bilinmeyen durumu adlandırılır) olduğunu ve kritik veya sağlıksız durumda misiniz?
* Hangi sanal makinelerin işletim sistemi (OS) tarafından düzgün çalışmayan bir durum raporlama ve kaç?
* Kaç tane Vm'niz bir işlemci, disk, bellek veya ağ bağdaştırıcısı, sistem durumuna göre kategorilere algılanan bir sorun nedeniyle sağlıksız misiniz?  
* Kaç tane Vm'niz hizmetiyle sistem durumuna göre kategorilere ayrılmış bir çekirdek işletim sistemi, algılanan bir sorun nedeniyle sağlıksız misiniz?

Burada hızlı, proaktif olarak VM izleme durumu ölçütlerini tarafından algılanan en kritik sorunları belirlemek ve VM sistem durumu uyarı ayrıntılarını gözden geçirin ve tanılama ve düzeltme sorunun yardımcı olmak ilişkili Bilgi Bankası makalesi yöneliktir.  Açmak için önem derecelerinin birini seçin [tüm uyarıları](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md#all-alerts-page) sayfası, önem derecesine göre filtrelendi.

**İşletim sistemine göre VM Dağıtım** listede Windows sürümünü veya sürümlerini birlikte Linux dağıtımı tarafından listelenen VM'ler gösterilmektedir. Her işletim sistemi kategorisinde, Vm'leri başka ayrılmıştır VM Durumu'na göre. 

![VM Insights sanal makine dağıtım perspektifi](./media/monitoring-vminsights-health/vminsights-vmdistribution-by-os.png)

Bir VM için tanımlanan sistem durumları şunlardır: 

* **Sağlıklı** – sanal makine için herhangi bir sorun algılandı ve gerektiği gibi çalıştığını.  
* **Kritik** – bir veya daha fazla kritik sorunlar algılandığında, beklendiği gibi normal işlevselliğini geri yüklemek için ele alınması gerekiyor. 
* **Uyarı** -bir veya daha fazla sorun algılandığında, ilgilenilmesi gereken ya da sağlık durumu kritik duruma gelebilir.  
* **Bilinmeyen** – hizmet durumu değişiklikleri bilinmeyen bir duruma VM ile bağlantı kurmak tamamlayamadı.  

Herhangi bir sütun öğe üzerinde - tıklayabilirsiniz **VM sayısı**, **kritik**, **uyarı**, **sağlıklı** veya **bilinmeyen** için Detaya **sanal makineler** sayfa seçilen sütunun eşleşen filtrelenmiş sonuç listesini görürsünüz. Örneğin, çalışan tüm sanal makineler gözden geçirmek istiyorsanız **Red Hat Enterprise Linux sürüm 7.5**, tıklayarak **VM sayısı** değeri, işletim sistemi ve şu sayfaya eşleşen sanal makineler listesi açılır Bu filtre ve şu anda bilinen sistem durumu.  

![Red Hat Linux sanal makinelerinin örnek paketi](./media/monitoring-vminsights-health/vminsights-rollup-vm-rehl-01.png)
 
Üzerinde **sanal makineler** sayfa adı sütunu altında bir sanal Makineyi seçerseniz **VM adı**, uyarıların daha fazla ayrıntı içeren VM örneği sayfasına yönlendirilir ve sistem durumu ölçütlerini sorunlar tanımlandı olan Seçili VM etkileyen.  Buradan tıklayarak sağlık durumunu Ayrıntılar filtreleyebilirsiniz **sistem durumu** hangi bileşenlerin sağlıksız olduğunu görmek için sayfayı veya üst sol köşesindeki simgeyi sağlıksız bir bileşen tarafından gerçekleştirilen VM sistem durumu uyarılarını görüntüleyebilirsiniz Uyarı önem derecesine göre kategorilere ayrılmıştır.    

Bir sanal makine adına tıklayarak VM liste görünümünde açılır **sistem durumu** seçtiyseniz, sayfa için VM, benzer şekilde seçildi. **Insights (Önizleme)** VM'den doğrudan.

![Seçili Azure sanal makinesinin VM öngörüleri](./media/monitoring-vminsights-health/vminsights-directvm-health.png)

Burada, bir toplama gösterir **sistem durumu** sanal makine için ve **uyarılar**, temsil eden sistem durumu için sağlıksız için sistem durumu değiştiğinde harekete geçirilen VM sistem durumu Uyarıları önem derecesine göre kategorilere ayrılmış bir Sistem durumu ölçütleri.  Seçme **kritik durumdaki VM'ler** bir kritik sağlık durumunda bir veya daha fazla VM'lerin listesini içeren bir sayfa açılır.  Listedeki Vm'lerden birinin göstermek için durumunu tıklayarak **sistem tanılama** VM görünümü.  Burada, hangi durumu ölçütlerini yansıtan çıkış bir sistem durumu sorunu bulabilirsiniz. Zaman **sistem tanılama** sayfası açıldığında, VM ve bunların ilişkili sistem durumu ölçütlerini geçerli bir sağlık durumundaki tüm bileşenleri gösterir.  Başvurmak [sistem durumu tanı](#health-diagnostics) ayrıntılı bilgi için.  

Seçme **tüm sistem durumu ölçütlerini görüntülemek** bu özellik ile kullanılabilir tüm sistem durumu ölçütlerini listesini gösteren bir sayfa açar.  Bilgileri daha fazla bağlı olarak aşağıdaki seçenekleri filtrelenebilir:

* **Tür** – koşulları değerlendirin ve izlenen VM genel sistem durumu geri ölçüt türü üç tür durum vardır.  
    a. **Birim** – sanal makine bazı yönlerini ölçer. Bu sistem durumu ölçüt türü bileşeninin performansı belirlemek için bir performans sayacı yapay bir işlem gerçekleştirmek veya hata veren bir olayı izlemek için bir betik çalıştırarak denetimi yapıyor olabilir.  Varsayılan olarak, filtre birimine ayarlanır.  
    b. **Bağımlılık** -farklı varlıklar arasındaki sistem durumu toplaması sağlar. Bu durumu ölçütlerini başka bir tür için başarılı bir işlem kullanır varlık durumunu bağımlı bir varlık durumunu sağlar.  
    c. **Toplama** -benzer durumu ölçütlerini birleşik sistem durumu sağlar. Birim ve bağımlılık sistem durumu ölçütü, genellikle bir toplama sistem durumu ölçütü altında yapılandırılır. Bir varlıkta hedeflenen birçok farklı durumu ölçütlerini daha iyi bir genel düzenleme sağlamanın yanı sıra toplama sistem durumu ölçütü varlıkların farklı kategorileri için benzersiz bir sistem durumu sağlar.

* **Kategori** -durumu ölçütlerini raporlama amacıyla benzer bir tür ölçütünü gruplamak için kullanılan tür.  Bunlar **kullanılabilirlik** veya **performans**.

Hangi örneklerinin altındaki bir değere tıklayarak sağlıksız olduğunu görmek için aşağıya inebilir **sağlıksız bileşen** sütun.  Sayfada bir tablo, bir kritik sağlık durumunda olan bileşenler listelenir.    

## <a name="health-diagnostics"></a>Sistem durumu tanılama
**Sistem durumu tanılama** sayfası VM'nin, ilişkili durumu ölçütlerini, durum değişikliklerini, tüm bileşenleri görmenizi sağlar ve diğer önemli sorunları nesneleri izlerken karşılaştı VM ilgili. 

![Bir sanal makine için sistem durumu tanılama sayfası örneği](./media/monitoring-vminsights-health/health-diagnostics-page-01.png)

Sistem durumu tanılama aşağıdaki yollarla başlatabilirsiniz.

* Azure İzleyici'de toplama VM açısından tüm sanal makineler için toplama sistem durumuna göre.  Üzerinde **sistem durumu** sayfasında, simgesine tıklayarak **kritik**, **uyarı**, **sağlıklı**, veya **bilinmeyen** Sistem durumu bölümünün altında **Konuk VM sistem durumu** ve tüm sanal makineleri listeler sayfanın Veriler'i eşleşen filtrelenmiş kategori.  Değer tıklayarak **sistem durumu** sütun, sistem durumu, belirli bir VM için kapsamlı tanılama açılır.      

* Toplam VM açısından Azure İzleyici'de işletim sistemi tarafından. Altında **VM Dağıtım**, sütun değerlerini herhangi birini seçerek açılır **sanal makineler** sayfasında ve filtrelenmiş kategori eşleşen tabloda bir liste döndürür.  Değerin altında tıklayarak **sistem durumu** sütun, seçili VM için sistem durumu tanılama açılır.    
 
* Konuk sanal makinesinde sanal makineler için Azure İzleyici'den **sistem durumu** sekmesini seçerek **görüntüleme durumu tanılama** 

Sistem durumu tanılama sistem durumu bilgilerini şu kategorilere göre düzenler: 

* Kullanılabilirlik
* Performans
 
Seçili hedef uygun kategoride görüntüler için tanımlanan tüm sistem durumu ölçütlerini. 

Sistem durumu ölçütü için sistem durumu – üç durumdan birini tanımlanır *kritik*, *uyarı* ve *sağlıklı*. Başka bir durum yoktur *bilinmeyen*, sistem durumu ile ilişkili değil, ancak özellik tarafından bilinen izleme durumunu temsil eder.  

Aşağıdaki tabloda, sistem durumu Tanılama'da gösterilen sistem durumları hakkında ayrıntılı bilgi sağlar.

|Simge |Sistem durumu |Anlamı |
|-----|-------------|------------|
| |Sorunsuz |Sistem durumu içinde tanımlanan sistem durumu koşullarını ise iyi durumda. Üst döküm İzleyicisi söz konusu olduğunda, sistem durumu dökümü yapar artırma ve alt best-case veya iki katına durumunu yansıtır.|
| |Kritik |Sistem durumu içinde tanımlanan sistem durumu koşulu değilse önemlidir. Üst döküm İzleyicisi söz konusu olduğunda, sistem durumu dökümü yapar artırma ve alt best-case veya iki katına durumunu yansıtır.|
| |Uyarı |Burada bir gösterir tanımlanan sistem durumu koşulu, iki eşikleri arasında olup olmadığını uyarı sistem durumu bir *uyarı* durumu ve diğer gösteren bir *kritik* durumu. Olması durumunda bir üst döküm İzleyicisi, bir veya daha alt bir uyarı durumunda, ardından üst yansıtır *uyarı* durumu. İçinde bir alt varsa bir *kritik* ve başka bir alt bir *uyarı* durumu, üst toplama sistem durumu gösterilir *kritik*.|
| |Bilinmeyen |Sistem durumu bulunduğu bir *bilinmeyen* pek çok nedenden dolayı sistem durumu gibi hesaplanamıyor olduğunda durumu başlatılmamış vb. hizmet, veri toplamak kullanabilirsiniz.| 

Sistem durumu tanılama sayfası üç ana bölümü vardır:

* Bileşen Modeli 
* Durum Ölçütleri
* Durum Değişiklikleri 

![Sistem durumu tanılama sayfası bölümleri](./media/monitoring-vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Bileşen modeli
Bir sistem durumu tanılama sayfası en soldaki sütunda bileşen modelidir. Tüm bileşenleri ve VM ile ilişkili, keşfedilen örnekleri bu sütunda görüntülenir. 

Aşağıdaki örnekte bulunan disk, mantıksal disk, işlemci, bellek ve işletim sistemi bileşenlerdir. Bu bileşenlerin birden çok örneği bulundu ve bu sütunda, iki mantıksal disk örnekleri ile görüntülenen **/**, **/önyükleme**, ve   **/mnt/kaynak**, ağ bağdaştırıcısının bir örnek **eth0**, iki disk örnekleri **sda** ve **sdb**, işlemci iki örneğini **0 ve 1**ve bir **Red Hat Enterprise Linux Server sürüm 7.4 (Maipo) (işletim sistemi)**. 

![Sistem durumu tanılamada sunulan örnek bileşen modeli](./media/monitoring-vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Sistem durumu ölçütü
Sistem durumu tanılama sayfası merkezi sütunda **durumu ölçütlerini** sütun. VM için tanımlanan sistem durumu modeli, hiyerarşik bir ağaç şeklinde görüntülenir. Bir sanal makine için sistem durumu modeli birim, bağımlılık ve toplama durumu ölçütlerini oluşur.  

![Sistem durumu tanılamada sunulan örnek durumu ölçütü](./media/monitoring-vminsights-health/health-diagnostics-page-healthcriteria.png)

Sistem durumu ölçüt bir eşik değeri veya bir varlık, vb. durumunda bazı ölçütleri ile izlenen örnek sağlığını ölçer. Bir sistem durumu ölçütü, yukarıdaki bölümde açıklandığı gibi iki veya üç sistem durumuna sahiptir. Belirli bir noktada, sistem durumu ölçütü potansiyel durumlarının sadece birinde olabilir. 

Bir hedef genel durumunu, her birinin sistem durumu modeli içerisinde belirlenen sistem durumu ölçütlerini sağlığı'ndan belirlenir. Bu hedefte doğrudan hedeflenen durumu ölçütlerini durumu ölçütlerini bağımlılık sistem durumu ölçütü aracılığıyla hedefine çalışırken bileşenleri hedeflenmiş bir birleşimi olacaktır. Bu hiyerarşi gösterilmiştir **durumu ölçütlerini** sistem tanılama sayfası bölümü. Sistem durumu nasıl toplanır İlkesi toplam değer ve bağımlılık durumu ölçütlerini yapılandırmasının bir parçasıdır. Bölümü altında bu özelliği bir parçası olarak çalışan sistem durumu ölçütlerini varsayılan kümesini listesini bulabilirsiniz [izleme yapılandırma ayrıntılarını](#monitoring-configuration-details).  

Aşağıdaki örnekte, toplama sistem durumu ölçütü **çekirdek Windows Hizmetleri dökümü** Windows tabanlı bir VM için ayrı ayrı hizmet sistem durumu ölçütlere göre en önemli Windows hizmetlerinin sistem durumunu değerlendirir. DNS, DHCP vb. gibi her bir hizmetin durumunu değerlendirilir ve sistem durumu için karşılık gelen toplama sistem durumu ölçütü (aşağıda gösterildiği gibi) toplanır.  

![Sistem durumu toplama örneği](./media/monitoring-vminsights-health/health-diagnostics-windows-svc-rollup.png)

Sistem durumunu **çekirdek Windows Hizmetleri dökümü** sistem durumunu toplar **işletim sistemi kullanılabilirlik**, sonunda an döner **kullanılabilirlik** VM. 

Sistem durumu ölçütlerini **birim** türü kadar doğru ve seçme elips bağlantısına tıklayarak değiştiren yapılandırmalarına sahip **ayrıntıları göster** yapılandırma bölmesini açmak için. 

![Bir sistem durumu ölçütlerini örnek yapılandırma](./media/monitoring-vminsights-health/health-diagnostics-linuxvm-example-03.png)

Bu örnekte Seçilen sistem durumu ölçütlerini için yapılandırma bölmesinde **mantıksal diski % boş alan** , yalnızca değişiklikleri anlamına gelen bir iki durumlu izleme, olduğu gibi kendi eşiği için farklı bir sayısal değer ile yapılandırılabilir Sistem durumları sağlıklıdan kritik.  Diğer sistem durumu ölçütü üç durumu, uyarı ve kritik sistem durumu eşiği için bir değer yapılandırabileceğiniz olabilir.  

>[!NOTE]
>Bir örneği için sistem durumu ölçütlerini yapılandırma değişikliği uygulanıyor izlenen tüm örneklerine uygulanır.  Örneğin, **/mnt/kaynak** ve değiştirme **mantıksal diski % boş alan** eşiği yalnızca bu örneği uygulamanız değil, ancak diğer tüm mantıksal disk örnekleri bulunması veya üzerinde izlenmesi VM.
>

![Bir sistem durumu ölçütlerini bir birim İzleyicisi örneği yapılandırma](./media/monitoring-vminsights-health/health-diagnostics-linuxvm-example-04.png)


### <a name="state-changes"></a>Durum değişiklikleri
Sistem durumu Tanılama sayfasında en sağdaki sütunda **durum değişikliklerini**. Seçili durumu ölçütlerini ilişkilendirilmiş tüm durum değişikliklerini listeler **durumu ölçütlerini** bölüm veya bir VM'ye gelen seçildiyse sanal makinenin durumu değişikliği **bileşen modeli** veya **Durumu ölçütlerini** tablosunda sütun. 

![Sistem durumu tanılamada sunulan örnek durum değişiklikleri](./media/monitoring-vminsights-health/health-diagnostics-page-statechanges.png)

Bu bölümde durumu ölçütlerini ve üstte en son durumuna göre sıralanmış ilişkili zaman oluşur.   

### <a name="association-of-component-model-health-criteria-and-state-change-columns"></a>Bileşen modeli, durumu ölçütlerini ve durumu ilişkiyi değiştirmek sütunları 
Üç sütun birbirleri ile birbirine bağlıdır. Bir kullanıcı bileşen modeli içinde bulunan bir örneği seçtiğinde **durumu ölçütlerini** bölüm o bileşen görünüme filtre ve gelenlere **durum değişikliği** temel alınarak güncelleştirilir Sistem durumu ölçütleri. 

![İzlenen örneği ve sonuçları seçme örneği](./media/monitoring-vminsights-health/health-diagnostics-linuxvm-example-02.png)

Yukarıdaki örnekte bir seçtiğinde **/mnt (Mantıksal Disk)**, durumu ölçütlerini ağaç filtrelenmiştir **/mnt (Mantıksal Disk)**. **Kullanılabilirlik** ve **performans** sekmeleri filtrelenir buna göre çok. **Durum değişikliği** sütun kullanılabilirliğine bağlı durum değişikliği gösteren **/mnt (Mantıksal Disk)**. 

Güncelleştirilmiş durumunu görmek için sistem durumu tanılama sayfası tıklayarak yenileyebilirsiniz **Yenile** bağlantı.  Önceden tanımlanmış bir yoklama aralığı temel sistem durumu ölçütü'nın sistem durumu için bir güncelleştirme varsa, bu görevi bekleme sürelerinden kurtulun sağlar ve en son sistem durumu yansıtır.  **Durumu ölçütlerini** filtre vermektir - seçili sistem durumuna bağlıdır sonuçları kapsam için sağlam, uyarı ve kritik, bilinmeyen ve tüm.  **Son güncelleştirilen** sağ üst köşedeki zaman sistem durumu tanılama sayfası ne zaman yenilendiğini son zamanı temsil eder.  

## <a name="alerting-and-alert-management"></a>Uyarı ve uyarı Yönetimi 
VM sistem durumu özelliği için Azure İzleyici ile tümleştirilir [Azure uyarıları](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) ve koşul algılandığında önceden tanımlanmış durumu ölçütlerini sağlıklıdan için kötü bir durum değiştiğinde bir uyarı başlatır. Uyarı önem derecesi - önem derecesi 0 ile 4, önem derecesi en yüksek önem derecesine temsil eden 0 ile tarafından kategorilere ayrılmıştır.  

VM sistem durumu Uyarıları önem derecesine göre kategorilere toplam sayısı, üzerinde kullanılabilir **sistem durumu** bölümünde Pano **uyarılar**. Toplam uyarı sayısını veya bir önem derecesi düzeyine karşılık gelen sayısı seçtiğinizde **uyarılar** sayfası açılır ve seçiminizi eşleşen tüm uyarıları listeler.  Örneğin, satır karşılık gelen seçtiyseniz, **önem derecesi düzeyi 1**, aşağıdaki görmek sonra:

![Tüm önem düzeyi 1 uyarı örneği](./media/monitoring-vminsights-health/vminsights-sev1-alerts-01.png)

Bu görünümde, sayfanın üst kısmındaki açılan menüler, değerleri seçerek filtreleyebilirsiniz.

|Sütun |Açıklama | 
|-------|------------| 
|Abonelik |Bir Azure aboneliği seçin. Yalnızca seçili Abonelikteki uyarılar görünümünde dahil edilir. | 
|Kaynak Grubu |Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. | 
|Kaynak türü |Bir veya daha fazla kaynak türlerini seçin. Yalnızca seçilen türdeki hedefleri olan uyarılar görünümünde dahil edilir. Bu sütun, yalnızca bir kaynak grubu belirttikten sonra kullanılabilir. | 
|Kaynak |Bir kaynak seçin. Yalnızca bu kaynak bir hedef olarak uyarılarla Görünümü'nde dahil edilir. Bu sütun, yalnızca bir kaynak türünü belirttikten sonra kullanılabilir. | 
|Severity |Uyarı önem seçmeniz ya da seçin *tüm* her türlü önem derecesi, uyarı eklenecek. | 
|İzleme Koşulu |Uyarıları filtrelemek için bir izleme koşulu verilmiş olması durumunda seçin *Fired* sistem tarafından veya *çözümlenmiş* koşul artık etkin olduğunda sistem tarafından. Veya *tüm* tüm koşulların uyarıları eklemek için. | 
|Uyarı durumu |Bir uyarı durumu seçin *yeni*, *kabul*, *kapalı*, ya da seçin *tüm* tüm durumların uyarıları eklemek için. | 
|İzleme hizmet |Bir hizmet veya seçin, *tüm* tüm hizmetleri dahil etmek için. Altyapı öngörüleri uyarılardan yalnızca bu özellik için destekleniyor. | 
|Zaman aralığı| Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. | 

**Uyarı ayrıntısı** sayfası, bir uyarı durumuna değiştirmenize olanak tanır ve uyarının ayrıntılarını sağlayan seçtiğinizde görüntülenir. Uyarı kuralları ile çalışma ve Uyarıları yönetme hakkında daha fazla bilgi için bkz: [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

![Seçilen bir uyarının uyarı ayrıntıları bölmesine](./media/monitoring-vminsights-health/alert-details-pane-01.png)

Uyarı durumu da değiştirilebilir bir veya birden çok uyarı için bunları seçip ardından seçerek **durumunu değiştir** gelen **tüm uyarıları** sayfasında sol üst köşesinde. Üzerinde **uyarı durumunu değiştir** durumlardan biriyle seçtiğiniz bölmesi ekleme değişiklik açıklamasını **açıklama** alan ve ardından **Tamam** değişikliklerinizi işleyebilirsiniz. Bilgiler doğrulanır ve değişiklikler uygulanır, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.  

## <a name="next-steps"></a>Sonraki adımlar
Performans sorunlarını ve genel kullanımı ile Vm'leri performansınızı tanımlamak için bkz. [Azure VM performansını görüntüleme](monitoring-vminsights-performance.md), ya da bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](monitoring-vminsights-maps.md). 