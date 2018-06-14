---
title: Hizmet Eşlemesi çözümünün adım adım tanıtımı | Microsoft Docs
description: Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen bir Azure çözümüdür.  Bu adımlı tanıtımda bir web uygulamasındaki sanal bir sorunu belirleyip tanılamak üzere Hizmet Eşlemesi kullanma işlemi gösterilmektedir.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: 35fe4e95eae8b63425abc8ed2970c0ad51073883
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
ms.locfileid: "28932486"
---
# <a name="self-paced-demo---service-map"></a>Adımlı tanıtım - Hizmet Eşlemesi
Bu adımlı tanıtımda bir web uygulamasındaki sanal bir sorunu belirleyip tanılamak üzere Azure’da [Hizmet Eşlemesi çözümü](operations-management-suite-service-map.md) kullanma işlemi gösterilmektedir.  Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler.  Ayrıca, performansı çözümlemenize ve sorunları belirlemenize yardımcı olmak üzere diğer hizmet ve çözümler tarafından toplanan verileri birleştirir.  Ayrıca, kök sorunu belirlemek üzere toplanan verilerin ayrıntısına inmek için [Log Analytics’teki günlük aramalarını](../log-analytics/log-analytics-log-searches.md) kullanırsınız.


## <a name="scenario-description"></a>Senaryo açıklaması
ACME Müşteri Portalı uygulamasının performans sorunları yaşadığına dair bir bildirim aldınız.  Yalnızca bu sorunların bugün yaklaşık olarak sabah 4:00’da başladığını biliyorsunuz.  Bir dizi web sunucusu dışında portalın bağımlı olduğu tüm bileşenlerin ne olduğundan emin değilsiniz.  

## <a name="components-and-features-used"></a>Kullanılan bileşenler ve özellikler
- [Hizmet Eşlemesi çözümü](operations-management-suite-service-map.md)
- [Log Analytics günlük aramaları](../log-analytics/log-analytics-log-searches.md)


## <a name="walk-through"></a>İzlenecek yol

### <a name="1-connect-to-the-oms-experience-center"></a>1. OMS Deneyim Merkezi’ne bağlanma
Bu kılavuzda, örnek verilerle tam bir Log Analytics ortamı sağlayan [Operations Management Suite Deneyim Merkezi](https://experience.mms.microsoft.com/) kullanılmaktadır. Öncelikle bu bağlantıyı takip edin, bilgilerinizi sağlayın ve ardından **Insight and Analytics** senaryosunu seçin.


### <a name="2-start-service-map"></a>2. Hizmet Eşlemesi’ni başlatma
**Hizmet Eşlemesi** kutucuğuna tıklayarak Hizmet Eşlemesi çözümünü başlatın.

![Hizmet Eşlemesi Kutucuğu](media/operations-management-suite-walkthrough-servicemap/tile.png)

Hizmet Eşlemesi konsolu gösterilir.  Sol bölme, ortamınızda Hizmet Eşlemesi aracısının yüklü olduğu bilgisayarların listesidir.  Bu listeden görüntülemek istediğiniz bilgisayarı seçin.

![Bilgisayar listesi](media/operations-management-suite-walkthrough-servicemap/computer-list.png)


### <a name="3-view-computer"></a>3. Bilgisayarı görüntüleme
Web sunucularının adının AcmeWFE001 ve AcmeWFE002 olduğunu biliyoruz; dolayısıyla buradan başlamak mantıklıdır.  **AcmeWFE001**’e tıklayın.  Bu işlem AcmeWFE001 eşlemesini ve tüm bağımlılıklarını gösterir.  Seçili bilgisayarda hangi işlemlerin çalıştığını ve bu işlemlerin hangi dış hizmetlerle iletişim kurduğunu görebilirsiniz.

![Web sunucusu](media/operations-management-suite-walkthrough-servicemap/web-server.png)

Bizi ilgilendiren konu web uygulamamızın performansı olduğu için **AcmeAppPool (IIS App Pool)** işlemine tıklayın.  Burada, bu işlemin ayrıntıları gösterilir ve bağımlılıkları vurgulanır.  

![Uygulama Havuzu](media/operations-management-suite-walkthrough-servicemap/app-pool.png)


### <a name="4-change-time-window"></a>4. Zaman penceresini değiştirme

Sorunun sabah 4:00’da başladığını duyduk; öyleyse, o sırada neler olduğuna bakalım. **Saat Aralığı**’na tıklayın ve saati 20 dakikalık süre için sabah 4:00 PST olarak değiştirin (geçerli tarihi değiştirmeyin ve yerel saat diliminize göre ayarlayın).

![Saat Seçici](./media/operations-management-suite-walkthrough-servicemap/time-picker.png)


### <a name="5-view-alert"></a>5. Uyarı görüntüleme

Şu anda **acmetomcat** bağımlılığı için gösterilen bir uyarı olduğunu görüyoruz, dolayısıyla bu potansiyel sorunumuzdur.  Uyarının ayrıntılarını görüntülemek için **acmetomcat** içindeki uyarı simgesine tıklayın.  Kritik CPU kullanımına sahip olduğumuzu görebiliriz ve daha fazla ayrıntı için uyarıyı genişletebiliriz.  Performansın yavaşlamasına neden olan sorun büyük olasılıkla budur. 

![Uyarı](./media/operations-management-suite-walkthrough-servicemap/alert.png)


### <a name="6-view-performance"></a>6. Performansı görüntüleme

Şimdi **acmetomcat**’e daha yakından bakalım.  **Acmetomcat** öğesinin sağ üst kısmına tıklayın ve **Sunucu Eşlemesini Yükle**’ye tıklayarak bu makineye ait ayrıntıları ve bağımlılıkları görüntüleyin. Şüphemizi doğrulamak için bu performans sayaçlarına biraz daha yakından bakabilirsiniz.  **Performans** sekmesini seçerek zaman aralığında [Log Analytics tarafından toplanan performans sayaçlarını](../log-analytics/log-analytics-data-sources-performance-counters.md) görüntüleyin.  İşlemci ve bellekte düzenli ani artışlar olduğunu görebiliriz.

![Performans](./media/operations-management-suite-walkthrough-servicemap/performance.png)


### <a name="7-view-change-tracking"></a>7. Değişiklik izlemeyi görüntüleme
Bu yüksek kullanıma neyin neden olabileceğini bulabilecek miyiz, görelim.  **Özet** sekmesine tıklayın.  Bu sekmede Log Analytics’in bilgisayardan topladığı başarısız bağlantılar, kritik uyarılar ve yazılım değişiklikleri gibi bilgiler verilir.  Alakalı yeni bilgileri olan bölümler zaten genişletilmiştir ve içerdikleri bilgileri incelemek üzere diğer bölümleri genişletebilirsiniz.


**Değişiklik İzleme** henüz açık değilse genişletin.  Bu bölümde [Değişiklik İzleme çözümü](../log-analytics/log-analytics-change-tracking.md) tarafından toplanan bilgiler gösterilir.  Bu zaman penceresinde bir yazılım değişikliği yapılmış gibi görünüyor.  Ayrıntılarını görmek için **Yazılım**’a tıklayın.  Sabah saat 4:00’dan sonra makineye bir yedekleme işlemi eklenmiş, dolayısıyla aşırı kaynak kullanımının nedeni bu gibi görünüyor.

![Değişiklik izleme](./media/operations-management-suite-walkthrough-servicemap/change-tracking.png)



### <a name="8-view-details-in-log-search"></a>8. Günlük Araması’nda ayrıntıları görüntüleme
Log Analytics çalışma alanında toplanan ayrıntılı performans bilgilerine bakarak sorunu doğrulayabiliriz.  **Uyarılar** sekmesine ve ardından **Yüksek CPU** uyarılarından birine tıklayın.  **Günlük Aramasında Göster**’e tıklayın.  Bu işlem, çalışma alanına kaydedilmiş verilere karşı [günlük aramaları](../log-analytics/log-analytics-log-searches.md) yapabileceğiniz Günlük Araması penceresini açar.  Hizmet Eşlemesi, ilgilendiğimiz uyarıyı almak için bir sorguya zaten girilmiştir.  

![Günlük araması](./media/operations-management-suite-walkthrough-servicemap/log-search.png)


### <a name="9-open-saved-search"></a>9. Kayıtlı aramayı açma
Bu uyarıyı oluşturan performans bilgileri toplama işlemiyle ilgili daha fazla ayrıntı alalım ve sorunların bu yedekleme işleminden kaynaklandığına yönelik şüphemizi doğrulayalım.  Saat aralığını **6 saat** olarak değiştirin.  Ardından **Sık Kullanılanlar**’a tıklayıp **Hizmet Eşlemesi** için kaydedilmiş aramalara inin.  Bu sorguları özellikle bu analiz için oluşturduk.  **Acmetomcat için CPU’ya Göre İlk 5 İşlem**’e tıklayın.

![Kayıtlı arama](./media/operations-management-suite-walkthrough-servicemap/saved-search.png)


Bu sorgu, **acmetomcat** üzerinde en fazla işlemci kullanan 5 işlemin bir listesini döndürür.  Günlük aramaları için kullanılan sorgu diline yönelik giriş bilgilerini almak için sorguyu inceleyebilirsiniz.  Diğer bilgisayarlardaki işlemlerle ilgileniyorsanız, bu bilgileri almak için sorguyu değiştirebilirsiniz.

Bu örnekte, yedekleme işleminin sürekli olarak uygulama sunucusu CPU’sunun yaklaşık %60’ını kullandığını görebiliyoruz.  Performans sorunumuzdan bu yeni işlemin sorumlu olduğu açıktır.  Çözümümüz bu yeni yedekleme yazılımını uygulama sunucusundan kaldırmak olacaktır.  Aslında bu işlemin bu kritik sistemlerde hiçbir zaman çalışmadığından emin olmaya yönelik ilkeler tanımlamak üzere Azure Otomasyonu tarafından yönetilen Desired State Configuration’ı (DSC) kullanabiliriz.


## <a name="summary-points"></a>Özet maddeleri
- [Hizmet Eşlemesi](operations-management-suite-service-map.md), tüm sunucu ve bağımlılıklarını bilmeseniz bile tüm uygulamanızın görünümünü sağlar.
- Hizmet Eşlemesi, uygulamanızla ve temel alınan altyapıyla ilgili sorunları belirlemenize yardımcı olmak üzere diğer yönetim çözümleri tarafından toplanan verileri ortaya çıkarır.
- [Günlük aramaları](../log-analytics/log-analytics-log-searches.md), Log Analytics çalışma alanında toplanan belirli verilere inmenizi sağlar.    

## <a name="next-steps"></a>Sonraki adımlar
- [Hizmet Eşlemesi](operations-management-suite-service-map.md) hakkında daha fazla bilgi edinin.
- Hizmet Eşlemesini [dağıtın ve yapılandırın](operations-management-suite-service-map-configure.md).
- Hizmet Eşlemesi için gereken ve aracılar tarafından depolanan işlem verilerini kaydeden [Log Analytics](../log-analytics/log-analytics-overview.md) hakkında bilgi edinin.