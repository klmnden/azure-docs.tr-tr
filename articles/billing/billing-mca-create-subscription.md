---
title: Faturalandırma hesabınız için ek bir Azure aboneliği oluşturun | Microsoft Docs
description: Azure portalında yeni bir Azure aboneliği eklemeyi öğrenin.
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 105b8481486c088a05e3acb95081d3ee55b55f52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60372031"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için ek bir Azure aboneliği oluşturun

Fatura hesabınıza geliştirme ve test, güvenlik için ayrı ortamlar ayarlayın veya uyumluluk nedenleriyle verilerin yalıtmak için ek abonelikleri oluşturun.

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement). Abonelikler için faturalama diğer hesapları oluşturmak istiyorsanız, bkz. [başka bir abonelik oluşturun Azure Portalı'nda](billing-create-subscription.md).

Bir abonelik oluşturmak için olmalıdır bir **fatura bölümüne sahip**, **fatura bölümüne katkıda bulunan**, veya **Azure aboneliği Oluşturucusu**. Daha fazla bilgi için [abonelik fatura rolleri ve görevleri](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Diğer Azure abonelikleri için fatura hesabınıza oluşturma izni sağlamak için bkz: [diğerlerinin Azure abonelikleri oluşturabilmesi için izinler verebilirsiniz](#give-others-permission-to-create-azure-subscriptions).

## <a name="create-a-subscription-in-the-azure-portal"></a>Azure portalında bir abonelik oluşturun

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Arama **abonelikleri**.

   ![Abonelik Portalı'nda arama gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Seçin **Ekle**

4. Faturalandırma birden çok hesaba erişim varsa, Microsoft Müşteri sözleşmenizi Faturalama hesabı seçin.

   ![Abonelik sayfasını gösteren ekran görüntüsü oluşturma](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Faturalandırma profili seçin. Aboneliğiniz için ücretler fatura profilin faturasına yansıtılır ve kendi ödeme yöntemleri kullanılarak ödenmez. Yalnızca bir faturalandırma profili erişiminiz varsa seçimi gri görünür.

6. Fatura bölümü seçin. Aboneliğiniz için ücretler fatura profilin fatura üzerinde bu bölümün ücreti yansıtılır. Tek bir fatura bölümüne erişiminiz varsa seçimi gri görünür.

7. Abonelik için bir plan seçin. Seçin **Microsoft Azure geliştirme ve test planlama**, bu abonelik için geliştirme kullanmayı planlıyorsanız veya başka test iş yükleri kullanırsanız **Microsoft Azure-planı**. Yalnızca bir plan erişiminiz varsa seçimi gri görünür.

8. Abonelik için bir ad girin. Adı kolayca abonelik Azure portalında tanımlamanıza yardımcı olur.

9. **Oluştur**’u seçin.

## <a name="give-others-permission-to-create-azure-subscriptions"></a>Diğer Azure abonelikleri oluşturabilmesi için izin ver

Kullanıcıları, bunları Azure abonelikleri oluşturabilmesi için izin vermek için Azure aboneliği oluşturuculara bir fatura bölümü ekleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Abonelik Portalı'nda arama gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Fatura bölümüne gidin. Erişiminizi bağlı olarak, bir faturalama hesabı veya faturalandırma profili seçmeniz gerekebilir. Fatura hesabı veya profili makinesinden seçin **fatura bölümleri** ve ardından bir fatura bölümü.

4. Seçin **erişim yönetimi (IAM)** sol üst taraftaki.

5. Sayfanın üst kısmında **Ekle**'yi seçin.

6. Seçin **Azure aboneliği Oluşturucusu** rolü için.

   ![Bir kullanıcıya Azure aboneliğini oluşturan rol atama gösteren ekran görüntüsü](./media/billing-mca-create-subscription/billing-mca-add-azure-subscription-creator.png)

7. Erişim vermek istediğiniz kullanıcının e-posta adresi girin.

8. **Kaydet**’i seçin.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Başkalarının yerleşik rolleri kullanarak Azure kaynakları oluşturma izni verin](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Bir windows sanal makinesi oluşturma](../virtual-machines/windows/quick-create-portal.md)
- [Bir linux sanal makinesi oluşturma](../virtual-machines/linux/quick-create-portal.md)
- [Kaynak kuruluşta ve Yönetim için Yönetim grupları oluşturma](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
