---
title: İzleme ve Azure Güvenlik Merkezi'nde güvenlik olaylarını işleme | Microsoft Docs
description: Azure Vm'lerinizden ve Azure dışı bilgisayarların güvenlik olaylarını görmek için Güvenlik Merkezi'nin olaylar Panosu nasıl kullanabileceğinizi öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: rkarlin
ms.openlocfilehash: bc0fd83bd45e7c5c671b387d124cdddc75244ade
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64573516"
---
# <a name="monitoring-and-processing-security-events-in-azure-security-center"></a>İzleme ve Azure Güvenlik Merkezi'nde güvenlik olaylarını işleme
Olaylar Panosu, zaman ve ilgilenmenizi gerektiren önemli olayların bir listesi üzerinden toplanan güvenlik olay sayısı için genel bir bakış sağlar.  

> [!NOTE]
> Güvenlik olayları Pano 31 Temmuz 2019 üzerinde kullanımdan kaldırılacaktır. Daha fazla bilgi ve diğer hizmetler için bkz. [devre dışı bırakılması, Güvenlik Merkezi özelliklerini (Temmuz 2019)](security-center-features-retirement-july2019.md#menu_events).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="what-is-a-security-event"></a>Bir güvenlik olayı nedir?
Güvenlik Merkezi kullanan çeşitli güvenlik toplamak için Microsoft Monitoring Agent, makinelerinizden yapılandırmaları ve olayları ilgili ve bu olayları, çalışma Alanlarınızda depolanır. Bu tür verilerin örnekleri şunlardır: işletim sistemi günlükleri (Windows olay günlükleri), çalışan işler ve güvenlik çözümlerinden gelen olayları Güvenlik Merkezi ile tümleşiktir. Microsoft Monitoring Agent ayrıca kilitlenme bilgi dökümü dosyalarını da çalışma alanlarınıza kopyalar.

## <a name="requirements"></a>Gereksinimler
Bu özelliği kullanmak için çalışma alanınızı Log Analytics sürüm 2 çalıştırıyor olması ve Güvenlik Merkezi'nin standart katmanında olmanız gerekir. Güvenlik Merkezi'ni [fiyatlandırma sayfası](security-center-pricing.md) standart katman hakkında daha fazla bilgi.

## <a name="events-processed-dashboard"></a>İşlenen olaylar Panosu
Size erişim **olayları** Güvenlik Merkezi ana menüsünde veya Güvenlik Merkezi panosunda **genel bakış** dikey penceresi.  

![İşlenen olaylar Panosu][1]

**Olayları** altında kutucuğuna **Güvenlik Merkezi** Azure Vm'lerinizden ve Azure harici bilgisayarları Güvenlik Merkezi ile akan olay sayısını görüntüler.

**Olaylar Panosu** işlenen olaylar mesai sayısı ve olaylarının bir listesi hakkında genel bir bakış sağlar.

 ![Pano][2]

 Panonun üst kısmında geçen hafta işlenen tüm olaylar eğilimler. Alt önemli olayları ve türe göre tüm olaylar Panosu yarısını listeler:

 - **Önemli olayları** Güvenlik Merkezi'nin sağladığı olay sorgularını ve ve eklediğiniz olay sorguları içerir. Pano, ayrıca her önemli olay sayısı hızlıca görüntülemenizi sağlar.
 - **Türe göre tüm olaylar** alınan olay türleri ve her tür sayısını gösterir. SecurityEvent, CommonSecurityLog WindowsFirewall ve w3cııslog olay türü örnekleridir.

> [!NOTE]
> Önemli olayları dahil [web temeli değerlendirmesi](https://docs.microsoft.com/azure/operations-management-suite/oms-security-web-baseline-assessment). Web Temeli değerlendirmesinin amacı, savunmasız olabilecek web sunucusu ayarlarını bulmaktır.

## <a name="view-processed-event-details"></a>İşlenen olay ayrıntılarına bakın
1. Altında **Güvenlik Merkezi** ana menüsünde, select **olayları**.
2. **Olaylar Panosu** çalışma alanı seçicisini açın. Yalnızca bir çalışma alanı varsa, bu çalışma alanı Seçici görüntülenmez. Birden fazla çalışma alanı varsa, işlenen olay ayrıntılarını görüntülemek için bir çalışma alanı seçmeniz gerekir. Birden fazla çalışma alanı varsa, listeden bir çalışma alanı seçin.

   ![Çalışma alanı listesi][3]

3. **Olaylar Panosu** açılarak, seçilen çalışma alanı için olay ayrıntılarını gösterir. Önemli olayları ve türe göre tüm olaylar görüntüleyebilirsiniz.  Biz bu örnekte, seçili **önemli olayları**.

   ![Önemli olay][4]

4. Bir olay türü seçerek çalışma alanı altında daha fazla veri sorgulayabilirsiniz. Biz bu örnekte, seçili **SecurityEvent**.

   ![Bir olay türü seçme][5]

5. **Günlük arama** olayın türüne ek ayrıntı ile açılır.

   ![Günlük araması][6]

## <a name="add-a-notable-event"></a>Önemli olay Ekle
Güvenlik Merkezi, Giden kutusu önemli olayları sağlar. Kendi sorgu kullanımına dayalı önemli olayları ekleyebilirsiniz [Kusto sorgu dili](../log-analytics/log-analytics-search-reference.md). İçin getireceğiz **olaylar Panosu** önemli bir olay eklemek için.

1. Seçin **önemli olay Ekle**.

   ![Önemli olay Ekle][7]

2. **Özel önemli olay Ekle** açılır.  Altında **görünen ad**, önemli olay için bir ad girin. Altında **arama sorgusu**, olay için sorgunuzu girin.

   ![Sorgunuzu girin][8]

4. **Tamam**’ı seçin.

## <a name="update-your-workspace-for-events-processing"></a>Olayları işleme için çalışma alanınızı güncelleştirmek
Çalışma alanınız, Log Analytics sürüm 2 çalıştırıyor olması ve olay işleme Güvenlik Merkezi'nde kullanılacak Güvenlik Merkezi'nin standart katmanında olmanız gerekir. **Olaylar Panosu** çalışma alanı Seçici bu gereksinimleri karşılamayan çalışma alanlarını tanımlar.

![Çalışma alanı gereksinimlerini karşılamıyor][9]

Çalışma alanı satır:

- İçeren **yükseltme gerekiyor** -Log Analytics sürüm 2 için çalışma alanınızı güncelleştirmek için ihtiyacınız
- İçeren **planı YÜKSELT** – çalışma alanınızda, Güvenlik Merkezi'nin standart katmana yükseltme gerekiyor
- Boş - çalışma alanınız gereksinimlerini karşıladığından ve bir çalışma alanını seçmeyi alır, panoya

> [!NOTE]
> Altında **olaylar Panosu**, **olayları** sütunu her çalışma olayları miktarını gösterir.  Bu sütun, Güvenlik Merkezi'nin ücretsiz katmanı, bu çalışma alanına uygulandığından bazı çalışma alanları için boştur. Ücretsiz katmanı altında Güvenlik Merkezi olayları toplar ancak olayları Azure İzleyici günlüklerine kaydedilmez ve Pano kullanılabilir değil.
>
>

## <a name="update-workspace-to-log-analytics-version-2"></a>Çalışma alanı Log Analytics'e sürüm 2 güncelleştirme
1. Bir çalışma alanı seçin, **için güncelleştirme gerekiyor**.
2. **Arama yükseltmesi** açılır. Seçin **şimdi yükseltin**.

   ![Şimdi yükseltin][10]

## <a name="upgrade-to-security-centers-standard-tier"></a>Güvenlik Merkezi'nin standart katmana yükseltme
1. Bir çalışma alanı ile **planı YÜKSELT**.
2. **Olaylar Panosu** açılır. Seçin **deneyin olaylar Panosu**.

   ![Panosunu deneyin][11]

3. Altında **Gelişmiş güvenliğe ekleme**, Yükseltmekte olduğunuz çalışma alanını seçin.
4. Altında **fiyatlandırma**seçin **standart**.
5. **Kaydet**’i seçin.

   ![Standart katmana yükseltme][12]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'nin olay panonun nasıl kullanılacağı hakkında bilgi edindiniz. Pano nasıl çalıştığı hakkında daha fazla bilgi edinin ve kendi olay sorguları yazma bakın:

- [Azure İzleyici günlüklerine nedir?](../log-analytics/log-analytics-overview.md) – Azure İzleyici günlüklerine genel bakış
- [Günlük aramalarını anlama içinde Kusto](../log-analytics/log-analytics-log-search-new.md) - günlük aramaları Azure İzleyici günlüklerine nasıl kullanıldığını açıklar ve bir günlük araması oluşturmadan önce anlaşılması kavramlar sağlar
- [Kusto Arama başvurusu](../log-analytics/log-analytics-search-reference.md) – günlüğünde sorgu dilini kullanarak kendi olay sorguları yazmayı öğrenin

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

- [Güvenlik merkezine genel bakış](security-center-intro.md) – Describes Güvenlik Merkezi'nin temel özellikleri

<!--Image references-->
[1]: ./media/security-center-events-dashboard/events-processed.png
[2]: ./media/security-center-events-dashboard/dashboard.png
[3]: ./media/security-center-events-dashboard/view-processed-event.png
[4]: ./media/security-center-events-dashboard/notable-event.png
[5]: ./media/security-center-events-dashboard/events-by-type.png
[6]: ./media/security-center-events-dashboard/log-search-detail.png
[7]: ./media/security-center-events-dashboard/add-notable-event.png
[8]: ./media/security-center-events-dashboard/create-query.png
[9]: ./media/security-center-events-dashboard/requires-update.png
[10]: ./media/security-center-events-dashboard/search-upgrade.png
[11]: ./media/security-center-events-dashboard/try-dashboard.png
[12]: ./media/security-center-events-dashboard/onboard-workspace.png
