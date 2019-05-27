---
title: Stretch Database - Azure için saydam veri şifrelemesini etkinleştirme | Microsoft Docs
description: Azure'da SQL Server Stretch Database için saydam veri şifrelemesi (TDE) etkinleştir
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/14/2016
author: blazem-msft
ms.author: blazem
ms.reviewer: jroth
manager: jroth
ms.openlocfilehash: 61f556476958484b78b9c3dff2583eb6db043637
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003050"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Azure'da Stretch Database için saydam veri şifrelemesi (TDE) etkinleştirme
> [!div class="op_single_selector"]
> * [Azure portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Saydam veri şifrelemesi (TDE), kötü amaçlı etkinlik tehditlerine karşı gerçek zamanlı şifreleme ve şifre çözme veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları bekleme sırasında uygulamada değişiklik gerektirmeden gerçekleştirerek koruma yardımcı olur.

TDE, tüm veritabanı depolama veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı'nı kullanarak şifreler. Veritabanı şifreleme anahtarı, bir yerleşik bir sunucu sertifikası tarafından korunur. Azure her sunucu için benzersiz yerleşik sunucu sertifikasıdır. Microsoft, bu sertifikalar otomatik olarak en az 90 günde döndürür. TDE genel bir açıklaması için bkz. [saydam veri şifrelemesi (TDE)].

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
Azure için TDE'yi etkinleştirmek için Esnetme özellikli SQL Server veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Veritabanında açın [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde **ayarları** düğmesi
3. Seçin **saydam veri şifrelemesi** seçeneği ![][1]
4. Seçin **üzerinde** ayarlama ve ardından **Kaydet**
   ![][2]

## <a name="disabling-encryption"></a>Şifreleme devre dışı bırakma
Azure için TDE devre dışı bırakmak için Esnetme özellikli SQL Server veritabanından veri depolama veritabanı geçişi, şunları yapın:

1. Veritabanında açın [Azure portalı](https://portal.azure.com)
2. Veritabanı dikey penceresinde **ayarları** düğmesi
3. Seçin **saydam veri şifrelemesi** seçeneği
4. Seçin **kapalı** ayarlama ve ardından **Kaydet**

<!--Anchors-->
[Saydam Veri Şifrelemesi (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
