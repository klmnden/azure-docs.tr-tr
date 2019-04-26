---
title: Azure kredisi bakiyesi izlemek için Microsoft Müşteri Sözleşmesi | Microsoft Docs
description: Azure kredisi bakiyesi denetlemek için Microsoft Müşteri sözleşmesi öğrenin.
services: ''
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
ms.author: amberb
ms.openlocfilehash: 1e8c3e6863b9cd8f2f5ced18a57918c32c865e75
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60372269"
---
# <a name="track-azure-credit-balance-for-microsoft-customer-agreement"></a>Azure kredisi bakiyesi izlemek için Microsoft Müşteri Sözleşmesi

Azure portalında Azure kredi bakiyesi için Microsoft Müşteri sözleşmesi kontrol edebilirsiniz. KREDİLERİ ürünleri için ödeme yapmayı KREDİLERİ kullanın.

KREDİLERİ kapsamında olmayan ürünler kullandığınızda veya kredi bakiyeniz kullanımınızı aşıyor ücretlendirilir. Daha fazla bilgi için [Azure KREDİLERİ kapsamında olmayan ürünleri.](#products-that-arent-covered-by-azure-credits)

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

## <a name="check-credit-balance-in-the-azure-portal"></a>Azure portalında onay kredi bakiyesi

1. [Azure Portal]( https://portal.azure.com) oturum açın.

2. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Maliyet Yönetimi + faturalandırma Portalı'nda arama gösteren ekran görüntüsü](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3. Faturalama profiline gidin. Erişiminize bağlı olarak, bir ödeme hesabı seçmeniz gerekebilir. Fatura hesap sayfasında **profilleri faturalama** ve ardından bir faturalandırma profili.

4. Seçin **Azure KREDİLERİ**.

5. Azure KREDİLERİ sayfasında aşağıdaki bilgileri görüntüler:

   ![Kredi bakiyesi ve işlemleri için bir faturalandırma profili ekran görüntüsü](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Sözleşme Dönemi               | Tanım                           |
   |--------------------|--------------------------------------------------------|
   | Tahmini bakiye  | Tahmini kredi miktarı sonra tüm dikkate faturalandırılır ve bekleyen işlemler sahip |
   | Geçerli bakiye    | Son faturanızdan itibaren kredi miktarı. Bekleyen işlemleri içermez |
   | İşlemler       | Azure kredi bakiyeniz etkilenen tüm faturalandırma işlemleri |

   Tahmini bakiyeniz 0 olarak düştüğünde kapsamında ürünler dahil olmak üzere tüm kullanım için ücretlendirilirsiniz.

6. Seçin **KREDİLERİ listesi** için faturalandırma profili için kredi listesini görüntüleyin. Krediler listesi aşağıdaki bilgileri sağlar:

   ![Kredileriniz ekran için bir faturalandırma profili listeler](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Sözleşme Dönemi                 | Tanım                           |
   |----------------------|--------------------------------------------------------|
   | Kaynak               | Kredi edinme kaynağı |
   | Başlangıç tarihi           | Kredi satın aldığınız zaman tarihi |
   | Son kullanma tarihi      | Kredi süresinin dolduğu tarih |
   | Bakiye              | Bakiye, son faturanızdan itibaren |
   | Orijinal Tutar      | Özgün kredi miktarı |
   | Durum               | Kredi geçerli durumu. Kullanılan, süresi doldu veya süresi doluyor durumu etkin olabilir |

## <a name="how-credits-are-used-in-microsoft-customer-agreement"></a>Krediler Microsoft Müşteri anlaşması'nda nasıl kullanılır

Microsoft Müşteri sözleşmesi için bir faturalandırma hesabında, faturaları ve ödeme yöntemlerini yönetmek için fatura profillerini kullanın. Bir aylık fatura için fatura her profil oluşturulur ve fatura ödemek için ödeme yöntemleri kullanın.

Azure KREDİLERİ ödeme yöntemleri biridir. Promosyon alacak ve hizmet düzeyi kredi gibi kredi Microsoft'tan alın. KREDİLERİ, faturalama profil atanır. Fatura için faturalandırma profili oluşturulduğunda KREDİLERİ de ödeme yapmam gerekiyor tutarı hesaplamak için toplam faturalandırılan miktar için otomatik olarak uygulanır. Onay gibi başka bir ödeme yöntemi olduğu tutarına ödeme veya aktarım bağlayabilirsiniz.

## <a name="products-that-arent-covered-by-azure-credits"></a>Azure KREDİLERİ kapsamında olmayan ürünler

 Şu ürünler, Azure kredilerinizi kapsamında olmayan. Bu ürünler kredi bakiyeniz bağımsız olarak kullanmak için ücret ödersiniz:

- Canonical
- Citrix XenApp Essentials
- Citrix XenDesktop 
- Kayıtlı kullanıcı
- Openlogic
- Uzaktan erişim hakları XenApp Essentials kayıtlı kullanıcı
- Ubuntu Advantage
- Visual Studio Enterprise (aylık)
- Visual Studio Enterprise (Yıllık)
- Visual Studio Professional (aylık)
- Visual Studio Professional (Yıllık)
- Azure Market ürünleri
- Azure destek planları

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Müşteri sözleşmesi için Faturalama hesabı anlama](billing-mca-overview.md)
- [Microsoft Müşteri sözleşmesi faturanızla ilgili koşulları anlama](billing-mca-understand-your-invoice.md)