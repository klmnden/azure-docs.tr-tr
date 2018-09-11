---
title: Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme | Microsoft Docs
description: Kullanıcılarınızın erişim etkinliğini ve kimlikle ilgili sorunları izleme amacıyla Azure Güvenlik Merkezi'ndeki tanımlama ve erişim özelliklerini nasıl kullanacağınızı öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: rkarlin
ms.openlocfilehash: 17fd9907a5e3e3f4485b35c8e74d6e46fecb7fda
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303515"
---
# <a name="monitor-identity-and-access-in-azure-security-center-preview"></a>Kimlik ve erişim (Önizleme) Azure Güvenlik Merkezi'nde izleme
Bu makale kullanıcılarınızın kimliğini ve erişim etkinliğini izleme amacıyla Azure Güvenlik Merkezi'ni kullanmanıza yardımcı olur.

> [!NOTE]
> Kimlik ve erişim izleme önizlemede ve Güvenlik Merkezi'nin standart katmanında yalnızca kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
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
>

Bkz: [önerileri](security-center-identity-access.md#recommendations) Güvenlik Merkezi tarafından sağlanan kimlik ve erişim önerilerin bir listesi.

## <a name="monitoring-security-health"></a>Güvenlik durumunu izleme
Kaynaklarınızın güvenlik durumunu izleyebilirsiniz **Güvenlik Merkezi-genel bakış** Pano. **Kaynakları** bölüm önem dereceleri için her kaynak türü gösteren bir sistem durumu göstergesi değildir.

Seçerek, tüm sorunların bir listesini görüntüleyebilirsiniz **önerileri**. Altında **kaynakları**, işlem ve uygulamalar, veri güvenliği, ağ veya kimlik ve erişim için belirli sorunların bir listesini görüntüleyebilirsiniz. Önerileri uygulama hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md).

Kimlik ve erişim önerilerin tam listesi için bkz. [önerileri](security-center-identity-access.md#recommendations).

Devam etmek için seçin **kimlik ve erişim** altında **kaynakları** veya Güvenlik Merkezi ana menüsünde.

![Güvenlik Merkezi panosu][1]

## <a name="monitor-identity-and-access"></a>İzleyici kimlik ve erişim
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

1.  Bir abonelik seçin. Özet görünümü ile üç sekme açar:

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

| Öneri | Açıklama |
| --- | --- |
| Aboneliğinizde birden çok sahip belirleyin | Yönetici erişimi fazlalığı sağlamak için birden fazla abonelik sahibi belirlemeniz önerir. |
| Aboneliğinizde en fazla 3 sahipleri belirleyin | Güvenliği aşılmış bir sahip tarafından ihlal olasılığını azaltmak için 3'ten az abonelik sahipleri belirlemek önerir. |
| Aboneliğinizi sahip izinleri olan hesaplar için mfa'yı etkinleştirme | Hesapları veya kaynak ihlalini önlemek için yönetici ayrıcalıkları olan tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirmenizi önerir. |
| Aboneliğinizde yazma izinleri olan hesaplar için mfa'yı etkinleştirme | Hesapları veya kaynak ihlalini önlemek için yazma ayrıcalıklarına sahip tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirmenizi önerir. |
| Aboneliğinizde Okuma izinleri olan hesaplar için mfa'yı etkinleştirme | Hesapları veya kaynak ihlalini önlemek için okuma ayrıcalıklarına sahip tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirmenizi önerir. |
| Okuma izinleri olan dış hesapları aboneliğinizden kaldırın | İzlenmeyen erişimi engellemek için aboneliğinizden okuma ayrıcalıklarına sahip dış hesapların kaldırmanızı önerir. |
| Yazma izinleri olan dış hesapları aboneliğinizden kaldırın | İzlenmeyen erişimi engellemek için aboneliğinizden yazma ayrıcalıklarına sahip dış hesapların kaldırmanızı önerir. |
| Sahip izinleri olan dış hesapları aboneliğinizden kaldırın | Sahibi izinleri olan dış hesapları aboneliğinizden izlenmeyen erişimi engellemek için kaldırmanızı önerir. |
| Kullanım dışı bırakılmış hesapları abonelikten Kaldır | Önerir kaldırdığınız hesapları aboneliklerinizden kullanım dışı. |
| Sahip izinleri ile kullanım dışı bırakılmış hesapları abonelikten Kaldır | Önerir kaldırdığınız aboneliklerinizden sahip izinleri ile hesapları kullanım dışı. |

## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Makineleri ve Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-virtual-machine-recommendations.md)
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
