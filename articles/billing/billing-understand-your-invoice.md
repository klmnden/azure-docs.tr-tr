---
title: Azure faturanızı anlamak
description: Azure aboneliğinizin kullanımını ve faturasını okuma ve anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: tonguyen10
manager: tonguyen
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.openlocfilehash: 38126e4539719ba56e6e5eac5e860cea9b49d446
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="understand-terms-on-your-microsoft-azure-invoice"></a>Microsoft Azure fatura koşullarınızda anlama

Fatura ücretlerinizi özetini sağlar ve ödeme için yönergeler sağlar. Taşınabilir Belge Biçimi (.pdf) İndirilebilecek [Azure portal](https://portal.azure.com/) veya e-posta ile gönderilebilir. Daha fazla bilgi için bkz: [faturayı ve günlük kullanım verileri faturalama Azure alma](billing-download-azure-invoice-daily-usage-date.md).

Dikkat edilecek bazı noktalar:

-   Ücretsiz bir deneme aboneliği kullanıyorsanız, Azure portalından, ayrıntılı kullanım bilgilerini alabilir ancak fatura bulunmuyor.

-   Önceki fatura döneminin sonunda kullanım 24 saat kadar geçerli faturanıza gösterebilir.

-   Uluslararası müşteriler için fatura tablolarda listelenen ücretleri, yalnızca tahmin amaçlıdır. Banka dönüştürme oranları farklı maliyetlerini olabilir.

>[!VIDEO https://www.youtube.com/embed/jWG1lyJe3Mg]

## <a name="detailed-terms-and-descriptions-of-your-invoice"></a>Ayrıntılı hüküm ve faturanızı açıklamaları
Aşağıdaki bölümlerde her dönem için fatura ve açıklamaları üzerinde gördüğünüz önemli terimler listelenmektedir.

### <a name="account-information"></a>Hesap bilgileri

Fatura hesap bilgileri bölümünü ilk sayfanın üst kısmında olduğu ve profil ve abonelik hakkında bilgi gösterir.

![Fatura hesabı bilgi bölümü](./media/billing-understand-your-invoice/1.png)

| Sözleşme Dönemi | Açıklama |
| --- | --- |
| Müşteri SAS No |Sizin tarafınızdan izleme için atanan bir isteğe bağlı satınalma siparişi numarası |
| Fatura No. |İzleme amaçları için kullanılan benzersiz bir oluşturulan Microsoft fatura numarası |
| Fatura döngüsü |Bu fatura kapsayan tarih aralığı |
| Fatura tarihi |Fatura, genellikle bir gün sonra Faturalama döngüsünün sonuna oluşturulduğu tarih |
| Ödeme yöntemi |Hesapta kullanılan ödeme türü (ör. fatura veya kredi kartı) |
| Fatura adresi |Hesap için listelenen faturalama adresi |
| Abonelik sunar ("Kullandıkça Öde") |Satın alınan (Kullandıkça Öde, BizSpark artı, Azure geçişi, vb.) abonelik Teklif türü. Daha fazla bilgi için bkz: [Azure teklif türleri](https://azure.microsoft.com/support/legal/offer-details/). |
| Hesap sahibinin e-postası | Microsoft Azure hesabının kayıtlı olduğu hesaba ait e-posta adresi. <br /><br />E-posta adresini değiştirmek için bkz: [Azure hesabınızın iletişim e-posta adresi ve telefon numarası gibi profil bilgileri değiştirmek nasıl](billing-how-to-change-azure-account-profile.md). |

### <a name="understand-the-invoice-summary"></a>Fatura özetini anlama
**Fatura özeti** fatura bölümünü son, fatura döneminin ve geçerli Kullanım ücretlerinizi itibaren toplam işlem tutarlar listeler.

![Fatura Özet bölümü](./media/billing-understand-your-invoice/2.png)

Abonelik adı ("üretim depolama") bu fatura için abonelik adıdır.

#### <a name="understand-the-previous-charges"></a>Önceki ücretlerini anlama
Önceki bakiyesi, ödemeler ve bekleyen Bakiye fatura bölümünü son fatura süreniz itibaren işlemleri özetlenmektedir.

| Sözleşme Dönemi | Açıklama |
| --- | --- |
| Önceki bakiye |Toplam tutar nedeniyle son fatura döneminden |
| Ödemeler |Toplam ödemeler ve son fatura süreniz uygulanan KREDİLERİ |
| Borç-alacak bakiyesi (önceki fatura döngüsünden) |Herhangi bir KREDİLERİ veya kalan bakiyesi hesabınızda son fatura süreniz itibaren |

#### <a name="understand-the-current-charges"></a>Geçerli ücretleri anlama
Fatura geçerli ücretler bölümünü geçerli fatura dönemi için aylık ücretler ayrıntılarını gösterir.

| Sözleşme Dönemi | Açıklama |
| --- | --- |
| Kullanım ücretleri |Bir abonelikte geçerli fatura dönemi için toplam Aylık Ücretler kullanım ücretleri var|
| İndirimler |Geçerli fatura dönemi için uygulanan hizmet indirim|
| Düzeltmeler |Çeşitli krediler (boş kullanım, KREDİLERİ, vb.) veya ödenmemiş borçları, geçerli fatura dönemi için uygulanır.<br/><br/>Örneğin, Visual Studio Enterprise MSDN teklif ile varsa, aylık krediyi bakın. Aboneliğinizi iptal ederseniz, abonelik teklifiniz ile elde edilen aylık krediyi aşan herhangi aylık kullanım ücretlerine bakın. Aboneliği iptal tarihine kadar geçerli fatura dönemi başlangıcında ücretler uygulanır. |

#### <a name="sold-to-and-payment-instructions"></a>İçin satılan ve ödeme yönergeleri

Aşağıdaki tablo için satılan açıklar ve ödeme yönergeleri faturanızı ikinci sayfasında gösterilir.

| Sözleşme Dönemi |Açıklama |
| --- | --- |
| Kime Satıldı |Üzerinde hesap profili adresi. <br/><br/>Adres değiştirmeniz gerekiyorsa, bkz: [Azure hesabınızın iletişim e-posta adresi ve telefon numarası gibi profil bilgileri değiştirmek nasıl](billing-how-to-change-azure-account-profile.md).|
| Ödeme yönergeleri |Ödeme yöntemine bağlı olarak ödeme konusunda yönergeler (gibi olarak göre karta veya fatura kredisi). |

#### <a name="usage-charges"></a>Kullanım Ücretleri

Fatura kullanım ücretleri bölümünü ücretlerinizi üzerinde ölçer düzey bilgileri görüntüler.

![Kullanım ücretleri bölümü](./media/billing-understand-your-invoice/3.png)

Aşağıdaki tabloda, fatura üzerinde gösterilen kullanım ücretleri sütun üst bilgileri açıklar.

| Sözleşme Dönemi |Açıklama |
| --- | --- |
| Ad |Kullanım için üst düzey hizmet tanımlar |
| Tür |Hızı etkileyebilir Azure hizmet türünü tanımlar |
| Kaynak |Tüketilen ölçer ölçü tanımlar |
| Bölge |Veri merkezi konum temelinde fiyatlandırılır belirli hizmetler için veri merkezi konumunu tanımlar |
| Kullanılan |Fatura döneminde kullanılan ölçer miktarı |
| Dahil |Ücret ödemeden, geçerli fatura dönemi içinde bulunan ölçer miktarı |
| Faturalanabilir |Tüketilen miktarı ve dahil edilen miktar arasındaki farkı gösterir. Bu tutar için fatura. Teklifi dahil herhangi bir tutar ile Kullandıkça Öde tekliflere bu toplam Tüketilen Miktar aynıdır |
| Fiyat |Faturalanabilir birim başına ücret ödersiniz oranı |
| Değer |Fazla kullanım miktarı sütun oranı sütuna göre çarparak sonucunu gösterir. Tüketilen Miktar dahil edilen miktar aşmadığından, bu sütunda hiçbir ücret yoktur. |
| Alt toplam |Bu fatura dönemi için tüm ücretleri öncesi vergi toplamı |
| Genel toplam |Bu fatura dönemi için vergi sonra tüm ücretlerinizi toplamı |

## <a name="how-do-i-make-sure-that-the-charges-in-my-invoice-are-correct"></a>My fatura ücretlere doğru olup olmadığını nasıl emin?
Daha fazla ayrıntı istediğiniz faturanızı üzerinde bir ücret ise bakın [Microsoft Azure için faturanızı anlamak.](billing-understand-your-bill.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
