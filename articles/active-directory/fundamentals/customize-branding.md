---
title: Azure AD kiracınızın oturum açma sayfasını özelleştirme | Microsoft Docs
description: Azure oturum açma sayfasına şirket markası ekleme hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 07/20/2018
ms.author: lizross
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 7804d6b0d4a100997fb545e678458424dac6ceed
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39227313"
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Hızlı başlangıç: Azure AD'de oturum açma sayfanıza şirket markası ekleme
Birçok şirket, karışıklığı önlemek için yönettikleri hizmetlerde ve web sitelerinde tutarlı bir genel görünüm uygulamak ister. Azure Active Directory (Azure AD), oturum açma sayfasının görünümünü şirket logonuzla ve özel renk düzenleriyle özelleştirmenize olanak tanıyarak size bu özelliği sunar. Oturum açma sayfası, Office 365 gibi kimlik sağlayıcısı olarak Azure AD'yi kullanan web tabanlı uygulamalarda oturum açmak istediğinizde görüntülenir. Kimlik bilgilerinizi girmek için bu sayfayla etkileşim kurarsınız.

> [!NOTE]
> * Şirket markası yalnızca Azure AD için Premium veya Temel lisans satın aldıysanız ya da Office 365 lisansınız varsa kullanılabilir. Bir özelliğin lisans türünüz tarafından desteklenip desteklenmediğini öğrenmek için [Azure Active Directory fiyatlandırma bilgileri sayfasına](https://azure.microsoft.com/pricing/details/active-directory/) bakın.
> 
> * Azure AD Premium ve Temel sürümleri, Azure Active Directory'nin dünya çapındaki örneğini kullanan Çin'deki müşterilerin kullanımına sunulmuştur. Azure AD Premium ve Temel sürümleri, şu anda Çin'de 21Vianet tarafından işletilen Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için [Azure Active Directory Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/)'nda bizimle konuşun.

## <a name="customizing-the-sign-in-page"></a>Oturum açma sayfasını özelleştirme

<!--You can customize the following elements on the sign-in page: <attach image>-->

Şirket markası özelleştirmeleri kullanıcılar [*https://outlook.com/contoso.com*](https://outlook.com/contoso.com) gibi kiracıya özgü bir URL'ye eriştiğinde veya [*https://passwordreset.microsoftonline.com/?whr=contoso.com*](https://passwordreset.microsoftonline.com/?whr=contoso.com) gibi bir URL'de etki alanını değişkenini ilettiğinde Azure AD oturum açma sayfasında görünür.

Örneğin kullanıcılar www.office.com adresini ziyaret ettiğinde, kullanıcı henüz kimlik bilgilerini girmediğinden oturum açma sayfasında şirket markası özelleştirmeleri görünmez. Kullanıcı kimliği girildikten veya kullanıcı kutucuğu silindikten sonra şirket markası görünür.

> [!NOTE]
> * Etki alanı adının şirket markasını yapılandırdığınız Azure portalının **Etki Alanları** bölümünde "Etkin" olarak görünmesi gerekir. Daha fazla bilgi için bkz. [Özel etki alanı adı ekleme](add-custom-domain.md).
> * Oturum açma sayfasında bulunan marka, Microsoft'un kişisel hesaplara yönelik oturum açma sayfasına aktarılmaz. Çalışanlarınız veya şirket konuklarınız kişisel bir Microsoft hesabıyla oturum açtığında, oturum açma sayfaları kuruluşunuzun markasını yansıtmaz.


### <a name="banner-logo"></a>Başlık logosu 

Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Başlık logosu, oturum açma ve Erişim paneli sayfalarında görüntülenir.<br>Oturum açma sayfasında logo, kullanıcı adı girildikten sonra gösterilir. | Saydam JPG veya PNG<br>Maksimum yükseklik: 36 piksel<br>Maksimum genişlik: 245 piksel | Burada kuruluşunuzun logosunu kullanın.<br>Saydam bir görüntü kullanın. Arka planın beyaz olacağını düşünmeyin.<br>Logonuzun veya görüntünüzün etrafına iç boşluk eklemeyin, aksi halde logonuz orantısız bir şekilde küçük görünür.

### <a name="username-hint"></a>Kullanıcı adı ipucu   
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu seçenek, kullanıcı adı alanındaki ipucu metnini özelleştirir. | Maksimum 64 karakterlik Unicode metni<br>Yalnızca düz metin | Kuruluşunuzun dışındaki konuk kullanıcıların uygulamanızda oturum açmasını bekliyorsanız bu seçeneği ayarlamamanızı öneririz.
            
### <a name="sign-in-page-text"></a>Oturum açma sayfası metni   
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu seçenek, oturum açma formunun en altında görünür ve yardım masanızın telefon numarası veya yasal bildirim gibi ek bilgiler için kullanılabilir. | Maksimum 256 karakterlik Unicode metni<br>Yalnızca düz metin (bağlantılar veya HTML etiketleri olmadan)    

### <a name="sign-in-page-image"></a>Oturum açma sayfası görüntüsü  
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu seçenek oturum açma sayfasının arka planında görüntülenir, görünür alanın ortasına sabitlenir ve tarayıcı penceresini dolduracak şekilde ölçeklenir ve kırpılır.    <br>Cep telefonları gibi dar ekranlarda bu görüntü gösterilmez.<br>Sayfa yüklendiğinde bu görüntünün üzerine 0,55 opaklık değerine sahip siyah bir maske uygulanır. | JPG veya PNG<br>Görüntü boyutları: 1920x1080 piksel<br>Dosya boyutu: &lt; 300 KB | <br>Konu odağının güçlü olmadığı görüntüler kullanın. Opak oturum açma formu bu görüntünün ortasında görüntülenir ve tarayıcı penceresinin boyutuna bağlı olarak görüntünün herhangi bir bölümünü kaplayabilir.<br>Yükleme süresinin kısa olması için dosya boyutunu küçük tutun. 

### <a name="sign-in-page-background-color"></a>Oturum açma sayfası arka plan rengi
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu renk, düşük bant genişliği durumlarında arka plan görüntüsünün yerine kullanılır. | Onaltılık biçimde RGB rengi (örnek: #FFFFFF) | Başlık logosunun birincil rengini veya kurumsal renginizi kullanmanızı öneririz.

### <a name="square-logo-image"></a>Kare logo görüntü
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu görüntü yeni Enterprise Windows 10 bilgisayarlar için kurulum sırasında görüntülenir. Yeni iş bilgisayarının kurulumunu yapan çalışanlar için bir bağlam sunar. Görüntü iş cihazlarını dağıtmak için [Windows AutoPilot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) kullanan kiracılarda ve diğer Windows 10 deneyimlerindeki parola giriş sayfalarında görüntülenir. | Saydam PNG (tercih edilir) veya JPG<br>Görüntü boyutları: 240x240 piksel<br>Dosya boyutu: &lt; 10 KB | Burada kuruluşunuzun logosunu kullanın.<br> Saydam bir görüntü kullanın.<br>Arka planın beyaz olacağını düşünmeyin.<br>Logonuza veya görüntünüze iç boşluk eklemeyin, aksi halde logonuz orantısız bir şekilde küçük görünür.

### <a name="show-option-to-remain-signed-in"></a>Oturumu açık bırak seçeneğini göster
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Azure AD oturum açma deneyimi, kullanıcıya tarayıcıyı kapatıp yeniden açtıktan sonra oturumun açık kalması seçeneğini sunar. Bu ayar, bu seçeneği gizler.<br>Bu seçeneği kullanıcılardan gizlemek için **Hayır** seçeneğini belirleyin. | &nbsp; | Bu seçeneğin gizlenmesi, oturum ömrünü etkilemez.<br>SharePoint Online ve Office 2010’un bazı özellikleri kullanıcıların oturumun açık kalmasını seçebilmesine bağlıdır. Bu ayarı **Hayır** olarak ayarlarsanız kullanıcılarınız oturum açmaya yönelik ek ve beklenmeyen istemler görebilir.

> [!NOTE]
> Tüm öğeler isteğe bağlıdır. Örneğin arka plan görüntüsü olmayan bir başlık logosu belirtirseniz oturum açma sayfasında logonuz ve hedef site (örneğin Office 365) için belirlenen arka plan görüntüsü gösterilir.

## <a name="add-company-branding-to-your-directory"></a>Dizininize şirket markası ekleme

1. Kiracı için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. **Azure Active Directory** > **Şirket markası** > **Düzenle**'yi seçin.
  
  ![Özel markalamayı açma](./media/customize-branding/navigation-to-branding.png)
3. Özelleştirmek istediğiniz öğeleri değiştirin. Tüm öğeler isteğe bağlıdır.
  
  ![Özel markalamayı düzenleme](./media/customize-branding/edit-branding.png)
4. İşiniz bittiğinde **Kaydet**'i seçin.

Oturum açma sayfası markasında yaptığınız değişikliklerin görünmesi bir saate kadar sürebilir.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Dizininize dile özgü şirket markası ekleme

1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. **Azure Active Directory** > **Şirket markası** > **Yeni dil**'i seçin.
  
  ![Dile özgü markalama öğeleri ekleme](./media/customize-branding/add-language.png)
3. Özelleştirmek istediğiniz öğeleri değiştirin. Tüm öğeler isteğe bağlıdır.
4. İşiniz bittiğinde **Kaydet**'i seçin.

Oturum açma sayfası markasında yaptığınız değişikliklerin görünmesi bir saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Azure AD dizininize şirket markası eklemeyi öğrendiniz. 

Aşağıdaki bağlantıyı kullanarak Azure AD içindeki şirket markanızı Azure portaldan yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Şirket markası yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 