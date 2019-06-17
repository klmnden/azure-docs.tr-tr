---
title: Test Sürüşü barındırılan | Azure Market
description: Bir koruma bir Market barındırılan Test Sürüşü ayarlama
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: b8f9ca96ac9386037460ad5c1c9f56fe7b9c2e18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939992"
---
# <a name="hosted-test-drive"></a>Barındırılan Test Sürüşü

Bir barındırılan Test Sürüşü Microsoft barındırma tarafından kurulumun karmaşıklığını ortadan kaldırır ve Test Sürüşü kullanıcı sağlama ve sağlamayı kaldırma gerçekleştiren hizmetin bakımını. Bu makalede Appsource'ta teklifini olan veya yeni bir tane oluşturan ve ile bir Dynamics 365 müşteri katılımı, Finans ve işlemleri veya Dynamics 365 Business Central için Dynamics 365 için bağlayan bir barındırılan Test Sürüşü sunmak isteyen yayımcılar içindir. örneği.

## <a name="how-to-publish-a-test-drive"></a>Bir Test sürüşüne yayımlama

Var olan bir teklif için gidin veya yeni bir teklif oluşturun.

Test Sürüşü seçeneği yan menüden seçim yapın.

Seçin \'Evet\' için \'Test Sürüşü etkinleştirme\' seçeneği.

Aşağıdaki alanları sağlar \'ayrıntıları\' bölümü.

- **Açıklama**: Test Sürüşünüz genel bir bakış sağlar. Bu metin, Test Sürüşü sağlanırken kullanıcıya gösterilir. Biçimlendirilmiş içerik sağlamak istiyorsanız bu alan HTML destekler.
- **Kullanıcı el kitabına**: Test Sürüşü kullanıcı uygulamanızı kullanmayı öğrenmenize yardımcı olan ayrıntılı kullanıcı kılavuzuna (tür .pdf dosyası) karşıya yükleyin.
- **Test sürücü tanıtım videosu**: İsteğe bağlı olarak uygulamanızı gösteren bir video karşıya yükleyin.

Sağlama ve sağlamasını kaldırma bulunan yönergeleri kullanarak kiracınızdaki kullanıcılar Test Sürüşü için izin verme AppSource [burada](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md).

Bu adımda, oluşturacak \'Azure AD uygulama kimliği\' ve \'Azure AD uygulama anahtarı\' değerleri belirtilen aşağıda.

Aşağıdaki alanları sağlar \'teknik yapılandırma\' bölümü:

- **Test Sürüşü türünü**: Seçin \'Microsoft Hosted (örneğin Dynamics 365 müşteri katılımı)' seçeneği. Bu, Microsoft barındırma ve Test Sürüşü kullanıcı sağlama ve sağlamayı kaldırma gerçekleştiren hizmetin bakımını gösterir.
- **Maksimum eşzamanlı Test Sürüşleri**: Bu alan zaman belirli bir anda etkin olan bir Test Sürüşü olan eş zamanlı kullanıcı sayısıyla ayarlayın. Test Sürüşü kullanıcılar için kullanılabilir olan en az bu kadar çok Dynamics lisans olduğundan emin olmak ihtiyacınız olacak şekilde, Test Sürüşü etkin olduğu sürece her kullanıcı bir Dynamics lisansı kullanacaktır. Önerilen değeri 3-5.
- **Test sürücü süresi (saat)** : Bu alan, Test Sürüşü için etkin kullanıcılar saat sayısını ayarlayın. Bu kadar çok saat sonra kiracınızdan kullanıcı sağlaması. Uygulamanızı karmaşıklığına bağlı olarak 2-24 saat değerini önerilir. Süre bitti çalıştırıp Test Sürüşü yeniden erişmek istediğiniz kullanıcı her zaman başka bir Test Sürüşü isteyebilir.
- **Örnek URL**: Kullanıcıdan Test Sürüşü başlatırken Test Sürüşü kullanıcı için başlangıçta gidilecek bir URL sağlayın. Uygulamanızın vardır, Dynamics 365 örneğinizin URL'si genellikle budur ve örnek veriler üzerinde yüklü. Örnek değer: https:\//testdrive.crm.dynamics.com
- **Azure AD Kiracı kimliği**: Dynamics 365 örneğinizin Azure Kiracı kimliği sağlayın. Bu değer, Azure portalında oturum açın alıp gidin \'Azure Active Directory\'  - \> seçin menü dikey penceresi - özelliklerinden\> dizin kimliği kopyalayın Örnek değer: 72f988bf-86f1-41af-91ab-2d7cd0111234
- **Azure AD uygulama kimliği**: 7. adımda oluşturduğunuz Azure AD uygulama kimliği. \ örnek değeri: 53852862-a2ae-4e43-9461-faa49650a096
- **Azure AD uygulama anahtarı**: Oluşturulan Azure AD uygulaması için gizli 7. adımda. \ örnek değeri: IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=
- **Azure AD Kiracı adı**: Dynamics 365 Örneğiniz için Azure Kiracı adını sağlayın. Biçimini kullanın \<kiracıadı.\> onmicrosoft.com. Örnek değer: testdrive.onmicrosoft.com
- **Örnek Web API'si URL**: Web API URL'si için Dynamics 365 örneğinizi sağlayın. Bu değer, Microsoft Dynamics 365 örneğine günlüğe kaydetme ve gezinme alabilirsiniz ayarı -\> özelleştirme -\> Geliştirici Kaynakları -\> örnek Web API'si (kopya bu URL). Örnek değer: https:\//testdrive.crm.dynamics.com/api/data/v9.0 
- **Rol adı**: Özel Dynamics 365 güvenlik Test Sürüşü için oluşturduğunuz rolü adını sağlayın. Bu, Test Sürüşü sırasında kullanıcılara atanan rolüdür. Örnek değer: testdriverole

## <a name="next-steps"></a>Sonraki adımlar

Ne zaman hazır **yayımlama** teklifinizi, sertifika, uygulamanızı geçtikten sonra olması bir **Önizleme** teklifinizin. Test Sürüşü kullanıcı Arabiriminde başlatın ve Test Sürüşleri doğru şekilde çalıştığını doğrulayın. Şimdi Önizleme teklifinizle birlikte hissedene sonra durumuna gelir **Canlı izleyin!**
