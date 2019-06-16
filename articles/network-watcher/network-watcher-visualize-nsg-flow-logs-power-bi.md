---
title: Power BI ile visualizing Azure ağ güvenlik grubu akış günlüklerini | Microsoft Docs
description: Bu sayfa, Power BI ile NSG akış günlüklerini görselleştirme açıklar.
services: network-watcher
documentationcenter: na
author: mattreatMSFT
manager: vitinnan
editor: ''
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: mareat
ms.openlocfilehash: 6df49f9cd308f4bb9b1fef6e5860872526ce8bb7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60860878"
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Power BI ile visualizing ağ güvenlik grubu akış günlüklerini

Ağ güvenlik grubu akış günlüklerini üzerindeki ağ güvenlik gruplarının giriş ve çıkış IP trafiğini hakkındaki bilgileri görüntülemek izin verin. Bu kural başına temelinde, akışı NIC giden ve gelen akışlar uygulandığı günlüklerini gösterme, 5 demet bilgi (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) akışla ilgili ve trafiklere izin verildiğini veya, akış.

Günlük dosyalarını el ile arama yaparak akış günlüğü verileri Öngörüler edinmek zor olabilir. Bu makalede, en son, akış günlüklerini Görselleştirme ve ağınızdaki trafiği hakkında bilgi edinmek için bir çözüm sunuyoruz.

> [!Warning]  
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için bkz [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md). Aşağıdaki yönergeler, değişiklik yapmadan günlük dosyalarının sürüm 2 ile çalışmaz.

## <a name="scenario"></a>Senaryo

Aşağıdaki senaryoda, NSG akış günlüğü verilerimiz için bir havuz olarak yapılandırdık depolama hesabına Power BI desktop bağlanın. Biz depolama hesabımız bağlandıktan sonra Power BI indirir ve ağ güvenlik grupları tarafından günlüğe kaydedilen trafik görsel bir temsilini sağlamak için günlükleri ayrıştırır.

İncelemek şablonda sağlanan görseller kullanma:

* Popüler konuşmacılar
* Yöne ve kural kararına göre zaman serisi akış verileri
* Ağ arabirimi MAC adresine göre akışlar
* NSG ve kural göre akışlar
* Hedef bağlantı noktasına göre akışlar

Yeni veri, görsel öğe eklemek için değiştirin ya da gereksinimlerinize uyacak şekilde sorguları Düzenle sağlanan şablon düzenlenemez.

## <a name="setup"></a>Kurulum

Başlamadan önce ağ güvenlik grubu akış hesabınızdaki bir veya daha fazla ağ güvenlik grupları etkin günlüğü olması gerekir. Ağ güvenliği akış günlüklerini etkinleştirme hakkında yönergeler için şu makaleye başvurun: [Ağ güvenlik grupları için akış günlüğü giriş](network-watcher-nsg-flow-logging-overview.md).

Ayrıca, Power BI Desktop istemcisi makinenizde ve makinenize indirmek ve depolama hesabınızda var olan günlük verileri yüklemek için yeterli boş alan yüklü olması gerekir.

![Visio diyagramı][1]

### <a name="steps"></a>Adımlar

1. İndirin ve aşağıdaki Power BI şablonu Power BI Desktop uygulamasında açın [şablonu Ağ İzleyicisi Powerbı akış günlükleri](https://aka.ms/networkwatcherpowerbiflowlogstemplate)
1. Gerekli sorgu parametrelerini girin
   1. **StorageAccountName** – yüklenemedi ve görselleştirmek istediğiniz NSG akış günlüklerini içeren depolama hesabı adını belirtir.
   1. **NumberOfLogFiles** – indirip Power BI'da görselleştirin istediğiniz günlük dosyalarının sayısını belirtir. Örneğin, 50 belirtilirse, en son 50 günlük dosyaları. Biz bu hesap için NSG akış günlükleri göndermek için etkinleştirilmiş ve yapılandırılmış 2 Nsg'ler varsa, son 25 saat günlüklerinin görüntülenebilir.

      ![Power BI ana][2]

1. Depolama hesabınızın erişim anahtarını girin. Geçerli erişim tuşlarını seçerek portal ve Azure depolama hesabınıza giderek bulabilirsiniz **erişim anahtarlarını** Ayarlar menüsünden. Tıklayın **Connect** sonra değişiklikleri uygulayın.

    ![Erişim anahtarları][3]

    ![erişim anahtarı 2][4]

4. Günlüklerinizi indirmek durumda, ayrıştırılmış ve artık önceden oluşturulmuş görselleri kullanabilir.

## <a name="understanding-the-visuals"></a>Görsellerin anlama

Şablonda sağlanan bir NSG akış günlüğü verileri anlamlı yardımcı görsellerin yer alır. Aşağıdaki resimlerde bir örnek Pano verilerle doldurmuş nasıl göründüğünü gösterir. Aşağıda daha ayrıntılı her bir görselin inceleyeceğiz 

![Power BI][5]
 
Bağlantıların çoğu dönemi boyunca başlattınız IP'ler belirtilen üst konuşmacılar visual gösterir. Kutularının boyutu, göreli bağlantı sayısını karşılık gelir. 

![toptalkers][6]

Aşağıdaki zaman serisi grafikleri dönemdeki akışların sayısını gösterir. Üst grafik, akış yönü göre segmentlere ve alt, (izin ver veya Reddet) yapılan karar tarafından bölümlenmiş. Bu görselle ne zaman içinde trafiği eğilimleri incelemek ve herhangi bir anormal ani nokta veya trafiği segmentasyonunun veya trafiği reddedebilir.

![flowsoverperiod][7]

Aşağıdaki grafikleri tarafından yapılan karar akış yönünü ve daha düşük ile bölümlenmiş üst ile ağ arabirimi başına akışlar bölümlenmiş gösterir. Bu bilgileri kullanarak, sanal makinelerinizin en diğerleri göre bildirilmesi ve belirli bir VM'ye trafik yüklenmekte olan Öngörüler elde edebilir izin verilen veya reddedilen.

![flowspernic][8]

Aşağıdaki halka tekerleği grafik, hedef bağlantı noktasına göre akışlar dökümünü gösterir. Bu bilgileri kullanarak, belirtilen süre içinde kullanılan en yaygın olarak kullanılan hedef bağlantı noktaları görüntüleyebilirsiniz.

![Halka][9]

Aşağıdaki çubuk grafik, NSG ve kural akışı gösterilmektedir. Bu bilgileri kullanarak Nsg'ler çoğu trafik ve trafik dökümü sorumlu üzerinde bir NSG kuralı tarafından görebilirsiniz.

![barchart][10]
 
Aşağıdaki bilgi grafikleri mevcut günlüklerinde, nokta ve yakalanan erken günlüğünün tarih üzerinden yakalanan akışlarının sayısı Nsg'ler ilgili bilgileri görüntüler. Bu bilgiler, hangi Nsg'ler hakkında bir fikir olduğunu günlüğe kaydedilmesini ve akışlar tarih aralığını sağlar.

![infochart1][11]

![infochart2][12]

Bu şablon, yalnızca en çok ilgilendiğiniz verileri görüntülemek izin vermek için aşağıdaki dilimleyiciler içerir. Kaynak grupları, Nsg'leri ve kuralları filtreleyebilirsiniz. Ayrıca, 5 demet bilgi, karar ve günlüğe yazıldığı zaman filtreleyebilirsiniz.

![Dilimleyiciler][13]

## <a name="conclusion"></a>Sonuç

Biz bu senaryoda Ağ İzleyicisi ve Power BI tarafından sağlanan ağ güvenlik grubu akış günlüklerini kullanarak, Görselleştirme ve anlama trafiği mümkün olduğunu göstermiştir. Belirtilen şablonu kullanarak Power BI, günlükleri doğrudan Storage'dan ve bunları yerel olarak işler. Şablon yüklenmesi için geçen süre istenen dosyaları sayısına bağlı olarak değişir ve indirilen dosyaların toplam boyutu.

Bu şablonu kendi gereksinimlerinize göre özelleştirmek çekinmeyin. Power BI ile ağ güvenlik grubu akış günlüklerini kullanabileceğiniz birçok birçok yolu vardır. 

## <a name="notes"></a>Notlar

* Günlükleri varsayılan olarak depolanır `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Başka bir dizinde başka veriler varsa, bunlar çekme ve işlem veri sorguları değiştirilmesi gerekir.

* Sağlanan şablon 1 GB'den fazla günlükleri ile kullanım için önerilmez.

* Günlükleri, büyük bir miktarını varsa, Data Lake veya SQL server gibi başka bir veri deposunu kullanan bir çözüm araştırmanız önerilir.

## <a name="next-steps"></a>Sonraki Adımlar

NSG akış günlüklerinizi Elastic Stack ile ederek görselleştirmeyi öğrenmek [açık kaynak araçlar kullanarak görselleştirme Azure Ağ İzleyicisi NSG akış günlükleri](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
