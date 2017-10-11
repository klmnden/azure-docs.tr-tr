---
title: "Log Analytics'te veri kullanımını çözümleme | Microsoft Belgeleri"
description: "Log Analytics hizmetine ne kadar veri gönderildiğini görmek ve neden büyük miktarda veri gönderildiğiyle ilgili sorunları gidermek için Log Analytics'in Kullanım panosunu kullanın."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: magoedte
ms.openlocfilehash: 9a4709f298131722e9c473a19f7eee0aebf7e1e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-data-usage-in-log-analytics"></a>Log Analytics'te veri kullanımını çözümleme
Log Analytics toplanan veri miktarı, verileri hangi bilgisayarların gönderdiği ve gönderilen farklı veri türleri hakkındaki bilgileri içerir.  Log Analytics hizmetine gönderilen veri miktarını görmek için **Log Analytics Kullanımı** panosunu kullanın. Panoda her çözüm tarafından ne kadar veri toplandığı ve bilgisayarlarınızın ne kadar veri gönderdiği gösterilir.

## <a name="understand-the-usage-dashboard"></a>Kullanım panosunu anlama
**Log Analytics kullanım** panosu aşağıdaki bilgileri gösterir:

- Veri hacmi
    - Zaman içindeki veri hacmi (geçerli zaman kapsamınıza bağlı olarak)
    - Çözüme göre veri hacmi
    - Bir bilgisayar ile ilişkilendirilmemiş olan veriler
- Bilgisayarlar
    - Veri gönderen bilgisayarlar
    - Son 24 içinde hiç veri göndermeyen bilgisayarlar
- Teklifler
    - Öngörü ve Analiz düğümleri
    - Otomasyon ve Kontrol düğümleri
    - Güvenlik düğümleri
- Performans
    - Verileri toplamak ve dizinlemek için harcanan süre
- Sorgu listesi

![kullanım panosu](./media/log-analytics-usage/usage-dashboard01.png)

### <a name="to-work-with-usage-data"></a>Kullanım verileriyle çalışma
1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. **Hub** menüsünde **Diğer hizmetler**’e tıklayıp kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i tıklayın.  
    ![Azure hub'ı](./media/log-analytics-usage/hub.png)
3. **Log Analytics** panosunda çalışma alanlarınızın listesi gösterilir. Bir çalışma alanı seçin.
4. *Çalışma alanı* panosunda **Log Analytics kullanımı**’na tıklayın.
5. **Log Analytics Kullanım** panosunda **Zaman: Son 24 saat**’e tıklayarak zaman aralığını değiştirin.  
    ![zaman aralığı](./media/log-analytics-usage/time.png)
6. İlginizi çeken alanları gösteren kategori dikey pencerelerini görüntüleyin. Bir dikey pencere seçin [Günlük Arama](log-analytics-log-searches.md)’te ayrıntılarını görüntülemek istediğiniz öğeye tıklayın.  
    ![örnek veri kullanım dikey penceresi](./media/log-analytics-usage/blade.png)
7. Günlük Arama panosunda aramanın döndürdüğü sonuçları inceleyin.  
    ![örnek kullanım günlüğü araması](./media/log-analytics-usage/usage-log-search.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Toplanan veriler beklenenden fazlaysa uyarı oluşturma
Bu bölümde, aşağıdaki durumlarda nasıl uyarı oluşturulacağı açıklanır:
- Veri hacmi belirtilen bir miktarı aştığında.
- Veri hacminin belirtilen bir miktarı aşacağı tahmin edildiğinde.

Log Analytics [uyarılarında](log-analytics-alerts-creating.md) arama sorguları kullanılır. Aşağıdaki sorgu, son 24 saatte 100 GB'den fazla veri toplandığında bir sonuç verir:

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`

Aşağıdaki sorgu, ne zaman bir günde 100 GB'den fazla veri toplanacağını tahmin etmek için basit bir formül kullanır: 

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`

Farklı bir veri hacminde uyarıda bulunmak için, sorgulardaki 100 değerini uyarılmak istediğiniz GB sayısıyla değiştirin.

Toplanan veri beklenen miktarı aştığında size bildirilmesini sağlamak için, [Uyarı kuralı oluşturma](log-analytics-alerts-creating.md#create-an-alert-rule) başlığı altında açıklanan adımları kullanın.

İlk sorgu için, yani 24 saat içinde 100 GB'den fazla veri toplandığında uyarı oluştururken şu ayarları yapın:
- **Ad**: *24 saat içinde 100 GB'den büyük veri hacmi*
- **Önem derecesi**: *Uyarı*
- **Arama sorgusu**: `Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`
- **Zaman penceresi**: *24 Saat*.
- **Uyarı sıklığı**: Kullanım verileri yalnızca bir saat arayla güncelleştirildiğinden bir saat.
- **Şuna bağlı olarak uyarı oluştur**: *sonuç sayısı*
- **Sonuç sayısı**: *Şundan büyüktür: 0*

Uyarı kuralı olarak bir e-postayı, web kancasını veya runbook eylemini yapılandırmak için, [Uyarı kurallarına eylemler ekleme](log-analytics-alerts-actions.md) başlığı altında açıklanan adımları kullanın.

İkinci sorgu için, yani 24 saat içinde 100 GB'den fazla veri olacağı tahmin edildiğinde uyarı oluştururken şu ayarları yapın:
- **Ad**: *24 saat içinde veri hacminin 100 GB'den büyük olacağı tahmin ediliyor*
- **Önem derecesi**: *Uyarı*
- **Arama sorgusu**: `Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`
- **Zaman penceresi**: *3 Saat*.
- **Uyarı sıklığı**: Kullanım verileri yalnızca bir saat arayla güncelleştirildiğinden bir saat.
- **Şuna bağlı olarak uyarı oluştur**: *sonuç sayısı*
- **Sonuç sayısı**: *Şundan büyüktür: 0*

Uyarı aldığınızda, kullanımın neden beklenenden fazla olduğu konusundaki sorunları gidermek için aşağıdaki bölümde yer alan adımları kullanın.

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Kullanımın neden beklenenden daha yüksek olduğuyla ilgili sorunları giderme
Kullanım panosu, kullanımın (dolayısıyla da maliyetin) neden beklediğinizden yüksek olduğunu belirlemenize yardımcı olur.

Yüksek kullanımın nedeni aşağıdakilerden biri veya her ikisidir:
- Log Analytics'da beklenenden daha fazla veri gönderiliyordur
- Log Analytics'e beklenenden daha fazla düğüm veri gönderiyordur

### <a name="check-if-there-is-more-data-than-expected"></a>Beklenenden daha fazla veri olup olmadığını denetleme 
Kullanım sayfasında, çok veri toplanmasına neyin neden olduğunu belirlemenize yardımcı olacak iki önemli bölüm vardır.

*Zaman içindeki veri hacmi* grafiği, gönderilen toplam veri hacmini ve en çok veriyi gönderen bilgisayarları gösterir. Üstteki grafik, genel olarak veri kullanımınızın arttığını, sabit kaldığını veya azaldığını görmenizi sağlar. Bilgisayar listesi, en çok veri gönderen 10 bilgisayarı gösterir.

*Çözüme göre veri hacmi* grafiği, her çözüm tarafından gönderilen veri hacmini ve en çok veri gönderen çözümleri gösterir. Üstteki grafik, zaman içinde her çözüm tarafından gönderilen toplam veri hacmini gösterir. Bu bilgiler bir çözümün zaman içinde daha fazla veri gönderdiğini, yaklaşık aynı miktarda veri gönderdiğini veya daha az veri gönderdiğini belirlemenize olanak tanır. Çözüm listesinde, en çok veriyi gönderen 10 çözüm gösterilir. 

Bu iki grafik, tüm verileri görüntüler. Bazı veriler faturalanabilir, bazıları ise ücretsizdir. Yalnızca faturalanabilir verilere odaklanmak için arama sayfasındaki sorguyu `IsBillable=true` ifadesini içerecek şekilde değiştirin.  

![veri hacmi grafikleri](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

*Zaman içinde veri hacmi* grafiğine bakın. Belirli bir bilgisayara en çok veriyi gönderen çözümleri ve veri türlerini görmek için, bilgisayarın adına tıklayın. Listedeki ilk bilgisayarın adına tıklayın.

Aşağıdaki ekran görüntüsünde, bilgisayar için en fazla gönderilen veri *Günlük Yönetimi / Perf* veri türündedir. 

![bilgisayar için veri hacmi](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)

Ardından, *Kullanım* panosuna dönün ve *Çözüme göre veri hacmi* grafiğine bakın. Bir çözümle ilgili en fazla veriyi gönderen bilgisayarları görmek için, listede çözümün adına tıklayın. Listedeki ilk çözümün adına tıklayın. 

Aşağıdaki ekran görüntüsünde, Günlük Yönetimi çözümü için en çok veriyi *acmetomcat* bilgisayarının gönderdiği doğrulanır.

![çözüm için veri hacmi](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)

Gerekirse, bir çözüm veya veri türü içindeki büyük hacimleri belirlemek için ek çözümlemeler yapın. Örnek sorgular şunları içerir:

+ **Güvenlik** çözümü
  - `Type=SecurityEvent | measure count() by EventID`
+ **Günlük Yönetimi** çözümü
  - `Type=Usage Solution=LogManagement IsBillable=true | measure count() by DataType`
+ **Perf** veri türü
  - `Type=Perf | measure count() by CounterPath`
  - `Type=Perf | measure count() by CounterName`
+ **Event** veri türü
  - `Type=Event | measure count() by EventID`
  - `Type=Event | measure count() by EventLog, EventLevelName`
+ **Syslog** veri türü
  - `Type=Syslog | measure count() by Facility, SeverityLevel`
  - `Type=Syslog | measure count() by ProcessName`
+ **AzureDiagnostics** veri türü
  - `Type=AzureDiagnostics | measure count() by ResourceProvider, ResourceId`

Toplanan günlük hacmini azaltmak için aşağıdaki adımları kullanın:

| Yüksek veri hacminin kaynağı | Veri hacmi nasıl azaltılır |
| -------------------------- | ------------------------- |
| Güvenlik olayları            | [Yaygın veya en az güvenlik olaylarını](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) seçin <br> Güvenlik denetimi ilkesini yalnızca gerekli olayları toplayacak şekilde değiştirin. Özellikle, şunlarla ilgili olayları toplamak gerekip gerekmediğini gözden geçirin: <br> - [filtre platformu denetimi](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [kayıt defteri denetimi](https://docs.microsoft.com/windows/device-security/auditing/audit-registry)<br> - [dosya sistemi denetimi](https://docs.microsoft.com/windows/device-security/auditing/audit-file-system)<br> - [çekirdek nesnesi denetimi](https://docs.microsoft.com/windows/device-security/auditing/audit-kernel-object)<br> - [tanıtıcı değiştirme denetimi](https://docs.microsoft.com/windows/device-security/auditing/audit-handle-manipulation)<br> - [çıkarılabilir depolama birimi denetimi](https://docs.microsoft.com/windows/device-security/auditing/audit-removable-storage) |
| Performans sayaçları       | [Performans sayacı yapılandırmasını](log-analytics-data-sources-performance-counters.md) şöyle değiştirin: <br> - Koleksiyonun sıklığını azaltın <br> - Performans sayaçlarının sayısını azaltın |
| Olay günlükleri                 | [Olay günlüğü yapılandırmasını](log-analytics-data-sources-windows-events.md) şöyle değiştirin: <br> - Toplanan olay günlüklerinin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneğin, *Bilgi* düzeyindeki olayları toplamayın |
| Syslog                     | [Syslog yapılandırmasını](log-analytics-data-sources-syslog.md) şu şekilde değiştirin: <br> - Toplanan tesislerin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneği *Bilgi* ve *Hata Ayıklama* düzeyindeki olayları toplamayın |
| AzureDiagnostics           | Aşağıdaki amaçlarla kaynak günlüğü koleksiyonunu değiştirin: <br> - Log Analytics’e günlük gönderen kaynak sayısını azaltma <br> - Yalnızca gerekli günlükleri toplama |
| Çözüm ihtiyacı olmayan bilgisayarlardan toplanan çözüm verileri | Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) özelliğini kullanın. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Beklenenden çok düğüm olup olmadığını denetleme
*Düğüm başına (OMS)* fiyatlandırma katmanındaysanız kullandığınız düğüm ve çözüm sayısına göre ücretlendirilirsiniz. Kullanım panosunun *teklifler* bölümünde her tekliften kaç düğümün kullanıldığını görebilirsiniz.

![kullanım panosu](./media/log-analytics-usage/log-analytics-usage-offerings.png)

Belirli bir teklifle ilgili veri gönderen bilgisayarların tam listesini görüntülemek için **Tümünü göster...** öğesine tıklayın.

Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) özelliğini kullanın.


## <a name="next-steps"></a>Sonraki adımlar
* Arama dilini nasıl kullanacağınızı öğrenmek için bkz. [Log Analytics'te günlük aramaları](log-analytics-log-searches.md). Kullanım verilerinde başka analizler yapmak için arama sorgularını kullanabilirsiniz.
* Bir arama ölçütü karşılandığında size bildirilmesini sağlamak için, [Uyarı kuralı oluşturma](log-analytics-alerts-creating.md#create-an-alert-rule) başlığı altında açıklanan adımları kullanın
* Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) özelliğini kullanın
* [Yaygın veya en az güvenlik olaylarını](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) seçin
* [Performans sayacı yapılandırmasını](log-analytics-data-sources-performance-counters.md) değiştirin
* [Olay günlüğü yapılandırmasını](log-analytics-data-sources-windows-events.md) değiştirin
* [Syslog yapılandırmasını](log-analytics-data-sources-syslog.md) değiştirin
