---
title: Azure ödeme işleme şeması - İlkesi gereksinimleri
description: PCI DSS gereksinim 12
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: a79d59d8-20e3-4efe-8686-c8f4ed80e220
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 2fb238e9b95180d6156159c87ec008a71943e698
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="policy-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için ilke gereksinimlerini  
## <a name="pci-dss-requirement-12"></a>PCI DSS gereksinim 12

**Bilgi güvenliği için tüm personel adresleri bir ilke koru**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Güçlü bir güvenlik ilkesi güvenlik sesi tüm varlık için ayarlar ve bunları beklenenle personel bildirir. Tüm personel veri ve onu korumak için sorumluluklarını duyarlılığını haberdar olmanız gerekir. Gereksinim 12 amaçları doğrultusunda, tam zamanlı ve zamanlı çalışanlar, geçici çalışanlar, Yükleniciler ve varlığın sitesinde "yerleşik" veya aksi halde kart sahibi veri ortamı erişimi danışmanlar "personel" ifade eder.

## <a name="pci-dss-requirement-121"></a>PCI DSS gereksinim 12,1

**12,1** kurma, yayımlama, korumak ve güvenlik ilkesini Dağıt.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler, oluşturma ve bir bilgi güvenlik ilkesini sürdürme sorumludur.|



### <a name="pci-dss-requirement-1211"></a>PCI DSS gereksinim 12.1.1

**12.1.1** güvenlik ilkesi en az yıllık gözden geçirin ve ortam değiştiğinde ilkesini güncelleştirin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler, bilgi güvenlik ilkelerini en az yıllık güncelleştirme veya kart sahibi veri ortamlarına (CDE) değişiklik olduğunda sorumludur.|



## <a name="pci-dss-requirement-122"></a>PCI DSS gereksinim 12.2

**12.2** uygulayan bir risk değerlendirmesi işlem:
- En az yıllık ve önemli değişiklikler (örneğin, edinme, birleşme, konumu değiştirme, vb.) ortamına bağlı gerçekleştirilir
- Kritik varlıkları, Tehditler ve güvenlik açıkları tanımlar
- Bir resmi, belgelenmiş analizi sonuçlarında risk.
- > Risk değerlendirme yöntemlerini örnekleri içerir, ancak dizisi, ISO 27005 ve NIST SP 800-30 için sınırlı değildir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler, gereksinim 12.2 içinde listelenen tüm tehditler adresleri bir risk değerlendirmesi işlemi uygulamak için sorumludur.|



## <a name="pci-dss-requirement-123"></a>PCI DSS gereksinim 12.3

**12.3** kritik teknolojileri için kullanım ilkeleri geliştirin ve bu teknolojiler doğru kullanımı tanımlayın.

> [!NOTE]
> Kritik teknolojileri örnekleri içerir, ancak uzaktan erişim ve kablosuz teknolojileri, dizüstü bilgisayarlar, tabletler, elektronik çıkarılabilir medya, e-posta kullanım ve Internet kullanımı için sınırlı değildir.
Bu kullanım ilkeleri aşağıdakileri gerektirir emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1231"></a>PCI DSS gereksinim 12.3.1

**12.3.1** yetkili tarafların açık onayı

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1232"></a>PCI DSS gereksinim 12.3.2

**12.3.2** teknolojisinin kimlik doğrulaması kullanmak için

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1233"></a>PCI DSS gereksinim 12.3.3

**12.3.3** gibi tüm cihazlar ve erişime sahip personel listesi

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1234"></a>PCI DSS gereksinim 12.3.4

**12.3.4** doğru şekilde ve kolayca sahibi, kişi bilgileri ve amacı (örneğin, etiketleme, kodlama ve/veya aygıtların envantere) belirlemek için bir yöntem

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1235"></a>PCI DSS gereksinim 12.3.5

**12.3.5** kabul edilebilir teknolojisini kullanır

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1236"></a>PCI DSS gereksinim 12.3.6

**12.3.6** teknolojileri için kabul edilebilir ağ konumları

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler, bulut tabanlı VM'ler, depolama ve destekleyici hizmetler için kabul edilebilir ağ konumlarını belirlemek için sorumludur.|



### <a name="pci-dss-requirement-1237"></a>PCI DSS gereksinim 12.3.7

**12.3.7** şirket onaylı ürünler listesi

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler, bulut tabanlı VM'ler, depolama ve destekleyici hizmetler için kabul edilebilir ağ konumlarını belirlemek için sorumludur.|



### <a name="pci-dss-requirement-1238"></a>PCI DSS gereksinim 12.3.8

**12.3.8** oturumları uzaktan erişim teknolojileri için belirli bir süre işlem yapılmadığında otomatik Kes

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure oturumu kilit aşımı ayarlarına bir süre işlem yapılmadığında zorlar Microsoft Kurumsal AD oturum kilidi işlevi kullanır. Ağ bağlantıları 30 dakika işlem yapılmadığında sonlandırılır. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1239"></a>PCI DSS gereksinim 12.3.9

**12.3.9** satıcılar ve iş ortakları ve satıcılar ve iş ortaklarıyla kullandıktan sonra hemen devre dışı bırakma tarafından yalnızca gerekli olduğunda uzaktan erişim teknoloji etkinleştirme

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-12310"></a>PCI DSS gereksinim 12.3.10

**12.3.10** uzaktan erişim teknolojileri aracılığıyla kart sahibi verilere personel engellemek için kopyalama, taşıma ve yerel sabit sürücüler ve elektronik çıkarılabilir üzerine kart sahibi verilerinin depolanması açıkça tanımlanmış bir yetki sürece İş gereksinimi.
Bir yetkili iş gereksinimlerini olduğunda, veriler, tüm geçerli PCI DSS gereksinimlerine uyacak şekilde korunmalıdır kullanım ilkeleri gerektirir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler, uzaktan erişim teknolojileri aracılığıyla kart sahibi verilere personel kopyalama, taşıma ve kart sahibi veri yerel sabit sürücüler ve elektronik çıkarılabilir depolama açıkça yetki sürece yasaklandığı sağlamaktan sorumludur tanımlı iş gereksinimini.|



## <a name="pci-dss-requirement-124"></a>PCI DSS gereksinim 12.4 ancak

**12.4 ancak** güvenlik ilkesi ve yordamları açıkça tüm personel için bilgi güvenliği sorumluluklarını tanımlayan emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1241"></a>PCI DSS gereksinim 12.4.1

**12.4.1** yalnızca hizmet sağlayıcıları için ek gereksinimi: Executive yönetim kart sahibi veri ve dahil etmek için bir PCI DSS uyumluluk program koruma sorumluluğunu kurmak:
- PCI DSS uyumluluğu korumak için genel sorumluluk
- Üst yönetim PCI DSS uyumluluk program ve iletişim için bir kurucu tanımlama 

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Hizmet sağlayıcıları kullanmaya başlayan müşteriler kendi PCI uyumluluk program belgelemesinden sorumlu.|



## <a name="pci-dss-requirement-125"></a>PCI DSS gereksinim 12,5

**12,5** bir kişiye atayın veya aşağıdaki bilgi güvenliği Yönetimi sorumlulukları ekip.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri tanımlama ve bilgi çalışanları güvenlik Sorumluluklar atamak için sorumludur.|



### <a name="pci-dss-requirement-1251"></a>PCI DSS gereksinim 12.5.1

**12.5.1** kurma, belge ve güvenlik ilkeleri ve yordamları dağıtın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri tanımlama ve bilgi çalışanları güvenlik Sorumluluklar atamak için sorumludur.|



### <a name="pci-dss-requirement-1252"></a>PCI DSS gereksinim 12.5.2

**12.5.2** İzleyici ve güvenlik uyarıları ve bilgileri analiz ve uygun personele yeniden dağıtın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri tanımlama ve bilgi çalışanları güvenlik Sorumluluklar atamak için sorumludur.|



### <a name="pci-dss-requirement-1253"></a>PCI DSS gereksinim 12.5.3

**12.5.3** kurma, belge ve tüm durumlarda zamanında ve etkili işlenmesini sağlamak için güvenlik olay yanıtlama ve yükseltme yordamları dağıtabilirsiniz.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1254"></a>PCI DSS gereksinim 12.5.4

**12.5.4** ekleme, silme ve değiştirme işlemleri dahil olmak üzere, kullanıcı hesaplarını yönetme.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1255"></a>PCI DSS gereksinim 12.5.5

**12.5.5** izleme ve denetim tüm verilere erişebilirsiniz.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve uygun kullanım, uygulama ve bunların CDE içinde kritik teknolojileri için kimlik doğrulama dikte ilkelerinin korunması müşterileri sorumludur.|



## <a name="pci-dss-requirement-126"></a>PCI DSS gereksinim 12,6

**12,6** tüm personel kart sahibi veri güvenlik ilkelerini ve yordamlarını önemini haberdar olmak için bir resmi güvenlik tanıma programı uygulayın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Oluşturma ve güvenlik tanıma CDE erişimi olan bir ekibin çevreleyen ilkelerinin korunması müşterileri sorumludur.|



### <a name="pci-dss-requirement-1261"></a>PCI DSS gereksinim 12.6.1

**12.6.1** personel işe alma bağlıdır ve en az yıllık bilgilendirin. 

> [!NOTE]
> Yöntemleri personelin rolü ve kart sahibi verilere erişim düzeyini bağlı olarak değişebilir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler personel sağlayan almak ve bilgi güvenliği ve en az yıllık eğitim PCI-DSS tanıma kabul.|



### <a name="pci-dss-requirement-1262"></a>PCI DSS gereksinim 12.6.2

**12.6.2** en az yıllık bunlar okudum ve anladım güvenlik ilkesi ve yordamları onaylamak personeli gerektirir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler personel sağlayan almak ve bilgi güvenliği ve en az yıllık eğitim PCI-DSS tanıma kabul.|



## <a name="pci-dss-requirement-127"></a>PCI DSS gereksinim 12.7

**12.7** iç kaynaklardan saldırısı riskini en aza indirmek için işe ekran olası personel öncesi. (Arka plan kontrollerinden örnekler önceki iş geçmişi, cezai kaydı, kredi geçmişi ve başvuru denetler.) 

> [!NOTE]
> Bu olası personel belirli işe yalnızca bir kart sayı aynı anda erişimi olan deposu Kasiyerlerin gibi konumlandırır bir işlem kolaylaştırmanın olduğunda, bu gereksinim yalnızca önerilir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler Personel erişim sağlayan CDE için kapsamlı bir arka plan kontrollerinden geçmelerini.|



## <a name="pci-dss-requirement-128"></a>PCI DSS gereksinim 12,8

**12,8** Koru ve uygulama ilkeleri ve yordamlarıyla kimle paylaşıldığı kart sahibi verileri paylaşılır veya etkisi kart sahibi verilerin güvenliğini gibi hizmet sağlayıcılarını yönetme.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri kimle paylaşıldığı kart sahibi veri paylaşılan veya CDE güvenliğini etkileyebilecek hizmet sağlayıcıları için PCI uyumluluğunu izleme için sorumludur. Müşteriler gerekir korumak tüm hizmet listesini, kullanılan içinde kendi CDE sağlar.|



### <a name="pci-dss-requirement-1281"></a>PCI DSS gereksinim 12.8.1

**12.8.1** sağlanan hizmet açıklaması dahil olmak üzere hizmet sağlayıcılarının bir listesini korumak.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri kimle paylaşıldığı kart sahibi veri paylaşılan veya CDE güvenliğini etkileyebilecek hizmet sağlayıcıları için PCI uyumluluğunu izleme için sorumludur. Müşteriler gerekir korumak tüm hizmet listesini, kullanılan içinde kendi CDE sağlar.|



### <a name="pci-dss-requirement-1282"></a>PCI DSS gereksinim 12.8.2

**12.8.2** hizmet sağlayıcıları hizmet sağlayıcılarının sahip veya aksi halde depolamak, işlem veya müşteri adına iletme kart sahibi verilerin güvenliğini sorumlu olan bir bildirim içeren yazılı anlaşma korumak veya Müşteri'nin kart sahibi veri ortamı güvenlik etkileyebilir toplasa bile. 

> [!NOTE]
> Bir bildirim tam ifadesi, iki taraf, sağlanan hizmet ayrıntılarını ve taraflara atanan sorumlulukları arasında anlaşmasında bağlıdır. Bu gereksinim sağlanan tam ifadesi dahil etmek onay yok.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Kart sahibi verilerin güvenliğini korumak için sorumluluk aktarımının hizmet sağlayıcıları ile yazılmış anlaşmaları bakımından sorumlu müşterilerdir.|



### <a name="pci-dss-requirement-1283"></a>PCI DSS gereksinim 12.8.3

**12.8.3** engagement'a tespitlerini önceki uygun dahil olmak üzere hizmet sağlayıcıları çekici için kurulmuş bir işlem olduğundan emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler için katılım tespitlerini önceki uygun dahil olmak üzere hizmet sağlayıcıları çekici bir kurulan işlem sağlamaktan sorumludur.|



### <a name="pci-dss-requirement-1284"></a>PCI DSS gereksinim 12.8.4

**12.8.4** en az yıllık hizmet sağlayıcıları PCI DSS uyumluluk durumunu izlemek için bir program korumak.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler en az yıllık hizmet sağlayıcıları PCI DSS uyumluluk durumunu izlemek için bir program bakımından sorumlu.|



### <a name="pci-dss-requirement-1285"></a>PCI DSS gereksinim 12.8.5

**12.8.5** hangi PCI DSS gereksinimler her bir hizmet sağlayıcısı tarafından yönetilir ve varlık tarafından yönetildiği hakkında bilgilerini korur.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterilerin bir kopyasını koruyarak için sorumlu [sorumluluk özeti matris](https://aka.ms/pciblueprintcrm32), müşteri ve Microsoft Azure sorumluluğunda olan olanlar sorumluluğundadır PCI DSS gereksinimleri özetlenmektedir.|



## <a name="pci-dss-requirement-129"></a>PCI DSS gereksinim 12.9

**12.9** yalnızca hizmet sağlayıcıları için ek gereksinimi: hizmet sağlayıcıları kabul hizmet sağlayıcının sahip veya aksi halde depolar, işlemler, kart sahibi veri güvenliği için sorumlu olan müşteriler için yazma veya müşterinin veya azami ölçüde adınıza Müşteri'nin kart sahibi veri ortamı güvenlik etkileyebilir iletir. 

> [!NOTE]
> Bir bildirim tam ifadesi, iki taraf, sağlanan hizmet ayrıntılarını ve taraflara atanan sorumlulukları arasında anlaşmasında bağlıdır. Bu gereksinim sağlanan tam ifadesi dahil etmek onay yok.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Hizmet sağlayıcıları olan müşteriler için PCI uyumluluğunu korumak için sorumluluklarını aktarımının sorumludur. |



## <a name="pci-dss-requirement-1210"></a>PCI DSS gereksinim 12.10

**12.10** bir olay yanıtı planınızı uygulamak. Hemen bir sistem ihlalde verilecek tepki göstermeye hazırlıklı olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri IR planı geliştirmek için sorumludur ve paylaşılan dokunma noktaları ve Azure'nın altyapı yararlanarak herhangi bir müşteri uygulama işlemeyle ilgili herhangi bir müşteri denetim sınama göz önünde bulundurur. Bu, kendi uygulama ya da veri etkileyebilir kendilerine bildirilecek bir olay gerekiyor durumunda Azure'a doğru kişi bilgilerini sağlamak için Müşteri'nin sorumluluğundadır.|



### <a name="pci-dss-requirement-12101"></a>PCI DSS gereksinim 12.10.1

**12.10.1** sistem ihlali durumunda uygulanacak olay yanıt planı oluşturun. Plan adresleri en az için aşağıdakilerden emin olun:
- Rolleri, sorumlulukları ve en az ödeme markalar bildirimi de dahil olmak üzere bir tehlikeye durumunda iletişimi ve iletişim stratejileri
- Belirli olay yanıtlama yordamları
- İş kurtarma ve devamlılığı yordamları
- Veri yedekleme işlemleri
- Güvenlik ihlalleri raporlama için yasal gereksinimler analizi
- Kapsamı ve tüm önemli sistem bileşenlerinin yanıtları
- Başvuru veya ödeme markalar olay yanıtlama yordamlardan birini ekleme

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri IR planı geliştirmek için sorumludur ve paylaşılan dokunma noktaları ve Azure'nın altyapı yararlanarak herhangi bir müşteri uygulama işlemeyle ilgili herhangi bir müşteri denetim sınama göz önünde bulundurur. Bu, kendi uygulama ya da veri etkileyebilir kendilerine bildirilecek bir olay gerekiyor durumunda Azure'a doğru kişi bilgilerini sağlamak için Müşteri'nin sorumluluğundadır.|



### <a name="pci-dss-requirement-12102"></a>PCI DSS gereksinim 12.10.2

**12.10.2** gözden geçirin ve test planına, tüm öğeler de dahil olmak üzere listelenen gereksinim 12.10.1, en az yıllık.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri IR planı geliştirmek için sorumludur ve paylaşılan dokunma noktaları ve Azure'nın altyapı yararlanarak herhangi bir müşteri uygulama işlemeyle ilgili herhangi bir müşteri denetim sınama göz önünde bulundurur. Bu, kendi uygulama ya da veri etkileyebilir kendilerine bildirilecek bir olay gerekiyor durumunda Azure'a doğru kişi bilgilerini sağlamak için Müşteri'nin sorumluluğundadır.|



### <a name="pci-dss-requirement-12103"></a>PCI DSS gereksinim 12.10.3

**12.10.3** uyarılara yanıt 7/24 temelinde kullanılabilir olması için belirli personel belirleyin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri IR planı geliştirmek için sorumludur ve paylaşılan dokunma noktaları ve Azure'nın altyapı yararlanarak herhangi bir müşteri uygulama işlemeyle ilgili herhangi bir müşteri denetim sınama göz önünde bulundurur. Bu, kendi uygulama ya da veri etkileyebilir kendilerine bildirilecek bir olay gerekiyor durumunda Azure'a doğru kişi bilgilerini sağlamak için Müşteri'nin sorumluluğundadır.|



### <a name="pci-dss-requirement-12104"></a>PCI DSS gereksinim 12.10.4

**12.10.4** uygun personeli için güvenlik ihlali ile yanıt sorumlulukları eğitim.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri IR planı geliştirmek için sorumludur ve paylaşılan dokunma noktaları ve Azure'nın altyapı yararlanarak herhangi bir müşteri uygulama işlemeyle ilgili herhangi bir müşteri denetim sınama göz önünde bulundurur. Bu, kendi uygulama ya da veri etkileyebilir kendilerine bildirilecek bir olay gerekiyor durumunda Azure'a doğru kişi bilgilerini sağlamak için Müşteri'nin sorumluluğundadır.|



### <a name="pci-dss-requirement-12105"></a>PCI DSS gereksinim 12.10.5

**12.10.5** dahil ancak bunlarla sınırlı olmamak izinsiz giriş algılama, izinsiz giriş önleme, güvenlik duvarları, sistemleri ve dosya bütünlüğünü izleme sistemleri izleme güvenlik uyarıları içerir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri IR planı geliştirmek için sorumludur ve paylaşılan dokunma noktaları ve Azure'nın altyapı yararlanarak herhangi bir müşteri uygulama işlemeyle ilgili herhangi bir müşteri denetim sınama göz önünde bulundurur. Bu, kendi uygulama ya da veri etkileyebilir kendilerine bildirilecek bir olay gerekiyor durumunda Azure'a doğru kişi bilgilerini sağlamak için Müşteri'nin sorumluluğundadır.|



### <a name="pci-dss-requirement-12106"></a>PCI DSS gereksinim 12.10.6

**12.10.6** derslerle göre olay yanıtlama planı geliştirin ve değiştirmek için ve endüstri gelişmeler birleştirmek için bir işlem geliştirin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri IR planı geliştirmek için sorumludur ve paylaşılan dokunma noktaları ve Azure'nın altyapı yararlanarak herhangi bir müşteri uygulama işlemeyle ilgili herhangi bir müşteri denetim sınama göz önünde bulundurur. Bu, kendi uygulama ya da veri etkileyebilir kendilerine bildirilecek bir olay gerekiyor durumunda Azure'a doğru kişi bilgilerini sağlamak için Müşteri'nin sorumluluğundadır.|



## <a name="pci-dss-requirement-1211"></a>PCI DSS gereksinim 12.11

**12.11** **yalnızca hizmet sağlayıcıları için ek gereksinimi:** incelemeler personel güvenlik ilkeleri ve işletimsel yordamları izleyerek onaylamak için en az üç aylık gerçekleştirin.
Gözden geçirmeler aşağıdaki işlemler kapsamalıdır:
- Günlük günlük incelemeleri
- Güvenlik duvarı kural kümesi incelemeleri
- Yapılandırma standartlar yeni sisteme uygulanıyor
- Güvenlik uyarılara yanıt verme
- Değişiklik yönetimi işlemleri 

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Hizmet sağlayıcıları kullanmaya başlayan müşteriler kendi incelemeler PCI uyumluluk denetimi performansını onayladığınız için işlemlerin belgelemesinden sorumlu.|



### <a name="pci-dss-requirement-12111"></a>PCI DSS gereksinim 12.11.1

**12.11.1** yalnızca hizmet sağlayıcıları için ek gereksinimi: eklemek için üç aylık gözden geçirme işleminin belgeleri korumak:
- Gözden geçirmeler sonuçlarını belgeleme
- PCI DSS uyumluluk program sorumluluğunu gözden geçirme ve oturum kapatma sonuçları personeli tarafından atanan 

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Hizmet sağlayıcıları kullanmaya başlayan müşteriler kendi incelemeler PCI uyumluluk denetimi performansını onayladığınız için işlemlerin belgelemesinden sorumlu.|




