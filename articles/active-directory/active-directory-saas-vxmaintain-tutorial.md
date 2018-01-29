---
title: "Öğretici: Azure Active Directory Tümleştirme ile vxMaintain | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile vxMaintain arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 66165b2586304f3726f5d712fb334fe67e2cd02b
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-azure-active-directory-integration-with-vxmaintain"></a>Öğretici: Azure Active Directory Tümleştirme vxMaintain ile

Bu öğreticide, Azure Active Directory (Azure AD) ile vxMaintain tümleştirmek öğrenin.

Bu tümleştirme birkaç önemli avantaj sağlar. Şunları yapabilirsiniz:

- VxMaintain erişimi, Azure AD'de denetler.
- Otomatik olarak vxMaintain çoklu oturum açma (SSO) ile Azure AD hesaplarını kullanarak oturum açmak etkinleştirin.
- Hesaplarınızı tek bir merkezi konumda yönetme: Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme vxMaintain ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- VxMaintain abonelik SSO'su etkin

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bir üretim ortamında kullanmamanızı öneririz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğretici özetlenmektedir senaryo iki ana yapı taşlarını oluşur:

* Galeriden vxMaintain ekleme
* Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-vxmaintain-from-the-gallery"></a>Galeriden vxMaintain Ekle
Azure AD ile vxMaintain tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden vxMaintain eklemeniz gerekir.

Galeriden vxMaintain eklemek için aşağıdakileri yapın:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** düğmesi. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Kurumsal uygulamalar" bölmesi][2]
    
3. Bir uygulama eklemek için **tüm uygulamaları** iletişim kutusunda **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **vxMaintain**.

    !["Modu tek oturum açma" açılan liste](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. Sonuçlar listesinde **vxMaintain**ve ardından **Ekle**.

    ![VxMaintain bağlantı](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırma ve Azure AD SSO "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı vxMaintain kullanarak test etme

Çalışmak SSO için Azure AD Azure AD kullanıcı vxMaintain karşılık gelen bilmesi gerekir. Diğer bir deyişle, Azure AD kullanıcısının ve karşılık gelen vxMaintain kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Bağlantı ilişkisi oluşturmak için vxMaintain atamak **kullanıcı adı** değeri olarak Azure AD **kullanıcıadı** değeri.

Yapılandırmak ve vxMaintain kullanarak Azure AD SSO sınamak için aşağıdaki yapı taşları tamamlayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO yapılandırın

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirmek hem aşağıdakileri yaparak vxMaintain uygulamanızda SSO yapılandırın:

1. Azure portalında üzerinde **vxMaintain** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    !["Çoklu oturum açmayı" komutu][4]

2. SSO, etkinleştirmek için **tek oturum açma modu** aşağı açılan listesinden, **SAML tabanlı oturum açma**.
 
    !["SAML tabanlı oturum açma" komutu](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. Altında **vxMaintain etki alanı ve URL'leri**, aşağıdakileri yapın:

    ![Etki alanı ve URL'ler bölüm vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. İçinde **tanımlayıcısı** kutusunda, aşağıdaki söz dizimini bir URL yazın:`https://<company name>.verisae.com`

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki söz dizimini bir URL yazın:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Gerçek tanımlayıcısı ile güncelleştirin ve URL yanıt. Değerleri almak için başvurun [vxMaintain destek ekibi](http://www.verisae.com/contact-us).
 
4. Altında **SAML imzalama sertifikası**seçin **meta veri XML**ve meta veri dosyası bilgisayarınıza kaydedin.

    !["SAML imzalama sertifikası" bölümü](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. **Kaydet**’i seçin.

    ![Kaydet düğmesi](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. Yapılandırmak için **vxMaintain** SSO, indirilen Gönder **meta veri XML** dosya [vxMaintain destek ekibi](http://www.verisae.com/contact-us).

> [!TIP]
> Uygulama ayarlama gibi önceki yönergeleri kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com). Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesini tıklatın ve sonra katıştırılmış erişim belgelerinden **yapılandırma** bölümü. 
>
>Embedded belgeler özelliği hakkında daha fazla bilgi için bkz: [Kurumsal uygulamaları için çoklu oturum açmayı yönetme](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümde, aşağıdakileri yaparak Azure portalında test kullanıcısı Britta Simon oluşturun:

![Azure AD test kullanıcısı][100]

1. İçinde **Azure portal**, sol bölmede seçin **Azure Active Directory** düğmesi.

    !["Azure Active Directory" düğmesi](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. Kullanıcıların bir listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
    
    !["Tüm kullanıcılar" bağlantı](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)  
    **Tüm kullanıcılar** iletişim kutusu açılır. 

3. Açmak için **kullanıcı** iletişim kutusunda **Ekle**.
 
    ![Ekle düğmesi](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdakileri yapın:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** test kullanıcısı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından oluşturulduğu değerini not edin **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-vxmaintain-test-user"></a>VxMaintain test kullanıcısı oluşturma

Bu bölümde, test kullanıcı Britta Simon vxMaintain oluşturun. VxMaintain platform kullanıcıları eklemek için çalışmak [vxMaintain destek ekibi](http://www.verisae.com/contact-us). SSO kullanmadan önce oluşturma ve kullanıcıları etkinleştirin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, test kullanıcısı Britta vxMaintain erişim vererek Azure SSO kullanılacak Simon etkinleştirin. Bunu yapmak için aşağıdakileri yapın:

![Görünen ad listesinde test kullanıcısı][200] 

1. Azure portalında **uygulamaları** görünümü, Git **Directory** Görünüm > **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Tüm uygulamaları" bağlantı][201] 

2. İçinde **uygulamaları** listesinde **vxMaintain**.

    ![VxMaintain bağlantı](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Seçin **Ekle** , daha sonra **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **kullanıcılar** listesinde **Britta Simon**ve ardından **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusunda **atamak**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD SSO yapılandırmanızı sınayın.

Seçme **vxMaintain** erişim paneli parçasında oturum size vxMaintain uygulamanızı otomatik olarak.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

