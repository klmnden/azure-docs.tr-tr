---
title: SQL veritabanları Azure Güvenlik Merkezi'nde üzerinde denetim ve tehdit Algılama'yı etkinleştirmek | Microsoft Docs
description: Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir ** denetim ve tehdit algılama SQL veritabanları ** özelliğini etkinleştirin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 8f4febdaa4497fee0dc690b59cd6eaa415c5e5cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866208"
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a>SQL veritabanları Azure Güvenlik Merkezi'nde denetim ve tehdit algılamayı etkinleştir
Azure Güvenlik Merkezi, Denetim ve tehdit algılama tüm SQL veritabanları için denetimi, kapatmanız ve tehdit algılama zaten etkin önerir. Denetim ve tehdit algılama, yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve anormallikleri, kavramanıza yardımcı olabilir iş endişeleri veya güvenlik ihlalleri şüpheli.

Denetim ayarladıktan sonra tehdit algılama ayarlarını ve e-postaları güvenlik uyarıları almak üzere yapılandırabilirsiniz. Tehdit algılama veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Bu, algılamak ve bunlar ortaya çıktığında, olası risklere yanıt olanak sağlar.

Bu öneri, yalnızca Azure SQL Hizmeti için geçerlidir; sanal makinelerde çalışan SQL içermez.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **önerileri** dikey penceresinde, select **SQL veritabanlarında denetimi etkinleştirme ve tehdit algılama**.  Bu açılır **SQL veritabanlarında denetimi etkinleştirme ve tehdit algılama** dikey.

   ![SQL veritabanlarında denetimi etkinleştirme][1]
2. Denetimi etkinleştirmek için bir SQL veritabanı seçin. Bu açılır **denetim ve tehdit algılama** dikey.

3. Üzerinde **denetim ve tehdit algılama** dikey penceresinde, select **ON** altında **denetim**.

   ![Denetim ve tehdit algılamayı etkinleştirme][2]
4. Adımları [SQL veritabanı tehdit algılama Azure portalında](../sql-database/sql-database-threat-detection-portal.md) etkinleştirmek ve tehdit algılama yapılandırmak ve anormal etkinlikler algılandığında güvenlik uyarı alacak olan e-postaları listesini yapılandırmak için.

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede "etkinleştirme denetim ve tehdit algılama SQL veritabanlarında." Güvenlik Merkezi öneriyi uygulamayı nasıl oluşturulacağını gösterir SQL veritabanınızın güvenliğini sağlama hakkında daha fazla bilgi için aşağıdakilere bakın:

* [SQL Veritabanınızı güvenli hale getirme](../sql-database/sql-database-security-overview.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
