---
title: "Azure Active Directory kiracınızın için oturum açma sayfasını özelleştirme | Microsoft Docs"
description: "Şirket Azure oturum açma sayfasına markası ekleyeceğiniz hakkında bilgi edinin"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/19/2018
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 03a6b82f769ed9a36c5d3ff9934de75d1536e1ae
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Hızlı Başlangıç: Şirket Azure AD'de, oturum açma sayfasına markası ekleme
Birçok şirket, karışıklığı önlemek için yönettikleri hizmetlerde ve web sitelerinde tutarlı bir genel görünüm uygulamak ister. Azure Active Directory (Azure AD), şirket Logonuzla ve özel renk düzenleri ile oturum açma sayfasının görünümünü özelleştirmenize olanak tanıyarak bu yeteneği sağlar. Azure AD kimlik sağlayıcınız olarak kullanan web tabanlı uygulamalara Office 365 gibi oturum açtığında oturum açma sayfası görüntülenir. Kimlik bilgilerinizi girmek için bu sayfayı ile etkileşim.

> [!NOTE]
> * Şirket markası yalnızca Azure AD Premium veya Basic lisansı satın aldıysanız kullanılamıyor veya bir Office 365 lisansına sahip. Bir özellik lisans türü tarafından desteklenip desteklenmediğini bilgi edinmek için [Azure Active Directory fiyatlandırma bilgileri sayfası](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Azure AD Premium ve Basic sürümleri Azure Active Directory dünya çapındaki örneğini kullanan Çin müşteriler için kullanılabilir. Azure AD Premium ve Basic sürümleri şu anda Çin'de 21Vianet tarafından işletilen Azure hizmetinde desteklenmez. Daha fazla bilgi için bizimle iletişim kuran [Azure Active Directory Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>Oturum açma sayfasını özelleştirme

<!--You can customize the following elements on the sign-in page: <attach image>-->

Kullanıcılar bir kiracıya özgü URL gibi eriştiğinde marka özelleştirmeleri görünür Azure AD oturum açma sayfasında şirket [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Örneğin, kullanıcılar www.office.com ziyaret ettiğinizde, oturum açma sayfasında kullanıcının henüz kimlik bilgilerini girdiği değil çünkü özelleştirmeler markalama şirket göstermez. Bir kullanıcı kendi kullanıcı Kimliğini girer veya kullanıcı kutucuğunu seçer sonra şirket markası görüntüler.

> [!NOTE]
> * "Etkin" olarak görünmelidir etki alanı adınızı **etki alanları** yapılandırılmış markalama Azure portal kısmı. Daha fazla bilgi için bkz: [özel etki alanı ekleme](add-custom-domain.md).
> * Oturum açma sayfasında bulunan marka, Microsoft'un kişisel hesaplara yönelik oturum açma sayfasına aktarılmaz. Çalışanlarınıza veya iş konuklar kişisel bir Microsoft hesabıyla oturum açarsanız, oturum açma sayfası kuruluşunuzun markası yansıtmaz.


### <a name="banner-logo"></a>Başlık logosu 

Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Başlık logosu, oturum açma ve erişim paneli sayfasında görüntülenir.<br>Kullanıcı adı girildikten sonra oturum açma sayfasında logosu gösterir. | Saydam JPG veya PNG<br>En fazla yükseklik: 36 piksel<br>En büyük genişliği: 245 piksel | Kuruluşunuzun logosu burada kullanın.<br>Saydam bir görüntü kullanın. Arka plan beyaz olacağını varsaymayın.<br>Logonuzun çevresindeki dolgunun görüntüde eklemeyin veya logonuzun orantısız küçük arar.

### <a name="username-hint"></a>Kullanıcı adı İpucu   
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu seçenek, kullanıcı adı alanında ipucu metnini özelleştirir. | Maksimum 64 karakterlik Unicode metni<br>Yalnızca düz metin | Uygulamanıza oturum açmak için kuruluşunuz dışındaki Konuk kullanıcılar bekliyorsanız, bu seçeneği ayarlı değil öneririz.
            
### <a name="sign-in-page-text"></a>Oturum açma sayfası metni   
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu seçenek oturum açma formunun altında görünür ve Yardım masanıza ya da yasal bildirim telefon numarası gibi ek bilgileri iletişim kurmak için kullanılır. | Maksimum 256 karakterlik Unicode metni<br>Yalnızca düz metin (bağlantılar veya HTML etiketleri olmadan)    

### <a name="sign-in-page-image"></a>Oturum açma sayfası görüntüsü  
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu seçenek, oturum açma sayfası arka planda görünür görüntülenebilir alanı ölçekler ve tarayıcı penceresini doldurmak için kırpar merkezine sabitlenmiştir.    <br>Bu görüntü cep telefonları gibi dar ekranlarda gösterilmez.<br>Sayfa yüklendiğinde 0.55 opaklık siyah maskeyle bu görüntünün üzerinde uygulanır. | JPG veya PNG<br>Görüntü boyutları: 1920 x 1080 piksel<br>Dosya boyutu: &lt; 300 KB | <br>Görüntüleri kullanmak hiç burada güçlü konu odak. Donuk oturum açma formu bu görüntüyü center görünür ve tarayıcı penceresini boyutuna bağlı olarak görüntünün herhangi bir bölümünü kapsar.<br>Dosya boyutu hızlı yükleme süreleri sağlamak için küçük tutun. 

### <a name="sign-in-page-background-color"></a>Oturum açma sayfası arka plan rengi
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu renk, arka plan görüntüsü düşük bant genişliğine sahip bağlantılarda yerine kullanılır. | RGB renk onaltılık (örnek: #FFFFFF | Başlık logosu ya da kuruluş renginiz birincil rengini kullanarak öneririz.

### <a name="square-logo-image"></a>Kare logo görüntüsü
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu görüntü, Kurulum sırasında yeni Enterprise Windows 10 bilgisayarları için görünür. Yeni çalışmalarını PC ayarladığınızda çalışanlara bağlamı sağlar. Görüntü kullanan kiracılar için görüntülenen [Windows AutoPilot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) iş cihazlarını dağıtmak için ve diğer Windows 10 sayfalarında parola girişinde karşılaştığında. | Saydam PNG (önerilen) veya JPG<br>Görüntü boyutları: 240 x 240 piksel<br>Dosya boyutu: &lt; 10 KB | Kuruluşunuzun logosu burada kullanın.<br> Saydam bir görüntü kullanın.<br>Arka plan beyaz olacağını varsaymayın.<br>Doldurma logonuzun görüntüdeki eklemeyin veya logonuzun orantısız küçük arar.

### <a name="show-option-to-remain-signed-in"></a>Oturumu açık bırak seçeneğini göster
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Azure AD oturum açma kalmasına seçeneğini kapatın ve yeniden tarayıcısında açtığınız kullanıcı sağlar. Bu ayar, bu seçenek gizler.<br>Kümesine **Hayır** bu seçenek, kullanıcılardan gizlemek için. | &nbsp; | Seçenek gizleme oturum yaşam etkilemez.<br>SharePoint Online ve Office 2010 özelliklerinden bazıları kullanıcıların oturum açmış durumda kalmasına seçmek yapamamasına bağlıdır. Bu seçeneği belirlemek, **Hayır**, kullanıcılarınız oturum açmak için ek ve beklenmeyen komut istemlerini görebilirsiniz.

> [!NOTE]
> Tüm öğeler isteğe bağlıdır. Örneğin, hiçbir arka plan görüntüsü ile bir başlık logosu belirtirseniz oturum açma sayfası logonuzu ve hedef site (örneğin, Office 365) için arka plan resmi gösterir.

## <a name="add-company-branding-to-your-directory"></a>Şirket dizininize markası ekleme

1. Oturum [Azure AD Yönetim Merkezi](https://aad.portal.azure.com) Kiracı için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar ve gruplar** > **şirket markası** > **Düzenle**.
  
  ![Özel marka açma](./media/customize-branding/navigation-to-branding.png)
3. Özelleştirmek istediğiniz öğeleri değiştirin. Tüm öğeler isteğe bağlıdır.
  
  ![Özel marka Düzenle](./media/customize-branding/edit-branding.png)
5. İşiniz bittiğinde seçin **kaydetmek**.

Bunu görünmesi marka oturum açma sayfasına yapılan değişiklikler bir saate kadar sürebilir.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Dile özgü şirket dizininize markası ekleme

1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. Seçin **kullanıcılar ve gruplar** > **şirket markası** > **yeni dil**.
  
  ![Dile özgü marka öğeleri ekleme](./media/customize-branding/add-language.png)
5. Özelleştirmek istediğiniz öğeleri değiştirin. Tüm öğeler isteğe bağlıdır.
6. İşiniz bittiğinde seçin **kaydetmek**.

Bunu görünmesi marka oturum açma sayfasına yapılan değişiklikler bir saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç şirket Azure AD dizininizi markası ekleme öğrendiniz. 

Azure portalından Azure AD'de markalama şirket yapılandırmak için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Şirket markası yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 