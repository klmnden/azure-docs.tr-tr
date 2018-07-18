---
title: Sunucu düzeyinde güvenlik duvarı kuralları
description: Sunucu düzeyinde güvenlik duvarı kuralları
keywords: SQL bağlantı dizesi
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: 07bbba28b7e1b048ef3a4e1afa4db255967dc093
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117282"
---
1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Sol taraftaki listeden seçin **tüm hizmetleri**.

3. Kaydırın ve **SQL sunucuları**.

    ![Portalda Azure SQL veritabanı sunucunuzun bulma][b21-FindServerInPortal]
5. Filtre metin kutusuna sunucunuzun adını yazmaya başlayın. Bir satır görüntülenir.

6. Sunucunuz için bir satır seçin. Sunucunuz için bir dikey pencere görüntülenir.

7. Sunucu dikey penceresinde, seçin **ayarları**.

8. Seçin **Güvenlik Duvarı**.

    ![Ayarları seçin > Güvenlik Duvarı][b31-SettingsFirewallNavig]
9. Seçin **istemci Ekle IP**. İlk metin kutusuna, yeni kural için bir ad yazın.

10. Etkinleştirmek istediğiniz aralığı için düşük ve yüksek IP adresi değerleri yazın.

    * Düşük değer end ile sağlamak için kullanışlı olabilir **.0** ve sonunda yüksek değerli **.255**.

11. **Kaydet**’i seçin.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
