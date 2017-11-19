---
title: "Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama | Microsoft Belgeleri"
description: "Klasik portalda Azure AD Uygulama Ara Sunucusu ile şirket içi uygulamaları bulutta yayımlayın."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 19f52181a2847ab52029adac4d58e402a76d5f30
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Klasik Azure portalı](active-directory-application-proxy-publish.md)

Azure AD Uygulama Proxy’si internet üzerinden erişilecek şirket içi uygulamalar yayımlayarak uzak çalışanları desteklemenize yardımcı olur. Bu noktaya kadar [Klasik Azure portalında Uygulama Proxy'si etkinleştirmiş olmanız](active-directory-application-proxy-enable.md) gerekir. Bu makalede, yerel ağınızda çalışan uygulamaları yayımlamaya ve ağınızın dışından güvenli uzaktan erişim sağlamaya ilişkin adımlar bulunur. Bu makaleyi tamamladıktan sonra uygulamayı kişiselleştirilmiş bilgiler veya güvenlik gereksinimleri ile yapılandırmaya hazır olursunuz.

> [!NOTE]
> Uygulama Ara Sunucusu özelliğini, yalnızca Azure Active Directory'nin Premium veya Basic sürümüne yükseltmeniz halinde kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md). Uygulama Proxy’sini kullanmak isterseniz [Azure portalında uygulama yayımlayabilirsiniz](application-proxy-publish-azure-portal.md).

## <a name="publish-an-app-using-the-wizard"></a>Sihirbazı kullanarak uygulama yayımlama
1. [Klasik Azure portalında](https://manage.windowsazure.com/) yönetici olarak oturum açın.
2. Active Directory'ye gidip Uygulama Ara Sunucusunu etkinleştirdiğiniz dizini seçin.
   
    ![Active Directory - simge](./media/active-directory-application-proxy-publish/ad_icon.png)
3. **Applications (Uygulamalar)** sekmesine tıklayın ve ardından ekranın altındaki **Add (Ekle)** düğmesine tıklayın.
   
    ![Uygulama ekleme](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)
4. **Publish an application that will be accessible from outside your network (Ağınızın dışından erişilebilecek olan bir uygulamayı yayımlama)** seçeneğini belirleyin.
   
    ![Ağınızın dışından erişilebilecek olan bir uygulamayı yayımlama](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)
5. Uygulamanız ile ilgili şu bilgileri sağlayın:
   
   * **Name (Ad)**: Uygulamanız için kolay ad. Bu ad, dizininizde benzersiz olmalıdır.
   * **Internal URL (İç URL)**: Uygulama Ara Sunucusu Bağlayıcısının özel ağınızdan uygulamaya erişmek için kullandığı adres. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde aynı sunucuda farklı siteleri yayımlayabilir; her biri için farklı bir ad ve erişim kuralları belirleyebilirsiniz.
     
     > [!TIP]
     > Bir yol yayımlarsanız uygulamanıza ilişkin tüm gerekli görüntüleri, betikleri ve stil sayfalarını içerdiğinden emin olun. Örneğin, uygulamanız https://yourapp/app üzerindeyse ve https://yourapp/media üzerindeki görüntüleri kullanıyorsa yolu https://yourapp/ olarak yayımlamanız gerekir.
     > 
     > 
   * **Ön Kimlik Doğrulama Yöntemi**: Uygulama Proxy’nizin uygulamanıza erişim izni vermeden önce kullanıcıları doğrulama yöntemi. Açılan menüdeki seçeneklerden birini belirleyin.
     
     * Azure Active Directory: Uygulama Proxy’si, kullanıcıları Azure AD'de oturum açmaya yönlendirir. Burada, kullanıcıların dizin ve uygulama izinlerine yönelik kimlik doğrulaması gerçekleştirilir.
     * Geçiş: Kullanıcıların uygulamaya erişmek için kimliklerini doğrulaması gerekmez.
     
     ![Uygulama özellikleri](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  
6. Sihirbazı tamamlamak için ekranın altındaki onay işaretine tıklayın. Uygulama artık Azure AD içinde tanımlıdır.

## <a name="assign-users-and-groups-to-the-application"></a>Uygulamaya kullanıcı ve grup atama
Kullanıcılarınızın yayımlanan uygulamanıza erişmeleri için onları ayrı ayrı veya gruplar halinde atamanız gerekir. (Kendinize de erişim atamayı unutmayın.) Atadığınız her kullanıcının Azure Basic veya sonraki sürüm için lisansı olmalıdır. Lisansları ayrı ayrı veya gruplara atayabilirsiniz. Daha fazla bilgi için bkz. [Uygulamaya kullanıcı atama](active-directory-applications-guiding-developers-assigning-users.md). 

Ön kimlik doğrulama gerektiren uygulamalar için kullanıcı atama işlemi, uygulamayı kullanma izni verir. Ön kimlik doğrulama gerektirmeyen uygulamalar için kullanıcı atama, kullanıcının uygulamaya erişim panelinden erişebileceği anlamına gelir.

1. Uygulama Ekleme sihirbazını tamamladıktan sonra uygulamanıza ilişkin Hızlı Başlangıç sayfasını görürsünüz. Uygulamaya kimlerin erişebildiğini yönetmek için **Kullanıcılar ve gruplar**'ı seçin.
   
    ![Uygulama Ara Sunucusu hızlı başlangıç kullanıcı atama - ekran görüntüsü](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)
2. Dizininizde belirli grupları arayın veya tüm kullanıcılarınızı gösterin. Sonuçları görüntülemek için onay işaretine tıklayın.
   
      ![Grup veya kullanıcı arama - ekran görüntüsü](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)
3. Bu uygulamaya atamak istediğiniz tüm kullanıcıları ve grupları seçip **Ata**'ya tıklayın. Bu eylemi onaylamanız istenir.

> [!NOTE]
> Tümleşik Windows Kimlik Doğrulaması Uygulamaları için yalnızca şirket içi Active Directory'nizden eşitlenen kullanıcıları ve grupları atayabilirsiniz. Microsoft hesabı ile oturum açan kullanıcılar ve konuklar, Azure Active Directory Uygulama Ara Sunucusu ile yayımlanan uygulamalar için atanamaz. Kullanıcılarınızın yayımladığınız uygulama ile aynı etki alanının parçası olan kimlik bilgileriyle oturum açtıklarından emin olun.
> 
> 

## <a name="test-your-published-application"></a>Yayımlanan uygulamanızı test etme
Uygulamanızı yayımladıktan sonra, yayımladığınız URL'ye giderek uygulamayı sınayabilirsiniz. Uygulamaya erişebildiğinizden, doğru şekilde işlediğinden ve her şeyin beklendiği gibi çalıştığından emin olun. Sorun varsa veya bir hata iletisi alırsanız [sorun giderme kılavuzunu](active-directory-application-proxy-troubleshoot.md) deneyin.

## <a name="configure-your-application"></a>Uygulamanızı yapılandırma
Yapılandırma sayfasında, yayımlanan uygulamaları değiştirebilir veya gelişmiş seçenekler belirleyebilirsiniz. Bu sayfada, ad değiştirerek veya bir logoyu karşıya yükleyerek uygulamanızı özelleştirebilirsiniz. Ayrıca, ön kimlik doğrulama yöntemi veya çok faktörlü kimlik doğrulaması gibi erişim kurallarını yönetebilirsiniz.

![Gelişmiş yapılandırma](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)

Uygulamalar, Azure Active Directory Uygulama Ara Sunucusu kullanılarak yayımladıktan sonra Azure AD'de Uygulamalar listesinde görünür ve onları burada yönetebilirsiniz.

Uygulamaları yayımladıktan sonra Uygulama Proxy hizmetlerini devre dışı bırakırsanız uygulamalara artık özel ağınızın dışından erişemezsiniz. Kullanıcılarınız uygulamalara normalde olduğu gibi şirket içinden erişmeye devam edebilir.

Bir uygulamayı görüntülemek ve erişilebilir durumda olduğundan emin olmak için uygulamanın adına çift tıklayın. Uygulama Ara Sunucusu hizmeti devre dışı bırakılır ve uygulama kullanılamazsa ekranın üstünde bir uyarı iletisi görünür.

Bir uygulamayı silmek için listeden bir uygulama seçin ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](active-directory-application-proxy-custom-domains.md)
* [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
* [Koşullu erişimi etkinleştirme](application-proxy-enable-remote-access-sharepoint.md)
* [Talepleri kullanan uygulamalarla çalışma](active-directory-application-proxy-claims-aware-apps.md)

En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](http://blogs.technet.com/b/applicationproxyblog/) göz atın

