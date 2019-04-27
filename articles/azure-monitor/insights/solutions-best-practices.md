---
title: Azure en iyi uygulamaları Yönetimi çözümünde | Microsoft Docs
description: ''
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: d6d2414935bb5d1f095ad2b200acafa97b3b9b32
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60596647"
---
# <a name="best-practices-for-creating-management-solutions-in-azure-preview"></a>(Önizleme) Azure'da yönetim çözümleri oluşturmak için en iyi uygulamalar
> [!NOTE]
> Şu anda Önizleme aşamasında olan Azure yönetim çözümleri oluşturmak için başlangıç belgeleri budur. Aşağıda açıklanan herhangi bir şema tabi bir değişikliktir.  

Bu makale için en iyi uygulamalar sağlanır [yönetim çözüm dosyası oluşturuluyor](solutions-solution-file.md) azure'da.  Diğer en iyi yöntemleri tanımlandığı gibi bu bilgileri güncelleştirilir.

## <a name="data-sources"></a>Veri kaynakları
- Veri kaynakları olabilir [Resource Manager şablonu ile yapılandırılmış](../../azure-monitor/platform/template-workspace-configuration.md), ancak bir çözüm dosyasında eklenmemelidir.  Veri kaynakları yapılandırılarak şu anda çözümünüzü kullanıcının çalışma alanında mevcut yapılandırma üzerine yazabilir, yani bir kez etkili olduğunu nedenidir.<br><br>Örneğin, çözümünüz, uyarı ve hata olayları uygulama olay günlüğüne gerektirebilir.  Bu veri kaynağı olarak çözümünüzde belirtirseniz, kullanıcı bu kendi çalışma alanında yapılandırılmış olsaydı bilgi olayları kaldırma riski oluşur.  Tüm olaylar eklediyseniz, kullanıcının çalışma aşırı bilgi olaylarını toplama.

- Ardından çözümünüzü standart veri kaynaklardan birinden veri gerektiriyorsa, bu bir önkoşul olarak tanımlamalıdır.  Belgelerde, müşteri veri kaynağı, kendi yapılandırmanız gerektiğini belirtin.  
- Ekleme bir [veri akışı doğrulaması](../../azure-monitor/platform/view-designer-tiles.md) kullanıcıdan toplanacak gerekli verileri için yapılandırılması gereken veri kaynaklarında çözümünüzdeki tüm görünümleri iletisi.  Gerekli veri bulunamadığında görünümün kutucuğa bu ileti görüntülenir.


## <a name="runbooks"></a>Runbook'lar
- Ekleme bir [Otomasyon zamanlama](../../automation/automation-schedules.md) çözümünüzdeki bir zamanlamaya göre çalıştırmak için gereken her runbook için.
- Dahil [IngestionAPI Modülü](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) çözümünüzde, verileri Log Analytics deposuna yazma runbook'lar tarafından kullanılacak.  Çözümü yapılandırma [başvuru](solutions-solution-file.md#solution-resource) çözüm kaldırılırsa şekilde kalır. böylece bu kaynak.  Bu modül paylaşmak birden çok çözümü sağlar.
- Kullanım [Otomasyon değişkenleri](../../automation/automation-schedules.md) kullanıcılar daha sonra değiştirmek isteyebileceğiniz çözümdeki değerlerini sağlamak için.  Değerinin hala çözüm değişkeni içerecek şekilde yapılandırılmış olsa bile değiştirilebilir.

## <a name="views"></a>Görünümler
- Tüm çözümler, Kullanıcı Portalı'nda görüntülenen tek bir görünüm içermelidir.  Birden çok görünüm içerebilir [görselleştirme bölümleri](../../azure-monitor/platform/view-designer-parts.md) farklı veri kümelerini göstermek için.
- Ekleme bir [veri akışı doğrulaması](../../azure-monitor/platform/view-designer-tiles.md) kullanıcıdan toplanacak gerekli verileri için yapılandırılması gereken veri kaynaklarında çözümünüzdeki tüm görünümleri iletisi.
- Çözümü yapılandırma [içeren](solutions-solution-file.md#solution-resource) çözüm kaldırılırsa, BT kaldırılan şekilde görünümü.

## <a name="alerts"></a>Uyarılar
- Kullanıcı çözümü yükledikleri sırada tanımlayabilirsiniz böylece alıcı listesi çözüm dosyasını parametre olarak tanımlayın.
- Çözümü yapılandırma [başvuru](solutions-solution-file.md#solution-resource) uyarı kuralları, kullanıcının kendi yapılandırmasını değiştirebilirsiniz.  Alıcı listesi değiştirme, uyarı eşiğini değiştirme ya da uyarı kuralı devre dışı bırakma gibi değişiklikler yapmak isteyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
* ' In temel işleminde size kılavuzluk [tasarlama ve bina yönetim çözümünden](solutions-creating.md).
* Bilgi edinmek için nasıl [bir çözüm dosyası oluşturma](solutions-solution-file.md).
* [Kaydedilmiş aramaları ve Uyarıları Ekle](solutions-resources-searches-alerts.md) Yönetimi çözümünüz için.
* [Görünümler ekleme](solutions-resources-views.md) Yönetimi çözümünüz için.
* [Otomasyon runbook'ları ve diğer kaynaklar ekleme](solutions-resources-automation.md) Yönetimi çözümünüz için.

