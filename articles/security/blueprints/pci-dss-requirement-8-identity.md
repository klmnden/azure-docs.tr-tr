---
title: "Azure ödeme işleme şeması - kimlik gereksinimleri"
description: PCI DSS gereksinim 8
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 1a398601-8c48-4f8e-b3d4-eba94edad61c
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: f77cc3c9926b5316913c70e5f4412383e55c5193
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="identity-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için kimlik gereksinimleri 
## <a name="pci-dss-requirement-8"></a>PCI DSS gereksinim 8

**Tanımlamak ve sistem bileşenlerine erişimi kimlik doğrulaması**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Erişimi olan her kişi için benzersiz bir kimlik (ID) atama her kendi benzersiz olarak eylemlerinden olmasını sağlar. Böyle sorumluluk yerinde olduğunda kritik veri ve sistemlerinde yapılan eylemler tarafından gerçekleştirilen ve bilinen ve yetkili kullanıcıları ve işlemleri için izlenebilir.
Bir parola verimliliğini büyük ölçüde tasarım ve uygulama kimlik doğrulama sistemi tarafından belirlenir; özellikle, ne sıklıkta parola denemelerinin bir saldırganın ve ilişkin güvenlik yöntemlerini girişi noktasında kullanıcı parolaları korumak üzere yapılabilir iletim sırasında ve depolama sırasında.

> [!NOTE]
> Bu gereksinimleri satış noktası hesaplarını yönetim yetenekleri ve görüntülemek veya kart sahibi erişmek için kullanılan tüm hesapları ile de dahil olmak üzere tüm hesapları için geçerli olan veri veya erişim sistemlerine kart sahibi verilerle. Bu, (örneğin, destek veya bakım için) satıcıları ve diğer üçüncü taraflar tarafından kullanılan hesapları içerir. Bu gereksinimleri (örn., cardholders) tüketiciler tarafından kullanılan hesapları için geçerli değildir. Ancak, gereksinimleri 8.1.1, 8.2, 8.5, 8.2.3 8.2.5 ve 8.1.6 8.1.8 aracılığıyla aracılığıyla yalnızca kolaylaştırmak için tek bir işlem (bir kerede bir kartı numarası erişimi olan kullanıcı hesapları için satış noktası ödeme uygulamadaki uygulamak için amaçlanmamıştır Kasiyer hesapları gibi).
 
## <a name="pci-dss-requirement-81"></a>PCI DSS gereksinim 8.1

**8.1** tanımlayın ve uygulama ilkeleri ve yordamlarıyla tüketici olmayan kullanıcıların ve yöneticilerin tüm sistem bileşenleri için uygun kullanıcı kimlik yönetimi gibi emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore örnek dağıtımı için kullanılan bir örnek ve yöneticilerin doğru kullanım için bir açıklama sağlar.|



### <a name="pci-dss-requirement-811"></a>PCI DSS gereksinim 8.1.1

**8.1.1** erişim sistem bileşenleri veya kart sahibi veri vermeden önce tüm kullanıcıların benzersiz bir kimliği atayın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Azure Active Directory Contoso Webstore uygular ve Azure Active Directory Role-Based erişim denetimi (RBAC), tüm kullanıcıların emin olmak için benzersiz bir kimliği olan Daha fazla bilgi için bkz: [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-812"></a>PCI DSS gereksinim 8.1.2

**8.1.2** denetim ekleme, silme ve kullanıcı kimliği, kimlik bilgileri ve diğer tanımlayıcı nesneleri değiştirilmesini.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Azure Active Directory Contoso Webstore uygular ve Azure Active Directory Role-Based erişim denetimi (RBAC), tüm kullanıcıların emin olmak için benzersiz bir kimliği olan Daha fazla bilgi için bkz: [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-813"></a>PCI DSS gereksinim 8.1.3

**8.1.3** hemen iptal erişim herhangi kullanıcılar sonlandırıldı.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore kullanıcı yönetimi için Azure Active Directory kullanır. Active Directory'de kullanıcı iptal yapılabilir.|



### <a name="pci-dss-requirement-814"></a>PCI DSS gereksinim 8.1.4

**8.1.4** kaldırın veya 90 gün içinde etkin kullanıcı hesapları devre dışı bırakın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore kullanıcı yönetimi için Azure Active Directory kullanır. `-enableADDomainPasswordPolicy` Seçeneği, parolaların süreleri 90 gün içinde emin olmak için ayarlanabilir.|



### <a name="pci-dss-requirement-815"></a>PCI DSS gereksinim 8.1.5

**8.1.5** üçüncü taraflar tarafından erişmek için kullanılan kimlikleri yönetmek desteklemek ya da uzaktan erişim üzerinden sistem bileşenleri gibi koruyun:
- Gerekli ve kullanılmadığı zaman devre dışı yalnızca süre boyunca etkin.
- Kullanılmadığı zaman izlenen.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure geçerli şirket ve kuruluş güvenlik ilkeleri, bilgi güvenlik ilkesi de dahil olmak üzere benimsemiştir. İlkeleri onaylanan yayımlanan ve Microsoft Azure için iletilen. Bilgi güvenlik ilkesi verilebilmesi için Microsoft Azure varlıklara erişimi varlık sahibinin yetkilendirme ile iş gerekçesinin dayalı ve temel alınarak "gerek bilme" ve "en az ayrıcalıklı" ilkeleri sınırlı olmasını gerektirir. Ayrıca, İlkesi de dahil erişim sağlama, kimlik doğrulama, erişim yetkilendirme access Yönetimi yaşam döngüsü gereksinimlerini ele alır, temizleme erişim haklarını ve düzenli erişim gözden geçirme. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore demo Azure Active Directory ve Azure Active Directory Role-Based erişim denetimi yükleme için kullanıcı erişimi yönetmek üzere uyguladı. Daha fazla bilgi için bkz: [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-816"></a>PCI DSS gereksinim 8.1.6

**8.1.6** sınırı yinelenen erişim deneme geçmeyen altı denemeden sonra kullanıcı kimliği kilitleyerek.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore görevlerini (Çimle KAPLA) demo tüm kullanıcıları için açıkça ayrılması uyguladı. Daha fazla bilgi için ""Azure Active Directory kimlik koruması"konumundaki [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).|



### <a name="pci-dss-requirement-817"></a>PCI DSS gereksinim 8.1.7

**8.1.7** kilitleme süresi için en az 30 dakika veya bir yönetici kullanıcı kimliğini sağlar kadar ayarlayın

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Müşteriler, oluşturma, zorlama ve PCI DSS gereksinimleriyle uyumlu bir parola ilkesi izleme sorumludur.|



### <a name="pci-dss-requirement-818"></a>PCI DSS gereksinim 8.1.8

**8.1.8** terminal veya oturumu yeniden etkinleştirmek için yeniden kimlik doğrulaması kullanıcı oturumu 15 dakikadan fazla boşta durumunda gerektirir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Müşteriler, oluşturma, zorlama ve PCI DSS gereksinimleriyle uyumlu bir parola ilkesi izleme sorumludur.|



## <a name="pci-dss-requirement-82"></a>PCI DSS gereksinim 8.2

**8.2** benzersiz bir kimliği atamaya ek olarak, tüm kullanıcıların kimliğini doğrulamak için aşağıdaki yöntemlerden birini kullanarak tüketici olmayan kullanıcıların ve yöneticilerin tüm sistem bileşenleri için doğru kullanıcı kimlik yönetimi olun:
- Bildiğiniz bir şey, bir parola veya parola gibi
- Sahip olduğunuz şey, bir belirteç aygıt veya akıllı kart gibi
- Gibi bir biyometrik olduğunuz bir şey

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Gösteri için kullanım kolaylığı sağlamak üzere Contoso Webstore uygulama çok faktörlü kimlik doğrulaması için devre dışı bırakıldı. Çok faktörlü kimlik doğrulaması kullanılarak uygulanabilir [Azure çok faktörlü kimlik doğrulaması](https://azure.microsoft.com/services/multi-factor-authentication/).|



### <a name="pci-dss-requirement-821"></a>PCI DSS gereksinim 8.2.1

**8.2.1** güçlü şifreleme kullanmak işlemek tüm kimlik doğrulama kimlik bilgilerini (örneğin, parolaları/tümceleri) okunamaz iletim ve tüm sistem bileşenleri depolama sırasında.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure (örn., oluşturma, dağıtım, iptal) yaşam döngüleri boyunca şifreleme anahtarlarını yönetmek için anahtar yönetimi yordamları oluşturmuştur. Microsoft Azure Microsoft'un Kurumsal PKI altyapısı kullanır. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore güçlü parolalar, Dağıtım Kılavuzu'nda açıklandığı zorlar. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).<br /><br />|



### <a name="pci-dss-requirement-822"></a>PCI DSS gereksinim 8.2.2

**8.2.2** kimlik doğrulama kimlik değiştirmeden önce kullanıcı kimliğini doğrulayın — Örneğin, parola sıfırlama gerçekleştirme, yeni belirteçleri sağlama veya yeni anahtarlar oluşturma.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure (örn., oluşturma, dağıtım, iptal) yaşam döngüleri boyunca şifreleme anahtarlarını yönetmek için anahtar yönetimi yordamları oluşturmuştur. Microsoft Azure Microsoft'un Kurumsal PKI altyapısı kullanır. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore güçlü parolalar, Dağıtım Kılavuzu'nda açıklandığı zorlar. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-823"></a>PCI DSS gereksinim 8.2.3

**8.2.3** parolaları/parolaları aşağıdaki karşılaması gerekir:
- Minimum uzunluğu en az yedi karakter gerektirir.
- Sayısal ve alfabetik karakterler içeriyor.
Alternatif olarak, parolalar/parolanın karmaşıklık ve yukarıda belirtilen parametreler için en az eşdeğer gücü olması gerekir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore güçlü parolalar, Dağıtım Kılavuzu'nda açıklandığı zorlar.|



### <a name="pci-dss-requirement-824"></a>PCI DSS gereksinim 8.2.4

**8.2.4** 90 günde bir en az bir kez kullanıcı parolaları/parolaları değiştirme.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore kullanıcı yönetimi için Azure Active Directory kullanır. `-enableADDomainPasswordPolicy` Seçeneği, parolaların süreleri 90 günde bir en az bir kez emin olmak için ayarlanabilir.|



### <a name="pci-dss-requirement-825"></a>PCI DSS gereksinim 8.2.5

**8.2.5** yeni çözemiyorsa kullanılan son dört parolaları/parolaları hiçbirini ile aynı olan parola göndermek tek bir izin verme.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore güçlü parolalar, Dağıtım Kılavuzu'nda açıklandığı zorlar. Daha fazla bilgi için bkz: [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-826"></a>PCI DSS gereksinim 8.2.6

**8.2.6** ayarlamak parolaları/parolaları sıfırlama bağlıdır ve ilk kez kullanmak için benzersiz bir değer her kullanıcı için ve hemen ilk kullanımdan sonra değiştirin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore güçlü parolalar, Dağıtım Kılavuzu'nda açıklandığı zorlar. Daha fazla bilgi için bkz: [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).<br /><br />|



## <a name="pci-dss-requirement-83"></a>PCI DSS gereksinim 8.3

**8.3** güvenli bireysel tüm konsol içi yönetimsel erişim ve tüm uzaktan erişim çok faktörlü kimlik doğrulaması kullanarak kart sahibi veri ortamına (CDE).

> [!NOTE]
> Çok faktörlü kimlik doğrulaması en az iki (gereksinimi 8.2 kimlik doğrulama yöntemleri açıklamalarına bakın) üç kimlik doğrulama yöntemleri için kimlik doğrulaması kullanılması gerektirir. Bir faktör (iki ayrı parolalar kullanılarak iki kez Örneğin,) kullanarak çok faktörlü kimlik doğrulaması olarak kabul edilmez.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure yöneticileri Bakım ve yönetim sunucuları ve Azure sistemler için gerçekleştirirken erişmek için çok faktörlü kimlik doğrulaması kullanmak için gereklidir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı sırasında üç hesapları oluşturur: yönetici, sqladmin ve edna (tanıtım yürütme sırasında web uygulamasına oturum varsayılan kullanıcı). Çok faktörlü kimlik doğrulaması için demo uygulanmadı.|



### <a name="pci-dss-requirement-831"></a>PCI DSS gereksinim 8.3.1

**8.3.1** birleştirme çok faktörlü kimlik doğrulamasını tüm konsol içi erişimi için CDE içine yönetimsel erişime sahip personel.

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure yöneticileri Bakım ve yönetim sunucuları ve Azure sistemler için gerçekleştirirken erişmek için çok faktörlü kimlik doğrulaması kullanmak için gereklidir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı sırasında üç hesapları oluşturur: yönetici, sqladmin ve edna (tanıtım yürütme sırasında web uygulamasına oturum varsayılan kullanıcı). Çok faktörlü kimlik doğrulaması için demo uygulanmadı.|



### <a name="pci-dss-requirement-832"></a>PCI DSS gereksinim 8.3.2

**8.3.2** tüm uzaktan ağ erişimi (hem kullanıcı ve yönetici ve destek ve bakım için üçüncü taraf erişim dahil olmak üzere) için birleştirme çok faktörlü kimlik doğrulaması varlığın Ağ dışından kaynaklanan.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure yöneticileri Bakım ve yönetim sunucuları ve Azure sistemler için gerçekleştirirken erişmek için çok faktörlü kimlik doğrulaması kullanmak için gereklidir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı sırasında üç hesapları oluşturur: yönetici, sqladmin ve edna (tanıtım yürütme sırasında web uygulamasına oturum varsayılan kullanıcı). Çok faktörlü kimlik doğrulaması için demo uygulanmadı.|



## <a name="pci-dss-requirement-84"></a>PCI DSS gereksinim 8.4

**8.4** belge ve kimlik doğrulama ilkeleri ve yordamları da dahil olmak üzere tüm kullanıcılara iletişim:
- Güçlü kimlik doğrulaması kimlik bilgilerini seçme konusunda yönergeler
- Kullanıcıların kendi kimlik doğrulama kimlik bilgilerini nasıl korumalısınız Kılavuzu
- Yeniden önceden kullanılan parolaları için yönergeler
- Tüm şüpheyle parola ise parolalarını değiştirmek için yönergeleri tehlikeye girebilir

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Aşağıdaki yönergeler ve belgeleme ve kimlik doğrulama yordamları ve tüm kullanıcılara ilkeleri iletişim için müşterileri sorumludur.|



## <a name="pci-dss-requirement-85"></a>PCI DSS gereksinim 8.5

**8.5** paylaşılan, Grup veya genel kimlikleri, parolalar ve diğer kimlik doğrulama yöntemleri gibi kullanmayın:
- Genel kullanıcı kimlikleri devre dışı veya kaldırılır.
- Sistem Yönetimi ve diğer kritik işlevler için paylaşılan kullanıcı kimlikleri yok.
- Paylaşılan ve genel kullanıcı kimlikleri, tüm sistem bileşenlerini yönetmek için kullanılmaz.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı sırasında üç hesapları oluşturur: yönetici, sqladmin ve edna (tanıtım yürütme sırasında web uygulamasına oturum varsayılan kullanıcı). Çok faktörlü kimlik doğrulaması için demo uygulanmadı.|



### <a name="pci-dss-requirement-851"></a>PCI DSS gereksinim 8.5.1'e

**8.5.1'e** **yalnızca hizmet sağlayıcıları için ek gereksinimi:** hizmet sağlayıcıları ile müşterinin şirket içi için uzaktan erişim (örneğin, destek POS sistemleri veya sunucular) bir benzersiz kimlik doğrulama kimlik bilgisi (kullanması gerekir bir parola/deyimi gibi) her müşteri için. 

> [!NOTE]
> Bu gereksinim, kendi barındırma ortamı erişme paylaşılan barındırma hizmeti sağlayıcıları için uygulamak için birden çok müşteri ortamlarında barındırıldığı tasarlanmamıştır.

**Sorumlulukları:&nbsp;&nbsp;`Not Applicable`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure müşteriler için geçerli değildir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Microsoft Azure müşteriler için geçerli değildir.|



## <a name="pci-dss-requirement-86"></a>PCI DSS gereksinim 8.6

**8.6** diğer kimlik doğrulama mekanizmaları kullanıldığı durumlarda (örneğin, fiziksel veya mantıksal güvenlik belirteçleri, akıllı kartlar, sertifikalar, vb. için), bu mekanizmaların gibi atanması gerekir:
- Kimlik doğrulama mekanizmaları tek tek bir hesap için atanan ve birden çok hesaplar arasında paylaşılmayan gerekir.
- Fiziksel ve/veya mantıksal denetimleri yalnızca hedeflenen hesap erişim kazanmak için bu düzenek kullandığınızdan emin olun yerinde olması gerekir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı sırasında üç hesapları oluşturur: yönetici, sqladmin ve edna (tanıtım yürütme sırasında web uygulamasına oturum varsayılan kullanıcı). Çok faktörlü kimlik doğrulaması için demo uygulanmadı. Aracılığıyla yönetilen tüm erişim [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), şifreleme anahtarları ve gizli anahtarları bulut uygulamalar ve hizmetler tarafından kullanılan yardımcı olan koruyun. |



## <a name="pci-dss-requirement-87"></a>PCI DSS gereksinim 8.7

**8,7** (uygulamalar, Yöneticiler ve diğer tüm kullanıcılar tarafından erişim dahil olmak üzere) kart sahibi verileri içeren herhangi bir veritabanı tüm erişimi kısıtlanmış aşağıdaki gibidir:
- Tüm kullanıcı erişimi, kullanıcı sorguları ve kullanıcı eylemlerini veritabanlarında programlı yöntemleridir.
- Yalnızca Veritabanı yöneticileri doğrudan erişmek veya veritabanları sorgu seçeneğine sahipsiniz.
- Veritabanı uygulamaları için uygulama kimlikleri, yalnızca uygulamalar tarafından (ve tek tek kullanıcılara veya diğer uygulama dışı işlemler tarafından değil) kullanılabilir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore Azure anahtar kasası ile tüm kart sahibi verilerinizi korur ve kayıtların şifreleme dağıtım belgelerinde. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).<br /><br />|



## <a name="pci-dss-requirement-88"></a>PCI DSS gereksinim 8,8

**8,8** güvenlik ilkeleri ve tanımlama ve kimlik doğrulama için işlem yordamlarını, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Müşteriler, güvenlik ilkeleri ve tanımlama ve kimlik doğrulama için işlem yordamlarını, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen sağlamaktan sorumludur.|




