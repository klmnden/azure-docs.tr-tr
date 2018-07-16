---
title: Azure Active Directory SSS - Self Servis parola sıfırlama
description: Sık sorulan sorular hakkında Azure AD Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: c006e448b8da1acaf51c8339cbcd0b6170f29874
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054820"
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

* **Kullanıcılar kendi parola sıfırlama verileri kayıt oluşturabilir mi?**

  > **Y:** Evet. Parola sıfırlama etkinleştirildi ve lisansına sahip olduğunuz sürece, kullanıcılar parola sıfırlama kayıt portalı gidebilirsiniz (https://aka.ms/ssprsetup) kimlik doğrulama bilgilerini kaydetmek için. Kullanıcılar ayrıca erişim panelinden kaydetme (http://myapps.microsoft.com). Erişim panelinden kaydedilecek için ihtiyaç duydukları kendi profil resminizi seçin, **profili**ve ardından **parola sıfırlama için kaydolmasını** seçeneği.
  >
  >
* **S: parola'etkin değilse, bir grup için sıfırlama ve herkes için benim kullanıcılar gerekli yeniden kaydetmeniz etkinleştirmeye karar?**

  > **C:** Hayır. Kimlik doğrulama verilerini doldurulmuş kullanıcılar yeniden kaydolmak için gerekli değildir.
  >
  >
* **Adına kullanıcılarımın parola sıfırlama verileri tanımlamak miyim?**

  > **Y:** Evet, PowerShell, Azure AD Connect ile bunu yapabilirsiniz [Azure portalında](https://portal.azure.com), veya Office 365 Yönetim Merkezi. Daha fazla bilgi için [veri kullanılan Azure AD Self Servis parola sıfırlama](howto-sspr-authenticationdata.md).
  >
  >
* **Ben, şirket içi güvenlik sorusunun verileri eşitlemek miyim?**

  > **Y:** Hayır, bu bugün mümkün değildir.
  >
  >
* **Kullanıcılarım veri diğer kullanıcılar bu verileri göremez bir şekilde kayıt oluşturabilir mi?**

  > **Y:** Evet. Kullanıcılar kaydettiğinizde parola kullanarak verileri sıfırlama kayıt portalı, verileri yalnızca genel Yöneticiler ve kullanıcı için görünür olan özel kimlik doğrulama alanları kaydedilir.
  >
  >
* **Kullanıcılarımın parola sıfırlama kullanabilmeniz için önce kaydedilmesi gerekir s:?**

  > **C:** Hayır. Kendi adınıza yeterli kimlik doğrulama bilgilerini tanımlarsanız, kullanıcıların kaydetmek zorunda kalmaması. Uygun alanları dizinde depolanan verileri doğru şekilde biçimlendirildiğini sürece parola sıfırlama işleminin çalıştığı.
  >
  >
* **S: eşitlemek veya miyim kullanıcılar adına kimlik doğrulama telefonu, kimlik doğrulama e-posta veya alternatif kimlik doğrulama telefon alanlarını ayarlayın?**

  > **Y:** makalesinde bir genel yönetici tarafından ayarlanabilir alanlar tanımlanan [SSPR veri gereksinimleri](howto-sspr-authenticationdata.md).
  >
  >
* **S: kayıt portalı kullanıcılarımın göstermek için hangi seçeneklerin nasıl belirliyor?**

  > **Y:** sıfırlama kullanıcılarınız için etkinleştirilmiş olan seçenekleri kayıt portalı gösterir. Bu seçenekler altında bulunan **kullanıcı parolası sıfırlama İlkesi** dizininizin bölüm **yapılandırma** sekmesi. Güvenlik sorularını etkinleştirmezseniz, örneğin, ardından kullanıcılar için bu seçenek kaydedilecek yükümlü değiliz.
  >
  >
* **S: ne zaman bir kullanıcı olarak kabul edilir kayıtlı?**

  > **Y:** isteneceği zaman SSPR için kayıtlı bir kullanıcı kabul en az **sıfırlamak için gereken yöntemlerin sayısı** , ayarladığınız parolayı [Azure portalında](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Parola sıfırlama

* **S: birden çok kısa bir süre içinde bir parola sıfırlama girişimlerine karşı kullanıcıların önlemek?**

  > **Y:** Evet, kötüye korumak için parola sıfırlamayı yerleşik güvenlik özellikleri vardır. 
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
* **S: ne kadar süreyle bir e-posta, SMS veya telefon görüşmesi parola sıfırlaması almak için beklemeniz gerekir?**

  > **Y:** e-posta, SMS mesajları ve telefon aramaları geldiğinde bir dakikadan. 5-20 saniye normal bir durumdur.
    >Bu zaman çerçevesinde bildirimini almazsanız:
        > * Klasörünüzü kontrol edin.
        > * Numarası veya e-posta iletişim kurulan bir beklediğiniz olup olmadığını denetleyin.
        > * Kimlik doğrulama verileri dizine doğru olup olmadığını denetleyin biçimlendirilmiş, örneğin, + 1 4255551234 veya *user@contoso.com*. 
  >
  >
* **S: hangi dilleri, parola sıfırlama tarafından destekleniyor mu?**

  > **Y:** kullanıcı Arabirimi, parola sıfırlama sesli aramalar ve SMS mesajları Office 365'te desteklenen dillerde yerelleştirilmiş.
  >
  >
* **S: hangi bölümlerinin parola sıfırlama deneyiminde Kurumsal marka öğeleri dizinimdeki ayarladığınızda markalı alma kullanıcının yapılandırma sekmesi?**

  > **Y:** parola sıfırlama portalı, kuruluşunuzun logosu görünür ve "yöneticinize başvurun" bağlantısı, bir özel e-posta veya URL işaret edecek şekilde yapılandırmanıza olanak tanır. Parola sıfırlama tarafından gönderilen e-posta, e-postanın gövdesinde, kuruluşunuzun logosu, renk ve adını içerir ve bu belirli ad için ayarlardan özelleştirilebilir.
  >
  >
* **S: nasıl parolalarını sıfırlamak için Kullanıcılarım için nereye hakkında bilgilendirme?**

  > **Y:** önerilerine bazılarını deneyin bizim [SSPR dağıtım](howto-sspr-deployment.md#email-based-rollout) makalesi.
  >
  >
* **Bu sayfa bir mobil CİHAZDAN kullanabilirim miyim?**

  > **Y:** Evet, bu sayfayı mobil cihazlarda çalışır.
  >
  >
* **S: kullanıcıların parolalarını sıfırladığında yerel Active Directory hesaplarının kilidini açma desteği?**

  > **Y:** Evet. Azure AD Connect parola geri yazma dağıttıysanız, kullanıcı parolalarını sıfırlar, yöneticiler parolalarını sıfırladığında bu kullanıcı hesabının kilidi otomatik olarak kaldırılır.
  >
  >
* **S: parola sıfırlama doğrudan my kullanıcının masaüstü oturum açma deneyiminin içinde nasıl tümleştirebilirim?**

  > **Y:** bir Azure AD Premium müşterisiyseniz, Microsoft Identity Manager'ı hiçbir ek ücret ödemeden yükleyin ve şirket içi parola sıfırlama çözümü dağıtın.
  >
  >
* **Farklı yerel ayarlar için farklı güvenlik sorularını ayarlarım miyim?**

  > **Y:** Hayır, bu bugün mümkün değildir.
  >
  >
* **S: kaç soru güvenlik soruları kimlik doğrulaması seçeneği yapılandırabilirim?**

  > **Y:** içinde en fazla 20 özel güvenlik soruları yapılandırabileceğiniz [Azure portalında](https://portal.azure.com).
  >
  >
* **S: ne kadar güvenlik sorularını olabilir mi?**

  > **Y:** güvenlik soruları, 3 ila 200 karakter uzunluğunda olabilir.
  >
  >
* **S: ne kadar güvenlik sorularının yanıtlarının olabilir mi?**

  > **Y:** yanıtlar, 3-40 karakter uzunluğunda olabilir.
  >
  >
* **S: Yinelenen reddedilen güvenlik sorularının yanıtlarının misiniz?**

  > **Y:** Evet, biz yinelenen güvenlik sorularının yanıtlarının reddet.
  >
  >
* **Bir kullanıcı birden çok kez aynı Güvenlik sorusu kaydı miyim?**

  > **C:** Hayır. Bir kullanıcı belirli bir sorunun kaydettikten sonra bu soruyu ikinci kez kayda alınamıyor.
  >
  >
* **S: sıfırlama ve kayıt için güvenlik sorularını en az bir sınır mümkün mü?**

  > **Y:** kaydı ve sıfırlama için başka bir sınır Evet, ayarlanabilir. Üç ile beş güvenlik sorularını kaydı için gerekli olabilir, ve üç ile beş soru sıfırlama için gerekli olabilir.
  >
  >
* **S: Alanım İlkesi sıfırlama için güvenlik sorularını kullan gerektirmek için yapılandırılmış, ancak farklı şekilde yapılandırılması için Azure yöneticileri gibi görünüyor.**

  > **Y:** bu beklenen bir davranıştır. Microsoft, tüm Azure yöneticisi rolleri için bir tanımlayıcı varsayılan iki ağ geçidi parola sıfırlama İlkesi uygular. Bu, yöneticilerin güvenlik sorularını kullanmasını önler. Bu ilkede hakkında daha fazla bilgi bulabilirsiniz [parola ilkeleri ve kısıtlamaları, Azure Active Directory'de](concept-sspr-policy.md#administrator-password-policy-differences) makalesi.
  >
  >
* **S: nasıl bir kullanıcı en fazla sıfırlamak için gereken soru sayısı kaydedildiyse, güvenlik soruları sıfırlama sırasında seçilir?**

  > **Y:** *N* Güvenlik sorusu seçili rastgele bir kullanıcı, where, kayıtlı sorular toplam sayısı yetersiz *N* için ayarlanmış olduğu **numarası sıfırlamak için gereken soruların** seçeneği. Örneğin, kullanıcı beş güvenlik sorularını kaydetmiş olduğunu, ancak yalnızca üç parola sıfırlama için gerekli olan üç beş soru rastgele seçilir ve sıfırlama sırasında sunulur. Kullanıcı soruların yanıtlarını alırsa sözcüğüne, soru önlemek için yanlış seçim işlemi üzerinden başlatır.
  >
  >
* **S: ne kadar e-posta ve SMS bir kerelik geçiş kodlarını geçerli misiniz?**

  > **Y:** parola sıfırlama için oturum ömrünü 15 dakikadır. Parola sıfırlama işleminin başlangıcından kullanıcının parolasını sıfırlamak için 15 dakika gerekir. Bu süre dolduktan sonra e-posta ve SMS bir kerelik geçiş kodu geçersiz.
  >
  >
* **Kullanıcıların kendi parolalarını sıfırlama engellersem miyim?**

  > **Y:** SSPR'ı etkinleştirmek için bir grubu kullanıyorsanız, Evet, bireysel bir kullanıcı parolalarını sıfırlamalarına olanak tanıyan grubundan kaldırabilirsiniz. Genel yönetici kullanıcı, kullanıcılar parolalarını sıfırlamalarını korur ve bu devre dışı bırakılamaz.
  >
  >

## <a name="password-change"></a>Parola değiştirme

* **S: kullanıcılarımın parolalarını değiştirmek için nereye?**

  > **Y:** kullanıcıların parolalarını değiştirmesi gördüğünüz her yerde bunlar kendi profil resminizi veya simge gibi sağ üst köşedeki kendi [Office 365](https://portal.office.com) portalı veya [erişim paneli](https://myapps.microsoft.com) karşılaşır. Kullanıcıların kendi parolalarını değiştirmesi [erişim paneli profili sayfasını](https://account.activedirectory.windowsazure.com/r#/profile). Kullanıcılar ayrıca, parolalarının süresi dolmuş parolalarını otomatik olarak Azure AD oturum açma sayfasının değiştirmeyi sorulabilir. Son olarak, kullanıcılar için göz atabilir [Azure AD parola değiştirme portalı](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) doğrudan parolalarını geçmek istiyorsanız.
  >
  >
* **Şirket içi parolalarını süresi dolduğunda kullanıcılar Office Portalı'nda bildirilir miyim?**

  > **Y:** Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız Evet, bu bugün mümkündür. AD FS kullanıyorsanız, yönergeleri izleyin [parola ilkesi talepleri AD FS ile gönderme](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) makalesi. Parola Karması eşitleme kullanırsanız, bu bugün mümkün değildir. Bu nedenle bizim için deneyimleri buluta süre sonu bildirimleri göndermek mümkün değildir biz şirket içi dizinlere parola ilkelerinden eşitleme. Her iki durumda da filtrelenebilir [parolaları PowerShell aracılığıyla dolmak üzere kullanıcıları bilgilendir](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **Kullanıcıların parolalarını değiştirmesini engellersem miyim?**

  > **Y:** yalnızca bulutta yer alan kullanıcılar için parola değişiklikleri engellenemez. Şirket içi kullanıcılar için ayarladığınız **kullanıcı parolayı değiştiremez** seçili seçeneği. Seçili kullanıcıların parolalarını değiştiremezsiniz.
  >
  >

## <a name="password-management-reports"></a>Parola yönetimi raporları

* **S: parola yönetimi raporları gösterilecek veri ne kadar sürüyor?**

  > **Y:** veri üzerinde parola yönetim raporlarını 5-10 dakika içinde görünmelidir. Bazı durumlarda, işlemin bir saat için görüntülenecek sürebilir.
  >
  >
* **S: parola yönetim raporlarını filtre nasıl sağlayabilirsiniz?**

  > **Y:** parola yönetimi raporları filtrelemek için raporun üst kısmındaki sütun etiketleri aşırı sağındaki küçük Büyüteç seçin. Daha zengin filtreleme yapmak istiyorsanız, Excel raporunu indirin ve bir PivotTable oluşturun.
  >
  >
* **S: parola yönetim raporlarını içinde depolanan olayları sayısı nedir?**

  > **Y:** kadar 75.000 parola sıfırlama veya parola sıfırlama kayıt olaylarını geri 30 gün içinde sunulan ürünün kendinde kapsayan parola yönetim raporlarını depolanır. Bu sayı daha fazla olay içerecek şekilde genişletmek için çalışıyoruz.
  >
  >
* **S: parola yönetim raporlarını ne kadar geri dönün?**

  > **Y:** parola yönetimi raporları son 30 gün içinde gerçekleşen işlemleri göster. Şu an için bu verileri arşivlemek istiyorsanız, raporları düzenli aralıklarla indirebilir ve bunları farklı bir konuma kaydedin.
  >
  >
* **S: parola yönetimi raporlarda görüntülenen satır sayısı var mı?**

  > **Y:** Evet. Kullanıcı Arabiriminde gösterilir veya indirilen en fazla 75.000 satır parola yönetim raporlarını birini görünebilir.
  >
  >
* **S: parola sıfırlama veya raporlama verilerini kayıt erişmek için bir API var mı?**

  > **Y:** Evet. Veri akışı raporlama parola sıfırlama nasıl erişebileceğinizi öğrenmek için bkz [parola sıfırlama raporlama olayları program aracılığıyla erişmeyi öğrenin](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Parola geri yazma

* **S: parola geri yazma arka planda çalışır mı?**

  > **Y:** makaleye göz atın [parola geri yazma nasıl çalıştığını](howto-sspr-writeback.md) , etkinleştirdiğinizde ne olacak bir açıklama için parola geri yazma ve sistem üzerinden verilerin nasıl aktığını şirket içi ortamınıza geri.
  >
  >
* **S: parola geri yazma çalışması ne kadar sürüyor? Olan var. bir eşitleme gecikme ile parola karma eşitlemesi gibi?**

  > **Y:** parola geri yazma anlık. Bu, parola karması eşitleme tamamen farklı bir şekilde çalıştığı zaman uyumlu bir işlem hattı olur. Parola geri yazma kullanıcılara kendi parola sıfırlama başarısını hakkında gerçek zamanlı geri bildirim almak veya işlemi değiştirme olanağı sağlar. Ortalama başarılı bir parola geri yazma için 500 MS'nin altında zamandır.
  >
  >
* **S: şirket içi Hesabımı devre dışıysa, my bulut hesabı ve etkilenen erişimi nasıl mi?**

  > **Y:** şirket içi Kimliğinizi devre dışıysa, bulut kimliği ve erişim aynı zamanda Azure AD Connect aracılığıyla sonraki eşitleme aralıkta devre dışı bırakılır. Varsayılan olarak, bu eşitleme her 30 dakikadır.
  >
  >
* **S: şirket içi Hesabımı bir şirket içi Active Directory parola ilkesi tarafından kısıtlı, SSPR parolamı değiştirdiğimde Bu ilke uyma mu?**

  > **Y:** Evet, SSPR kullanır ve şirket içi Active Directory parola ilkesi tarafından uyduğundan. Bu ilke, tipik bir Active Directory etki alanı parola ilkesi yanı sıra, bir kullanıcıya hedeflenen tüm tanımlanmış, hassas parola ilkelerini içerir.
  >
  >
* **S: hangi tür hesabı için parola geri yazma çalışır mı?**

  > **Y:** dahil olmak üzere şirket içi Active Directory'den Azure AD'ye eşitlenen kullanıcı hesapları için parola geri yazma çalışır, Federasyon, parola karması eşitlenmiş ve doğrudan Autentication kullanıcılar.
  >
  >
* **S: parola geri yazma etki alanımın parola ilkelerini zorlamak mu?**

  > **Y:** Evet. Parola geri yazma özelliğini parola geçerlilik süresi, geçmiş, karmaşıklık, filtreler ve yerel etki alanınızda parolaları yerinde bırakabilecek herhangi bir kısıtlama uygular.
  >
  >
* **S: parola geri yazma güvenli mi?  Ben hesabınızın ele geçirilmesi gerekmez nasıl mutlaka?**

  > **Y:** Evet, parola geri yazma güvenlidir. Parola geri yazma hizmeti tarafından uygulanan dört güvenlik katmanı hakkında daha fazla bilgi için kullanıma [parola geri yazma güvenlik modeli](howto-sspr-writeback.md#password-writeback-security-model) konusundaki [parola geri yazma genel bakış](howto-sspr-writeback.md) makalesi.
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
