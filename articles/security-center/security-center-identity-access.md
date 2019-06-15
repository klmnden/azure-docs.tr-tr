---
title: Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme | Microsoft Docs
description: Kullanıcılarınızın erişim etkinliğini ve kimlikle ilgili sorunları izleme amacıyla Azure Güvenlik Merkezi'ndeki tanımlama ve erişim özelliklerini nasıl kullanacağınızı öğrenin.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2018
ms.author: monhaber
ms.openlocfilehash: a30bc55c564f852a5fef6e71aad9e607e6aa1065
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083668"
---
# <a name="monitor-identity-and-access-in-azure-security-center-preview"></a>Kimlik ve erişim (Önizleme) Azure Güvenlik Merkezi'nde izleme
Bu makale kullanıcılarınızın kimliğini ve erişim etkinliğini izleme amacıyla Azure Güvenlik Merkezi'ni kullanmanıza yardımcı olur.

> [!NOTE]
> "View *Klasik* kimlik ve erişim" bağlantı 31 Temmuz 2019 üzerinde kullanımdan kaldırılacak. Tıklayın [burada](security-center-features-retirement-july2019.md#menu_classicidentity) alternatif Services'ta öğrenin.

> [!NOTE]
> Kimlik ve erişim izleme önizlemede ve Güvenlik Merkezi'nin standart katmanında yalnızca kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>

Kimlik, kuruluşunuz için denetim düzlemi olmalıdır, kimliğinizi korumak ise en yüksek önceliğiniz olmalıdır. Güvenlik çevresi bir ağ çevresi bir kimlik çevre için geliştirilmiştir. Güvenlik savunma ağınız hakkında daha az ve daha fazlası hakkında verilerinizi savunma yanı uygulamaları ve kullanıcılarınızın güvenliğini yönetme olur. Günümüzde buluta taşınan veri ve uygulama miktarı arttıkça kimlik yeni savunma hattı haline geliyor.

Kimlik etkinliklerini izleyerek bir olay gerçekleşmeden önce öngörülü eylemlerde veya bir saldırı girişimini durdurmak için reaktif eylemlerde bulunabilirsiniz. Kimlik ve erişim panosu ile ilgili öneriler gibi sağlar:

- Aboneliğinizde ayrıcalıklı hesaplar için MFA'yı etkinleştirin
- Yazma izinleri olan dış hesapları aboneliğinizden kaldırın
- Ayrıcalıklı dış hesapları aboneliğinizden kaldırın

> [!NOTE]
> Aboneliğinizi 600'den fazla hesapları varsa, Güvenlik Merkezi kimlik önerileri, aboneliğe göre çalıştırılır silemiyor. Çalıştırılamaz öneriler "altında açıklanan kullanılamayan iç değerlendirmeler" altında listelenir.
Güvenlik Merkezi bir bulut çözümü sağlayıcısı (CSP) iş ortağının yönetim aracıları karşı kimlik önerileri çalıştırılamıyor.
>

Bkz: [önerileri](security-center-identity-access.md#recommendations) Güvenlik Merkezi tarafından sağlanan kimlik ve erişim önerilerin bir listesi.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Kaynaklarınızın güvenlik durumunu izleyebilirsiniz **Güvenlik Merkezi-genel bakış** Pano. **Kaynakları** bölüm önem dereceleri için her kaynak türü gösteren bir sistem durumu göstergesi değildir.

Seçerek, tüm sorunların bir listesini görüntüleyebilirsiniz **önerileri**. Altında **kaynakları**, işlem ve uygulamalar, veri güvenliği, ağ veya kimlik ve erişim için belirli sorunların bir listesini görüntüleyebilirsiniz. Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

Kimlik ve erişim önerilerin tam listesi için bkz. [önerileri](security-center-identity-access.md#recommendations).

Devam etmek için seçin **kimlik ve erişim** altında **kaynakları** veya Güvenlik Merkezi ana menüsünde.

![Güvenlik Merkezi panosu][1]

## <a name="monitor-identity-and-access"></a>Kimlik ve erişimi izleme
Altında **kimlik ve erişim**, iki sekme bulunur:

- **Genel Bakış**: Güvenlik Merkezi tarafından tanımlanan öneriler.
- **Abonelikleri**: abonelikler ve her birinin geçerli güvenlik durumunu listesi.

![Kimlik ve Erişim][2]

### <a name="overview-section"></a>Genel Bakış bölümünde
Altında **genel bakış**, önerilerin bir listesi bulunur. İlk sütunda öneriler listelenmiştir. İkinci sütunda, bu önerinin etkilediği abonelik toplam sayısını gösterir. Üçüncü sütunda, sorunun önem derecesini gösterir.

1. Bir öneri seçin. Öneri 's penceresi açılır ve görüntüler:

   - Öneri açıklaması
   - İyi durumda olmayan ve iyi durumda aboneliklerin listesi
   - Başarısız bir değerlendirmesi nedeniyle Taranmayan kaynaklar veya kaynak listesi ücretsiz katmanda çalışan bir abonelik altındadır ve değil olarak değerlendirilir

   ![Öneri'nın penceresi][3]

1. Ek ayrıntılı bilgi için listedeki bir abonelik seçin.

### <a name="subscriptions-section"></a>Abonelikleri bölümü
Altında **abonelikleri**, aboneliklerin listesi yoktur. İlk sütun abonelikleri listelenir. İkinci sütunda öneriler her abonelik için toplam sayısını gösterir. Üçüncü sütunda, sorunların önem derecelerinin gösterir.

![Aboneliğin sekmesi][4]

1. Bir abonelik seçin. Özet görünümü ile üç sekme açar:

   - **Öneriler**: Değerlendirme başarısız oldu Güvenlik Merkezi tarafından gerçekleştirilen göre.
   - **Değerlendirmeler geçirilen**: Güvenlik Merkezi tarafından geçirilen gerçekleştirdiği değerlendirmeler listesi.
   - **Kullanılamayan iç değerlendirmeler**: bir hata nedeniyle çalıştırılamadı değerlendirmelerinin listesini veya 600'den fazla hesap abonelik yok.

   Altında **önerileri** seçili abonelik için önerilerin bir listesi ve önerilerin önem derecesi.

   ![Abonelik seçme önerileri][5]

1. Öneri, iyi durumda olmayan ve iyi durumda Aboneliklerin listesini ve Taranmayan kaynaklar listesi açıklaması için bir öneri seçin.

   ![Öneri açıklaması][6]

   Altında **değerlendirmeleri geçirilen** geçilen iç değerlendirmeler listesidir.  Bu değerlendirmeler önemini her zaman büyük/küçük harf yeşildir.

   ![Geçilen iç değerlendirmeler][7]

1. Geçirilen bir değerlendirme değerlendirme açıklamasını listesini ve Sağlıklı Aboneliklerin listesini seçin. İyi durumda olmayan abonelik için başarısız olan ilgili tüm abonelikleri listeler bir sekme yoktur.

   ![Geçilen iç değerlendirmeler][8]

## <a name="recommendations"></a>Öneriler
Aşağıdaki tabloda kullanılabilir kimlik ve erişim öneriler ve uygulamanız durumunda her birinin yaptığı anlamanıza yardımcı olması için bir başvuru olarak kullanın.

|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|Abonelik|50|Aboneliğinizde sahip izinleri ile hesapları MFA etkinleştirilmelidir|Hesapları veya kaynak ihlalini önlemek için yönetici ayrıcalıkları olan tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirin.|
|Abonelik|40|MFA abonelik hesaplarınızı yazma izinlerine sahip etkinleştirilmiş olmalıdır|Hesapları veya kaynak ihlalini önlemek için yazma ayrıcalıklarına sahip tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirin.|
|Abonelik|30|Sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekiyor|Sahip izinleri olan dış hesapları aboneliğinizden izlenmeyen erişimi engellemek için kaldırın.|
|Abonelik|30|Mfa'yı Okuma izinleri olan abonelik hesaplarınızı etkinleştirilmiş olmalıdır|Hesapları veya kaynak ihlalini önlemek için okuma ayrıcalıklarına sahip tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirin.|
|Abonelik|25|Yazma sahip dış hesapların aboneliğinizden kaldırılması gerekiyor izinleri|Yazma izinleri olan dış hesapları aboneliğinizden izlenmeyen erişimi engellemek için kaldırın. |
|Abonelik|20|Sahip izinleri ile kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor|Sahip izinleri ile kullanım dışı bırakılmış hesapların aboneliklerinizden kaldırın.|
|Abonelik|5|Kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor|Kullanım dışı bırakılmış hesapların aboneliklerinizden yalnızca geçerli kullanıcılara erişimi etkinleştirmek için kaldırın. |
|Abonelik|5|Aboneliğinize atanmış birden fazla sahibi olmalıdır.|Yönetici erişimi fazlalığı sağlamak için birden fazla abonelik sahibi belirleyin.|
|Abonelik|5|En fazla 3 sahipleri, aboneliğiniz için belirlenen|Güvenliği aşılmış bir sahip tarafından ihlal olasılığını azaltmak için 3'ten az abonelik sahipleri belirleyin.|
|Key Vault|5|Tanılama günlükleri, anahtar Kasası'nda etkinleştirilmelidir.|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|Abonelik|15|Okuma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekiyor|Okuma ayrıcalıklarına sahip dış hesapların aboneliğinizden izlenmeyen erişimi engellemek için kaldırın.| 

> [!NOTE]
> MFA gerektirir, ancak ayarlanmış dışarıda bırakılacak sahip bir koşullu erişim ilkesi oluşturduğunuzda, Azure MFA olmadan oturum açmak bazı kullanıcılar sağladığından Güvenlik Merkezi MFA öneri değerlendirme İlkesi uyumlu olmayan, göz önünde bulundurur.

## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-recommendations.md)
- [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
- [Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi'nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını bulun.


<!--Image references-->
[1]: ./media/security-center-identity-access/overview.png
[2]: ./media/security-center-identity-access/identity-dashboard.png
[3]: ./media/security-center-identity-access/select-subscription.png
[4]: ./media/security-center-identity-access/subscriptions.png
[5]: ./media/security-center-identity-access/recommendations.png
[6]: ./media/security-center-identity-access/designate.png
[7]: ./media/security-center-identity-access/passed-assessments.png
[8]: ./media/security-center-identity-access/remove.png
