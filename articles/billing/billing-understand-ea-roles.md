---
title: Azure'da Kurumsal için yönetici rollerini anlama | Microsoft Docs
description: Azure'da Kurumsal yönetici rolleri hakkında bilgi edinin.
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2018
ms.author: banders
ms.openlocfilehash: 98ed28af8df246549fb521a81f1968e1f5c28cc4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370722"
---
# <a name="understand-azure-enterprise-agreement-administrative-roles-in-azure"></a>Azure Kurumsal Anlaşma Azure yönetici rollerini anlama

Kuruluşunuzun kullanım yönetmek ve Azure harcamalarınızı yardımcı olmak için bir Kurumsal Anlaşma'ile (EA) müşterileri, beş farklı yönetim rolleri atayabilirsiniz:

- Kuruluş Yöneticisi
- Kuruluş Yöneticisi (salt okunur)
- Bölüm Yöneticisi
- Departman Yöneticisi (salt okunur)
- Hesap Sahibi
 
Bu roller belirli Azure Kurumsal anlaşmalar yönetmeye ve Azure kaynaklarına erişimi denetlemek için sahip yerleşik roller yanı sıra. Daha fazla bilgi için [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md).

Aşağıdaki bölümlerde, her rolün özellikleri ve sınırlamaları açıklanmaktadır.

## <a name="user-limit-for-admin-roles"></a>Yönetici rolleri için Kullanıcı sınırı

|Rol| Kullanıcı sınırı|
|---|---|
|Kuruluş Yöneticisi|Sınırsız|
|Kuruluş Yöneticisi (salt okunur)|Sınırsız|
|Bölüm Yöneticisi|Sınırsız|
|Departman Yöneticisi (salt okunur)|Sınırsız|
|Hesap Sahibi|hesap başına 1<sup>1</sup>|

<sup>1</sup> her hesabı benzersiz bir Microsoft hesabı ya da iş veya Okul hesabı gerektirir.

## <a name="organization-structure-and-permissions-by-role"></a>Kuruluş yapısı ve rol izinleri

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okunur)|Bölüm Yöneticisi|Departman Yöneticisi (salt okunur)|Hesap Sahibi|
|---|---|---|---|---|---|
|Görünüm kuruluş yöneticileri|✔|✔|✘|✘|✘|
|Kuruluş Yöneticileri Ekle Kaldır|✔|✘|✘|✘|✘|
|Bildirim ilgili kişileri görüntüle<sup>2</sup> |✔|✔|✘|✘|✘|
|Bildirim ilgili kişileri ekleyip<sup>2</sup> |✔|✘|✘|✘|✘|
|Oluşturun ve Departmanlar yönetin |✔|✘|✘|✘|✘|
|Görünüm departman yöneticilerinin|✔|✔|✔|✔|✘|
|Ekleme veya departman yöneticilerinin kaldırma|✔|✔|✔|✘|✘|
|Kayıt görünümü hesapları |✔|✔|✔<sup>3</sup>|✔<sup>3</sup>|✘|
|Kayıt için hesapları ekleyin ve hesap sahibini değiştirme|✔|✘|✔<sup>3</sup>|✘|✘|
|Abonelikler ve Abonelik izinlerine oluşturun ve yönetin|✘|✘|✘|✘|✔|

- <sup>2</sup> bildirim ilgili kişileri Azure Kurumsal anlaşmasına hakkında iletişim e-posta gönderilir.
- <sup>3</sup> departmanınız hesaplarına görev sınırlıdır.


## <a name="usage-and-costs-access-by-role"></a>Kullanımı ve maliyetleri erişim rolüne göre

|Görevler| Kuruluş Yöneticisi|Kuruluş Yöneticisi (salt okunur)|Bölüm Yöneticisi|Departman Yöneticisi (salt okunur) |Hesap Sahibi|
|---|---|---|---|---|---|
|Parasal taahhüt dahil olmak üzere görünümü kredi bakiyesi|✔|✔|✘|✘|✘|
|Harcama kotalarını görünümü bölümü|✔|✔|✘|✘|✘|
|Departman harcama kotalarını ayarla|✔|✘|✘|✘|✘|
|Kuruluşunuzun Kurumsal Anlaşma fiyat listesini görüntüleme|✔|✔|✘|✘|✘|
|Kullanım ve maliyet ayrıntılarını görüntüle|✔|✔|✔<sup>4</sup>|✔<sup>4</sup>|✔<sup>5</sup>|
|Azure portalında kaynakları yönetme|✘|✘|✘|✘|✔|

- <sup>4</sup> kuruluş yöneticisi etkinleştirmenizi istemektedir **DA ücretleri görüntüle** Enterprise Portal'da ilkesi. Departman Yöneticisi daha sonra bölümü için maliyet ayrıntılarını görebilirsiniz.
- <sup>5</sup> kuruluş yöneticisi etkinleştirmenizi istemektedir **AO ücretleri görüntüle** Enterprise Portal'da ilkesi. Hesap sahibi, daha sonra hesabı için maliyet ayrıntılarını görebilirsiniz.


## <a name="pricing-in-azure-portal"></a>Azure portalında fiyatlandırması

Azure portalında yönetici rolünüz ve ilkeleri görüntüle ücretleri Kurumsal yönetici tarafından nasıl ayarlanacağını bağlı olarak farklı fiyatlandırma görebilirsiniz. Azure portalında gördüğünüz fiyatlandırma etkileyen iki ilke Enterprise Portal'da şunlardır:

- DA ücretleri görüntüle
- Saniye başına AO ücretleri görüntüle

Bu ilkeler hakkında bilgi edinmek için bkz: [Azure için fatura bilgilerini erişimi yönetme](billing-manage-access.md).

Aşağıdaki tabloda, Azure portalında Kurumsal Anlaşma yönetici rolleri, görünüm ücretleri İlkesi, Azure portalında ve fiyatlandırma gördüğünüz rol tabanlı erişim denetimi (RBAC) rolü arasındaki ilişki gösterilmektedir. Kuruluş yöneticisi her zaman, kuruluşun EA fiyatlandırmaya göre kullanım ayrıntıları görür. Ancak, departman Yöneticisi ve hesap sahibi görünümü ücret ilke ve bunların RBAC rolü dayanan farklı fiyatlandırma görünümleri bakın. Aşağıdaki tabloda listelenen departman yöneticisi rolü, departman Yöneticisi ve departman (salt okunur) yönetici rolleri için ifade eder.

|Kurumsal Anlaşma yöneticisi rolü|Rolü için ücretleri ilkeyi görüntüle|RBAC rolü|Fiyatlandırmayı görüntüleyin|
|---|---|---|---|
|Hesap sahibi veya departman Yöneticisi|Etkin ✔|Sahip|Kuruluşunuzun Kurumsal Anlaşma fiyatlandırması|
|Hesap sahibi veya departman Yöneticisi|✘ devre dışı bırakıldı|Sahip|Perakende fiyatlandırması|
|Hesap sahibi veya departman Yöneticisi|Etkin ✔ |Yok|Fiyatlandırma yok|
|Hesap sahibi veya departman Yöneticisi|✘ devre dışı bırakıldı |Yok|Fiyatlandırma yok|
|None|Geçerli değil |Sahip|Perakende fiyatlandırması|

Kuruluş yöneticisi rolünü ayarlayın ve görünümü olan Kurumsal Portal'a erişim ilkelerinde ücretleri. Azure portalında RBAC rolü güncelleştirilebilir. Daha fazla bilgi için [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure için fatura bilgilerini erişimi yönetme](billing-manage-access.md)
- [RBAC ve Azure portalı kullanarak erişimi yönetme](../role-based-access-control/role-assignments-portal.md)
- [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md)
