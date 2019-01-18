---
title: Görüntüleme ve Azure Log analytics'te veri çözümleme | Microsoft Docs
description: Azure İzleyici günlük sorgu deneyimi için Log Analytics günlük araması kullanıcıları için Yardım.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: bwren
ms.openlocfilehash: 89811e95ec24eb354020dd6384f6fdab6cee8c8f
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54392297"
---
# <a name="transition-from-log-analytics-log-search-to-azure-monitor-logs"></a>Azure İzleyici günlüklerine log Analytics günlük aramasından geçiş
Log analytics'te günlük araması, kısa süre önce Azure İzleyici günlükleri analiz etmek için yeni bir deneyim ile değiştirilmiştir. Günlük arama sayfası şu anda hala aracılığıyla erişilebilir **günlükleri (Klasik)** menü öğesi **Log Analytics çalışma alanları** sayfasında Azure portalı ancak 15 Şubat 2019 kaldırılacak. Bu makalede yardımcı olmak üzere iki deneyim arasındaki farklar günlük aramasından geçiş. 

## <a name="extract-custom-fields"></a>Özel alanları Ayıkla 
Günlük araması ', ayıklamak [özel alanlar](../platform/custom-fields.md) bulunduğu bir alanın menü eylemi içerdiği liste görünümündeki _tablosundan alanları Ayıkla_.

![Arama çıkartma alanları günlüğe kaydetme](media/log-search-transition/extract-fields-log-search.png)

Azure İzleyici günlüklerine özel alanlar Tablo görünümünde ayıklayın. Bir kayıt sol oka tıklayarak genişletin, ardından erişim için üç noktaya tıklayın _alanları Ayıkla_ eylem.

![Günlükleri alanları Ayıkla](media/log-search-transition/extract-fields-logs.png)

## <a name="filter-results-of-a-query"></a>Bir sorgunun sonuçlarını filtreleme
Günlük araması'nda, filtrelerinin listesi görüntülenir arama sonuçları teslim edilir. Bir filtre seçin ve tıklayın **Uygula** ile seçilen filtre sorgusunu çalıştıramadı.

![Günlük arama filtresi](media/log-search-transition/filter-log-search.png)

Azure İzleyici günlüklerine seçin *filtre (Önizleme)* filtreleri görüntülemek için. Ayrıca filtreleri görüntülemek için filtre simgesine tıklayın. Bir filtre seçin ve tıklayın **çalıştırın & Uygula** ile seçilen filtre sorgusunu çalıştıramadı.

![Günlükleri Filtrele](media/log-search-transition/filter-logs.png)

## <a name="functions-and-computer-groups"></a>İşlevler ve bilgisayar grupları
Günlük aramasında bir aramayı kaydetmek için seçin **kayıtlı aramalar** ve **Ekle** ad, kategori ve sorgu metni sağlamak için. Oluşturma bir [bilgisayar grubu](../platform/computer-groups.md) ekleyerek bir işlev diğer adı.

![Günlük araması'nı Kaydet](media/log-search-transition/save-search-log-search.png)

Azure İzleyici günlüklerine geçerli sorguyu kaydetmek için seçmeniz **Kaydet**. Değişiklik **Kaydet** için _işlevi_ ve sağlayan bir **işlev diğer adı** oluşturmak için bir [işlevi](functions.md).

![Günlüğü sorgusunu kaydedin](media/log-search-transition/save-query-logs.png)

## <a name="saved-searches"></a>Kayıtlı aramalar
Günlük araması'nda, kayıtlı aramalarınızı Eylem çubuğu öğesi kullanılabilen **kayıtlı aramalar**. Kaydedilen sorgularından Azure İzleyici günlüklerine erişim **sorgu Gezgini**.

![Sorgu gezgini](media/log-search-transition/query-explorer.png)

## <a name="take-action"></a>İşlem yap
Günlük araması'nda, aşağıdakileri yapabilirsiniz [runbook başlatma](take-action.md) selectionselecting tarafından bir arama sonuç **harekete**.

![İşlem yap](media/log-search-transition/take-action-log-search.png)

Azure İzleyici günlüklerine [günlük sorgudan bir uyarı oluşturmak](../platform/alerts-log.md). Uyarıya yanıt olarak çalıştırılacak bir veya daha fazla Eylemler ile bir eylem grubu yapılandırın.

![Eylem grubu](media/log-search-transition/action-group.png)

## <a name="next-steps"></a>Sonraki adımlar

- Yeni hakkında daha fazla bilgi [Azure İzleyici oturum deneyimi](get-started-portal.md).