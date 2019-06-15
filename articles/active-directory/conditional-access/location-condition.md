---
title: Konum koşulu, Azure Active Directory koşullu erişim nedir? | Microsoft Docs
description: Konum koşulu, bir kullanıcının ağ konumuna dayalı bulut uygulamalarınıza erişimi denetlemek için kullanmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.workload: identity
ms.date: 04/12/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 886118614427bea61f745e1ded28824b60225919
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112306"
---
# <a name="what-is-the-location-condition-in-azure-active-directory-conditional-access"></a>Konum koşulu, Azure Active Directory koşullu erişim nedir? 

İle [Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md), nasıl yetkili kullanıcılar denetleyebilir, bulut uygulamalarınızı erişebilirsiniz. Koşullu erişim ilkesinin konum koşulu kullanıcılarınızın ağ konumlarına erişim denetimleri ayarlarını bağlamasına olanak tanır.

Bu makalede konum koşulu yapılandırmak gereken bilgileri sağlar.

## <a name="locations"></a>Konumlar

Azure AD cihazları ve uygulamaları için çoklu oturum açma sağlar ve hizmetlere her yerden genel internet'te. Konum koşulu ile bir kullanıcının ağ konumuna dayalı bulut uygulamalarınıza erişimi denetleyebilirsiniz. Konum koşulu için yaygın kullanım örnekleri şunlardır:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektiren.
- Bir hizmetin belirli ülke veya bölgelerden erişen kullanıcılar için erişimi engelliyor.

Bir ağ konumu ya da temsil ettiğini belirtilen konum veya çok faktörlü kimlik doğrulaması güvenilen IP'ler için bir etiket konumdur.

## <a name="named-locations"></a>Adlandırılmış konumlar

Adlandırılmış konumlar ile mantıksal gruplandırmaları olan IP adresi aralıkları veya ülke ve bölge oluşturabilirsiniz.

Adlandırılmış konumlarınıza erişebileceğiniz **Yönet** koşullu erişim bölümü.

![Koşullu erişim adlandırılmış konumlar](./media/location-condition/02.png)

Adlandırılmış bir konuma aşağıdaki bileşenlere sahiptir:

![Yeni bir konum adlı oluşturun](./media/location-condition/42.png)

- **Ad** -adlandırılmış bir konuma görünen adı.
- **IP aralıklarını** -bir veya daha fazla IPv4 adres aralıklarını CIDR biçiminde. Bir IPv6 adres aralığı belirtilmesi desteklenmiyor.

   > [!NOTE]
   > IPv6 adresi rangess şu anda adlandırılmış bir konumda yer alamaz. Bu measn IPv6 aralıkları bir koşullu erişim ilkesinden dışarıda bırakılamaz.

- **Güvenilen konum olarak işaretle** -güvenilen bir konum belirtmek adlandırılmış bir konum için ayarlayabileceğiniz bir bayrak. Genellikle, güvenilen konumları BT departmanınız tarafından denetlenen ağ alanlardır. Koşullu erişim yanı sıra güvenilen adlandırılmış konumlar ayrıca Azure kimlik koruması ve Azure AD güvenlik raporları tarafından azaltmak için kullanılan [hatalı pozitif sonuçları](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations-1).
- **Ülkeler/bölgeler** -bu seçenek, bir veya daha fazla ülke veya bölge adlandırılmış bir konuma tanımlamak için seçmenize olanak sağlar.
- **Bilinmeyen alanları dahil et** -belirli bir ülke veya bölge için bazı IP adreslerini eşlenmedi. Bu seçenek, bu IP adresleri adlandırılmış bir konumda dahil edilip edilmeyeceğini seçmenize olanak tanır. Belirtilen konum kullanarak ilke bilinmeyen konumlara uygulanmasını gerektiren bu ayarı kullanın.

Adlandırılmış konumlar yapılandırabileceğiniz sayısı, Azure AD'de ilgili nesne boyutu tarafından sınırlanır. Kuruluşlarda en fazla 90 adlandırılmış konumlar yapılandırabilir, her 1200 IP aralıkları ile yapılandırılmış.

Koşullu erişim ilkesi, IPv4 ve IPv6 trafiği için geçerlidir. Şu anda adlandırılmış konumlar yapılandırılması için IPv6 aralıkları izin vermez. Bu sınırlama aşağıdaki durumlarda neden olur:

- Koşullu erişim ilkesi için özel IPv6 aralıkları hedeflenemez
- Koşullu erişim ilkesini belirli IPv6 aralıkları dışarıda tutulamaz

Bir ilke, "Herhangi bir yere" uygulamak için yapılandırılmışsa, IPv4 ve IPv6 trafiği için geçerli olur. Belirtilen ülke ve bölge için yapılandırılmış adlandırılmış konumlar yalnızca IPv4 adreslerini destekler. IPv6 trafiğidir yalnızca "Bilinmeyen alanları dahil et" seçeneğini seçtiyseniz dahil.

## <a name="trusted-ips"></a>Güvenilen IP'ler

Ayrıca, kuruluşunuzun yerel intranet temsil eden IP adresi aralıklarını yapılandırabilirsiniz [multi-Factor authentication hizmeti ayarlarını](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Bu özellik, 50 adede kadar IP adresi aralıklarını yapılandırmanızı sağlar. IP adresi aralıklarını CIDR biçimindedir. Daha fazla bilgi için [güvenilen IP'ler](../authentication/howto-mfa-mfasettings.md#trusted-ips).  

Güvenilen IP'ler yapılandırılmış varsa, bunlar olarak görünmesini **MFA güvenilen IP'ler** konum koşulu için konumları listesinde.

### <a name="skipping-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması atlanıyor

Çok faktörlü kimlik doğrulaması Hizmeti Ayarları sayfasında, Kurumsal intranet kullanıcıları seçerek belirleyebilirsiniz **Atla intranetimde bulunan Federasyon kullanıcılardan gelen istekleri için multi-Factor authentication**. Bu ayar gösteren şirket içi AD FS tarafından verilen, talep ağ verilecek güvenilen ve kurumsal ağ üzerinde olarak kullanıcıyı tanımlamak için kullanılır. Daha fazla bilgi için [koşullu erişim kullanarak güvenilen IP'ler özelliği etkinleştirmek](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Bu seçenek denetledikten sonra adlandırılmış konumu dahil olmak üzere **MFA güvenilen IP'ler** tüm ilkeler için bu seçenek ile uygulanır.

Oturum süreleri uzun ömürlü, mobil ve Masaüstü uygulamalar için koşullu erişim düzenli aralıklarla değerlendirilir. Varsayılan bir kez bir saattir. İç şirket ağına talep yalnızca ilk kimlik doğrulaması yapıldığı sırada verilen, güvenilen IP aralıkları listesini Azure AD'ye sahip olmayabilir. Bu durumda, kullanıcı hala şirket ağında olup olmadığını belirlemek daha zordur:

1. Kullanıcının IP adresi güvenilen IP aralıkları birinde olup olmadığını denetleyin.
2. İlk üç sekizlisinin aynı kullanıcının IP adresi IP adresi ilk kimlik doğrulaması için ilk üç sekizlisinin aynı eşleşip eşleşmediğini kontrol edin. İlk IP adresini karşılaştırılır iç kurumsal ağ talep kimlik doğrulaması ilk verildiği ve kullanıcı konumu doğrulandı.

Her iki adım başarısız olursa, bir kullanıcı artık bir güvenilen IP olarak değerlendirilir.

## <a name="location-condition-configuration"></a>Konum koşulu yapılandırma

Konum koşulu yapılandırdığınızda ayırt etmek için seçeneğiniz vardır:

- Herhangi bir yerde
- Tüm Güvenilen Konumlar
- Seçili konumlar

![Konum koşulu yapılandırma](./media/location-condition/01.png)

### <a name="any-location"></a>Herhangi bir yerde

Varsayılan olarak, seçerek **herhangi bir konuma** herhangi bir Internet adresi yani tüm IP adresleri için uygulanacak bir ilke neden olur. Bu ayar adlandırılmış konumu olarak yapılandırdığınız IP adreslerini sınırlı değildir. Seçtiğinizde, **herhangi bir konuma**, belirli konumlara bir ilkeden yine de hariç tutabilirsiniz. Örneğin, şirket ağı dışında tüm konumlara kapsamını ayarlamak için güvenilen konumları hariç tüm konumlara bir ilke uygulayabilirsiniz.

### <a name="all-trusted-locations"></a>Tüm Güvenilen Konumlar

Bu seçenek için geçerlidir:

- Güvenilen konum olarak işaretlenmiş tüm konumlar
- **MFA güvenilen IP'ler** (yapılandırılmışsa)

### <a name="selected-locations"></a>Seçili konumlar

Bu seçenek ile bir veya daha fazla adlandırılmış konumlar seçebilirsiniz. Bu ayar, uygulamak ile bir ilke için bir kullanıcı seçili konumlardan birini bağlanması gerekir. Tıkladığınızda **seçin** adlandırılmış ağlar listesi gösteren adlandırılmış ağ seçim denetim açar. Ağ konumu işaretli listede de gösterilir olarak güvenilir. Belirtilen konum adlı **MFA güvenilen IP'ler** çok faktörlü kimlik doğrulaması hizmeti ayar sayfasında yapılandırılabilir IP ayarları eklemek için kullanılır.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="when-is-a-location-evaluated"></a>Ne zaman bir konum olarak kabul edilir?

Koşullu erişim ilkeleri değerlendirilir olduğunda:

- Bir kullanıcı ilk kez bir web uygulaması, mobil veya masaüstü uygulamasına oturum açtığında.
- Modern kimlik doğrulaması kullanan bir mobil veya masaüstü uygulaması, yeni bir erişim belirteci almak için bir yenileme belirteci kullanır. Varsayılan olarak bu onay bir kez bir saattir.

Bu denetim için mobile anlamına gelir ve Masaüstü uygulamaları, modern kimlik doğrulaması kullanan bir saat içinde ağ konumunu değiştirme konumunda bir değişiklik algılanır. Modern kimlik doğrulaması kullanmayan mobil ve Masaüstü uygulamalar için her bir belirteç isteğinde ilke uygulanır. İstek sıklığını uygulama göre farklılık gösterebilir. Benzer şekilde, web uygulamaları için ilke ilk oturum açma işleminde uygulanır ve konumundaki web uygulaması oturumunun ömrü boyunca geçerlidir. Uygulamalar arasında oturum süreleri farklılıkları nedeniyle, ilke değerlendirmesi arasındaki süreyi de değişir. Uygulamanın yeni bir oturum açma belirteci, istekleri her zaman ilke uygulanır.

Varsayılan olarak, Azure AD üzerinden saatlik olarak bir belirteç verir. Bir saat içinde Kurumsal ağa taşıdıktan sonra modern kimlik doğrulaması kullanan uygulamalar için ilke uygulanmaz.

### <a name="user-ip-address"></a>Kullanıcının IP adresi

İlkesi değerlendirmesi içine kullanılan IP adresini, kullanıcının genel IP adresidir. Özel bir ağ cihazlarında için bu IP adresi kullanıcı cihazının intranet üzerindeki istemci IP'si değil, bu ağın genel internet'e bağlanmak için kullandığı adresidir.

> [!WARNING]
> Cihazınız yalnızca bir IPv6 adresi varsa, konum koşulu yapılandırılması desteklenmiyor.

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Toplu karşıya yükleme ve adlandırılmış konumları indirme

Oluşturma veya güncelleştirme adlandırılmış konumlar, toplu güncelleştirmeler için yükleme veya kullanabilirsiniz IP aralıklarını içeren bir CSV dosyalarını indirme. Karşıya yükleme IP aralıkları listesinde bu dosya ile değiştirir. Dosyanın her satırı CIDR biçiminde bir IP adresi aralığı içerir.

### <a name="cloud-proxies-and-vpns"></a>Bulut Ara sunucuları ve VPN

Bulutta barındırılan proxy ya da VPN çözümü kullandığınızda, bir ilke değerlendirmesi proxy IP adresi olduğu IP adresini Azure AD kullanır. Bir IP adresi faking bir yöntem sunmak şekilde güvenilir bir kaynaktan geldiği doğrulama olduğundan kullanıcının genel IP adresini içeren X-Forwarded-For (XFF) üst bilgisi kullanılmaz.

Bir yerde, bir etki alanına katılmış cihaz kullanılabilir zorunlu tutmak için kullanılan bir ilke veya içeriden bir bulut proxy olduğunda talebi AD fs'den.

### <a name="api-support-and-powershell"></a>API desteği ve PowerShell

API ve PowerShell henüz desteklenmiyor veya koşullu erişim ilkeleri adlandırılmış konumlar için.

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [gerektiren MFA belirli uygulamalar için Azure Active Directory koşullu erişim ile](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md).
