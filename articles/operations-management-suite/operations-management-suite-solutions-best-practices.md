---
title: "OMSManagement çözüm en iyi uygulamalar | Microsoft Docs"
description: 
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: b3d07ad3164609a5628c0d9805de55a32870ab94
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="best-practices-for-creating-management-solutions-in-operations-management-suite-oms-preview"></a>Operations Management Suite (OMS) (Önizleme) yönetim çözümleri oluşturmak için en iyi uygulamalar
> [!NOTE]
> Bu, şu anda önizlemede OMS yönetim çözümleri oluşturmak için başlangıç belgesidir. Aşağıda açıklanan herhangi bir şema değiştirilebilir ' dir.  

Bu makale için en iyi yöntemler sağlar [bir yönetim çözümü dosyası oluşturma](operations-management-suite-solutions-solution-file.md) Operations Management Suite (OMS).  Diğer en iyi yöntemleri tanımlandığı gibi bu bilgileri güncelleştirilir.

## <a name="data-sources"></a>Veri kaynakları
- Veri kaynakları olabilir [Resource Manager şablonu ile yapılandırılmış](../log-analytics/log-analytics-template-workspace-configuration.md), ancak bir çözüm dosyasında eklenmemelidir.  Veri kaynaklarını yapılandırma şu anda çözümünüzü kullanıcının çalışma alanında mevcut yapılandırmanın üzerine anlamına ıdempotent olmadığını nedenidir.<br><br>Örneğin, çözümünüzü uyarı ve hata olayları uygulama olay günlüğünden gerektirebilir.  Bu veri kaynağı olarak çözümünüzde belirtirseniz, kullanıcı bu kendi çalışma alanında yapılandırılmış olsaydı bilgi olayları kaldırma riski oluşur.  Tüm olayları dahil edilmişse, kullanıcının çalışma alanında aşırı bilgi Olay toplama.

- Ardından, çözümünüzün standart veri kaynaklarından biri verilerden gerektiriyorsa, bu bir önkoşul olarak tanımlamalısınız.  Belgelerde müşteri veri kaynağı, kendi yapılandırmanız gerektiğini belirtin.  
- Ekleme bir [veri akışı doğrulaması](../log-analytics/log-analytics-view-designer-tiles.md) çözümünüzdeki tüm görünümleri için gerekli verileri toplanacak yapılandırılması gereken veri kaynaklarında kullanıcı istemek üzere ileti.  Bu iletiyi görüntüle döşemeyi görüntülenir gerekli verileri bulunamadı.


## <a name="runbooks"></a>Runbook'lar
- Ekleme bir [Otomasyonu zamanlaması](../automation/automation-schedules.md) çözümünüzdeki bir zamanlamaya göre çalıştırmak için gereken her runbook için.
- Dahil [IngestionAPI Modülü](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) çözümünüzdeki günlük analizi depoya veri yazma runbook'lar tarafından kullanılacak.  Çözüme yapılandırma [başvuru](operations-management-suite-solutions-solution-file.md#solution-resource) böylece çözüm kaldırılırsa, BT'nin bu kaynak.  Bu modül paylaşmak birden çok çözümü sağlar.
- Kullanım [Otomasyon değişkenleri](../automation/automation-schedules.md) kullanıcılar daha sonra değiştirmek isteyebilirsiniz çözüme değerlerini sağlamak için.  Çözüm değişkeni içerecek şekilde yapılandırılmış olsa bile değeri hala değiştirilebilir.

## <a name="views"></a>Görünümler
- Tüm çözümleri, Kullanıcı Portalı'nda görüntülenen tek bir görünüm içermesi gerekir.  Birden çok görünüm içerebilir [görselleştirme bölümleri](../log-analytics/log-analytics-view-designer-parts.md) farklı veri kümelerinin göstermek için.
- Ekleme bir [veri akışı doğrulaması](../log-analytics/log-analytics-view-designer-tiles.md) çözümünüzdeki tüm görünümleri için gerekli verileri toplanacak yapılandırılması gereken veri kaynaklarında kullanıcı istemek üzere ileti.
- Çözüme yapılandırma [içeren](operations-management-suite-solutions-solution-file.md#solution-resource) çözümü kaldırılırsa, BT'nin kaldırılan şekilde görünümü.

## <a name="alerts"></a>Uyarılar
- Çözüm yüklediğinizde, kullanıcı onları tanımlayabilirsiniz şekilde Çözüm dosyasındaki bir parametre olarak alıcılar listesi tanımlayın.
- Çözüme yapılandırma [başvuru](operations-management-suite-solutions-solution-file.md#solution-resource) uyarı kuralları bu kullanıcının kendi yapılandırmasını değiştirebilirsiniz.  Alıcı listesini değiştirerek, uyarı eşiğini değiştirme veya uyarı kuralı devre dışı bırakma gibi değişiklikler yapmak isteyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
* Basic işleminde size kılavuzluk [tasarlama ve bir yönetim çözümü oluşturma](operations-management-suite-solutions-creating.md).
* Bilgi edinmek için nasıl [bir çözüm dosyası oluşturun](operations-management-suite-solutions-solution-file.md).
* [Kaydedilmiş aramaları ve Uyarıları Ekle](operations-management-suite-solutions-resources-searches-alerts.md) Yönetimi çözümünüz için.
* [Görünümler ekleme](operations-management-suite-solutions-resources-views.md) Yönetimi çözümünüz için.
* [Otomasyon runbook'ları ve diğer kaynakları eklemek](operations-management-suite-solutions-resources-automation.md) Yönetimi çözümünüz için.

