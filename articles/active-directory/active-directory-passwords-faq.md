---
title: "Sık sorulan sorular: Azure AD SSPR'yi | Microsoft Docs"
description: "Azure AD Self Servis parola hakkında sık sorulan sorular Sıfırla"
services: active-directory
keywords: "Active directory parola yönetimi, Azure AD parola yönetimi self servis parola sıfırlama"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 14c565bb67480681e1d398a0a21a11448f405e4e
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="password-management-frequently-asked-questions"></a>Parola yönetimi sık sorulan sorular

Parola sıfırlama için ilgili her şey için sık sorulan bazı sorular verilmiştir.

Azure AD hakkında genel bir sorunuz varsa ve Self Servis parola burada cevaplanıp değil, sıfırlama, sizin istemek Topluluk Yardım için üzerinde [Azure Ad forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Topluluk üyeleri mühendisleri, ürün yöneticileri, MVP ve arkadaşa BT uzmanları içerir.

Bu SSS, aşağıdaki bölümlere ayrılır:

* [**Parola sıfırlama kaydı hakkında sorular**](#password-reset-registration)
* [**Parola sıfırlama hakkında sorular**](#password-reset)
* [**Parola değiştirme hakkında sorular**](#password-change)
* [**Parola yönetimi hakkında sorular raporları**](#password-management-reports)
* [**Parola geri yazma hakkında sorular**](#password-writeback)

## <a name="password-reset-registration"></a>Parola sıfırlama kaydı

* **S: Kullanıcılarım kendi parola sıfırlama verileri kaydedebilirsiniz?**

  > **Y:** Evet, parola sıfırlama etkinleştirilmişse ve lisansına sahip olduğunuz sürece, kimlik doğrulama bilgilerini kaydetmek için http://aka.ms/ssprsetup parola sıfırlama kaydı portalında gidebilir. Kullanıcılar, http://myapps.microsoft.com adresinden erişim Paneli'nde gidip, profil sekmesini tıklatarak ve parola sıfırlama seçeneği kaydolun tıklatarak da kaydedebilirsiniz.
  >
  >
* **S: parola sıfırlama verileri Kullanıcılarım adına tanımlamak?**

  > **Y:** Evet, Azure AD Connect, PowerShell ile yapabilirsiniz [Azure portal](https://portal.azure.com), ya da Office Yönetim Portalı. Daha fazla bilgi için bkz: [Azure AD Self Servis parola sıfırlama tarafından kullanılan verileri](active-directory-passwords-data.md).
  >
  >
* **S: şirket içi güvenlik soruları verileri eşitlemek?**

  > **Y:** bu bugün mümkün değildir.
  >
  >
* **S: Kullanıcılarım veri diğer kullanıcılar bu verileri göremez yolla kaydedebilirsiniz?**

  > **Y:** Evet, kullanıcılar yalnızca genel Yöneticiler ve kullanıcı tarafından görülebilir özel kimlik doğrulama alanlara kaydedilen parola sıfırlama kayıt portalı kullanarak veri kaydettiğinizde.
  >
  >
* **S: Kullanıcılarım parola sıfırlamayı kullanabilmek için önce kaydedilmesi gerekiyor?**

  > **Y:** onların adına yeterli kimlik doğrulaması bilgilerini tanımlayın, Hayır, kullanıcıların kaydetmeniz gerekmez. Dizin uygun alanlarında depolanan verilerin düzgün biçimlendirilmiş olduğu sürece works parolasını sıfırlayın.
  >
  >
* **S: miyim eşitlemek veya kimlik doğrulama telefon, kimlik doğrulama e-posta veya diğer kimlik doğrulama telefon alanları Kullanıcılarım adına ayarlayın?**

  > **Y:** bu bugün mümkün değildir.
  >
  >
* **S: kayıt portalı Kullanıcılarım göstermek için hangi seçeneklerin nasıl biliyor?**

  > **Y:** yalnızca parola sıfırlama kayıt portalı kullanıcılarınız için etkinleştirdiğiniz yönelik seçenekleri gösterir. Bu seçenekler, dizinin Yapılandır sekmesi kullanıcı parolası sıfırlama ilkesini bölümü altında bulunur. Örneğin, bu güvenlik soruları etkinleştirmezseniz, ardından kullanıcıları için bu seçeneği kaydettirebilir anlamına gelir.
  >
  >
* **Kullanıcı ne zaman kabul s: kayıtlı?**

  > **Y:** kayıtlı olduğunda SSPR için kayıtlı kullanıcı kabul en az **sıfırlamak için gereken yöntemleri sayısı** , ayarladığınız [Azure portal](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Parola sıfırlama

* **S: parola sıfırlama bir e-posta, SMS veya telefon görüşmesi almayı ne kadar süre beklemesi gerekir?**

  > **Y:** SMS iletileri, e-posta ve telefon aramaları altında bir dakika içinde 5-20 saniye normal durum ulaşması.
    >Bu zaman dilimi içinde bildirim almazsanız:
        > * Gereksiz klasörünüzü kontrol edin.
        > * Onay numarası veya e-posta iletişim kurulan beklediğiniz biridir.
        > * Kimlik doğrulama verileri dizininde doğru biçimlendirilmiş denetleyin.
                >     * Örnek: "+ 1 4255551234" veya "user@contoso.com"
  >
  >
* **S: hangi dilde parola sıfırlama tarafından destekleniyor mu?**

  > **Y:** UI, parola sıfırlama SMS iletileri ve sesli aramalara yerelleştirilmiş aynı dillerde Office 365'te desteklenir.
  >
  >
* **S: ı my dizininde markalama kuruluş ayarladığınızda, parola sıfırlama deneyimi hangi kısımlarının markalı alma kullanıcının yapılandırma sekmesi?**

  > **Y:** parola sıfırlama portalı kuruluş logonuz gösterilir ve bir özel e-posta veya URL işaret edecek şekilde yönetici bağlantı kişi yapılandırmanıza olanak sağlar. Parola sıfırlama gönderilir e-posta kuruluşunuzun logosu, renkler, ad e-posta gövdesindeki içerir ve adından özelleştirilebilir.
  >
  >
* **S: kullanıcıların parolalarını sıfırlamaya Kullanıcılarım nereye hakkında nasıl eğitin?**

  > **Y:** bazı öneriler deneyin bizim [SSPR deployment makalesi](active-directory-passwords-best-practices.md#email-based-rollout)
  >
  >
* **S: bir mobil aygıttan bu sayfayı kullanın?**

  > **Y:** Evet, bu sayfayı mobil cihazlarda çalışır.
  >
  >
* **S: kullanıcıların parolalarını sıfırladığınızda kilidini açma yerel active directory hesaplarını destekler mi?**

  > **Y:** parolalarını sıfırladığınızda Evet, bir kullanıcının parolasını sıfırlar ve parola geri yazma dağıtılan Azure AD Connect'i kullanarak, bu kullanıcı hesabının kilidi otomatik olarak kaldırılır.
  >
  >
* **S: parola sıfırlama doğrudan my kullanıcının masaüstü oturum açma deneyimi nasıl tümleştirebilir miyim?**

  > **Y:** bir Azure AD Premium müşterisiyseniz, hiçbir ek ücret Microsoft Identity Manager yükleyin ve bu gereksinimi karşılamak için şirket içi parola sıfırlama çözümünü dağıtın.
  >
  >
* **S: farklı yerel ayarlar için farklı güvenlik soruları ayarlamak?**

  > **Y:** bu bugün mümkün değildir.
  >
  >
* **S: kaç sorular için güvenlik soruları kimlik doğrulaması seçeneği biz yapılandırabilir miyim?**

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
* **S: May bir kullanıcı kaydı aynı Güvenlik sorusu birden çok kez?**

  > **Y:** bir kullanıcı belirli bir sorunun kaydeder sonra Hayır, bunlar bu soruyu ikinci kez kayıt.
  >
  >
* **S: Güvenlik sorusu kaydı için en az bir sınır ayarlamak ve sıfırlamak mümkün mü?**

  > **Y:** kaydı ve sıfırlama için başka bir sınır Evet, ayarlanabilir. 3-5 güvenlik soruları kaydı için gerekli olabilir ve 3-5 sıfırlama için gerekli olabilir.
  >
  >
* **S: my ilkesi kullanıcıların sıfırlama için güvenlik soruları kullanmasını gerektirecek şekilde yapılandırılmış, ancak Azure Yöneticiler farklı şekilde yapılandırılmış gibi görünüyor.**

  > **Y:** bu beklenen bir davranıştır. Microsoft Azure Yönetici rolü için güçlü varsayılan iki ağ geçidi parolası sıfırlama ilkesini zorlar. Bu, yöneticilerin güvenlik soruları kullanarak devre dışı bırakır. Bu ilke hakkında daha fazla bilgi makalesinde bulunabilir [parola ilkeleri ve Azure Active Directory'de kısıtlamaları](active-directory-passwords-policy.md#administrator-password-policy-differences)
  >
  >
* **S: nasıl bir kullanıcı birden çok sıfırlamak için gereken soru sayısını kaydedildiyse, güvenlik soruları sıfırlaması sırasında seçilir?**

  > **Y:** N güvenlik soruları rastgele bir kullanıcı kayıtlı, burada N sorular toplam sayı dışında seçili **sıfırlamak için gereken soru sayısını**. Örneğin, bir kullanıcı 5 güvenlik soruları kayıtlı sahip, ancak yalnızca 3'ü sıfırlamak için gereklidir, 5 3 rastgele seçilir ve Sıfırla'sunulan. Kullanıcı sorulara yanıtlar yanlış alırsa, soru sözcüğüne önlemek için seçim işlemi oluşana.
  >
  >
* **S: kullanıcıları parola sıfırlama birçok kez kısa süre içinde denemelerini önlemek?**

  > **Y:** Evet, kötüye kullanımdan korumak amacıyla parola sıfırlama yerleşik güvenlik özellikleri vardır. Kullanıcılar yalnızca 5 parola sıfırlama girişimlerine 24 saat kilitlenmelerini önce bir saat içinde deneyebilirsiniz. Kullanıcılar yalnızca telefon numarası için 24 saat kilitlenmelerini önce bir saat içinde 5 kez doğrulamak deneyebilirsiniz. Kullanıcılar, 24 saat kilitlenmelerini önce 5 kez bir saat içinde tek kimlik doğrulama yöntemini yalnızca deneyebilirsiniz.
  >
  >
* **S: için ne kadar e-posta ve SMS bir kerelik geçiş kodu geçerli misiniz?**

  > **Y:** parola sıfırlama için oturum yaşam süresi 15 dakikadır. Parola sıfırlama işlemini baştan kullanıcının parolasını sıfırlamak için 15 dakika vardır. Bu süre dolduktan sonra e-posta ve SMS bir kerelik geçiş kodu geçersiz.
  >
  >
* **S:, parola sıfırlama kullanıcıların engellemek?**

  > **Y:** Self Servis parola sıfırlamayı etkinleştirmek için bir grubu kullanıyorsanız, Evet, bunları bu yeteneği sağlar grubundan kaldırabilirsiniz.
  >
  >

## <a name="password-change"></a>Parola değiştirme

* **S: kullanıcılar parolalarını değiştirmek için nereye?**

  > **A:** kullanıcıların parolalarını değişebilir gördüğünüz her yerde bunlar kendi profil resmi veya simgesini (sağ üst köşedeki ister kendi [Office 365](https://portal.office.com) veya [erişim paneli](https://myapps.microsoft.com) karşılaştığında. Kullanıcıların parolalarını gelen değişebilir [erişim paneli profili sayfasını](https://account.activedirectory.windowsazure.com/r#/profile). Kullanıcıların parolalarının süresinin dolduğu parolalarını Azure AD oturum açma ekranı otomatik olarak değiştirmek için de istenebilir. Son olarak, kullanıcılar gidin [Azure AD parola değişikliği Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) doğrudan parolalarını değiştirmek istiyorsanız.
  >
  >
* **S: şirket içi parolalarını sona erdiğinde Kullanıcılarım Office Portalı'nda bildirilir?**

  > **Y:** Buradaki yönergeleri izleyerek ADFS kullanıyorsanız, bu bugün mümkündür: [ADFS ile parola ilkesi talep gönderme](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Parola karma eşitlemesi kullanıyorsanız, bu bugün mümkün değildir. Bu durum, bize deneyimleri bulut için süre sonu bildirimleri göndermek mümkün değildir biz parola ilkeleri şirket içi, eşitleme değil çünkü. Her iki durumda da için mümkündür [parolaları PowerShell kullanarak dolmak üzere kullanıcıları bildir](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **S: kullanıcıların parolalarını değiştirmeden kullanıcıların engellemek?**

  > **Y:** bu olamaz engellenmesi yalnızca bulut kullanıcıları için. Şirket içi kullanıcılar için belirlediğiniz `User cannot change password` denetlenir ve bu kullanıcıların parolalarını değiştirmesi mümkün olmayacaktır.
  >
  >

## <a name="password-management-reports"></a>Parola yönetimi raporları

* **S: parola yönetimi raporları göstermeyi veri ne kadar sürer?**

  > **Y:** veri 5-10 dakika içinde parola yönetimi raporları görünmelidir. Bunu sürebilir saate görünmesi bazı örnekler.
  >
  >
* **S: parola yönetimi raporları filtre nasıl sağlayabilirsiniz?**

  > **Y:** sütun etiketleri, raporun en üstüne yakın aşırı sağındaki küçük Büyüteç'i tıklatarak parola yönetimi raporları filtreleyebilirsiniz. Daha zengin filtreleme yapmak istiyorsanız, excel ve Özet Tablo oluşturmak için raporu indirebilirsiniz.
  >
  >
* **S: olayların maksimum sayısını nedir depolanır parola yönetimi raporlarında?**

  > **Y:** olayları parola yönetimi raporlarda depolanan 75,000 parola sıfırlama veya parola sıfırlama kaydı en fazla 30 güne kadar geri kapsayıcı.  Daha fazla olay eklemek için bu sayıyı genişletmek için çalışıyoruz.
  >
  >
* **S: parola yönetimi raporları ne kadar geri dönün?**

  > **Y:** son 30 gün içinde yürütülen Göster işlem parola yönetimi raporları. Şu an için bu verileri arşivlemek gereken yaparsanız raporları düzenli aralıklarla karşıdan yüklemek ve bunları ayrı bir konuma kaydedin.
  >
  >
* **S: parola yönetimi raporları görünebilir satır sayısının üst sınırını var mı?**

  > **Y:** Evet, en fazla 75,000 satırları parola yönetimi raporları birini görünebilir mi gösterildikleri kullanıcı Arabiriminde veya karşıdan yükleniyor.
  >
  >
* **S: parola sıfırlama veya raporlama verilerini kayıt erişmek için bir API var mı?**

  > **Y:** Evet, veri akışı raporlama parola sıfırlama nasıl erişebileceğinizi öğrenmek için aşağıdaki belgelere bakın.  [Parola sıfırlama raporlama olayları programlı olarak erişmek öğrenin](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Parola geri yazma

* **S: parola geri yazma arka planda nasıl çalışır?**

  > **Y:** bkz [parola geri yazma nasıl çalıştığını](active-directory-passwords-writeback.md) , parola geri yazma ve veri sistem üzerinden şirket içi ortamınıza geri akışını sağlayan bir açıklama ne gerçekleşir.
  >
  >
* **S: parola geri yazma çalışma süresini?  Parola karma eşitlemesi ile gibi bir eşitleme gecikme var mı?**

  > **Y:** parola geri yazma anlık. Parola karma eşitlemesi temelde farklı çalışır zaman uyumlu bir ardışık düzen olur. Parola geri yazma, kendi parola sıfırlama başarısını hakkında gerçek zamanlı geri bildirim alma veya işlem değiştirmek kullanıcıların sağlar. Başarılı bir parola geri yazma için ortalama süre altında 500 ms ' dir.
  >
  >
* **S: şirket içi Hesabımı devre dışıysa, my bulut hesabı/erişimi nasıl etkileniyor mu?**

  > **Y:** şirket içi Kimliğinizi devre dışıysa, bulut kimliği/erişim de devre dışı bırakılacak bir sonraki eşitleme aralıkta AAD Connect yoluyla varsayılan olarak 30 dakikada budur.
  >
  >
* **S: şirket içi Hesabımı bir şirket içi Active Directory parola ilkesi tarafından kısıtlı, SSPR parolayı değiştirdiğinizde, bu ilkeyi uyma mu?**

  > **Y:** Evet, SSPR kullanır ve şirket içi tarafından uyduğundan AD parola ilkesi, belirli bir kullanıcıya hedeflenen hassas parola ilkelerini tanımlanan tipik AD etki alanı parola ilkesi, olarak da dahil olmak üzere.
  >
  >
* **S: hangi tür hesabı için parola geri yazma çalışıyor mu?**

  > **Y:** parola geri yazma federe ve parola karma eşitlenen kullanıcılar için çalışır.
  >
  >
* **S: parola geri yazma etki alanımın parola ilkelerini zorlamak mu?**

  > **Y:** Evet, parola geri yazma özelliğini parola geçerlilik süresi, geçmiş, karmaşıklık, filtreleri ve yerleştirdiğiniz parolaları bir yerde yerel etki alanınızda kısıtlama zorlar.
  >
  >
* **S: parola geri yazma güvenli mi?  I bilgisayar korsanlarının saldırısına uğrarsa olmaz nasıl mutlaka?**

  > **Y:** Evet, parola geri yazma güvenlidir. Parola geri yazma hizmeti tarafından uygulanan güvenlik dört katmanları hakkında daha fazla bilgi edinmek için kullanıma [parola geri yazma güvenlik modeli](active-directory-passwords-writeback.md#password-writeback-security-model) parola geri yazma nasıl çalıştığını, bölüm.
  >
  >

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisans ile ilgili sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
