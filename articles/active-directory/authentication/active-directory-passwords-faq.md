---
title: Azure Active Directory SSS - Self Servis parola sıfırlama
description: Sık sorulan sorular hakkında Azure AD Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77154ef35242c55724becb77595dbd5ecf8a4da9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60359067"
---
# <a name="password-management-frequently-asked-questions"></a>Parola yönetimi hakkında sık sorulan sorular

Bazı sık sorulan sorular (SSS) parola sıfırlama için ilgili her şey için aşağıda verilmiştir.

Azure Active Directory (Azure AD) hakkında genel bir sorum varsa ve burada cevaplanmamış, Self Servis parola sıfırlama (SSPR), sorabileceğiniz topluluktan Yardım almak için üzerinde [Azure AD Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Mühendisler, ürün yöneticileri, MVP'ler ve sizin gibi BT uzmanları, topluluk üyeleri içerir.

Bu SSS, aşağıdaki bölümlere ayrılmıştır:

* [Parola hakkında sorular sıfırlama kaydı](#password-reset-registration)
* [Parola sıfırlama ile ilgili sorular](#password-reset)
* [Parola değiştirme hakkında sorular](#password-change)
* [Parola yönetimi raporları hakkında sorular](#password-management-reports)
* [Parola geri yazma hakkında sorular](#password-writeback)

## <a name="password-reset-registration"></a>Parola sıfırlama kaydı

* **S:  Kullanıcılarımın parola sıfırlama verileri kendi kaydedebilir miyim?**

  > **C:** Evet. Parola sıfırlama etkinleştirildi ve lisansına sahip olduğunuz sürece, kullanıcılar parola sıfırlama kayıt portalı gidebilirsiniz (https://aka.ms/ssprsetup) kimlik doğrulama bilgilerini kaydetmek için. Kullanıcılar ayrıca erişim panelinden kaydetme (https://myapps.microsoft.com). Erişim panelinden kaydedilecek için ihtiyaç duydukları kendi profil resminizi seçin, **profili**ve ardından **parola sıfırlama için kaydolmasını** seçeneği.
  >
  >
* **S:  Parola'etkin değilse sıfırlamak için bir grup ve herkes için benim kullanıcılar gerekli yeniden kaydetmeniz etkinleştirmeye karar?**

  > **C:** Hayır. Kimlik doğrulama verilerini doldurulmuş kullanıcılar yeniden kaydolmak için gerekli değildir.
  >
  >
* **S:  Kullanıcılarım adına parola sıfırlama verileri tanımlamak?**

  > **C:** Evet, PowerShell, Azure AD Connect ile bunu yapabilirsiniz [Azure portalında](https://portal.azure.com), veya [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com). Daha fazla bilgi için [veri kullanılan Azure AD Self Servis parola sıfırlama](howto-sspr-authenticationdata.md).
  >
  >
* **S:  Verileri şirket içi güvenlik sorusunun eşitleyebilirim?**

  > **C:** Hayır, bu bugün mümkün değildir.
  >
  >
* **S:  Kullanıcılar diğer kullanıcılar bu verileri göremez bir şekilde veri kaydedebilir miyim?**

  > **C:** Evet. Kullanıcılar kaydettiğinizde parola kullanarak verileri sıfırlama kayıt portalı, verileri yalnızca genel Yöneticiler ve kullanıcı için görünür olan özel kimlik doğrulama alanları kaydedilir.
  >
  >
* **S:  Kullanıcılarımın parola sıfırlama kullanabilmeniz için önce kaydedilmesi gerekir mi?**

  > **C:** Hayır. Kendi adınıza yeterli kimlik doğrulama bilgilerini tanımlarsanız, kullanıcıların kaydetmek zorunda kalmaması. Uygun alanları dizinde depolanan verileri doğru şekilde biçimlendirildiğini sürece parola sıfırlama işleminin çalıştığı.
  >
  >
* **S:  Eşitleme veya miyim kullanıcılar adına kimlik doğrulama telefonu, kimlik doğrulama e-posta veya alternatif kimlik doğrulama telefon alanlarını ayarlayın?**

  > **C:** Bir genel yönetici tarafından ayarlanabilir alanlar makalesinde tanımlanan [SSPR veri gereksinimleri](howto-sspr-authenticationdata.md).
  >
  >
* **S:  Kayıt portalı kullanıcılarımın göstermek için hangi seçeneklerin nasıl belirliyor?**

  > **C:** Parola sıfırlama kayıt portalı, kullanıcılarınız için etkinleştirdiğiniz yalnızca yönelik seçenekleri gösterir. Bu seçenekler altında bulunan **kullanıcı parolası sıfırlama İlkesi** dizininizin bölüm **yapılandırma** sekmesi. Güvenlik sorularını etkinleştirmezseniz, örneğin, ardından kullanıcılar için bu seçenek kaydedilecek yükümlü değiliz.
  >
  >
* **S:  Ne zaman bir kullanıcı olarak kabul edilir kayıtlı?**

  > **C:** Bir kullanıcı isteneceği zaman SSPR için kayıtlı olarak kabul edilir en az **sıfırlamak için gereken yöntemlerin sayısı** , ayarladığınız parolayı [Azure portalında](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Parola sıfırlama

* **S:  Kullanıcılar birden çok kısa bir süre içinde bir parola sıfırlama girişimlerine karşı engelliyor mu?**

  > **C:** Evet, kötüye korumak için parola sıfırlamayı yerleşik güvenlik özellikleri de vardır. 
  >
  > Kullanıcılar, bunlar için 24 kilitli önce bir 24 saatlik süre içinde yalnızca beş parola sıfırlama girişimlerine deneyebilirsiniz. 
  >
  > Kullanıcılar, bir telefon numarası doğrulama, bir SMS göndermek veya güvenlik sorularını ve yanıtlarını önce bunlar için 24 kilitli bir saat içinde yalnızca beş kez doğrulamak deneyebilirsiniz. 
  >
  > Kullanıcılar, en fazla 10 kez bunlar 24 saat için kilitli önce 10 dakikalık bir süre içinde bir e-posta gönderebilir.
  >
  > Sayaç, bir kullanıcının parolasını sıfırlar sonra sıfırlanır.
  >
  >
* **S:  Bir e-posta, SMS veya telefon görüşmesi parola sıfırlaması almak için ne kadar beklemeli midir?**

  > **C:** İçindeki e-posta, SMS mesajları ve telefon aramaları bir dakikadan ulaşacak. 5-20 saniye normal bir durumdur.
  > Bu zaman çerçevesinde bildirimini almazsanız:
  > * Klasörünüzü kontrol edin.
  > * Numarası veya e-posta iletişim kurulan bir beklediğiniz olup olmadığını denetleyin.
  > * Kimlik doğrulama verileri dizine doğru olup olmadığını denetleyin biçimlendirilmiş, örneğin, + 1 4255551234 veya *kullanıcı\@contoso.com*. 
* **S:  Parola sıfırlamada hangi dilleri destekleniyor mu?**

  > **C:** Kullanıcı Arabirimi, parola sıfırlama sesli aramalar ve SMS mesajları Office 365'te desteklenen dillerde yerelleştirilmiş.
  >
  >
* **S:  Kuruluş ayarlarım, parola sıfırlama deneyiminde hangi bölümlerinin markalı öğeleri dizinimdeki marka sekmesinde yapılandırma kullanıcının?**

  > **C:** Parola sıfırlama portalı, kuruluşunuzun logosu görünür ve "yöneticinize başvurun" bağlantısı, bir özel e-posta veya URL işaret edecek şekilde yapılandırmanıza olanak tanır. Parola sıfırlama tarafından gönderilen e-posta, e-postanın gövdesinde, kuruluşunuzun logosu, renk ve adını içerir ve bu belirli ad için ayarlardan özelleştirilebilir.
  >
  >
* **S:  Kullanıcılarım parolalarını sıfırlamak için yapılması gerekenler hakkında bilgilendirme nasıl?**

  > **C:** Öneri bazılarını deneyin bizim [SSPR dağıtım](howto-sspr-deployment.md#sample-communication) makalesi.
  >
  >
* **S:  Bu sayfa bir mobil CİHAZDAN kullanabilir miyim?**

  > **C:** Evet, bu sayfayı mobil cihazlarda çalışır.
  >
  >
* **S:  Kullanıcıların parolalarını sıfırladığında yerel Active Directory hesaplarının kilidini açma destekliyorsunuz?**

  > **C:** Evet. Azure AD Connect parola geri yazma dağıttıysanız, kullanıcı parolalarını sıfırlar, yöneticiler parolalarını sıfırladığında bu kullanıcı hesabının kilidi otomatik olarak kaldırılır.
  >
  >
* **S:  Parola sıfırlama doğrudan my kullanıcının masaüstü oturum açma deneyiminin içinde nasıl tümleştirebilirim?**

  > **C:** Bir Azure AD Premium müşterisiyseniz, Microsoft Identity Manager'ı hiçbir ek ücret ödemeden yükleyin ve şirket içi parola sıfırlama çözümü dağıtın.
  >
  >
* **S:  Farklı yerel ayarlar için farklı güvenlik sorularını ayarlayabilirim?**

  > **C:** Hayır, bu bugün mümkün değildir.
  >
  >
* **S:  Güvenlik soruları kimlik doğrulama seçeneği için ne kadar fazla soruyu yapılandırabilirim?**

  > **C:** En fazla 20 özel güvenlik soruları, yapılandırabileceğiniz [Azure portalında](https://portal.azure.com).
  >
  >
* **S:  Güvenlik sorularını ne olabilir?**

  > **C:** Güvenlik sorularını 3 ila 200 karakter uzunluğunda olabilir.
  >
  >
* **S:  Güvenlik sorularını yanıtlar ne olabilir?**

  > **C:** Yanıtlar, 3-40 karakter uzunluğunda olabilir.
  >
  >
* **S:  Yinelenen reddedilen güvenlik sorularının yanıtlarının misiniz?**

  > **C:** Evet, yinelenen güvenlik sorularının yanıtlarının reddeder.
  >
  >
* **S:  Bir kullanıcı birden çok kez aynı Güvenlik sorusu kaydedebilir miyim?**

  > **C:** Hayır. Bir kullanıcı belirli bir sorunun kaydettikten sonra bu soruyu ikinci kez kayda alınamıyor.
  >
  >
* **S:  Güvenlik sorusu kaydı için en az bir sınır belirleyin ve sıfırlamak mümkündür?**

  > **C:** Evet, kaydı ve sıfırlama için başka bir sınırı ayarlanabilir. Üç ile beş güvenlik sorularını kaydı için gerekli olabilir, ve üç ile beş soru sıfırlama için gerekli olabilir.
  >
  >
* **S:  Sıfırlama için güvenlik sorularını kullan gerektirmek my ilke yapılandırdım ancak farklı şekilde yapılandırılması için Azure yöneticileri gibi görünüyor.**

  > **C:** Bu beklenen bir davranıştır. Microsoft, tüm Azure yöneticisi rolleri için varsayılan olarak güçlü ve iki aşamalı parola sıfırlama ilkesi uygular. Bu, yöneticilerin güvenlik sorularını kullanmasını önler. Bu ilkede hakkında daha fazla bilgi bulabilirsiniz [parola ilkeleri ve kısıtlamaları, Azure Active Directory'de](concept-sspr-policy.md) makalesi.
  >
  >
* **S:  Nasıl bir kullanıcı en fazla sıfırlamak için gereken soru sayısı kaydedildiyse, güvenlik soruları sıfırlama sırasında seçilir?**

  > **C:** *N* Güvenlik sorusu seçili rastgele bir kullanıcı, where, kayıtlı sorular toplam sayısı yetersiz *N* için ayarlanmış olduğu **sıfırlamakiçingerekensorularınsayısı** seçeneği. Örneğin, kullanıcı beş güvenlik sorularını kaydetmiş olduğunu, ancak yalnızca üç parola sıfırlama için gerekli olan üç beş soru rastgele seçilir ve sıfırlama sırasında sunulur. Kullanıcı soruların yanıtlarını alırsa sözcüğüne, soru önlemek için yanlış seçim işlemi üzerinden başlatır.
  >
  >
* **S:  E-posta ve SMS bir kerelik geçiş kodlarını geçerli ne kadar?**

  > **C:** Parola sıfırlama için oturum ömrünü 15 dakikadır. Parola sıfırlama işleminin başlangıcından kullanıcının parolasını sıfırlamak için 15 dakika gerekir. Bu süre dolduktan sonra e-posta ve SMS bir kerelik geçiş kodu geçersiz.
  >
  >
* **S:  Ben, kullanıcıların kendi parolalarını sıfırlama engelleyebilir miyim?**

  > **C:** Evet, SSPR'ı etkinleştirmek için bir grubu kullanıyorsanız, tek bir kullanıcı parolalarını sıfırlamalarına olanak tanır grubundan kaldırabilirsiniz. Genel yönetici kullanıcı, kullanıcılar parolalarını sıfırlamalarını korur ve bu devre dışı bırakılamaz.
  >
  >

## <a name="password-change"></a>Parola değiştirme

* **S:  Kullanıcılar parolalarını değiştirmek için nereye?**

  > **C:** Kullanıcıların parolalarını değiştirmesi gördüğünüz her yerde bunlar kendi profil resminizi veya simge gibi sağ üst köşedeki kendi [Office 365](https://portal.office.com) portalı veya [erişim paneli](https://myapps.microsoft.com) karşılaşır. Kullanıcıların kendi parolalarını değiştirmesi [erişim paneli profili sayfasını](https://account.activedirectory.windowsazure.com/r#/profile). Kullanıcılar ayrıca, parolalarının süresi dolmuş parolalarını otomatik olarak Azure AD oturum açma sayfasının değiştirmeyi sorulabilir. Son olarak, kullanıcılar için göz atabilir [Azure AD parola değiştirme portalı](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) doğrudan parolalarını geçmek istiyorsanız.
  >
  >
* **S:  Şirket içi parolalarını süresi dolduğunda kullanıcılar Office Portalı'nda bildirim alabilir?**

  > **C:** Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız Evet, bu bugün mümkündür. AD FS kullanıyorsanız, yönergeleri izleyin [parola ilkesi talepleri AD FS ile gönderme](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) makalesi. Parola Karması eşitleme kullanırsanız, bu bugün mümkün değildir. Bu nedenle bizim için deneyimleri buluta süre sonu bildirimleri göndermek mümkün değildir biz şirket içi dizinlere parola ilkelerinden eşitleme. Her iki durumda da filtrelenebilir [parolaları PowerShell aracılığıyla dolmak üzere kullanıcıları bilgilendir](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **S:  Ben, kullanıcıların parolasını değiştirmesini engelleyebilir miyim?**

  > **C:** Yalnızca bulutta yer alan kullanıcılar için parola değişiklikleri engellenemez. Şirket içi kullanıcılar için ayarladığınız **kullanıcı parolayı değiştiremez** seçili seçeneği. Seçili kullanıcıların parolalarını değiştiremezsiniz.
  >
  >

## <a name="password-management-reports"></a>Parola yönetimi raporları

* **S:  Parola yönetimi raporları gösterilecek verileri ne kadar sürüyor?**

  > **C:** Veri parola yönetimi raporları, 5-10 dakika içinde görüntülenmesi gerekir. Bazı durumlarda, işlemin bir saat için görüntülenecek sürebilir.
  >
  >
* **S:  Parola yönetim raporlarını filtre nasıl sağlayabilirsiniz?**

  > **C:** Parola yönetimi raporları filtrelemek için raporun üst kısmındaki sütun etiketleri aşırı sağındaki küçük Büyüteç'i seçin. Daha zengin filtreleme yapmak istiyorsanız, Excel raporunu indirin ve bir PivotTable oluşturun.
  >
  >
* **S: Parola yönetimi raporları depolanan olayları sayısı nedir?**

  > **C:** En fazla 75.000 parola sıfırlama veya parola sıfırlama kayıt olaylarını geri 30 gün içinde sunulan ürünün kendinde kapsayan parola yönetim raporlarını depolanır. Bu sayı daha fazla olay içerecek şekilde genişletmek için çalışıyoruz.
  >
  >
* **S:  Parola yönetimi raporları geride nasıl gidiyor?**

  > **C:** Parola yönetimi raporları, son 30 gün içinde gerçekleşen işlemleri gösterir. Şu an için bu verileri arşivlemek istiyorsanız, raporları düzenli aralıklarla indirebilir ve bunları farklı bir konuma kaydedin.
  >
  >
* **S:  Parola yönetimi raporlarda görüntülenen satır sayısı var mı?**

  > **C:** Evet. Kullanıcı Arabiriminde gösterilir veya indirilen en fazla 75.000 satır parola yönetim raporlarını birini görünebilir.
  >
  >
* **S:  Parola sıfırlama veya raporlama verilerini kayıt erişmek için bir API var mı?**

  > **C:** Evet. Veri akışı raporlama parola sıfırlama nasıl erişebileceğinizi öğrenmek için bkz [parola sıfırlama raporlama olayları program aracılığıyla erişmeyi öğrenin](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Parola geri yazma

* **S:  Parola geri yazma, perde arkasında nasıl çalışır?**

  > **C:** Makaleye göz atın [parola geri yazma nasıl çalıştığını](howto-sspr-writeback.md) , etkinleştirdiğinizde ne olacak bir açıklama için parola geri yazma ve sistem üzerinden verilerin nasıl aktığını şirket içi ortamınıza geri.
  >
  >
* **S:  Parola geri yazma çalışması ne kadar sürüyor? Olan var. bir eşitleme gecikme ile parola karma eşitlemesi gibi?**

  > **C:** Parola geri yazma anlık. Bu, parola karması eşitleme tamamen farklı bir şekilde çalıştığı zaman uyumlu bir işlem hattı olur. Parola geri yazma kullanıcılara kendi parola sıfırlama başarısını hakkında gerçek zamanlı geri bildirim almak veya işlemi değiştirme olanağı sağlar. Ortalama başarılı bir parola geri yazma için 500 MS'nin altında zamandır.
  >
  >
* **S:  Şirket içi Hesabımı devre dışı ise nasıl my bulut hesabı ve etkilenen erişim nedir?**

  > **C:** Şirket içi Kimliğinizi devre dışı bırakıldı, bulut kimliği ve erişim aynı zamanda Azure AD Connect aracılığıyla sonraki eşitleme aralıkta devre dışı bırakılır. Varsayılan olarak, bu eşitleme her 30 dakikadır.
  >
  >
* **S:  Şirket içi Hesabımı, bir şirket içi Active Directory parola ilkesi tarafından kısıtlanmış, SSPR parolamı değiştirdiğimde Bu ilke uyma mu?**

  > **C:** Evet, SSPR kullanır ve şirket içi Active Directory parola ilkesi tarafından uyduğundan. Bu ilke, tipik bir Active Directory etki alanı parola ilkesi yanı sıra, bir kullanıcıya hedeflenen tüm tanımlanmış, hassas parola ilkelerini içerir.
  >
  >
* **S:  Hesapları yoksa parola geri yazma hangi türde çalışır?**

  > **C:** Parola geri yazma çalışan şirket içi Active Directory'den Azure AD'ye eşitlenen kullanıcı hesapları için de dahil olmak üzere Federasyon, parola karma eşitleme ve geçiş Autentication kullanıcılar.
  >
  >
* **S:  Parola geri yazma etki alanımın parola ilkelerini zorunlu kılıyor mu?**

  > **C:** Evet. Parola geri yazma özelliğini parola geçerlilik süresi, geçmiş, karmaşıklık, filtreler ve yerel etki alanınızda parolaları yerinde bırakabilecek herhangi bir kısıtlama uygular.
  >
  >
* **S:  Parola geri yazma güvenli mi?  Ben hesabınızın ele geçirilmesi gerekmez nasıl mutlaka?**

  > **C:** Evet, parola geri yazma güvenlidir. Parola geri yazma hizmeti tarafından uygulanan birden çok güvenlik katmanı hakkında daha fazla bilgi için kullanıma [parola geri yazma güvenlik](concept-sspr-writeback.md#password-writeback-security) konusundaki [parola geri yazma genel bakış](howto-sspr-writeback.md) makalesi.
  >
  >

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
