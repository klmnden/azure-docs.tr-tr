---
title: "Power BI ile visualizing Azure ağ güvenlik grubu akış günlükleri | Microsoft Docs"
description: "Bu sayfa, NSG akış günlükleri Power BI ile görselleştirme açıklar."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 19bd7ed4bab915d7918a192a046653666cfaa498
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Power BI ile visualizing ağ güvenlik grubu akış günlükleri

Ağ güvenlik grubu akış günlükleri, ağ güvenlik grupları giriş ve çıkış IP trafiği hakkındaki bilgileri görüntülemek izin verir. Bunlar akışı, giden ve gelen akış kuralı başına temelinde, akış NIC uygulandığı günlüklerini göster, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol), 5-tanımlama grubu bilgilerini ve trafiğe izin verilen veya reddedilen.

Günlük dosyalarını el ile arama yaparak akışı verilerini günlüğe Öngörüler elde zor olabilir. Bu makalede, en son akış günlüklerinizi görselleştirmek ve ağınızdaki trafiği hakkında bilgi almak için bir çözüm sunuyoruz.

## <a name="scenario"></a>Senaryo

Aşağıdaki senaryoda, havuz bizim akış NSG günlüğü verilerini yapılandırdıktan depolama hesabı için Power BI desktop bağlayın. Biz bizim depolama hesabınıza bağlandıktan sonra Power BI indirir ve görsel bir ağ güvenlik grupları tarafından günlüğe kaydedilen trafiği sağlamak için günlükleri ayrıştırır.

İnceleyebilirsiniz şablonunda sağlanan görselleri kullanma:

* Üst Talkers
* Akış zaman serisi veri yönü ve kural karar tarafından
* Ağ arabirimi MAC adresiyle akışlar
* NSG ve kural tarafından akışlar
* Hedef bağlantı noktası tarafından akışlar

Sağlanan şablon düzenlenebilir nitelikte. yeni veri, görsel eklemek için değiştirin ya da sorgular, gereksinimlerinize uyacak şekilde düzenleyin.

## <a name="setup"></a>Kurulum

Başlamadan önce ağ güvenlik grubu akış hesabınızda bir veya daha çok ağ güvenlik grupları etkin günlüğü olması gerekir. Akış günlükleri ağ güvenliği etkinleştirme yönergeleri için aşağıdaki makaleye bakın: [akış günlüğü ağ güvenlik grupları için giriş](network-watcher-nsg-flow-logging-overview.md).

Ayrıca Power BI Desktop istemcisi makinenizde ve yeterli boş alan indirmek ve depolama hesabınız var. günlük verileri yüklemek için makinenizde yüklü olması gerekir.

![Visio diyagramı][1]

### <a name="steps"></a>Adımlar

1. Karşıdan yükleyip Power BI Desktop uygulamasında aşağıdaki Power BI şablonu açmak [Ağ İzleyicisi Powerbı akış günlükleri şablonu](https://aka.ms/networkwatcherpowerbiflowlogstemplate)
1. Gerekli sorgu parametrelerini girin
    1. **StorageAccountName** – yüklemek ve görselleştirme için istediğiniz NSG akış günlükleri içeren depolama hesabının adını belirtir.
    1. **NumberOfLogFiles** – indirin ve Power BI'da görselleştirin istediğiniz günlük dosyası sayısını belirtir. Örneğin, 50 belirtilirse, en son 50 günlük dosyaları. Bu hesap için NSG akış günlükleri göndermek için etkinleştirilmiş ve yapılandırılmış 2 Nsg'ler sahibiz FF sonra son 25 saat günlüklerinin görüntülenebilir.

    ![Power BI ana][2]

1. Depolama hesabınız için erişim anahtarı girin. Portal ve seçildiğinde Azure depolama hesabınıza giderek geçerli erişim tuşları bulabilirsiniz **erişim tuşları** Ayarlar menüsünden. Tıklatın **Bağlan** değişiklikler uygulanır.

    ![Erişim tuşları][3]

    ![erişim tuşu 2][4]

4.  Günlüklerinizi indirmek durumda, ayrıştırılmış ve önceden oluşturulmuş görselleri şimdi kullanabilir.

## <a name="understanding-the-visuals"></a>Görsel anlama

Şablonda sağlanan NSG akış günlük verilerini anlamlı Yardım görselleri kümesidir. Aşağıdaki görüntüleri Pano verilerle doldurmuş, nasıl göründüğünü, bir örnek göstermektedir. Daha ayrıntılı her görsel aşağıda inceleyeceğiz 

![powerbı][5]
 
Çoğu bağlantı süresi içinde başlattınız IP'leri belirtilen üst Talkers visual gösterir. Kutuları boyutunu göreli bağlantıları için sayısına karşılık gelir. 

![toptalkers][6]

Aşağıdaki zaman serisi grafikleri süre boyunca akışlarının sayısını gösterir. Üst grafik akış yönü tarafından ayrılmış ve alt (izin verme veya reddetme) yapılan karar tarafından ayrılmış. Bu görsel ile trafiği eğilimleri zaman içinde inceleyin ve herhangi bir anormal ani nokta veya trafiği veya trafiği segmentasyonunun reddedin.

![flowsoverperiod][7]

Aşağıdaki grafikleri tarafından yapılan karar akış yönü ve alt tarafından kesimli üst sahip ağ arabirimi başına akışları kesimli gösterir. Bu bilgilerle Vm'leriniz hangisinin en başkalarının göre bildirilmesi ve belirli bir VM'yi trafiği yüklenmekte olan Öngörüler geçirmesine izin verilen veya reddedilen.

![flowspernic][8]

Aşağıdaki halka tekerleği grafik hedef bağlantı noktası tarafından akışları dökümünü gösterir. Bu bilgileri kullanarak belirtilen dönem içinde kullanılan en yaygın olarak kullanılan hedef bağlantı noktalarını görüntüleyebilirsiniz.

![Halka][9]

Aşağıdaki çubuk grafik NSG ve kural tarafından akışı gösterilmektedir. Bu bilgi ile kural tarafından üzerinde bir NSG sorumlu trafiğinin çoğu ve trafik dökümünü Nsg'ler görebilirsiniz.

![barchart][10]
 
Aşağıdaki bilgi grafikleri dönemi ve yakalanan erken günlüğünün tarih yakalanan akışlarının sayısı günlüklerine mevcut Nsg'ler hakkında bilgileri görüntüler. Bu bilgiler, hangi Nsg'ler hakkında bir fikir olan günlüğe kaydedilmesini ve akışlar tarih aralığını sunar.

![infochart1][11]

![infochart2][12]

Bu şablon, yalnızca en çok ilgilendiğiniz verileri görüntülemek izin vermek için aşağıdaki dilimleyiciler içerir. Kaynak grupları, Nsg'ler ve kuralları filtreleyebilirsiniz. Ayrıca, 5-tanımlama bilgileri, karar ve günlüğe yazıldığı saatte filtreleyebilirsiniz.

![dilimleyicileri][13]

## <a name="conclusion"></a>Sonuç

Biz, bu senaryoda Ağ İzleyicisi'ni ve Power BI tarafından sağlanan ağ güvenlik grubu akış günlükleri kullanarak, biz görselleştirmek ve trafiği anlamak bağlanabildiğinizden emin gösterdi. Belirtilen şablonu kullanarak Power BI günlükleri doğrudan depolama biriminden yükler ve yerel olarak işler. Şablon yüklenmesi için geçen süre istenen dosyaları sayısına bağlı olarak değişir ve dosyaların toplam boyutu yüklenir.

Bu şablon, gereksinimleriniz için özelleştirmek çekinmeyin. Ağ güvenlik grubu akış günlükleri ile Power BI kullanabileceğiniz birçok pek çok yolu vardır. 

## <a name="notes"></a>Notlar

* Günlükler varsayılan olarak depolanır`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Diğer verileri başka bir dizinde varsa bunlar çekme ve işlem verileri sorgular değiştirilmesi gerekir.

* Sağlanan şablon, 1 GB'den fazla günlükleri ile kullanım için önerilmez.

* Günlükleri, büyük bir miktarını varsa, Data Lake veya SQL server gibi başka bir veri deposunu kullanan bir çözüm araştırmanız önerilir.

## <a name="next-steps"></a>Sonraki Adımlar

NSG akış günlüklerinizi Elastick yığın ile ziyaret ederek görselleştirmek öğrenin [açık kaynaklı Araçları'nı kullanarak Azure Ağ İzleyicisi NSG görselleştirmek akış günlükleri](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)

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
