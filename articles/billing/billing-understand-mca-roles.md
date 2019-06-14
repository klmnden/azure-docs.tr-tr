---
title: Microsoft Müşteri sözleşmesi - Azure için fatura yönetici rollerini anlama
description: Hesapları Azure için Microsoft Müşteri anlaşmalarını fatura için fatura rolleri hakkında bilgi edinin.
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberbhargava
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 780870cc71e95507a52ba6a9338026f895a96ac1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370977"
---
# <a name="understand-microsoft-customer-agreement-administrative-roles-in-azure"></a>Azure'da Microsoft Müşteri sözleşmesi yönetici rollerini anlama

Microsoft Müşteri sözleşmesi için fatura hesabınıza yönetmek için aşağıdaki bölümlerde açıklanan rolleri kullanın. Azure kaynaklarına erişimi denetlemek için sahip yerleşik roller yanı sıra bu rolleridir. Daha fazla bilgi için [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md).

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetleyin.

## <a name="billing-role-definitions"></a>Faturalandırma rol tanımları

Aşağıdaki tablo profiller faturalama fatura hesabınıza yönetmek için kullanın ve bölümleri Fatura Fatura roller açıklanmıştır.

|Rol|Açıklama|
|---|---|
|Fatura hesap sahibi|Fatura hesabı için her şeyi yönetme|
|Faturalama hesabı Katılımcısı|Fatura hesap izinlerini dışında her şeyi yönetme|
|Faturalama hesabı okuyucusu|Fatura hesabı üzerinde salt okunur görünümü her şeyin|
|Faturalandırma Profil sahibi|Profil fatura için her şeyi yönetme|
|Faturalandırma profili katkıda bulunanı|Faturalandırma profili izinlerini dışında her şeyi yönetme|
|Faturalandırma profil okuyucusu|Profil faturalandırma üzerinde salt okunur görünümü her şeyin|
|Fatura Yöneticisi|Görüntüleme ve faturalar profili faturalandırma için ödeme yaparsınız.|
|Fatura bölüm sahibi|Fatura bölümündeki her şeyi yönetme|
|Fatura bölümüne katkıda bulunan|Fatura bölüm izinlerini dışında her şeyi yönetme|
|Fatura bölüm okuyucusu|Fatura bölümündeki her şeyin salt okunur görünümü|
|Azure aboneliği Oluşturucusu|Azure abonelikleri oluşturma|

## <a name="billing-account-roles-and-tasks"></a>Faturalama hesabı roller ve görevler

Bir faturalama hesabı, kuruluşunuz için fatura bilgilerini yönetmenizi sağlar. Maliyetleri, İzleyici ücretleri ve faturalarını düzenleyin ve kuruluşunuz için fatura erişimi denetlemek için Faturalama hesabı kullanın. Daha fazla bilgi için [Faturalama hesabı anlamak](billing-mca-overview.md#understand-billing-account).

Aşağıdaki tablolarda, Faturalama hesabı bağlamında görevlerini yapmanız hangi rolü gösterilmektedir.

### <a name="manage-billing-account-permissions-and-properties"></a>Fatura hesap izinlerini ve özellikleri yönetme

|Görev|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Fatura hesap için mevcut izinlerini görüntüleme|✔|✔|✔|
|Başkalarının görüntülemek ve Faturalama hesabı yönetmek için izinleri verme|✔|✘|✘|
|Şirket adı, adresi ve daha fazla Faturalama hesabı özelliklerini görüntüleme gibi|✔|✔|✔|

### <a name="manage-billing-profiles-for-billing-account"></a>Fatura hesabı için fatura profillerini yönetme

|Görev|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Hesaptaki tüm faturalandırma profillerini görüntüleyin|✔|✔|✔|

### <a name="manage-invoices-for-billing-account"></a>Faturalar, Faturalama hesabı yönetme

|Görev|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Hesaptaki tüm faturaları görüntülemek|✔|✔|✔|
|Fatura, Azure kullanımı ve ücretleri dosyaları, fiyat listeleri indirin ve hesap belgelerde vergi|✔|✔|✔|

### <a name="manage-invoice-sections-for-billing-account"></a>Faturalandırma hesabınız için fatura bölümlere yönetme

|Görev|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Tüm hesap bölümlerde fatura görüntüle|✔|✔|✔|

### <a name="manage-transactions-for-billing-account"></a>Fatura hesabı için işlemleri yönetme

|Görev|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Hesap için tüm faturalandırma işlemleri görüntüle|✔|✔|✔|
|Hesap için satın aldığınız tüm ürünleri görüntülemek|✔|✔|✔|

### <a name="manage-subscriptions-for-billing-account"></a>Fatura hesabı için abonelikleri yönetme

|Görev|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu|
|---|---|---|---|
|Tüm Azure abonelikleri fatura hesabında görüntüleyin|✔|✔|✔|

## <a name="billing-profile-roles-and-tasks"></a>Faturalandırma profili rolleri ve görevleri

Faturalandırma profili faturaları ve ödeme yöntemlerini yönetmenizi sağlar. Bir aylık fatura Azure abonelik ve faturalandırma profili kullanılarak satın alınan diğer ürünler için oluşturulur. Fatura ödemek için ödeme yöntemleri kullanın. Daha fazla bilgi için [fatura profillerini anlayabilir](billing-mca-overview.md#understand-billing-profiles).

Aşağıdaki tablolarda, hangi rolü bağlamında faturalandırma profili, görevleri tamamlama gerek gösterilmektedir.

### <a name="manage-billing-profile-permissions-properties-and-policies"></a>Faturalandırma profili izinleri, özellikleri ve ilkelerini yönetme

|Görev|Faturalandırma Profil sahibi|Faturalandırma profili katkıda bulunanı|Faturalandırma profil okuyucusu|Fatura Yöneticisi|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Mevcut faturalandırma profili izinlerini görüntüleme|✔|✔|✔|✔|✔|✔|✔|
|Başkalarının görüntülemek ve faturalandırma profili yönetmek için izinleri verme|✔|✘|✘|✘|✘|✘|✘|
|SAS numarası, e-posta fatura tercih ve daha fazla fatura profil özelliklerini görüntüleme gibi|✔|✔|✔|✔|✔|✔|✔|
|Fatura profil özelliklerini güncelleştirme |✔|✔|✘|✘|✘|✘|✘|
|Faturalandırma profili gibi uygulanan ilkeleri görüntüle Azure rezervasyon satın alma işlemleri etkinleştirmek için Azure Marketi satın alma işlemleri ve daha fazlasını etkinleştirme|✔|✔|✔|✔|✔|✔|✔|
|Fatura profilinde ilkelerini uygula |✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-invoices-for-billing-profile"></a>Profil faturalama faturalar yönetme

|Görev|Faturalandırma Profil sahibi|Faturalandırma profili katkıda bulunanı|Faturalandırma profil okuyucusu|Fatura Yöneticisi|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Tüm faturalar için fatura profili görüntüle|✔|✔|✔|✔|✔|✔|✔|
|Fatura, Azure kullanımı ve ücretleri dosyaları, fiyat listeleri indirin ve belgeler için faturalandırma profili vergi|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-invoice-sections-for-billing-profile"></a>Faturalandırma profili için fatura bölümlere yönetme

|Görev|Faturalandırma Profil sahibi|Faturalandırma profili katkıda bulunanı|Faturalandırma profil okuyucusu|Fatura Yöneticisi|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Faturalandırma profili için tüm fatura bölümleri görüntüle|✔|✔|✔|✔|✔|✔|✔|
|Faturalandırma profili için yeni bir fatura bölüm oluşturun|✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-transactions-for-billing-profile"></a>Profil faturalama işlemleri yönetme

|Görev|Faturalandırma Profil sahibi|Faturalandırma profili katkıda bulunanı|Faturalandırma profil okuyucusu|Fatura Yöneticisi|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Tüm faturalandırma işlemleri için faturalandırma profili görüntüle|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-payment-methods-for-billing-profile"></a>Profil fatura için ödeme yöntemlerini Yönet

|Görev|Faturalandırma Profil sahibi|Faturalandırma profili katkıda bulunanı|Faturalandırma profil okuyucusu|Fatura Yöneticisi|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Görünüm ödeme yöntemleri için faturalandırma profili|✔|✔|✔|✔|✔|✔|✔|
|Azure kredisi bakiyesi faturalandırma profili için izleme|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-subscriptions-for-billing-profile"></a>Profil fatura için abonelikleri yönetme

|Görev|Faturalandırma Profil sahibi|Faturalandırma profili katkıda bulunanı|Faturalandırma profil okuyucusu|Fatura Yöneticisi|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Tüm Azure abonelikleri için fatura profili görüntüle|✔|✔|✔|✔|✔|✔|✔|

## <a name="invoice-section-roles-and-tasks"></a>Fatura bölüm rolleri ve görevleri

Bir fatura bölümü faturanızla ilgili maliyetleri düzenlemenize olanak sağlar. Kuruluşunuzun ihtiyaçlarına göre veya maliyetlerinizi geliştirme ortamını departmana göre düzenlemek için bir bölüm oluşturabilirsiniz. Diğer bölüm için Azure abonelikleri oluşturabilmesi için izin verin. Tüm kullanım ücretleri ve satın alma işlemleri için abonelikler sonra Fatura bölümündeki göster. Daha fazla bilgi için [anlayın fatura bölüm](billing-mca-overview.md#understand-invoice-sections).

Aşağıdaki tablolarda, hangi rolü bağlamında fatura bölümler, görevleri tamamlama için ihtiyacınız gösterilmektedir.

### <a name="manage-invoice-section-permissions-and-properties"></a>Fatura bölüm izinleri ve özellikleri yönetme

|Görevler|Fatura bölüm sahibi|Fatura bölümüne katkıda bulunan|Fatura bölüm okuyucusu|Azure aboneliği Oluşturucusu|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu | |
|---|---|---|---|---|---|---|---|---|
|Fatura bölümünde tüm izinleri görüntüleyin|✔|✔|✔|✔|✔|✔|✔| |
|Başkalarının görüntülemek ve fatura bölüm yönetmek için izinleri verme|✔|✘|✘|✘|✘|✘|✘| |
|Fatura bölümü özelliklerini görüntüleme|✔|✔|✔|✔|✔|✔|✔| |
|Fatura bölümü özelliklerini güncelleştir|✔|✔|✘|✘|✘|✘|✘|✘|

### <a name="manage-products-for-invoice-section"></a>Ürünleri fatura bölümü için yönetme

|Görevler|Fatura bölüm sahibi|Fatura bölümüne katkıda bulunan|Fatura bölüm okuyucusu|Azure aboneliği Oluşturucusu|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Tüm ürünler fatura bölümünde satın görüntüleme|✔|✔|✔|✘|✔|✔|✔|
|Fatura bölümün iptal gibi ürünler için faturalandırmayı yönetmek, otomatik yenileme ve daha fazla kapatma|✔|✔|✘|✘|✘|✘|✘|
|Ürünler için fatura bölümü Değiştir|✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-subscriptions-for-invoice-section"></a>Fatura bölümü için abonelikleri yönetme

|Görevler|Fatura bölüm sahibi|Fatura bölümüne katkıda bulunan|Fatura bölüm okuyucusu|Azure aboneliği Oluşturucusu|Fatura hesap sahibi|Faturalama hesabı Katılımcısı|Faturalama hesabı okuyucusu
|---|---|---|---|---|---|---|---|
|Tüm Azure abonelikleri için fatura bölümü görüntüleme|✔|✔|✔|✘|✔|✔|✔|
|Abonelikler için fatura bölüm değiştirme|✔|✔|✘|✘|✘|✘|✘|
|Diğer faturalandırma hesaplarında kullanıcılardan abonelik faturalandırma sahipliğini iste|✔|✔|✘|✘|✘|✘|✘|

## <a name="subscription-billing-roles-and-tasks"></a>Abonelik faturalama roller ve görevler

Aşağıdaki tablo, hangi rol için bir abonelik bağlamında görevleri tamamlama ihtiyacınız gösterir.

|Görevler|Fatura bölüm sahibi|Fatura bölümüne katkıda bulunan|Fatura bölüm okuyucusu|Azure aboneliği Oluşturucusu|
|---|---|---|---|---|
|Azure abonelikleri oluşturma|✔|✔|✘|✔|
|Abonelik için güncelleştirme maliyet merkezi|✔|✔|✘|✘|
|Abonelik için fatura bölümü Değiştir|✔|✔|✘|✘|

## <a name="manage-billing-roles-in-the-azure-portal"></a>Azure portalındaki faturalandırma rollerini yönetme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Azure portalı arama gösteren ekran görüntüsü](./media/billing-understand-mca-roles/billing-search-cost-management-billing.png)

3. Seçin **erişim denetimi (IAM)** Faturalama hesabı, faturalandırma profili ya da fatura bölümünde, erişim vermek istediğiniz gibi bir kapsamda.

4. Erişim denetimi (IAM) sayfası, kullanıcıları ve her rol için bu kapsama atanmış olan grupları listeler.

   ![Yöneticiler için fatura hesap listesini gösteren ekran görüntüsü](./media/billing-understand-mca-roles/billing-list-admins.png)

5. Bir kullanıcı için erişim vermek istiyorsanız, **Ekle** sayfanın üst. Rol aşağı açılan listeden bir rol seçin. Erişim vermek istediğiniz kullanıcının e-posta adresi girin. Seçin **Kaydet** rol atamak için.

   ![Bir faturalama hesabı için bir yönetici ekleme gösteren ekran görüntüsü](./media/billing-understand-mca-roles/billing-add-admin.png)

6. Bir kullanıcı erişimi kaldırmak için kaldırmak istediğiniz rol atamasına sahip kullanıcı seçin. Kaldır'ı seçin.

   ![Bir faturalama hesabı yönetici kaldırma gösteren ekran görüntüsü](./media/billing-understand-mca-roles/billing-remove-admin.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

Fatura hesabınıza hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Microsoft Müşteri sözleşmesi için fatura hesabınıza belirtilen](billing-mca-overview.md)
- [Azure aboneliğinin faturalandırma hesabınız için Microsoft Müşteri sözleşmesi oluşturma](billing-mca-create-subscription.md)
