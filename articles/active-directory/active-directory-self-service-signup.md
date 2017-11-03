---
title: "Azure için Self Servis kaydolma nedir? | Microsoft Belgeleri"
description: "Azure için bir genel bakış Self Servis kaydolma, kaydolma işlemini yönetme ve bir DNS etki alanı adı nasıl."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 511088156d3546e2e0f3ac40e72bf2b8e4ae2cb9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-self-service-signup-for-azure"></a>Azure için Self Servis kaydolma nedir?
Bu konuda, Self Servis kaydolma işlemini ve bir DNS etki alanı adı nasıl açıklanmaktadır.  

## <a name="why-use-self-service-signup"></a>Self Servis kaydolmanın neden kullanılır?
* Müşteriler için daha hızlı istedikleri Hizmetleri alın.
* E-posta tabanlı teklifleri için bir hizmet oluşturun.
* Hızlı bir şekilde kolay unutmayın iş e-posta benzersizse kullanarak kimlikleri oluşturmak kullanıcıların e-posta tabanlı kaydolma akışları oluşturun.
* Yönetilmeyen Azure dizinler yönetilen dizinlere daha sonra yapılabilir ve diğer hizmetler için yeniden kullanılabilir.

## <a name="terms-and-definitions"></a>Terimleri ve tanımları
* **Self Servis kaydolma**: Bu, bir kullanıcı için bir bulut hizmeti kaydolur ve bunlar için Azure Active Directory (Azure AD) otomatik olarak oluşturulan bir kimlik tabanlı sahip kendi e-posta etki yöntemdir.
* **Yönetilmeyen Azure directory**: Bu kimliğe oluşturulduğu dizindir. Yönetilmeyen bir dizin yok genel yönetici olan bir dizindir.
* **Kullanıcı e-posta doğrulandı**: Azure AD'de kullanıcı hesabı türü budur. Self Servis Teklif için kaydolan sonra otomatik olarak oluşturulan bir kimliğe sahip bir kullanıcı bir e-posta doğrulanmış kullanıcı olarak bilinir. Bir e-posta doğrulanan kullanıcı ile creationmethod etiketli bir dizin normal üyesidir EmailVerified =.

## <a name="user-experience"></a>Kullanıcı deneyimi
Örneğin, bir kullanıcı, e-posta düşünelim Dan@BellowsCollege.com e-posta yoluyla hassas dosyalar alır. Azure Rights Management (Azure RMS) tarafından korunan dosyaları. Ancak Can'ın kuruluş, Körüğü üniversitenin, Azure RMS için kaydolmuş değil veya Active Directory RMS dağıtıldığını. Bu durumda, Dan kişiler için RMS için ücretsiz bir abonelik için korumalı dosyaları okumak için kaydolabilirsiniz.

Dan BellowsCollege.com yönetilmeyen bir dizini BellowsCollege.com için Azure AD içinde oluşturulur sunumu, bu Self Servisi için kaydolmak için bir e-posta adresi ile ilk kullanıcı ise. Diğer kullanıcıların BellowsCollege.com etki alanından bu teklifi veya benzer bir Self Servis sunumu için kaydolursanız, bunlar Azure aynı yönetilmeyen dizininde oluşturulan e-posta doğrulanan kullanıcı hesapları da sahip olursunuz.

## <a name="admin-experience"></a>Yönetici deneyimi
Yönetilmeyen bir Azure directory DNS etki alanı adını sahip bir yönetici devralır veya dizin sahipliği kanıtlayan sonra birleştirin. Sonraki bölümlerde daha ayrıntılı yönetim deneyimi açıklanmaktadır, ancak bir özeti aşağıda verilmiştir:

* Yönetilmeyen bir Azure dizin üzerinde alırken, yalnızca yönetilmeyen dizin genel Yöneticisi haline gelir. Bazen, bir iç devralma da denir.
* Bu nedenle bir yönetilmeyen Azure dizini birleştirme, yönetilen Azure dizininize yönetilmeyen directory DNS etki alanı adını eklemek ve kullanıcıların kaynaklara eşlenmesini oluşturulur kullanıcıları kesintisiz hizmetlerine erişmek devam edebilir. Bazen, bir dış devralma da denir.

## <a name="what-gets-created-in-azure-active-directory"></a>Azure Active Directory'de oluşturulan?
#### <a name="directory"></a>Dizin
* Bir Azure Active directory etki alanı oluşturulduğunda, etki alanı başına bir dizin.
* Azure AD dizini yok genel yöneticisine sahip

#### <a name="users"></a>Kullanıcılar
* Kaydolan her kullanıcı için Azure AD dizininde bir kullanıcı nesnesi oluşturulur.
* Her kullanıcı nesnesi harici olarak işaretlenir.
* Her bir kullanıcı, bunlar kaydolup hizmetine erişim verilir.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Ne ı talep Self Servis Azure AD olduğum bir etki alanı için dizin?
Self Servis Azure AD talep etki alanı doğrulama gerçekleştirerek dizin. DNS kayıtlarını oluşturarak etki alanına sahip etki alanı doğrulama kanıtlar.

Azure AD dizini, DNS devralma gerçekleştirmenin iki yolu vardır:

* (yönetici yönetilmeyen Azure directory bulur ve yönetilen bir dizin kapatmak isteyen) iç devralma
* Dış devralma (kendi yönetilen Azure dizinine yeni bir etki alanı eklemek için yönetici çalışır)

Bir kullanıcı Self Servis kaydolma gerçekleştirilen veya varolan bir yönetilen dizin yeni bir etki alanı ekleme sonra yönetilmeyen bir dizin üzerinde aldığı bir etki alanı kendi doğrulanırken ilginizi çekebilir. Örneğin, contoso.com adlı bir etki alanına sahip ve contoso.co.uk veya contoso.uk adlı yeni bir etki alanı eklemek istediğiniz.

## <a name="what-is-domain-takeover"></a>Etki alanı devralma nedir?
Bu bölüm, bir etki alanına ait olduğunu doğrulamak nasıl kapsar.

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Etki alanı doğrulama nedir ve neden kullanılır?
Bir dizin üzerinde işlem gerçekleştirmek için Azure AD DNS etki alanı sahipliğini doğrulamanız gerekir.  Etki alanının doğrulama dizin ve ya da yükseltmek yönetilen bir dizin Self Servis dizinine talep izin verir veya varolan bir yönetilen dizin Self Servis dizin birleştirebilirsiniz.

## <a name="examples-of-domain-validation"></a>Etki alanı doğrulama örnekleri
Bir dizinin DNS devralma gerçekleştirmenin iki yolu vardır:

* İç devralma (örneğin, bir yönetici bir Self Servis, yönetilmeyen dizin bulur ve yönetilen bir dizin kapatmak isteyen)
* Dış devralma (örneğin, bir yönetici yönetilen bir dizine yeni bir etki alanı eklemek çalışır)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>İç devralma - yönetilen bir dizin olacak şekilde Self Servis, yönetilmeyen bir dizini Yükselt
İç devralma yaptığınızda, dizin yönetilen bir dizine yönetilmeyen bir dizinden dönüştürülen. Burada DNS bölgesinde MX kaydı veya bir TXT kaydını oluşturmak, DNS etki alanı adını doğrulamayı tamamlamak gerekir. Bu eylem:

* Etki alanına ait olduğunu doğrular
* Yönetilen dizin yapar
* Dizinin genel yönetici yapar

Okul kullanıcıların self servis teklifleri için sürümüne kaydolduğunuzdan Körüğü üniversitenin BT yöneticilerinden bulur varsayalım. DNS kayıtlı sahip adı gibi BellowsCollege.com, BT yöneticisi Azure DNS adının sahipliği doğrulamak ve yönetilmeyen dizin üzerinde gerçekleştirin. Dizin sonra yönetilen bir dizin haline gelir ve BT yöneticisi BellowsCollege.com dizin için genel Yönetici rolüne atanır.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Dış devralma - mevcut bir yönetilen dizin bir Self Servis dizin birleştirme
Bir dış devralma içinde yönetilen bir dizin zaten var ve tüm kullanıcılar ve gruplar yönetilmeyen bir dizininden Bu yönetilen dizine katılmak için istediğiniz yerine dizinleri kendi iki ayrı.

Yönetilen bir dizin Yöneticisi, olarak bir etki alanına eklemek ve bu etki alanı kendisiyle ilişkili bir yönetilmeyen dizinine sahip olur.

Örneğin, bir BT yöneticisi olduğunuz ve Contoso.com, kuruluşunuz için kayıtlı bir etki alanı adı için zaten bir yönetilen dizinine sahip varsayalım. Kuruluşunuzdaki kullanıcıların Self Servis oturum oluşturan bir sunum için e-posta etki alanı adını kullanarak gerçekleştirdiğiniz Bul user@contoso.co.uk, kuruluşunuzun sahip olduğu başka bir etki alanı adı değil. Bu kullanıcıların hesaplarını contoso.co.uk için yönetilmeyen bir dizindeki şu anda yok.

Mevcut yönetilen BT dizininize contoso.com contoso.co.uk yönetilmeyen dizinini birleştirmek için iki ayrı dizini, yönetmek istediğiniz yok.

Dış devralma iç devralma aynı DNS doğrulama süreci izler.  Fark edilmesini: kullanıcıların ve hizmetlerin yönetilen BT dizinine açmayla.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Bir dış devralma gerçekleştirmenin etkisini nedir?
Kullanıcıların hizmetlerine kesintisiz erişmeye devam edebilmek için bir dış devralma ile kullanıcıların kaynaklara eşlenmesini oluşturulur. Kişiler için RMS dahil olmak üzere birçok uygulama, kullanıcıların kaynaklara eşlenmesini iyi işler ve kullanıcıların bu hizmetlerin değişiklik olmadan erişmeye devam edebilirsiniz. Bir uygulama kullanıcıların kaynaklara eşlenmesini etkili bir şekilde işlemez, dış devralma açıkça zayıf bir deneyim kullanıcıların engellemek için engellenmiş olabilir.

#### <a name="directory-takeover-support-by-service"></a>Dizin hizmeti tarafından devralma desteği
Şu anda aşağıdaki hizmetleri devralma destekler:

* RMS

Aşağıdaki hizmetler yakında devralma destekleyecek:

* PowerBI

Aşağıdaki olmayan ve bir dış devralma sonra kullanıcı verilerini geçirmek için ek yönetim eylemi gerektirir.

* SharePoint/OneDrive

## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Bir DNS etki alanı adı devralma gerçekleştirme
Bir etki alanı doğrulama yapmak (ve isterseniz bir devralma yapmak) nasıl için birkaç seçeneğiniz vardır:

1. Azure Yönetim Portalı

   Bir etki alanı ekleme yaparak bir devralma tetiklenir.  Etki alanı için bir dizin zaten varsa, bir dış devralma gerçekleştirmeye yönelik seçeneği gerekir.

   Kimlik bilgilerinizi kullanarak Azure portalında oturum açın.  Varolan dizininize gidin ve ardından **etki alanı Ekle**.
2. Office 365

   Seçenekleri kullanabileceğiniz [etki alanlarını yönetme](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) etki alanları ve DNS kayıtları ile çalışmak için Office 365'te sayfası. Bkz: [Office 365'te etki alanınızı doğrulayın](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows PowerShell

   Windows PowerShell kullanarak bir doğrulamasını gerçekleştirmek için aşağıdaki adımları gerekir.

   | Adım | Kullanmak için Cmdlet |
   | --- | --- |
   | Bir kimlik bilgisi nesnesi oluşturun |Get-Credential |
   | Azure AD'ye Bağlanma |Connect-MsolService |
   | etki alanlarının bir listesini alma |Get-MsolDomain |
   | Bir challenge oluşturma |Get-MsolDomainVerificationDns |
   | DNS kaydı oluşturma |DNS sunucunuzda bunu |
   | Sınama doğrulayın |Confirm-MsolEmailVerifiedDomain |

Örneğin:

1. Self Servis teklifine yanıt vermek için kullanılan kimlik bilgilerini kullanarak Azure ad connect:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Etki alanlarının bir listesini alın:

    Get-MsolDomain
3. Ardından bir sınama oluşturmak için Get-MsolDomainVerificationDns cmdlet'i çalıştırın:

    Get-MsolDomainVerificationDns – DomainName *your_domain_name* – modu DnsTxtRecord

    Örneğin:

    Get-MsolDomainVerificationDns – DomainName contoso.com – modu DnsTxtRecord
4. Bu komuttan döndürülen değeri (sınama) kopyalayın.

    Örneğin:

    MS 32DD01B82C05D27151EA9AE93C5890787F0E65D9 =
5. Genel DNS ad alanınız içinde önceki adımda kopyaladığınız değeri içeren bir DNS txt kaydı oluşturun.

    Bu kayıt için ad üst etki alanı adı, Windows Server DNS rolünden kullanarak bu kaynak kaydı oluşturun, böylece kayıt adı boş ve hemen Yapıştır değeri metin kutusuna bırakın
6. Sınama doğrulamak için Onayla MsolDomain cmdlet'ini çalıştırın:

    Onayla MsolEmailVerifiedDomain - DomainName *your_domain_name*

    Örneğin:

    Onayla MsolEmailVerifiedDomain - DomainName contoso.com

Başarılı bir challenge, bir hata olmadan istemi döndürür.

## <a name="how-do-i-control-self-service-settings"></a>Self Servis ayarları nasıl denetlerim?
Yöneticilere iki Self Servis denetimleri bugün sahip. Bunlar denetleyebilirsiniz:

* Olsun veya olmasın kullanıcılar e-posta yoluyla dizin birleştirebilirsiniz.
* Desteklemediğini kullanıcıların kendilerini uygulamalar ve hizmetler için lisans verebilirsiniz.

### <a name="how-can-i-control-these-capabilities"></a>Bu özelliklerin nasıl kontrol edebilir mi?
Bir yönetici bu Azure AD cmdlet Set-MsolCompanySettings parametreleri kullanarak şu olanakları yapılandırabilirsiniz:

* **AllowEmailVerifiedUsers** bir kullanıcı oluşturabilir veya yönetilmeyen bir directory katılım olup olmadığını denetler. Bu parametreyi $false olarak ayarlarsanız, e-posta doğrulanan kullanıcı dizini birleştirebilirsiniz.
* **AllowAdHocSubscriptions** yukarı Self Servis oturum gerçekleştirmek kullanıcılara denetler. Bu parametreyi $false olarak ayarlarsanız, hiçbir kullanıcı Self Servis kaydolma gerçekleştirebilirsiniz.

### <a name="how-do-the-controls-work-together"></a>Denetimleri birlikte nasıl çalışır?
Bu iki parametre birlikte yukarı Self Servis oturum üzerinde daha kesin denetim tanımlamak için kullanılabilir. Örneğin, aşağıdaki komutu bu kullanıcıların Azure AD'de bir hesabınız zaten varsa Self Servis oturum, ancak kullanıcıların gerçekleştirmesine olanak sağlayacak (diğer bir deyişle, bir e-posta doğrulandı hesabı oluşturulması için gereken kullanıcıları Self Servis oturum yukarı gerçekleştiremezsiniz):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Aşağıdaki akış çizelgesi, Yukarı farklı birleşimler Bu parametreler için ve dizin ve Self Servis oturum için sonuçta elde edilen koşullar açıklanmaktadır.

![][1]

Daha fazla bilgi ve bu parametrelerini kullanma örnekleri için bkz: [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
