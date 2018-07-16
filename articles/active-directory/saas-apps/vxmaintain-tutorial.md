---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle vxMaintain | Microsoft Docs'
description: Azure Active Directory ve vxMaintain arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 6362bcb701b444c8cd71b270222ce4f87b4cc2e3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055867"
---
# <a name="tutorial-azure-active-directory-integration-with-vxmaintain"></a>Öğretici: Azure Active Directory vxMaintain ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile vxMaintain tümleştirme konusunda bilgi edinin.

Bu tümleştirme, birçok önemli avantaj sağlar. Şunları yapabilirsiniz:

- VxMaintain erişimi, Azure AD'de denetler.
- Otomatik olarak vxMaintain çoklu oturum açma (SSO) ile Azure AD hesaplarını kullanarak oturum açmak etkinleştirin.
- Tek bir merkezi konumda hesaplarınızı yönetin: Azure portalı.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile vxMaintain yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir vxMaintain SSO etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bir üretim ortamında kullanmamanızı öneririz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide özetler senaryo iki temel yapı taşları oluşur:

* Galeriden vxMaintain ekleme
* Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-vxmaintain-from-the-gallery"></a>Galeriden vxMaintain Ekle
Azure AD ile vxMaintain tümleştirmesini yapılandırmak için vxMaintain Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

Galeriden vxMaintain eklemek için aşağıdakileri yapın:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** düğmesi. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" bölmesi][2]
    
3. Bir uygulama eklemek için **tüm uygulamaları** iletişim kutusunda **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **vxMaintain**.

    !["Çoklu oturum açma modu" aşağı açılan listesi](./media/vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. Sonuç listesinden **vxMaintain**ve ardından **Ekle**.

    ![VxMaintain bağlantı](./media/vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD SSO vxMaintain, "Britta Simon." adlı bir test kullanıcı tabanlı kullanarak test etme

Çalışmak SSO için Azure AD, Azure AD kullanıcı vxMaintain karşılık gelen bilmek ister. Diğer bir deyişle, Azure AD kullanıcısı ile ilgili vxMaintain kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Bağlantı kurmak için vxMaintain atama **kullanıcı adı** değeri olarak Azure AD **kullanıcıadı** değeri.

Yapılandırma ve Azure AD SSO vxMaintain kullanarak test etmek için aşağıdaki yapı taşlarını tamamlayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Bu bölümde, Azure portalında Azure AD SSO etkinleştirme hem aşağıdakileri yaparak vxMaintain uygulamanıza SSO yapılandırın:

1. Azure portalında, üzerinde **vxMaintain** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    !["Çoklu oturum açma" komutu][4]

2. SSO, etkinleştirmek için **çoklu oturum açma modunu** aşağı açılan listesinden **SAML tabanlı oturum açma**.
 
    !["SAML tabanlı oturum açma" komutu](./media/vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. Altında **vxMaintain etki alanı ve URL'ler**, aşağıdakileri yapın:

    ![Etki alanı ve URL'ler bölüm vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. İçinde **tanımlayıcı** aşağıdaki söz dizimine sahip bir URL yazın: `https://<company name>.verisae.com`

    b. İçinde **yanıt URL'si** aşağıdaki söz dizimine sahip bir URL yazın: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Yanıt URL'si ve bunları gerçek tanımlayıcısıyla güncelleştirin. Değerlerini almak için iletişime geçin [vxMaintain Destek ekibine](https://www.hubspot.com/company/contact).
 
4. Altında **SAML imzalama sertifikası**seçin **meta veri XML**ve meta veri dosyası, bilgisayarınıza kaydedin.

    !["SAML imzalama sertifikası" bölümü](./media/vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. **Kaydet**’i seçin.

    ![Kaydet düğmesi](./media/vxmaintain-tutorial/tutorial_general_400.png)

6. Yapılandırmak için **vxMaintain** SSO, indirilen Gönder **meta veri XML** dosyasını [vxMaintain Destek ekibine](https://www.hubspot.com/company/contact).

> [!TIP]
> Uygulamasını ayarlama gibi önceki yönergeleri kısa bir sürümünü edinebilirsiniz [Azure portalında](https://portal.azure.com). Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesini ve sonra katıştırılmış erişin belgelerden **yapılandırma** bölümü. 
>
>Ekli belge özelliği hakkında daha fazla bilgi edinmek için [kurumsal uygulamalar için çoklu oturum açmayı yönetme](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, aşağıdakileri yaparak Azure portalında, Britta Simon test kullanıcısı oluşturun:

![Azure AD test kullanıcısı][100]

1. İçinde **Azure portalında**, sol bölmede seçin **Azure Active Directory** düğmesi.

    !["Azure Active Directory" düğmesi](./media/vxmaintain-tutorial/create_aaduser_01.png) 

2. Kullanıcıların bir listesini görüntülemek için Git **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
    
    !["Tüm kullanıcılar" bağlantısı](./media/vxmaintain-tutorial/create_aaduser_02.png)  
    **Tüm kullanıcılar** iletişim kutusu açılır. 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    ![Ekle düğmesi](./media/vxmaintain-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdakileri yapın:
 
    ![Kullanıcı iletişim kutusu](./media/vxmaintain-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** test kullanıcısı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından oluşturulduğu değeri Not **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-vxmaintain-test-user"></a>VxMaintain test kullanıcısı oluşturma

Bu bölümde, test kullanıcı Britta Simon vxMaintain oluşturun. VxMaintain platform kullanıcılar eklemek için çalışmak [vxMaintain Destek ekibine](https://www.hubspot.com/company/contact). SSO kullanmadan önce oluşturma ve kullanıcıları etkinleştirin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, test kullanıcısı Britta Simon, Azure SSO vxMaintain erişim vererek kullanacak şekilde etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Görünen ad listesinde test kullanıcısı][200] 

1. Azure portalında **uygulamaları** görünümü, Git **dizin** Görüntüle > **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Tüm uygulamalar" bağlantısı][201] 

2. İçinde **uygulamaları** listesinden **vxMaintain**.

    ![VxMaintain bağlantı](./media/vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202] 

4. Seçin **Ekle** ve daha sonra **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusundaki **kullanıcılar** listesinden **Britta Simon**ve ardından **seçin** düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD SSO yapılandırmanızı test.

Seçme **vxMaintain** kutucuk erişim Paneli'nde oturum size vxMaintain uygulamanızı otomatik olarak.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/vxmaintain-tutorial/tutorial_general_203.png

