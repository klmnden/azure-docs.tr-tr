---
title: "Oturum açma sayfanız Azure Active Directory'de özelleştirme | Microsoft Docs"
description: "Şirket Azure oturum açma sayfasına markası ekleyeceğiniz hakkında bilgi edinin"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 0855129c35c0c3d0f1814e8d29b6f3ae7d950db7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Hızlı Başlangıç: Şirket Azure AD'de, oturum açma sayfasına markası ekleme
Birçok şirket, karışıklığı önlemek için yönettikleri hizmetlerde ve web sitelerinde tutarlı bir genel görünüm uygulamak ister. Azure Active Directory (Azure AD), şirket Logonuzla ve özel renk düzenleri ile oturum açma sayfasının görünümünü özelleştirmenize olanak tanıyarak bu yeteneği sağlar. Oturum açma sayfası, Office 365 veya Azure AD kimlik sağlayıcınız olarak kullanan diğer web tabanlı uygulamalar için oturum açtığınızda görüntülenen sayfadır. Kimlik bilgilerinizi girmek için bu sayfayı ile etkileşim.

> [!NOTE]
> * Şirket markası yalnızca Premium ya da Azure AD Basic sürümüne yükselttiyseniz kullanılamıyor veya bir Office 365 lisansına sahip. Bir özellik lisans türü tarafından desteklenip desteklenmediğini bilgi edinmek için [Azure Active Directory fiyatlandırma bilgileri sayfası](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Azure Active Directory Premium ve Basic sürümleri, Azure Active Directory'nin dünya çapındaki örneğini kullanan Çin'deki müşterilerin kullanımına sunulmuştur. Azure Active Directory Premium ve Basic sürümleri, şu anda Çin'de 21Vianet tarafından işletilen Microsoft Azure hizmeti kapsamında desteklenmemektedir. Daha fazla bilgi için [Azure Active Directory Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/)'nda bizimle iletişime geçin.

## <a name="customizing-the-sign-in-page"></a>Oturum açma sayfasını özelleştirme

<!--You can customize the following elements on the sign-in page: <attach image>-->

Kullanıcılar bir kiracıya özgü URL gibi eriştiğinde marka özelleştirmeleri görünür Azure AD oturum açma sayfasında şirket [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Kullanıcıların www.office.com gibi genel bir URL ziyaret ettiğinizde, oturum açma sayfası şirket sistem kullanıcının kim olduğunu bilmek değil çünkü özelleştirmeler markası içermiyor. Kullanıcılar kendi kullanıcı Kimliğini girin veya kullanıcı kutucuğunu seçtikten sonra şirket markası gösterir.

> [!NOTE]
> * "Etkin" olarak görünmelidir etki alanı adınızı **etki alanları** yapılandırılmış markalama Azure portal kısmı. Daha fazla bilgi için bkz: [özel etki alanı ekleme](add-custom-domain.md).
> * Oturum açma sayfasında bulunan marka, Microsoft'un kişisel hesaplara yönelik oturum açma sayfasına aktarılmaz. Çalışanlarınıza veya iş konuklar kişisel bir Microsoft hesabıyla oturum açarsanız, oturum açma sayfası kuruluşunuzun markası yansıtmaz.


### <a name="banner-logo"></a>Başlık logosu 

Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Başlık logosu, oturum açma ve erişim paneli sayfasında görüntülenir.<br>Kullanıcı adı genellikle girildikten sonra müşteri kullanıcının kuruluş belirlendikten sonra oturum açma sayfasında, bu gösterir.  | Saydam JPG veya PNG<br>En fazla yükseklik: 36 piksel<br>En büyük genişliği: 245 piksel | Kuruluşunuzun logosu burada kullanın.<br>Saydam bir görüntü kullanın. Arka plan beyaz olacağını varsaymayın.<br>Logonuzun çevresindeki dolgunun görüntüde eklemeyin veya logonuzun orantısız küçük arar.

### <a name="username-hint"></a>Kullanıcı adı İpucu   
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu kullanıcı adı alanında ipucu metnini özelleştirir. | Maksimum 64 karakterlik Unicode metni<br>Yalnızca düz metin | Uygulamanıza oturum açmak için kuruluşunuz dışındaki Konuk kullanıcılar bekliyorsanız, bu ayarlamanızı değil öneririz.
            
### <a name="sign-in-page-text"></a>Oturum açma sayfası metni   
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu oturum açma formunun altında görünür ve Yardım masanıza ya da yasal bildirim telefon numarası gibi ek bilgileri iletişim kurmak için kullanılır. | Maksimum 256 karakterlik Unicode metni<br>Yalnızca düz metin (bağlantılar veya HTML etiketleri olmadan)   

### <a name="sign-in-page-image"></a>Oturum açma sayfası görüntüsü  
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu oturum açma sayfası arka planda görünür, görüntülenebilir alanı merkezine bağlantılı ölçekleme ve tarayıcı penceresini doldurmak için kırpma.    <br>Bu görüntü cep telefonları gibi dar ekranlarda gösterilmez.<br>Sayfa yüklendiğinde 0.55 opaklık siyah maskeyle bizim koduna göre bu görüntünün üzerinde uygulanır. | JPG veya PNG<br>Görüntü boyutları: 1920 x 1080 piksel<br>Dosya boyutu: &lt; 300 KB | <br>Görüntüleri kullanmak hiç burada güçlü konu odak. Donuk oturum açma formu bu görüntüyü center görünür ve görüntünün bir tarayıcı penceresi boyutuna bağlı olarak, herhangi bir bölümünü kapsar.<br>Dosya boyutu hızlı yükleme süreleri sağlamak olabildiğince küçük tutun. 

### <a name="background-color"></a>Arka plan rengi
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Bu renk, arka plan görüntüsü düşük bant genişliğine sahip bağlantılarda yerine kullanılır. | RGB renk onaltılık (örnek: #FFFFFF | Başlık logosu ya da kuruluş renginiz birincil rengini kullanarak öneririz.

### <a name="show-option-to-remain-signed-in"></a>Oturum açmış durumda kalmasına seçeneğini göster
Açıklama | Kısıtlamalar | Öneriler
------- | ------- | ----------
Azure AD oturum açma kullanıcı kapatın ve yeniden tarayıcısında açın, oturum açmış durumda kalmasına seçeneği sunar. Bu seçenek gizlemek için bunu kullanın.<br>Bunu ayarlamak için bu seçeneği, kullanıcılardan gizlemek için "Hayır". | &nbsp; | Bu, oturum yaşam etkilemez.<br>SharePoint Online ve Office 2010 özelliklerinden bazıları kullanıcıların oturum açmış durumda kalmasına seçmek yapamamasına bağlıdır. Bu gizli olarak ayarlarsanız, kullanıcılarınız oturum açmak için ek ve beklenmeyen komut istemlerini görebilirsiniz.

> [!NOTE]
> Tüm öğeler isteğe bağlıdır. Örneğin, hiçbir arka plan görüntüsü ile bir başlık logosu belirtirseniz oturum açma sayfası logonuzu ve hedef site (örneğin, Office 365) için arka plan resmi gösterir.

## <a name="add-company-branding-to-your-directory"></a>Şirket dizininize markası ekleme

1. Oturum [Azure portalı](https://portal.azure.com) dizini için genel yönetici olan bir hesapla.
2. Seçin **daha fazla hizmet**, girin **kullanıcılar ve gruplar** metin kutusuna ve ardından **Enter**.

   ![Açılış kullanıcı yönetimi](./media/customize-branding/user-management.png)
3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **şirket markası**.
4. Üzerinde **kullanıcılar ve gruplar - şirket markası** dikey penceresinde, select **Düzenle** komutu.

    ![Özel marka Düzenle](./media/customize-branding/edit-branding.png)
5. Özelleştirmek istediğiniz öğeleri değiştirin. Tüm öğeler isteğe bağlıdır.
6. **Kaydet** düğmesine tıklayın.

Bunu görünmesi marka oturum açma sayfasına yapılan değişiklikler bir saate kadar sürebilir.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Dile özgü şirket dizininize markası ekleme

1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. Seçin **kullanıcılar ve gruplar** metin kutusuna ve ardından **Enter**.

   ![Açılış kullanıcı yönetimi](./media/customize-branding/user-management.png)
3. Üzerinde **kullanıcılar ve gruplar** dikey penceresinde, select **şirket markası**.
4. Üzerinde **kullanıcılar ve gruplar - şirket markası** dikey penceresinde, select **dil eklemek** komutu.

    ![Dile özgü marka öğeleri ekleme](./media/customize-branding/add-language.png)
5. Özelleştirmek istediğiniz öğeleri değiştirin. Tüm öğeler isteğe bağlıdır.
6. **Kaydet** düğmesine tıklayın.

Bunu görünmesi marka oturum açma sayfasına yapılan değişiklikler bir saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç şirket Azure AD dizininizi markası ekleme öğrendiniz. 

Azure portalından Azure AD'de markalama şirket yapılandırmak için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Şirket markası yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 