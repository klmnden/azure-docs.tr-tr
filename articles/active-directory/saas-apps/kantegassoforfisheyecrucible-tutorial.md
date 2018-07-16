---
title: 'Öğretici: Azure Active Directory için FishEye/Crucible Kantega SSO ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Kantega SSO için FishEye/Crucible arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 200330421233ed9fb3735a9f68d856efc99c1216
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052338"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a>Öğretici: Azure Active Directory için FishEye/Crucible Kantega SSO ile tümleştirme

Bu öğreticide, Kantega SSO FishEye/Crucible için Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Kantega SSO FishEye/Crucible için Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- FishEye/Crucible Kantega SSO erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Kantega SSO (çoklu oturum açma) FishEye/Crucible için Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi için FishEye/Crucible Kantega SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir Kantega SSO FishEye/Crucible çoklu oturum açma için etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden FishEye/Crucible için Kantega SSO ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-kantega-sso-for-fisheyecrucible-from-the-gallery"></a>Galeriden FishEye/Crucible için Kantega SSO ekleme
Azure AD'de Kantega SSO için FishEye/Crucible tümleştirmesini yapılandırmak için Kantega SSO için FishEye/Crucible Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Kantega SSO için FishEye/Crucible eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Kantega SSO için FishEye/Crucible**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_search.png)

5. Sonuçlar panelinde seçin **Kantega SSO için FishEye/Crucible**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma için "Britta Simon" adlı bir test kullanıcı tabanlı FishEye/Crucible Kantega SSO ile test edin.

Tek iş için oturum açma için Azure AD ne Kantega SSO FishEye/Crucible için karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Kantega SSO FishEye/Crucible için ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini FishEye/Crucible Kantega SSO, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma için FishEye/Crucible Kantega SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Kantega SSO için FishEye/Crucible test kullanıcısı oluşturma](#creating-a-kantega-sso-for-fisheyecrucible-test-user)**  - Kantega SSO için kullanıcı Azure AD gösterimini bağlı FishEye/Crucible Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve, Kantega SSO FishEye/Crucible uygulama için çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma için FishEye/Crucible Kantega SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Kantega SSO FishEye/Crucible için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_samlbase.png)

3. İçinde **IDP** modunda başlatıldı **FishEye/Crucible etki alanı ve URL'ler için Kantega SSO** bölümü aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. İçinde **SP** başlatılan modu, onay **Gelişmiş URL ayarlarını göster** ve aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan FishEye/Crucible eklentisi yapılandırma sırasında alınır.

5. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_400.png)
    
7. Farklı bir web tarayıcı penceresinde FishEye/Crucible şirket içi sunucunuza yönetici olarak oturum açın.

8. Dişlisine gelin ve tıklayın **eklentileri**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon1.png)

9. Sistem ayarları bölümü altında **yeni eklentileri bulma**. 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/add-on2.png)

10. Arama **Kantega SSO için Crucible** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon2.png)

11. Eklenti yükleme başlar. 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon33.png)

12. Yükleme tamamlandıktan sonra. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon34.png)

13. **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon35.png)

14. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.    

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon3.png)

15. İçinde **SAML** bölümü. Seçin **Azure Active Directory (Azure AD)** gelen **Ekle kimlik sağlayıcısı** açılır.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon4.png)

16. Abonelik düzeyi olarak **temel**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon5.png)

17. Üzerinde **uygulama özellikleri** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon6.png)

    a. Kopyalama **uygulama kimliği URI'si** olarak kullanın ve değerini **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** üzerinde **FishEye/Crucible etki alanı ve URL'ler için Kantega SSO** bölümü Azure Portalı'nda.

    b. **İleri**’ye tıklayın.

18. Üzerinde **meta veri içeri aktarma** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon7.png)

    a. Seçin **meta veri dosyası bilgisayarımda**ve Azure portalından indirdiğiniz meta veri dosyası karşıya yükleme.

    b. **İleri**’ye tıklayın.

19. Üzerinde **adı ve SSO konumunu** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon8.png)

    a. Kimlik sağlayıcısı adını eklemek **kimlik sağlayıcı adı** metin (ör. Azure AD).

    b. **İleri**’ye tıklayın.

20. İmzalama sertifikası doğrulayın ve tıklayın **sonraki**.  

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon9.png)

21. Üzerinde **balık kullanıcı hesaplarını** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon10.png)

    a. Seçin **kullanıcılar FishEye'nın iç dizinde gerekirse oluşturun** ve kullanıcılar için uygun Grup adını girin (olabilir birden çok yok. virgülle ayırarak grupları).

    b. **İleri**’ye tıklayın.

22. **Son**'a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon11.png)

23. Üzerinde **etki alanları için Azure AD bilinen** bölümünde, aşağıdaki adımları uygulayın: 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/addon12.png)

    a. Seçin **etki alanları** sayfasının sol panelden.

    b. Etki alanı adını girin **etki alanları** metin.

    c. **Kaydet**’e tıklayın.  

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforfisheyecrucible-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-kantega-sso-for-fisheyecrucible-test-user"></a>Kantega SSO için FishEye/Crucible test kullanıcısı oluşturma

Azure AD kullanıcılarının FishEye/Crucible oturum açmayı etkinleştirmek için bunlar FishEye/Crucible sağlanması gerekir. FishEye/Crucible Kantega SSO, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Crucible şirket içi sunucunuza yönetici olarak oturum açın.

2. Dişlisine gelin ve tıklayın **kullanıcılar**.

    ![Çalışan Ekle](./media/kantegassoforfisheyecrucible-tutorial/user1.png) 

3. Altında **kullanıcılar** sekmesinde bölüm **Kullanıcı Ekle**.

    ![Çalışan Ekle](./media/kantegassoforfisheyecrucible-tutorial/user2.png)

4. Üzerinde **yeni kullanıcı Ekle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/kantegassoforfisheyecrucible-tutorial/user3.png) 

    a. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.
    
    b. İçinde **görünen adı** metin türü Britta Simon gibi kullanıcının görünen adı.
    
    c. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.  

    e. İçinde **parolayı onayla** metin kutusu, kullanıcı parolasını yeniden girin.

    f. **Ekle**'ye tıklayın.   

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için FishEye/Crucible Kantega SSO için erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Kantega SSO için FishEye/Crucible atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Kantega SSO için FishEye/Crucible**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Kantega SSO erişim Paneli'nde FishEye/Crucible kutucuğa tıkladığınızda, otomatik olarak, Kantega SSO için FishEye/Crucible uygulamayı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforfisheyecrucible-tutorial/tutorial_general_203.png

