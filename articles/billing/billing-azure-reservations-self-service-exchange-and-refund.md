---
title: Self Servis değişimleri ve Azure ayırmalar için para iadesi | Microsoft Docs
description: Bilgi nasıl değiştirebilir ya da para iadesi Azure ayırmalar.
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 54578746ea8029a760663edc456660f98358abc5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615974"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Self Servis değişimleri ve Azure ayırmalar için para iadesi

Azure ayırmalar, gelişen gereksinimlerini karşılamaya yardımcı olmak için esneklik sağlar. Bir rezervasyonu aynı türdeki başka bir rezervasyonla değiştirebilirsiniz. İhtiyaç duymadığınız rezervasyonları da iade ederek yılda 50.000 ABD dolarına kadar para iadesi alabilirsiniz.

Self servis değişim ve iptal özelliği US Government Kurumsal Anlaşma müşterileri tarafından kullanılamaz. Kullandıkça Öde ve CSP dahil olmak üzere diğer US Government aboneliği türleri desteklenir.

Exchange veya mevcut bir ayırma para iadesi için rezervasyon siparişi sahibi erişiminiz olmalıdır.

## <a name="exchange-an-existing-reserved-instance"></a>Mevcut bir ayrılmış örnek değişimi

Üç hızlı adımda, rezervasyon değiştirebilir [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

1. Para iadesi ve istediğiniz ayırmaları seçin **Exchange**.  
    ![Döndürülecek ayırmaları gösteren örnek resim](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-return.png)
2. Satın almak istediğiniz VM ürününü seçip miktar girin. Yeni satın alma toplam dönüş toplam birden fazla olduğundan emin olun. [Satın aldığınız önce doğru boyutta belirlemek](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).  
    ![Bir exchange ile satın almak için VM ürün gösteren örnek resim](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-select-purchase.png)
3. Gözden geçirin ve işlemi tamamlayın.  
    ![Bir exchange dönüş tamamlama, satın almak için VM ürün gösteren örnek resim](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-confirm-exchange.png)

Rezervasyon para iadesi için şuraya gidin: **rezervasyon ayrıntıları** tıklatıp **para iadesi**.

## <a name="how-transactions-are-processed"></a>İşlemler nasıl işlenir

İlk olarak, Microsoft var olan ayırmayı iptal eder ve eşit olarak miktarın bu ayırma için. Bir exchange varsa, yeni satın alma işlenir. Microsoft, ödeme yöntemini ve hesap türüne bağlı olarak, aşağıdaki yöntemlerden birini kullanarak para iadelerini işleme:

### <a name="enterprise-agreement-customers"></a>Kurumsal Anlaşma kapsamındaki müşteriler

Para değişimleri için parasal taahhüt eklenir ve satın alma biri kullanılarak yapıldıysa mahsup işlemleri. Orijinal satın alma işlemleri yeniden ve parasal taahhüt emin olmak için rerated olduğundan fazla kullanım faturanız kullanılır. Daha sonra Rezervasyon satın alınmış kullanarak parasal taahhüt terimi artık etkin olursa, kredi geçerli Kurumsal Anlaşma parasal taahhüt döneminizin için eklenir.

Satın alma bir kapasite aşımı olarak yapıldıysa, Microsoft iade faturası verir.

### <a name="pay-as-you-go-invoice-payments-and-csp-program"></a>Kullandıkça Öde faturalı ödemeleri ve CSP programı

Orijinal rezervasyon satın alma fatura iptal ve ardından yeni bir fatura para iadesi oluşturulur. Değişimleri için yeni bir fatura para iadesi ve yeni satın alma işlemini gösterir. İade miktarı, satın alma karşı ayarlanır. Yalnızca bir rezervasyonu para iadesi, Microsoft ile eşit olarak bölünmüş miktarı kalır ve bir sonraki rezervasyon satın alma karşı ayarlanır.

### <a name="pay-as-you-go-credit-card-customers"></a>Kullandıkça Öde kredi kartı müşterileri

Özgün fatura iptal edilir ve yeni bir fatura oluşturulur. Orijinal satın alma için kullanılan kredi kartını parası iade. Kartınız değiştirdiyseniz [desteğe](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="exchange-policies"></a>Exchange ilkeleri

- Aynı türde yeni bir rezervasyon satın almak için birden çok mevcut ayırmaları döndürebilir. Başka bir tür ayırmalarını exchange olamaz. Örneğin, bir SQL ayırma satın almak için bir VM ayırma döndüremez.
- Yalnızca ayırma sahipleri bir exchange işleyebilir. [Bilgi ayırma yönetebilen ekleme veya değiştirme kullanıcılara nasıl](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance#add-or-change-users-who-can-manage-a-reservation).
- Bir exchange bir para iadesi ve repurchase olarak işlenen – farklı işlemleri iptal ve yeni satın alma için oluşturulur. Eşit olarak bölünmüş ayırma miktarı, ayırmalar için iade söz konusu, takas. Ayrıca, yeni satın alma için tam ücret ödersiniz. Eşit olarak bölünmüş ayırma miktarı günlük saatlere eşit olarak dağıtılmış kalan döndürülen ayırma değeridir.
- Gönderip alabilir ya da para iadesi ayırmaları Kurumsal Anlaşma rezervasyon satın almak için kullanılan olsa bile süresi doldu ve yeni bir sözleşme kadar yenilendi.
- Boyut, bölge, miktarı ve bir exchange terimiyle gibi herhangi bir ayırma özelliği değiştirebilirsiniz.
- Yeni satın alma toplam eşit veya döndürülen miktar alt sınırından büyük olması gerekir.
- Exchange bir parçası olarak satın alınan yeni ayırma, exchange saatten başlayarak yeni bir dönemi kapsar.
- Hiçbir cezasına sebep veya yıllık sınırları değişimleri için yoktur.

## <a name="refund-policies"></a>İade ilkeleri

- Toplam iade 50.000 ABD Doları 12 aylık çalışırken penceresinde aşamaz.
- Yalnızca ayırma sahipleri bir para iadesi işleyebilir. [Bilgi ayırma yönetebilen ekleme veya değiştirme kullanıcılara nasıl](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Ceza ücret şu anda değil ancak Microsoft % 12 cezası herhangi döndürür için ücret hakkını saklı tutar.

## <a name="exchange-non-premium-storage-for-premium-storage"></a>Premium depolama için Exchange premium olmayan depolama

Satın alınan yapan karşılık gelen bir VM boyutu için premium depolama desteği olmayan bir VM boyutu için bir ayırma değiştirebilir. Örneğin, bir _F1_ için bir _F1s_. Değişikliği yapmak için ayırma ayrıntılarına gidin ve **Exchange**. Exchange ayrılmış örnek dönemi sıfırlama değil ya da yeni bir işlem oluşturun.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).
- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
    - [Azure'da ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
    - [Ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
    - [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
    - [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
    - [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
    - [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](/partner-center/azure-reservations)
