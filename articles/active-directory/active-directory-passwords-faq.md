---
title: "Sık sorulan sorular: Azure AD SSPR'yi | Microsoft Docs"
description: "Azure AD Self Servis parola hakkında sık sorulan sorular Sıfırla"
services: active-directory
keywords: "Active directory parola yönetimi, Azure AD parola yönetimi self servis parola sıfırlama"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: c5c3129a50f456e484bf8af2a866059f494bed1d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="password-management-frequently-asked-questions"></a>Parola yönetimi sık sorulan sorular

Aşağıdaki sık sorulan bazı sorular (SSS) parola sıfırlama için ilgili her şeyden içindir.

Azure Active Directory (Azure AD) hakkında genel bir sorunuz varsa ve burada yanıtlanan değil Self Servis parola sıfırlama (SSPR), isteyin Topluluk Yardım için üzerinde [Azure AD Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Topluluk üyeleri mühendisleri, ürün yöneticileri, MVP ve diğer BT uzmanlarının içerir.

Bu SSS, aşağıdaki bölümlere ayrılır:

* [Parola hakkında sorular sıfırlama kaydı](#password-reset-registration)
* [Parola sıfırlama hakkında sorular](#password-reset)
* [Parola değiştirme hakkında sorular](#password-change)
* [Parola yönetimi raporları hakkında sorular](#password-management-reports)
* [Parola geri yazma hakkında sorular](#password-writeback)

## <a name="password-reset-registration"></a>Parola sıfırlama kaydı

* **S: Kullanıcılarım kendi parola sıfırlama verileri kaydedebilirsiniz?**

  > **Y:** Evet. Parola sıfırlama etkindir ve lisansına sahip olduğunuz sürece, kullanıcıların kendi kimlik doğrulama bilgilerini kaydetmek için parola sıfırlama kayıt portalı için (http://aka.ms/ssprsetup) gidebilirsiniz. Kullanıcıların erişim paneli (http://myapps.microsoft.com) da kaydedebilirsiniz. Erişim paneli kaydetmek için için ihtiyaç duydukları kendi profil resmi seçin, **profil**ve ardından **parola sıfırlama için kaydetme** seçeneği.
  >
  >
* **S: parola sıfırlama verileri Kullanıcılarım adına tanımlamak?**

  > **Y:** Evet, Azure AD Connect, PowerShell ile yapabilirsiniz [Azure portal](https://portal.azure.com), veya Office 365 Yönetim Merkezi. Daha fazla bilgi için bkz: [Azure AD Self Servis parola tarafından kullanılan verileri sıfırlama](active-directory-passwords-data.md).
  >
  >
* **S: şirket içi güvenlik soruları için veri eşitleme?**

  > **Y:** Hayır, bu bugün mümkün değildir.
  >
  >
* **S: Kullanıcılarım veri diğer kullanıcılar bu verileri göremez yolla kaydedebilirsiniz?**

  > **Y:** Evet. Kullanıcıların kaydederken parolayı kullanarak veri kayıt portalı sıfırlama, veriler yalnızca genel Yöneticiler ve kullanıcı için görünür olan özel kimlik doğrulama alanlara kaydedilir.
  >
  >
* **S: Kullanıcılarım parola sıfırlamayı kullanabilmek için önce kaydedilmesi gerekiyor?**

  > **C:** Hayır. Onların adına yeterli kimlik doğrulaması bilgilerini tanımlayın, kullanıcıların kaydetmeniz gerekmez. Dizin uygun alanlarında depolanan verilerin düzgün biçimlendirilmiş olduğu sürece works parolasını sıfırlayın.
  >
  >
* **S: miyim eşitlemek veya kimlik doğrulama telefon, kimlik doğrulama e-posta veya diğer kimlik doğrulama telefon alanları Kullanıcılarım adına ayarlayın?**

  > **Y:** Hayır, bu bugün mümkün değildir.
  >
  >
* **S: kayıt portalı Kullanıcılarım göstermek için hangi seçeneklerin nasıl belirler?**

  > **Y:** parola sıfırlama kayıt portalı gösterir, kullanıcılarınız için etkinleştirilmiş olan seçenekleri. Bu seçenekler altında bulunan **kullanıcı parolası sıfırlama ilkesini** , dizinin bölümünü **yapılandırma** sekmesi. Güvenlik soruları etkinleştirmezseniz, örneğin, ardından kullanıcıları için bu seçeneği kaydetmek mümkün değildir.
  >
  >
* **Kullanıcı ne zaman kabul s: kayıtlı?**

  > **Y:** kayıtlı olduğunda SSPR için kayıtlı kullanıcı kabul en az **sıfırlamak için gereken yöntemleri sayısı** , ayarladığınız parolayı [Azure portal](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Parola sıfırlama

* **S: birden çok kısa bir süre içinde bir parola sıfırlama girişimlerine kullanıcıların engellemek?**

  > **Y:** Evet, kötüye kullanımdan korumak amacıyla parola sıfırlama yerleşik güvenlik özellikleri vardır. 
  >
  > Kullanıcılar, 24 saat için kilitli önce bir 24 saatlik süre içinde yalnızca beş parola sıfırlama girişimlerine deneyebilirsiniz. 
  >
  > Kullanıcılar, bir telefon numarası doğrulama, SMS gönder ya da bunlar için 24 saat kilitli önce bir saat içinde yalnızca beş kez güvenlik sorularını ve yanıtlarını doğrulamak deneyebilirsiniz. 
  >
  > Kullanıcılar, en fazla 10 kez bunlar 24 saat için kilitli önce 10 dakikalık süre içinde bir e-posta gönderebilir.
  >
  > Bir kullanıcının parolasını sıfırlar sonra sayaç sıfırlanır.
  >
  >
* **S: parola sıfırlama bir e-posta, SMS veya telefon görüşmesi almayı ne kadar süre beklemesi gerekir?**

  > **Y:** SMS iletileri, e-postalar ve telefon aramaları ulaştığını içinde altında bir dakika. 5-20 saniye normal bir durumdur.
    >Bu zaman dilimi içinde bildirim almadıysanız:
        > * Gereksiz klasörünüzü kontrol edin.
        > * Numarası veya e-posta iletişim kurulan bir beklediğiniz olup olmadığını denetleyin.
        > * Kimlik doğrulama verilerinin dizininde doğru olduğunu denetleyin biçimlendirilmiş, örneğin, + 1 4255551234 veya  *user@contoso.com* . 
  >
  >
* **S: hangi dilde parola sıfırlama tarafından destekleniyor mu?**

  > **Y:** UI, parola sıfırlama SMS iletileri ve sesli aramalara yerelleştirilmiş aynı dillerde Office 365'te desteklenir.
  >
  >
* **S: parola sıfırlama deneyimi hangi kısımlarının kuruluş marka öğeleri my dizininde ayarladığınızda markalı alma kullanıcının yapılandırma sekmesi?**

  > **Y:** parola sıfırlama Portalı'nı kuruluşunuzun logosu gösterir ve "yöneticinize başvurun" bağlantı özel e-posta ya da URL işaret edecek şekilde yapılandırmanıza olanak sağlar. Parola sıfırlama tarafından gönderilen e-posta e-posta gövdesindeki kuruluşunuzun logosu, renk ve adını içerir ve bu belirli ad için ayarlardan özelleştirilebilir.
  >
  >
* **S: kullanıcıların parolalarını sıfırlamaya Kullanıcılarım nereye hakkında nasıl eğitin?**

  > **Y:** bazı öneriler deneyin bizim [SSPR dağıtım](active-directory-passwords-best-practices.md#email-based-rollout) makalesi.
  >
  >
* **S: bir mobil aygıttan bu sayfayı kullanın?**

  > **Y:** Evet, bu sayfayı mobil cihazlarda çalışır.
  >
  >
* **S: kullanıcıların parolalarını sıfırladığınızda kilidini açma yerel Active Directory hesaplarını destekler mi?**

  > **Y:** Evet. İle Azure AD CONNECT'te parola geri yazma dağıtıldıysa kullanıcı parolalarını sıfırlar, parolalarını sıfırladığınızda, kullanıcının hesabını kilidi otomatik olarak kaldırılır.
  >
  >
* **S: parola sıfırlama doğrudan my kullanıcının masaüstü oturum açma deneyimi nasıl tümleştirebilir miyim?**

  > **Y:** bir Azure AD Premium müşteri değilseniz, hiçbir ek ücret Microsoft Identity Manager yükleme ve şirket içi parola sıfırlama çözümünü dağıtma.
  >
  >
* **S: farklı yerel ayarlar için farklı güvenlik soruları ayarlamak?**

  > **Y:** Hayır, bu bugün mümkün değildir.
  >
  >
* **S: kaç sorular için güvenlik soruları kimlik doğrulaması seçeneği ı yapılandırabilir miyim?**

  > **Y:** içinde en çok 20 özel güvenlik soruları yapılandırabilirsiniz [Azure portal](https://portal.azure.com).
  >
  >
* **S: ne kadar süreyle güvenlik soruları olabilir mi?**

  > **Y:** güvenlik soruları 3 ila 200 karakter uzunluğunda olabilir.
  >
  >
* **S: ne kadar süreyle güvenlik soruların yanıtlarını olabilir mi?**

  > **Y:** yanıtlar 3-40 karakter uzunluğunda olabilir.
  >
  >
* **S: Yinelenen soruların yanıtları için reddedilen güvenlik misiniz?**

  > **Y:** Evet, biz yinelenen güvenlik soruların yanıtlarını reddedin.
  >
  >
* **S: bir kullanıcı aynı Güvenlik sorusu birden çok kez kaydedebilirsiniz?**

  > **C:** Hayır. Bir kullanıcı belirli bir sorunun kaydolduktan sonra bu soruyu ikinci kez kaydı yapılamıyor.
  >
  >
* **S: Güvenlik sorusu kaydı için en az bir sınır ayarlamak ve sıfırlamak mümkün mü?**

  > **Y:** kaydı ve sıfırlama için başka bir sınır Evet, ayarlanabilir. Üç beş güvenlik soruları kaydı için gerekli olabilir, beş ve üç soruları sıfırlama için gerekli olabilir.
  >
  >
* **S: my ilkesi kullanıcıların sıfırlama için güvenlik soruları kullanmasını gerektirecek şekilde yapılandırılmış, ancak Azure Yöneticiler farklı şekilde yapılandırılmış gibi görünüyor.**

  > **Y:** bu beklenen bir davranıştır. Microsoft Azure Yönetici rolü için güçlü varsayılan iki ağ geçidi parolası sıfırlama ilkesini zorlar. Bu, yöneticilerin güvenlik soruları kullanmasını önler. Bu ilkede hakkında daha fazla bilgi bulabilirsiniz [parola ilkeleri ve kısıtlamaları Azure Active Directory'de](active-directory-passwords-policy.md#administrator-password-policy-differences) makalesi.
  >
  >
* **S: nasıl bir kullanıcı birden çok sıfırlamak için gereken soru sayısını kaydedildiyse, güvenlik soruları sıfırlaması sırasında seçilir?**

  > **Y:** *N* Güvenlik sorusu adedini seçili rastgele bir kullanıcı için where kayıtlı soruları toplam sayısı içinden *N* için ayarlanmış miktardır  **Sıfırlamak için gereken soru sayısını** seçeneği. Örneğin, bir kullanıcı beş güvenlik soruları kayıtlı olan, ancak yalnızca üç parola sıfırlama için gereklidir, üç beş soruları rastgele seçilir ve sıfırlama sırasında sunulur. Kullanıcı soruların yanıtlarını alır sözcüğüne, soru engellemek için yanlış seçim işlemi üzerinden başlatır.
  >
  >
* **S: e-posta ve SMS bir kerelik geçiş kodlarını geçerli ne kadar?**

  > **Y:** parola sıfırlama için oturum yaşam süresi 15 dakikadır. Parola sıfırlama işlemi başından kullanıcının parolasını sıfırlamak için 15 dakika vardır. Bu süre dolduktan sonra e-posta ve SMS bir kerelik geçiş kodu geçersiz.
  >
  >
* **S:, parola sıfırlama kullanıcıların engellemek?**

  > **Y:** SSPR etkinleştirmek için bir grubu kullanıyorsanız, Evet, tek bir kullanıcı parolalarını sıfırlama olanağı tanıyan grubundan kaldırabilirsiniz.
  >
  >

## <a name="password-change"></a>Parola değiştirme

* **S: kullanıcılar parolalarını değiştirmek için nereye?**

  > **Y:** kullanıcılar parolalarını değiştirebilir gördüğünüz her yerde bunlar kendi profil resmi veya simge gibi sağ üst köşesinde kendi [Office 365](https://portal.office.com) portal veya [erişim paneli](https://myapps.microsoft.com) karşılaştığında. Kullanıcılar parolalarını gelen değiştirebilir [erişim paneli profili sayfasını](https://account.activedirectory.windowsazure.com/r#/profile). Kullanıcıların parolalarının süresinin dolduğu parolalarını Azure AD oturum açma sayfasında otomatik olarak değiştirmek için de sorulan. Son olarak, kullanıcılar için göz atabilir [Azure AD parola değişikliği portalı](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) doğrudan parolalarını değiştirmek istiyorsanız.
  >
  >
* **S: şirket içi parolalarını sona erdiğinde Kullanıcılarım Office Portalı'nda bildirilir?**

  > **Y:** Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsanız, Evet, bu bugün mümkündür. AD FS kullanıyorsanız,'ndaki yönergeleri izleyin [AD FS ile parola ilkesi talep gönderme](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) makalesi. Parola karma eşitlemesi kullanırsanız, bu bugün mümkün değildir. Bize deneyimleri bulut için süre sonu bildirimleri göndermek mümkün değildir biz parola ilkeleri şirket dizinlerinden eşitlenmez. Her iki durumda da için mümkündür [parolaları PowerShell aracılığıyla dolmak üzere kullanıcıları bildir](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **S: kullanıcıların parolalarını değiştirmeden kullanıcıların engellemek?**

  > **Y:** yalnızca bulut kullanıcıları için parola değişikliklerini engellenemez. Şirket içi kullanıcılar için belirlediğiniz **kullanıcı parolayı değiştiremez** seçili seçeneği. Seçili kullanıcıların parolalarını değiştiremezsiniz.
  >
  >

## <a name="password-management-reports"></a>Parola yönetimi raporları

* **S: parola yönetimi raporları göstermeyi veri ne kadar sürer?**

  > **Y:** veri 5-10 dakika içinde parola yönetimi raporları görünmelidir. Bazı durumlarda, bir saat görünmesi sürebilir.
  >
  >
* **S: parola yönetimi raporları filtre nasıl sağlayabilirsiniz?**

  > **Y:** parola yönetimi raporları filtrelemek için raporun en üstüne yakın sütun etiketlerini aşırı sağındaki küçük Büyüteç seçin. Daha zengin filtreleme yapmak istiyorsanız, raporun Excel'e indirin ve bir PivotTable oluşturun.
  >
  >
* **S: parola yönetimi raporlarda depolanan olayları sayısı nedir?**

  > **Y:** kadar 75,000 parola sıfırlama veya parola sıfırlama kayıt olayları 30 gün durum yeniden kapsayıcı parola yönetimi raporları depolanır. Daha fazla olay eklemek için bu sayıyı genişletmek için çalışıyoruz.
  >
  >
* **S: parola yönetimi raporları ne kadar geri dönün?**

  > **Y:** son 30 gün içinde oluştu Göster işlemleri parola yönetimi raporları. Şu an için bu verileri arşivlemek gereken yaparsanız raporları düzenli aralıklarla karşıdan yüklemek ve bunları ayrı bir konuma kaydedin.
  >
  >
* **S: parola yönetimi raporları görünebilir satır sayısının üst sınırını var mı?**

  > **Y:** Evet. Kullanıcı Arabiriminde gösterilen veya indirilir 75,000 satır en parola yönetimi raporları birini görünebilir.
  >
  >
* **S: parola sıfırlama veya raporlama verilerini kayıt erişmek için bir API var mı?**

  > **Y:** Evet. Veri akışı raporlama parola sıfırlama nasıl erişebileceğinizi öğrenmek için bkz: [parola sıfırlama raporlama olayları programlı olarak erişmek öğrenin](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Parola geri yazma

* **S: parola geri yazma arka planda nasıl çalışır?**

  > **Y:** makalesine bakın [parola geri yazma nasıl çalıştığını](active-directory-passwords-writeback.md) , etkinleştirdiğinizde neler bir açıklama için parola geri yazma ve sistem üzerinden veri akışını şirket içi ortamınıza geri.
  >
  >
* **S: parola geri yazma çalışma süresini? Olan var. bir eşitleme gecikme parola karma eşitlemesi ile gibi?**

  > **Y:** parola geri yazma anlık. Parola karma eşitlemesi temelde farklı çalışır zaman uyumlu bir ardışık düzen olur. Parola geri yazma, kendi parola sıfırlama başarısını hakkında gerçek zamanlı geri bildirim alma veya işlem değiştirmek kullanıcıların sağlar. Başarılı bir parola geri yazma için ortalama süre altında 500 ms ' dir.
  >
  >
* **S: şirket içi Hesabımı devre dışıysa, my bulut hesabı ve etkilenen erişimi nasıl nedir?**

  > **Y:** şirket içi Kimliğinizi devre dışıysa, bulut kimliği ve erişim ayrıca Azure AD Connect aracılığı sonraki eşitleme aralıkta devre dışı bırakılır. Varsayılan olarak, bu eşitleme her 30 dakikadır.
  >
  >
* **S: şirket içi Hesabımı bir şirket içi Active Directory parola ilkesi tarafından kısıtlı, SSPR parolamı değiştirdiğinizde, bu ilkeyi uyma mu?**

  > **Y:** Evet, SSPR kullanır ve şirket içi Active Directory parola ilkesi tarafından bağlıdır. Bu ilke, normal Active Directory etki alanı parola ilkesi yanı sıra, bir kullanıcıya hedeflenen tüm tanımlanan, hassas parola ilkeleri içerir.
  >
  >
* **S: hangi tür hesabı için parola geri yazma çalışıyor mu?**

  > **Y:** parola geri yazma, Federasyon için çalışır ve parola karması eşitlenen kullanıcılar.
  >
  >
* **S: parola geri yazma etki alanımın parola ilkelerini zorlamak mu?**

  > **Y:** Evet. Parola geri yazma özelliğini parola geçerlilik süresi, geçmiş, karmaşıklık, filtreleri ve yerel etki alanında parolaları bir yerde bırakabilecek herhangi bir kısıtlama zorlar.
  >
  >
* **S: parola geri yazma güvenli mi?  I bilgisayar korsanlarının saldırısına uğrarsa olmaz nasıl mutlaka?**

  > **Y:** Evet, parola geri yazma güvenlidir. Parola geri yazma hizmeti tarafından uygulanan güvenlik dört katmanları hakkında daha fazla bilgi edinmek için kullanıma [parola geri yazma güvenlik modeli](active-directory-passwords-writeback.md#password-writeback-security-model) bölümüne [parola geri yazma genel bakış](active-directory-passwords-writeback.md) makalesi.
  >
  >

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
