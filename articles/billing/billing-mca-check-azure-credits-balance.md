---
title: Azure kredisi bakiyesi izlemek için bir Microsoft Müşteri Sözleşmesi
description: Microsoft Müşteri sözleşmesi için Azure kredi bakiyesi denetleme hakkında bilgi edinin.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 352737b3ea61a51a39e066d4211c8f4ceae74184
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490956"
---
# <a name="track-microsoft-customer-agreement-azure-credit-balance"></a>Microsoft Müşteri sözleşmesi Azure kredi bakiyesi izleyin

Azure portalında Azure kredi bakiyesi için Microsoft Müşteri sözleşmesi kontrol edebilirsiniz. KREDİLERİ ücret karşılığında Ödeme yapmalarını sağlayan KREDİLERİ kullanın.

KREDİLERİ kapsamında olmayan ürünler kullandığınızda veya kredi bakiyeniz kullanımınızı aşıyor ücretlendirilir. Daha fazla bilgi için bkz. [Azure KREDİLERİ kapsamında olmayan ürünler. () #products-that-aren't-covered-by-azure-credits).

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

## <a name="check-your-credit-balance"></a>Kredi bakiyeniz denetleyin

1. [Azure Portal]( https://portal.azure.com) oturum açın.

2. Arama **maliyet Yönetimi + faturalandırma**.

    ![Maliyet Yönetimi + faturalandırma Portalı'nda arama gösteren ekran görüntüsü](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3.  Seçin **Azure KREDİLERİ** sol taraftaki. Erişiminizi bağlı olarak, bir faturalama hesabı ya da fatura profili seçin ve ardından gerekebilir **Azure KREDİLERİ**.

4. Azure KREDİLERİ sayfasında aşağıdaki bilgileri görüntüler:

   ![Kredi bakiyesi ve işlemleri için bir faturalandırma profili ekran görüntüsü](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Terim               | Tanım                           |
   |--------------------|--------------------------------------------------------|
   | Tahmini bakiyesi  | Tahmini kredi miktarı sonra tüm dikkate faturalandırılır ve bekleyen işlemler sahip |
   | Toplam tutar    | Son faturanızdan itibaren kredi miktarı. Bekleyen işlemleri içermez |
   | İşlemler       | Azure kredi bakiyeniz etkilenen tüm faturalandırma işlemleri |

   Tahmini bakiyeniz 0 olarak düştüğünde kapsamında ürünler dahil olmak üzere tüm kullanım için ücretlendirilirsiniz.

6. Seçin **KREDİLERİ listesi** için faturalandırma profili için kredi listesini görüntüleyin. Krediler listesi aşağıdaki bilgileri sağlar:

   ![Kredileriniz ekran için bir faturalandırma profili listeler](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Terim | Tanım |
   |---|---|
   | Tahmini bakiyesi | Faturalandırılmamış kredi bakiyeniz geçerli uygun ücretlerden çıkardıktan sonra sahip olduğunuz Azure kredisi miktarı|
   | Toplam tutar | Faturalandırılmamış kredi uygun ücretleri olduğunu düşünmeden önce sahip olduğunuz Azure kredisi miktarı. Yeni Azure KREDİLERİ aldığınız kredi bakiyesine, son faturanızdan zamanında eklenmesiyle hesaplanır|
   | source | Kredi edinme kaynağı |
   | Başlangıç tarihi | Kredi satın aldığınız zaman tarihi |
   | Sona erme tarihi | Kredi süresinin dolduğu tarih |
   | Bakiye | Bakiye, son faturanızdan itibaren |
   | Orijinal Tutar | Özgün kredi miktarı |
   | Durum | Kredi geçerli durumu. Kullanılan, süresi doldu veya süresi doluyor durumu etkin olabilir |

## <a name="how-credits-are-used"></a>Krediler nasıl kullanılır

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

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Müşteri sözleşmesi için Faturalama hesabı anlama](billing-mca-overview.md)
- [Microsoft Müşteri sözleşmesi faturanızla ilgili koşulları anlama](billing-mca-understand-your-invoice.md)
