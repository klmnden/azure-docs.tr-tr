---
title: İzleme ve Azure Güvenlik Merkezi'nde güvenlik olaylarını işleme | Microsoft Docs
description: Güvenlik olayları, Azure Vm'leri ve Azure olmayan bilgisayarlardan görmek için Güvenlik Merkezi'nin olayları Pano nasıl kullanabileceğinizi öğrenin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: terrylan
ms.openlocfilehash: 367067874b167268bd690a9e0b55412e92e08122
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23926532"
---
# <a name="monitoring-and-processing-security-events-in-azure-security-center"></a>İzleme ve Azure Güvenlik Merkezi'nde güvenlik olaylarını işleme
Olayları Pano zaman ve dikkat etmeniz gereken önemli olaylar listesi toplanan güvenlik olaylarının sayısına genel bir bakış sağlar.  

> [!NOTE]
> Bu özelliği kullanmak için çalışma alanınızı günlük analizi sürüm 2 çalıştırıyor olabilir ve Güvenlik Merkezi'nin standart katmanında olması gerekir. Güvenlik Merkezi bkz [fiyatlandırma sayfası](security-center-pricing.md) standart katmanı hakkında daha fazla bilgi.
>
>

## <a name="what-is-a-security-event"></a>Güvenlik olayı nedir?
Güvenlik Merkezi kullanımlarını çeşitli güvenlik toplamak için Microsoft Monitoring Agent, makinelerden ilgili yapılandırmaları ve olayları ve bu olaylar, çalışma alanları depolar. Bu tür verilerin örnekleri şunlardır: işletim sistemi günlükleri (Windows olay günlükleri), çalışan işler ve güvenlik çözümleri olaylarından Tümleşik Güvenlik Merkezi ile. Microsoft Monitoring Agent ayrıca kilitlenme bilgi dökümü dosyalarını da çalışma alanlarınıza kopyalar.

## <a name="events-processed-dashboard"></a>İşlenen olayların Panosu
Size erişim **olayları** Güvenlik Merkezi ana menüsünden veya Güvenlik Merkezi Panosu **genel bakış** dikey.  

![İşlenen olayların Panosu][1]

**Olayları** altında döşeme **Güvenlik Merkezi** Güvenlik Merkezi'nden Azure Vm'leri ve Azure olmayan bilgisayarları içine akan olayların sayısını görüntüler.

**Olayları Pano** işlenen olayların mesai sayısını ve olayların listesini genel bir bakış sağlar.

 ![Pano][2]

 Pano'nin üst yarısı son bir hafta içinde işlenen tüm olayları eğilimlerin. Alt önemli olaylar ve türe göre tüm olaylar Pano yarısı listeler:

 - **Önemli olayları** Güvenlik Merkezi sağlar olay sorguları ve oluşturmak ve eklediğiniz olay sorgularını içerir. Pano ayrıca her önemli olay sayısı içine hızlı bir görünümünü sağlar.
 - **Türe göre tüm olayları** alınma olay türlerini ve her tür için bir sayı gösterir. Olay türü SecurityEvent, CommonSecurityLog, WindowsFirewall ve W3CIISLog gösterilebilir.

> [!NOTE]
> Önemli olayları dahil [web temeli değerlendirme](https://docs.microsoft.com/azure/operations-management-suite/oms-security-web-baseline-assessment). Web Temeli değerlendirmesinin amacı, savunmasız olabilecek web sunucusu ayarlarını bulmaktır.

## <a name="view-processed-event-details"></a>İşlenen olay ayrıntılarını görüntüleyin
1. Altında **Güvenlik Merkezi** ana menü, select **olayları**.
2. **Olayları Pano** çalışma alanı seçicisinde açın. Yalnızca bir çalışma alanı varsa, bu çalışma alanı seçicisinde görünmez. Birden fazla çalışma alanı varsa, işlenen olay ayrıntılarını görüntülemek için bir çalışma alanı seçmeniz gerekir. Birden fazla çalışma alanı varsa, bir çalışma alanı listeden seçin.

  ![Çalışma alanı listesi][3]

3. **Olayları Pano** seçilen çalışma alanı için olay ayrıntılarına gösteren açar. Önemli olaylar ve türe göre tüm olayları görüntüleyebilirsiniz.  Bu örnekte, seçtik **önemli olayları**.

  ![Önemli olay][4]

4. Olay türü seçerek çalışma alanı altında daha fazla veri sorgulama yapabilirsiniz. Bu örnekte, seçtik **SecurityEvent**.

  ![Olay türü seçme][5]

5. **Arama oturum** ek ayrıntılı olay türü ile açılır.

  ![Günlük araması][6]

## <a name="add-a-notable-event"></a>Önemli bir olay ekleyin
Güvenlik Merkezi Giden kutusu önemli olayları sağlar. Kendi sorgu kullanımına dayalı önemli olayları ekleyebilirsiniz [günlük analizi sorgu dili](../log-analytics/log-analytics-search-reference.md). İçin getireceğiz **olayları Pano** önemli bir olay eklemek için.

1. Seçin **önemli olay eklemek**.

  ![Önemli bir olay ekleyin][7]

2. **Özel dikkat çekici olay eklemek** açar.  Altında **görünen adı**, önemli olay için bir ad girin. Altında **arama sorgusu**, olayı için sorgunuzu girin.

  ![Sorgunuz girin][8]

4. **Tamam**’ı seçin.

## <a name="update-your-workspace-for-events-processing"></a>Olayları işleme için çalışma alanınızı güncelleştir
Çalışma alanınızı günlük analizi sürüm 2 çalıştırıyor olabilir ve Güvenlik Merkezi'nin standart katmanı Güvenlik Merkezi'nde Olay işleme kullanmak için açık olması gerekir. **Olayları Pano** çalışma alanı seçicisinde bu gereksinimleri karşılamayan çalışma alanları tanımlar.

![Çalışma alanı gereksinimlerini karşılamıyor][9]

Çalışma alanı satır:

- İçeren **gerektirir güncelleştirme** -sürüm 2 günlük analizi çalışma alanınız güncelleştirmeniz gerekir
- İçeren **yükseltme planlama** – çalışma alanınızı Güvenlik Merkezi'nin standart katmanına yükseltme gerekiyor
- Boş - çalışma alanınızı gereksinimlerini karşıladığından ve bir çalışma alanı seçerek alır, Pano için

> [!NOTE]
> Altında **olayları Pano**, **olayları** sütun her çalışma olayları miktarını belirtir.  Bu çalışma alanına Güvenlik Merkezi'nin ücretsiz katmanı uygulandığından bu bazı çalışma alanları için boş bir sütundur. Ücretsiz katmanı altında Güvenlik Merkezi olayları toplar ancak olayları günlük analizi kaydedilmez ve Panoda kullanılabilir değil.
>
>

## <a name="update-workspace-to-log-analytics-version-2"></a>Sürüm 2 günlük analizi çalışma alanı güncelleştir
1. Bir çalışma alanı seçin, **güncelleştirme gerekiyor**.
2. **Arama yükseltme** açar. Seçin **Şimdi Yükselt**.

  ![Şimdi Yükselt][10]

## <a name="upgrade-to-security-centers-standard-tier"></a>Güvenlik Merkezi'nin standart katmanına yükseltme
1. Bir çalışma alanıyla seçin **yükseltme planlama**.
2. **Olayları Pano** açar. Seçin **deneyin olayları Pano**.

  ![Pano deneyin][11]

3. Altında **Onboarding Gelişmiş Güvenlik**, Yükseltmekte olduğunuz çalışma alanını seçin.
4. Altında **fiyatlandırma**seçin **standart**.
5. **Kaydet**’i seçin.

  ![Standart katmana yükseltin][12]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'nin olay Panoyu kullanma hakkında bilgi edindiniz. Pano nasıl çalıştığı hakkında daha fazla bilgi edinin ve kendi olay sorguları yazmaya bakın:

- [Log Analytics nedir?](../log-analytics/log-analytics-overview.md) – Günlük analizi genel bakış
- [Anlama günlük arar günlük analizi](../log-analytics/log-analytics-log-search-new.md) - günlük aramaları günlük analizi nasıl kullanıldığını açıklar ve günlük arama oluşturmadan önce anlaşılmalıdır kavramları sağlar
- [Günlük analizi arama başvuru](../log-analytics/log-analytics-search-reference.md) – günlüğüne sorgu dili kullanarak kendi olay sorguları yazma öğrenin

Güvenlik Merkezi hakkında daha fazla bilgi için bkz:

- [Güvenlik Merkezi'ne genel bakış](security-center-intro.md) – Describes Güvenlik Merkezi'nin anahtar özellikleri

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
