---
title: Azure Güvenlik Merkezi'nde SQL sunucularında denetim ve tehdit algılamayı etkinleştirme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerinin nasıl uygulanacağını gösterir **SQL sunucularında denetimi etkinleştirme ve tehdit algılama**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 5b41af83122d74fc766e6c5179d98803979269f7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60704671"
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde SQL sunucularında denetim ve tehdit algılamayı etkinleştirme
Azure Güvenlik Merkezi, denetimi açın ve tehdit algılama, Azure SQL sunucularında denetimi, tüm veritabanları için zaten etkin önerir. Denetim ve tehdit algılama, mevzuatla uyumluluk, veritabanı etkinliğini anlama ve ifade eden tutarsızlıklar ve anomaliler, elde yardımcı olabilir işletme sorunlarını veya şüpheli güvenlik ihlallerini.

Denetim ayarladıktan sonra tehdit algılama ayarları ve e-postaları güvenlik uyarıları almak için yapılandırabilirsiniz. Tehdit algılama, veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Bu, oluşunca potansiyel tehditleri algılayıp sağlar.

Bu öneri, yalnızca Azure SQL Hizmeti için geçerlidir; Bu, Azure altyapı hizmetlerine (Azure Iaas) sanal makinelerinizde çalışan SQL Server dahil değildir.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **önerileri** dikey penceresinde **SQL sunucularında denetimi etkinleştirme ve tehdit algılama**.  Bu açılır **SQL sunucularında denetimi etkinleştirme ve tehdit algılama** dikey penceresi.

   ![SQL sunucularında denetimi etkinleştirme][1]
2. Denetim ve tehdit algılamayı etkinleştirmek için bir SQL server'ı seçin. Bu açılır **denetim ve tehdit algılama** dikey penceresi.

3. Üzerinde **denetim ve tehdit algılama** dikey penceresinde **ON** altında **denetim**.

   ![Denetim ayarları Aç][2]
4. Bağlantısındaki [SQL veritabanı Azure Portalı'nda denetimi](../sql-database/sql-database-auditing-portal.md) denetim günlüklerinize depolanacağı depolama yapılandırmak için. Veri toplama için aboneliğe ait depolama hesabı, varsayılan depolama hesabıdır.
5. Bağlantısındaki [SQL veritabanı tehdit algılamayı kullanmaya başlama](../sql-database/sql-database-threat-detection.md) açabilir ve tehdit algılamayı yapılandırma ve anormal etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postaların listesini yapılandırın.

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede Güvenlik Merkezi önerisini "etkin denetim ve tehdit algılamayı SQL sunucularında." uygulamak nasıl oluşturulacağını gösterir SQL veritabanınızın güvenliğini sağlama hakkında daha fazla bilgi için şunlara bakın:

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
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
