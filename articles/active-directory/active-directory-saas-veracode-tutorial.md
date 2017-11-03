---
title: "Öğretici: Azure Active Directory Tümleştirme ile Veracode | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Veracode arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d49349c5ae08e67d91e30967f3644623211823ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a>Öğretici: Azure Active Directory Tümleştirme Veracode ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Veracode tümleştirmek öğrenin.

Veracode Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Veracode erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Veracode (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Veracode ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Veracode çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Veracode Ekle
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-veracode-from-the-gallery"></a>Galeriden Veracode Ekle
Azure AD Veracode tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Veracode eklemeniz gerekir.

**Galeriden Veracode eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Veracode**seçin **Veracode** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Veracode](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Veracode sınayın.

Tekli çalışmaya oturum için Azure AD Veracode karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Veracode ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Veracode içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Veracode ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Veracode test kullanıcısı oluşturma](#create-a-veracode-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Veracode sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Veracode uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Veracode yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Veracode** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. Üzerinde **Veracode etki alanı ve URL'leri** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiş gibi tüm adımları gerçekleştirin. 

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. Bu bölümün amacı kullanıcıların Veracode için kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

    Özel öznitelik eşlemelerini eklemenizi gerektirir belirli bir biçimde SAML onaylar Veracode uygulamanızı bekler, **saml belirteci öznitelikleri** yapılandırma. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Öznitelikleri](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "öznitelikleri")

6. Gerekli öznitelik eşlemelerini eklemek için aşağıdaki adımları gerçekleştirin:

    | Öznitelik adı | Öznitelik değeri |
    |--- |--- |
    | FirstName |User.givenName |
    | Soyadı |User.surname |
    | E-posta |User.Mail |
    
    a. Her veri satırının için yukarıdaki tabloda **kullanıcı özniteliği eklemek**.
    
    ![Öznitelikleri](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "öznitelikleri")
    
    ![Öznitelikleri](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "öznitelikleri")
    
    b. İçinde **öznitelik adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. İçinde **öznitelik değeri** metin kutusuna, ilgili satır için gösterilen öznitelik değerini seçin.
    
    d. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. Üzerinde **Veracode yapılandırma** 'yi tıklatın **yapılandırma Veracode** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Veracode yapılandırma](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. Farklı web tarayıcısı penceresinde Veracode şirket sitenize yönetici olarak oturum açın.

10. Üstteki menüde tıklatın **ayarları**ve ardından **yönetici**.
   
    ![Yönetim](./media/active-directory-saas-veracode-tutorial/ic802911.png "Yönetim")

11. Tıklatın **SAML** sekmesi.

12. İçinde **kuruluş SAML ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yönetim](./media/active-directory-saas-veracode-tutorial/ic802912.png "Yönetim")
   
    a.  İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** Azure portalından kopyalanan.
    
    b. Azure Portalı'ndan indirilen sertifikanızı karşıya yüklemek için tıklayın **Dosya Seç**.
   
    c. Seçin **etkinleştirmek kendi kendine kayıt**.

13. İçinde **kendi kendine kayıt ayarları** bölümünde, aşağıdaki adımları uygulayın ve ardından **kaydetmek**:
   
    ![Yönetim](./media/active-directory-saas-veracode-tutorial/ic802913.png "Yönetim")
   
    a. Olarak **yeni kullanıcı etkinleştirme**seçin **Hayır etkinleştirmesinin**.
   
    b. Olarak **kullanıcı veri güncelleştirmeleri**seçin **tercih Veracode kullanıcı verilerini**.
   
    c. İçin **SAML özniteliği ayrıntılarını**, aşağıdakileri seçin:
      * **Kullanıcı rolleri**
      * **İlke Yöneticisi**
      * **Gözden Geçiren**
      * **Güvenlik sağlama**
      * **Executive**
      * **Gönderen**
      * **Oluşturan**
      * **Tüm taraması türleri**
      * **Takım üyelikleri**
      * **Varsayılan takım**

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-veracode-test-user"></a>Veracode test kullanıcısı oluşturma
Azure AD kullanıcıların Veracode oturum etkinleştirmek için bunların Veracode sağlanmalıdır. Veracode söz konusu olduğunda, sağlama otomatik bir görev haline gelmektedir. Sizin için eylem öğe yok. Kullanıcıları otomatik olarak ilk tek oturum açma girişimi sırasında gerekirse oluşturulur.

> [!NOTE]
> API Azure AD kullanıcı hesaplarını sağlamak için Veracode tarafından sağlanan veya herhangi diğer Veracode kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Veracode için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Veracode için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Veracode**.

    ![Uygulamalar listesinde Veracode bağlantı](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Veracode parçasında tıklattığınızda, otomatik olarak Veracode uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

