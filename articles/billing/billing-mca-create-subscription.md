---
title: Faturalandırma hesabınız için ek bir Azure aboneliği oluşturun
description: Azure portalında yeni bir Azure aboneliği eklemeyi öğrenin.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 558410d980d261780f7287d1e27ed704b356fc2b
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490923"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için ek bir Azure aboneliği oluşturun

Fatura hesabınıza geliştirme ve test, güvenlik için ayrı ortamlar ayarlayın veya uyumluluk nedenleriyle verilerin yalıtmak için ek abonelikleri oluşturun.

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access). Diğer faturalandırma hesap türleri için abonelikler oluşturmak istiyorsanız, bkz. [başka bir abonelik oluşturun Azure Portalı'nda](billing-create-subscription.md).

Bir abonelik oluşturmak için olmalıdır bir **fatura bölümüne sahip**, **fatura bölümüne katkıda bulunan**, veya **Azure aboneliği Oluşturucusu**. Daha fazla bilgi için [abonelik fatura rolleri ve görevleri](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Diğer Azure abonelikleri için fatura hesabınıza oluşturma izni sağlamak için bkz: [diğerlerinin Azure abonelikleri oluşturabilmesi için izinler verebilirsiniz](#give-others-permission).

## <a name="create-a-subscription"></a>Abonelik oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **abonelikleri**.

   ![Abonelik Portalı'nda arama gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-search-subscriptions.png)

3. Seçin **Ekle**

4. Faturalandırma birden çok hesaba erişim varsa, Microsoft Müşteri sözleşmenizi Faturalama hesabı seçin.

   ![Abonelik sayfasını gösteren ekran görüntüsü oluşturma](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Faturalandırma profili seçin. Aboneliğiniz için ücretler, seçili faturalandırma profili için faturalandırılırsınız. Yalnızca bir faturalandırma profili erişiminiz varsa seçimi gri görünür.

6. Fatura bölümü seçin. Aboneliğiniz için ücretler fatura profilin fatura bu bölüme faturalandırılır. Tek bir fatura bölümüne erişiminiz varsa seçimi gri görünür.

7. Abonelik için bir plan seçin. Seçin **Microsoft Azure geliştirme ve test planlama**, bu abonelik için geliştirme kullanmayı planlıyorsanız veya başka test iş yükleri kullanırsanız **Microsoft Azure-planı**. Yalnızca bir plan erişiminiz varsa seçimi gri görünür.

8. Abonelik için bir ad girin. Adı kolayca abonelik Azure portalında tanımlamanıza yardımcı olur.

9. **Oluştur**’u seçin.

## <a name="give-others-permission"></a>Diğerlerinin izin ver

Kullanıcıları, bunları Azure abonelikleri oluşturabilmesi için izin vermek için Azure aboneliği oluşturuculara bir fatura bölümü ekleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

   ![Abonelik Portalı'nda arama gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Fatura bölümüne gidin. Erişiminizi bağlı olarak, bir faturalama hesabı veya faturalandırma profili seçmeniz gerekebilir. Fatura hesabı veya profili makinesinden seçin **fatura bölümleri** ve ardından listeden bir fatura bölümü. Bu fatura bölümüne kullanıcılar tarafından oluşturulan tüm abonelikleri faturalandırılır.
   
   ![Seçme gösteren ekran görüntüsü fatura bölümleri](./media/billing-mca-create-subscription/mca-select-invoice-sections.png)        

4. Seçin **erişim yönetimi (IAM)** sol üst taraftaki.

5. Sayfanın üst kısmında **Ekle**'yi seçin.

6. Seçin **Azure aboneliği Oluşturucusu** rolü için.

7. Erişim vermek istediğiniz kullanıcının e-posta adresi girin.

8. **Kaydet**’i seçin.

## <a name="check-access"></a>Erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Başkalarının yerleşik rolleri kullanarak Azure kaynakları oluşturma izni verin](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Bir windows sanal makinesi oluşturma](../virtual-machines/windows/quick-create-portal.md)
- [Bir linux sanal makinesi oluşturma](../virtual-machines/linux/quick-create-portal.md)
- [Kaynak kuruluşta ve Yönetim için Yönetim grupları oluşturma](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
