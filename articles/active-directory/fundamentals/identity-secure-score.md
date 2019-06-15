---
title: Güvenli kimlik puanı nedir? - Azure Active Directory
description: Dizininizin güvenlik duruşunu kimlik güvenli puanı nasıl kullanabileceğinizi
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: tilarso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cdff2305914ca6e4144f7784d1a60026a1d27c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65988712"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory"></a>Azure Active Directory'de kimlik güvenli puanı nedir?

Azure AD kiracınız ne düzeyde güvenli? Bu soruya nasıl bilmiyorsanız, bu makalede kimlik güvenli puanı izlemenizi ve kimlik güvenlik duruşunu nasıl yardımcı olduğunu açıklar.

## <a name="what-is-an-identity-secure-score"></a>Kimlik güvenliği puanı nedir?

Kimlik güvenli puan, 1 ile Microsoft'un en iyi yöntem önerileri güvenlik ile olduğunuz nasıl hizalanmış bir gösterge olarak işlev 223 arasında sayısıdır. Her bir kimlik güvenli puanı geliştirme eylemin belirli yapılandırmanızı uyarlanmıştır.  

![Güvenlik puanı](./media/identity-secure-score/identity-secure-score-overview.png)

Puanın yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz
- Kimlik güvenliği geliştirmelerini planlayabilirsiniz
- Geliştirmelerinizin başarısını gözden geçirebilirsiniz

Puana ve ilgili bilgilere kimlik güvenliği puanı panosundan erişebilirsiniz. Bu panoda şunları bulursunuz:

- Kimlik güvenli puanınız
- Aynı sektör ve benzer boyutta diğer kiracılar için kimliğinizi puanı güvenliğini nasıl gösteren bir karşılaştırma grafiği karşılaştırır.
- Kimlik güvenli puanınız zamanla nasıl değiştiğini gösteren bir eğilim Grafiği
- Olası geliştirmeleri listesi

Geliştirme eylemlerini izleyerek:

- Güvenlik duruşunuzu ve puanınız geliştirin
- Kuruluşunuz kimlik yatırımlarınızı bir parçası olarak sunulan özellikler yararlanın

## <a name="how-do-i-get-my-secure-score"></a>Güvenlik puanımı nasıl alabilirim?

Kimlik güvenli puanı, Azure AD tüm sürümlerinde kullanılabilir. Puanınıza erişmek için, [Azure AD Genel Bakış panosuna](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore) gidin.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Her 48 saatte bir Azure güvenlik yapılandırmanıza bakar ve sizin ayarlarınızı önerilen en iyi yöntemlerle karşılaştırır. Bu değerlendirme sonucuna bağlı olarak, yeni bir puan dizininiz için hesaplanır. Bu, güvenlik yapılandırması ile en iyi uygulama kılavuzunu tam olarak hizalı değil ve geliştirme eylemleri yalnızca kısmen karşılandığından mümkündür. Bu senaryolarda, yalnızca bir kısmını denetimi için kullanılabilen en fazla puan verilecektir.

Her öneri, Azure AD yapılandırmanız temelinde ölçülür. En iyi yöntem öneri etkinleştirmek için üçüncü taraf ürünler kullanıyorsanız, bu yapılandırma ayarlarında geliştirme eylem belirtebilirsiniz. Ayrıca, ortamınız için uygulamazsanız yok sayılacak önerileri ayarlanacak seçeneğiniz de vardır. Yoksayılan öneri puanınızın hesaplanmasına katılmaz.

![Yoksay veya eylem üçüncü taraf tarafından kapsanan olarak işaretle](./media/identity-secure-score/identity-secure-score-ignore-or-third-party-reccomendations.png)

## <a name="how-does-it-help-me"></a>Bana nasıl yardımcı olur?

Güvenlik puanının yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz
- Kimlik güvenliği geliştirmelerini planlayabilirsiniz
- Geliştirmelerinizin başarısını gözden geçirebilirsiniz

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="who-can-use-the-identity-secure-score"></a>Kimlik güvenliği puanını kimler kullanabilir?

Kimlik güvenliği puanı aşağıdaki roller tarafından kullanılabilir:

- Genel yönetici
- Güvenlik yöneticisi
- Güvenlik okuyucuları

### <a name="how-are-controls-scored"></a>Denetimleri nasıl puanlanır?

Denetimler, iki yolla puanlanmış. Bazı ikili biçimde puanlanır - puanı, %100 özelliği varsa veya ayarı yapılandırıldıysa önerimizde üzerinde temel alın. Diğer puanlarının toplam yapılandırma yüzdesi olarak hesaplanır. İyileştirme önerisi tüm kullanıcılarınız MFA ile korumak ve korumalı 100 toplam kullanıcı 5 varsa yalnızca 30 noktaları alırsınız belirtiliyorsa, örneğin, kısmi bir puan yaklaşık 2 nokta verilmesi (korumalı 5 / toplamda 100 * 30 max nk 2 nk kısmi puan =) .

### <a name="what-does-not-scored-mean"></a>[Puanlanmadı] ne anlama geliyor?

[Puanlanmadı] olarak etiketlenen eylemler, kuruluşunuzda gerçekleştirebileceğiniz ama araca bağlanmadığından (henüz!) puanlanmayacak olan eylemlerdir. Dolayısıyla, yine de güvenliğinizi geliştirebilirsiniz ama şu anda bu eylemler için puan almazsınız.

### <a name="how-often-is-my-score-updated"></a>Puanım ne sıklıkta güncelleştirilir?

Puan günde bir kez hesaplanır (yaklaşık 24:00 PST). Ölçülen eylemde değişiklik yaparsanız, puan ertesi gün otomatik olarak güncelleştirilir. Değişikliğin puanınıza yansıması 48 saat kadar sürebilir.

### <a name="my-score-changed-how-do-i-figure-out-why"></a>Puanım değişti. Nedenini nasıl anlayabilirim?

Gidin [Microsoft 365 Güvenlik Merkezi](https://security.microsoft.com/), tüm Microsoft Güvenli puanınız burada bulabilirsiniz. Tüm değişiklikleri güvenli puanınız geçmiş sekmesinde kapsamlı değişiklikler gözden geçirerek kolayca görebilirsiniz.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>Güvenli puanı ihlal alma my riskini ölçmek mu?

Kısacası, hayır. Güvenli puanı mutlak bir ölçü ifade etmez, büyük olasılıkla nasıl ihlal üzeresiniz. Güvenlik ihlaline uğrama riskini azaltabilecek özellikleri ne düzeyde benimsediğinizi ortaya koyar. Hiçbir hizmet değil ihlal ve güvenli puanı herhangi bir şekilde garantisi olarak yorumlanmamalıdır garanti edebilir.

### <a name="how-should-i-interpret-my-score"></a>Puanımı nasıl yorumlayabilirim?

Önerilen güvenlik özelliklerini yapılandırmanızdan veya güvenlikle ilgili görevleri (raporları okuma gibi) gerçekleştirmenizden dolayı size puan verilir. Kullanıcılarınız için Multi-Factor Authentication'ı (MFA) etkinleştirmeniz gibi bazı eylemler, kısmen tamamlanmış olarak puanlanır. Güvenli puanınız kullandığınız Microsoft Güvenlik Hizmetleri doğrudan temsilcisidir. Kullanılabilirlik ile güvenlik dengelenmelidir unutmayın. Tüm güvenlik denetimlerinin bir de kullanıcı etkisi bileşeni vardır. Düşük kullanıcı etkisine sahip denetimlerin, kullanıcılarınızın gündelik işlemleri üzerinde çok az etkisi olması veya hiç olmaması gerekir.

Puan geçmişini görmek için attıktan [Microsoft 365 Güvenlik Merkezi](https://security.microsoft.com/) ve genel Microsoft Güvenli puanınız gözden geçirin. Genel değişiklik gözden geçirebilirsiniz güvenli puanı görünümü geçmişi tıklayarak. Belirli bir tarih seçerek o gün hangi denetimlerin etkinleştirildiğini ve her birinden kaç puan kazandığınızı görün.

### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>Kimlik güvenliği puanı ile Office 365 güvenlik puanı arasında nasıl bir ilişki vardır?

[Microsoft Güvenli puanı](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score) beş içeren farklı denetim ve puan kategorileri:

- Kimlik
- Veriler
- Cihazlar
- Altyapı
- Uygulamalar

Kimlik güvenli puanı Microsoft Güvenli puanı kimliği bölümünü temsil eder. Bu çakışma puanı önerilerinizi kimliği için güvenli ve Microsoft Identity puanı aynıdır anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Güvenli puan hakkında daha fazla bilgi edinin](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score)
