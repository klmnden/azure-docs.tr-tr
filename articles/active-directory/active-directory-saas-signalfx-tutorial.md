---
title: 'Öğretici: Azure Active Directory Tümleştirme ile SignalFx | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile SignalFx arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 9db01b4ea9a4f0d307db8bb9f8b6d6437a06815d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Öğretici: Azure Active Directory Tümleştirme SignalFx ile

Bu öğreticide, Azure Active Directory (Azure AD) ile SignalFx tümleştirmek öğrenin.

SignalFx Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SignalFx erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için SignalFx (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme SignalFx ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir SignalFx çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden SignalFx ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-signalfx-from-the-gallery"></a>Galeriden SignalFx ekleme
Azure AD SignalFx tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden SignalFx eklemeniz gerekir.

**Galeriden SignalFx eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **SignalFx**seçin **SignalFx** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde SignalFx](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SignalFx sınayın.

Tekli çalışmaya oturum için Azure AD SignalFx karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının SignalFx ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SignalFx ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SignalFx test kullanıcısı oluşturma](#create-a-signalfx-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı SignalFx sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma SignalFx uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile SignalFx yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **SignalFx** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_samlbase.png)

3. Üzerinde **SignalFx etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SignalFx etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://api.signalfx.com/v1/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > Önceki değerin gerçek değeri değil. Fiili yanıt, öğreticide daha sonra açıklanan URL ile değeri güncelleştirin.

4. SignalFx uygulaması SAML onaylar belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.   

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | User.FirstName          | User.givenName |
    | User.email          | User.Mail |
    | PersonImmutableID       | User.userPrincipalName    |
    | User.LastName       | User.surname    |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-signalfx-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/active-directory-saas-signalfx-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.
 
6. Üzerinde **SAML imzalama sertifikası** bölümünde, aşağıdaki adımları gerçekleştirin: 

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_certificate.png)

    a. Kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    b. Tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-signalfx-tutorial/tutorial_general_400.png)

8. Üzerinde **SignalFx yapılandırma** 'yi tıklatın **yapılandırma SignalFx** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![SignalFx yapılandırma](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_configure.png) 

9. SignalFx şirket sitenize yönetici olarak oturum.

10. Üst tıklatıldığında SignalFx içinde **tümleştirmeler** tümleştirmeler sayfasını açın.

    ![SignalFx tümleştirme](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_intg.png)

11. Tıklayın **Azure Active Directory** altında döşeme **oturum açma hizmetleri** bölümü.
 
    ![SignalFx saml](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_saml.png)

12. Tıklayın **yeni tümleştirme** ve altında **yükleme** sekmesinde, aşağıdaki adımları gerçekleştirin:
 
    ![SignalFx samlintgpage](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_azure.png)

    a. İçinde **adı** metin kutusuna, yeni bir tümleştirme ad ister **OurOrgName SAML SSO**.

    b. Kopya **tümleştirme kimliği** değer ve ile ilave **yanıt URL'si** gibi `https://api.signalfx.com/v1/saml/acs/<integration ID>` içinde **yanıt URL'si** , metin kutusuna **SignalFx etki alanı ve URL'leri** Azure portalı bölümünde.

    c. Tıklayın **dosyasını karşıya yükle** karşıya yüklemek için **Base64 ile kodlanmış sertifika** Azure portalında indirilen **sertifika** metin kutusu.

    d. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.

    e. İçinde **meta veri URL'sini** metin kutusuna, Yapıştır **uygulama Federasyon meta veri URL'sini** Azure portalından kopyalanan.

    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-signalfx-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-signalfx-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-signalfx-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-signalfx-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-signalfx-test-user"></a>SignalFx test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon içinde SignalFx adlı bir kullanıcı oluşturmaktır. SignalFx yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa SignalFx erişme denemesi sırasında oluşturulur.

Bir kullanıcı için SignalFx SAML SSO ilk kez oturum açtığında [SignalFx destek ekibi](mailto:kmazzola@signalfx.com) kimlik doğrulaması için bunlar aracılığıyla tıklatmalısınız bir bağlantı içeren bir e-posta gönderir. Bu, yalnızca ilk kez oturum açtığında gerçekleşir; sonraki oturum açma denemesi e-posta doğrulama gerektirmez.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [SignalFx destek ekibi](mailto:kmazzola@signalfx.com)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta SignalFx için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**SignalFx için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **SignalFx**.

    ![Uygulamalar listesinde SignalFx bağlantı](./media/active-directory-saas-signalfx-tutorial/tutorial_signalfx_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli SignalFx parçasında tıklattığınızda, otomatik olarak SignalFx uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-signalfx-tutorial/tutorial_general_203.png

