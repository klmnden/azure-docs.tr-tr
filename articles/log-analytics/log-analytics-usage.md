---
title: "Log Analytics&quot;te veri kullanımını çözümleme | Microsoft Belgeleri"
description: "OMS hizmetine gönderilen veri miktarını görüntülemek için Log Analytics&quot;teki Kullanım panosunu inceleyebilirsiniz."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/12/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 7e3d4b83fefdc70f292cf85b682cf8ed756bf4c5
ms.openlocfilehash: e7f04df679604f274c8ad9bf4daddc63c8b5418a
ms.lasthandoff: 02/22/2017


---
# <a name="analyze-data-usage-in-log-analytics"></a>Log Analytics'te veri kullanımını çözümleme
Log Analytics, verileri toplar ve düzenli aralıklarla OMS hizmetine gönderir.  OMS hizmetine gönderilen veri miktarını görüntülemek için **Log Analytics'teki Kullanım** panosunu inceleyebilirsiniz. Pano aynı zamanda çözümler tarafından ne kadar veri gönderildiğini ve sunucularınızın ne sıklıkta veri gönderdiğini de gösterir.

> [!NOTE]
> Ücretsiz hesabınız varsa OMS hizmetine günlük olarak en fazla 500 MB veri gönderebilirsiniz. Günlük limite ulaştıysanız veri çözümlemesi durur ve sonraki günün başlangıcında devam eder. Bu durumda OMS tarafından kabul edilmemiş ve işlenmemiş tüm verileri de yeniden göndermeniz gerekir.

Günlük kullanım limitini aştıysanız veya aşmak üzereyseniz, OMS hizmetine gönderdiğiniz veri miktarını azaltmak için isteğe bağlı olarak bir çözümü kaldırabilirsiniz. Çözümleri kaldırma hakkında daha fazla bilgi için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md).

![kullanım panosu](./media/log-analytics-usage/usage-dashboard01.png)

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

## <a name="understanding-nodes-for-oms-offers"></a>OMS teklifleri için düğümleri anlama

*Düğüm başına (OMS)* fiyatlandırma katmanındaysanız etkinleştirdiğiniz düğüm ve çözüm sayısına göre ücretlendirilirsiniz. Kullanım panosunun *teklifler* bölümünde her tekliften kaç düğümün kullanıldığını görebilirsiniz.

![kullanım panosu](./media/log-analytics-usage/log-analytics-usage-offerings.png)

## <a name="to-work-with-usage-data"></a>Kullanım verileriyle çalışma
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


## <a name="next-steps"></a>Sonraki adımlar
* Toplanan ve OMS’ye gönderilen ayrıntılı bilgileri özelliklere ve çözümlere göre görüntülemek için [Log Analytics’te günlük aramaları](log-analytics-log-searches.md) sayfasına bakın.

