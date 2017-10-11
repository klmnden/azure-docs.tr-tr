---
title: "Azure Güvenlik Merkezi'nde saydam veri şifreleme etkinleştirme | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir ** etkinleştirmek saydam veri şifreleme **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2a2963affdbff3710ad08f86c6ed4e6304335559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde saydam veri şifreleme etkinleştir
Azure Güvenlik Merkezi TDE zaten etkin değilse SQL veritabanlarını saydam veri şifreleme (TDE) etkinleştirmek önerir. TDE, verilerinizi korur ve veritabanınızı, ilişkili yedeklemelerinizi ve geri kalan, işlem günlüğü dosyalarını uygulamanızda değişiklik yapılması gerekmeksizin şifreleyerek uyumluluk gereksinimlerinin karşılanmasına yardımcı olur. Daha fazla bilgi edinmek için [saydam veri şifrelemesi ile Azure SQL veritabanı](https://msdn.microsoft.com/library/dn948096).

Bu öneri, yalnızca Azure SQL Hizmeti için geçerlidir; sanal makinelerde çalışan SQL içermez.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **önerileri** dikey penceresinde, select **etkinleştirmek saydam veri şifreleme**.
   ![Saydam Veri Şifrelemesini Etkinleştirme][1]
2. Bu açılır **etkinleştirmek saydam veri şifreleme SQL veritabanlarında** dikey. TDE etkinleştirmek için bir SQL veritabanı seçin.
   ![SQL TDE'nin etkinleştirmek için DB seçin][2]
3. Üzerinde **saydam veri şifreleme** dikey penceresinde, select **ON** veri şifreleme ve select altında **kaydetmek** dikey pencerenin üst Şeritte.
   ![TDE üzerinde Aç][3]

   TDE seçili SQL veritabanını etkinleştirildikten sonra **şifreleme durumu** değiştirir **şifreli**.    

   ![Şifreleme durumu][4]

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede "Saydam veri şifrelemeyi etkinleştirir." Güvenlik Merkezi öneriyi uygulamayı nasıl oluşturulacağını gösterir SQL TDE'nin hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Saydam veri şifrelemesi ile Azure SQL veritabanı](https://msdn.microsoft.com/library/dn948096)
* [Saydam veri şifreleme (TDE) ile çalışmaya başlama](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
