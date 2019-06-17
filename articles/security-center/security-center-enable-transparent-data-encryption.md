---
title: Azure Güvenlik Merkezi'nde saydam veri şifrelemesini etkinleştirme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerinin nasıl uygulanacağını gösterir **saydam veri şifrelemesini etkinleştirme**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: d5ddec40a1b20e377ec18ce871018f674557e7b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60704041"
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde saydam veri şifrelemesini etkinleştirme
Azure Güvenlik Merkezi, TDE zaten etkin değilse, SQL veritabanlarında saydam veri şifrelemesi (TDE) etkinleştirmenizi önerir. TDE, verilerinizi korur ve, veritabanı, ilişkili yedeklemeler ve işlem günlük dosyaları, bekleyen değişiklikleri uygulamanıza gerek kalmadan şifreleyerek uyumluluk gereksinimlerini karşılamanıza yardımcı olur. Daha fazla bilgi edinmek için [Azure SQL veritabanı ile saydam veri şifrelemesi](https://msdn.microsoft.com/library/dn948096).

Bu öneri, yalnızca Azure SQL Hizmeti için geçerlidir; sanal makineler üzerinde çalışan SQL içermez.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **önerileri** dikey penceresinde **saydam veri şifrelemesini etkinleştirme**.
   ![Saydam Veri Şifrelemesini Etkinleştirme][1]
2. Bu açılır **SQL veritabanlarında saydam veri şifrelemesini etkinleştirme** dikey penceresi. Üzerinde TDE'yi etkinleştirmek için bir SQL veritabanı'nı seçin.
   ![SQL DB, üzerinde TDE'yi etkinleştirmek için işaretleyin][2]
3. Üzerinde **saydam veri şifrelemesi** dikey penceresinde **ON** veri şifreleme ve select altında **Kaydet** dikey pencerenin üst Şeritte.
   ![TDE'yi etkinleştirmek][3]

   Seçili SQL veritabanında TDE etkinleştirildikten sonra **şifreleme durumu** değişir **şifreli**.    

   ![Şifreleme durumu][4]

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede Güvenlik Merkezi'nin önerisini "Saydam veri şifrelemesini etkinleştirme" uygulamak nasıl oluşturulacağını gösterir SQL TDE'nin hakkında daha fazla bilgi için şunlara bakın:

* [Azure SQL veritabanı ile saydam veri şifrelemesi](https://msdn.microsoft.com/library/dn948096)
* [Saydam veri şifrelemesi (TDE) ile çalışmaya başlama](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
