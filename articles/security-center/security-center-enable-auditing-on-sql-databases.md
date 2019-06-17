---
title: Azure Güvenlik Merkezi'nde SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerinin nasıl uygulanacağını gösterir **SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 1108265101f37433860d0112e4e80aee0002ab5c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127234"
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme
Azure Güvenlik Merkezi, Denetim, tüm SQL veritabanları için Denetim ve tehdit algılamayı etkinleştirmek ve tehdit algılama zaten etkin önerir. Denetim ve tehdit algılama, mevzuatla uyumluluk, veritabanı etkinliğini anlama ve ifade eden tutarsızlıklar ve anomaliler, elde yardımcı olabilir işletme sorunlarını veya şüpheli güvenlik ihlallerini.

Denetim ayarladıktan sonra tehdit algılama ayarları ve e-postaları güvenlik uyarıları almak için yapılandırabilirsiniz. Tehdit algılama, veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Bu, oluşunca potansiyel tehditleri algılayıp sağlar.

Bu öneri, yalnızca Azure SQL Hizmeti için geçerlidir; Bu, sanal makineler üzerinde çalışan SQL içermez.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **önerileri** dikey penceresinde **SQL veritabanlarında denetimi etkinleştirme ve tehdit algılamayı**.  Bu açılır **SQL veritabanlarında denetimi etkinleştirme ve tehdit algılamayı** dikey penceresi.

   ![SQL veritabanlarında denetimi etkinleştirme][1]
2. Denetimi etkinleştirmek için bir SQL veritabanı'nı seçin. Bu açılır **denetim ve tehdit algılama** dikey penceresi.

3. Üzerinde **denetim ve tehdit algılama** dikey penceresinde **ON** altında **denetim**.

   ![Denetim ve tehdit algılamayı etkinleştirme][2]
4. Bağlantısındaki [Azure portalında SQL veritabanı tehdit algılama](../sql-database/sql-database-threat-detection-portal.md) açabilir ve tehdit algılamayı yapılandırma ve anormal etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postaların listesini yapılandırın.

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede Güvenlik Merkezi'nin önerisini "etkin denetim ve tehdit algılama SQL veritabanlarında." uygulama nasıl oluşturulacağını gösterir SQL veritabanınızın güvenliğini sağlama hakkında daha fazla bilgi için şunlara bakın:

* [SQL Veritabanınızı güvenli hale getirme](../sql-database/sql-database-security-overview.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
