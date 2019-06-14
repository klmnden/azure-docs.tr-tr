---
title: Azure faturanızı anlama | Microsoft Docs
description: Azure aboneliğinizin kullanımını ve faturasını okuma ve anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: bandersmsft
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: banders
ms.openlocfilehash: 77c1a85136b2117af7396b8eec2d8b92b335d61d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60369973"
---
# <a name="understand-terms-on-your-microsoft-azure-invoice"></a>Microsoft Azure faturanızla ilgili koşulları anlama

Faturada ücretlerinizin özeti ve ödeme yönergeleri yer alır. Taşınabilir Belge Biçimi (.pdf) indirme için kullanılabilir [Azure portalında](https://portal.azure.com/) veya e-posta ile gönderilebilir. Daha fazla bilgi için [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md).

Dikkat edilecek bazı noktalar:

-   Ücretsiz bir deneme aboneliği kullanıyorsanız, Azure Portalı'ndan ayrıntılı kullanım bilgilerinizi alabilirsiniz ancak fatura zorunda değilsiniz.

-   24 saatlik kullanım önceki fatura döneminin sonuna kadar geçerli faturanıza gösterebilir.

-   Uluslararası müşterilerin fatura ekstrelerinde listelenen ücretler yalnızca tahmin amaçlıdır. Bankaların döviz kuru dönüştürme maliyetleri farklı olabilir.

>[!VIDEO https://www.youtube.com/embed/jWG1lyJe3Mg]

## <a name="detailed-terms-and-descriptions-of-your-invoice"></a>Ayrıntılı hüküm ve faturanızı açıklamaları
Aşağıdaki bölümlerde her dönem için fatura ve açıklamaları görmek önemli terimleri listelenmektedir.

### <a name="account-information"></a>Hesap bilgileri

Fatura hesap bilgileri bölümü, ilk sayfa üst kısmındaki ve profili ve üyelik bilgilerini gösterir.

![Fatura hesap bilgileri bölümü](./media/billing-understand-your-invoice/1.png)

| Terim | Açıklama |
| --- | --- |
| Müşteri Sipariş No |İzleme için size atanan bir isteğe bağlı satınalma siparişi numarası |
| Fatura No. |İzleme amacıyla kullanılan benzersiz bir Microsoft oluşturulan fatura numarası |
| Fatura döngüsü |Bu fatura kapsar ve tarih aralığı |
| Fatura tarihi |Fatura, genellikle bir günün ardından faturalama döngüsü sonuna oluşturulduğu tarih |
| Ödeme yöntemi |Hesapta kullanılan ödeme türü (ör. fatura veya kredi kartı) |
| Fatura adresi |Hesap için listelenen fatura adresi |
| Abonelik teklifi ("Kullandıkça Öde") |Satın alınan (Kullandıkça Öde, BizSpark Plus, Azure Pass, vb.) abonelik Teklif türü. Daha fazla bilgi için [Azure teklif türleri](https://azure.microsoft.com/support/legal/offer-details/). |
| Hesap sahibinin e-postası | Microsoft Azure hesabının kayıtlı olduğu hesaba ait e-posta adresi. <br /><br />E-posta adresini değiştirmek için bkz [Azure hesabınızla ilgili kişi e-posta adresi ve telefon numarası gibi profil bilgilerini değiştirme](billing-how-to-change-azure-account-profile.md). |

### <a name="understand-the-invoice-summary"></a>Fatura özetini anlama
**Fatura özeti** fatura bölümünü son fatura döneminize ve geçerli Kullanım ücretlerinizi bu yana toplam işlem miktarları listeler.

![Fatura Özeti bölümü](./media/billing-understand-your-invoice/2.png)

Abonelik adı ("üretim depolama"), bu fatura aboneliği adıdır.

#### <a name="understand-the-previous-charges"></a>Önceki ücretlerini anlama
Önceki Bakiye, ödemeler ve bekleyen Bakiye fatura bölümünü son fatura döneminize beri işlemler özetlenir.

| Terim | Açıklama |
| --- | --- |
| Önceki bakiye |Kalan ödenmemiş toplam tutar son fatura döneminden |
| Ödemeler |Toplam ödemeler ve krediler son fatura döneminize uygulanan |
| Borç-alacak bakiyesi (önceki fatura döngüsünden) |Herhangi bir kredi veya kalan Bakiye, son fatura döneminden itibaren hesabınızdaki |

#### <a name="understand-the-current-charges"></a>Geçerli ücretleri anlama
Fatura geçerli ücretler bölümü, geçerli fatura dönemi için aylık ücretlerinizle ilgili ayrıntıları gösterir.

| Terim | Açıklama |
| --- | --- |
| Kullanım ücretleri |Kullanım ücretleri bir Abonelikteki geçerli fatura dönemi için aylık toplam ücretlerdir|
| İndirimler |Geçerli fatura döneminize uygulanan hizmet indirimleri|
| Düzeltmeler |(Ücretsiz kullanım KREDİLERİ, vb.) çeşitli krediler veya ödenmemiş geçerli fatura döneminize uygulanır.<br/><br/>Örneğin, Visual Studio Enterprise with MSDN teklifinden varsa, bir aylık kredi görürsünüz. Aboneliğinizi iptal ederseniz, abonelik teklifinizle alma aylık KREDİLERİ aşan aylık kullanım ücretlerine bakın. Aboneliğinizin iptal edildiği tarihe kadar geçerli fatura döneminizin başındaki ücretleri. |

#### <a name="sold-to-and-payment-instructions"></a>Kime satıldı ve ödeme yönergeleri

Aşağıdaki tablo için satılan açıklar ve faturanızı ikinci sayfada gösterilen ödeme yönergeleri.

| Terim |Açıklama |
| --- | --- |
| Kime Satıldı |Hesapta profili adresi. <br/><br/>Adresini değiştirmeniz gerekirse, bkz. [Azure hesabınızla ilgili kişi e-posta adresi ve telefon numarası gibi profil bilgilerini değiştirme](billing-how-to-change-azure-account-profile.md).|
| Ödeme yönergeleri |Ödeme yöntemine bağlı olarak ödeme yapma (örneğin ile kartı veya fatura kredi) hakkında yönergeler. |

#### <a name="usage-charges"></a>Kullanım Ücretleri

Fatura kullanım ücretleri bölümünü üzerinde yapılacak bir ücret ölçüm düzey bilgileri görüntüler.

![Kullanım ücretleri bölümü](./media/billing-understand-your-invoice/3.png)

Aşağıdaki tabloda, faturanızı üzerinde gösterilen kullanım ücretleri sütun üst bilgilerini açıklar.

| Terim |Açıklama |
| --- | --- |
| Ad |Kullanımı için üst düzey hizmeti belirtir |
| Tür |Hızı etkileyen Azure hizmet türü tanımlar. |
| Resource |Tüketilen ölçüm için ölçü birimini belirtir |
| Bölge |Veri Merkezi konumuna göre ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir |
| Kullanılan |Fatura dönemi boyunca kullanılan ölçüm miktarı |
| Dahil |Geçerli fatura döneminize ücretsiz dahil olan ölçüm miktarı |
| Faturalanabilir |Tüketilen Miktar dahil edilen miktar arasındaki fark gösterir. Bu miktar için faturalandırılırsınız. İle teklife hiçbir miktarın dahil olmadığı Kullandıkça Öde teklifleri için bu toplam tüketilen miktara aynıdır |
| Fiyat |Faturalanabilir birim başına ücretlendirildiğiniz oranı |
| Değer |Fazla kullanım miktarı sütununun ücret sütunuyla çarpılmasıyla elde edilen sonucu gösterir. Tüketilen Miktar dahil edilen miktarı aşmıyorsa, bu sütunda ücret alınmaz. |
| Alt toplam |Bu fatura dönemi için tüm ücretleri öncesi vergi toplamı |
| Genel toplam |Bu fatura dönemi için vergi sonra tüm ücretlerinizin toplamı |

## <a name="how-do-i-make-sure-that-the-charges-in-my-invoice-are-correct"></a>My fatura ücretleri doğru olduğunu nasıl emin olabilirim?
Daha fazla ayrıntı istediğiniz faturanızla ilgili bir ücret yoksa bkz [Microsoft Azure için faturanızı anlayın.](billing-understand-your-bill.md)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
