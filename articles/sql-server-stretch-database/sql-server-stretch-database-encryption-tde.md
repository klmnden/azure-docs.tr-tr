---
title: "Saydam veri şifreleme Esnetme veritabanı - Azure için etkinleştirme | Microsoft Docs"
description: "Saydam veri şifreleme (TDE) Azure üzerinde SQL Server Esnetme veritabanı için etkinleştirin"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: barbkess
editor: 
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: ceb355d2ba872ed5d3886c6dc82ca75b1854db0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Azure üzerinde Esnetme veritabanı için saydam veri şifreleme (TDE) etkinleştir
> [!div class="op_single_selector"]
> * [Azure portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Saydam veri şifreleme (TDE) gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemelerinizi ve geri kalan işlem günlüğü dosyalarını uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirerek kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olur.

TDE, veritabanının tamamını Depolama veritabanı şifreleme anahtarını adlı bir simetrik anahtar kullanarak şifreler. Veritabanı şifreleme anahtarını bir yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik bir sunucu sertifikası Azure her sunucu için benzersizdir. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. TDE genel bir açıklaması için bkz [saydam veri şifreleme (TDE)].

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
TDE Azure etkinleştirmek için bir SQL Server Esnetme etkinleştirilmiş veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Veritabanında açmak [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde tıklayın **ayarları** düğmesi
3. Seçin **saydam veri şifreleme** seçeneği![][1]
4. Seçin **üzerinde** ayarlama ve ardından **Kaydet**
   ![][2]

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
Azure TDE devre dışı bırakmak için bir SQL Server Esnetme etkinleştirilmiş veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Veritabanında açmak [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde tıklayın **ayarları** düğmesi
3. Seçin **saydam veri şifreleme** seçeneği
4. Seçin **kapalı** ayarlama ve ardından **Kaydet**

<!--Anchors-->
[saydam veri şifreleme (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
