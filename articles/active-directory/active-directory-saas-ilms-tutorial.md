---
title: "Öğretici: Azure Active Directory Tümleştirme ile iLMS | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile iLMS arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 22c72020200138e78835ed7dd2661f18b824c785
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Öğretici: Azure Active Directory Tümleştirme iLMS ile

Bu öğreticide, Azure Active Directory (Azure AD) ile iLMS tümleştirmek öğrenin.

İLMS Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- İLMS erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak Azure AD hesaplarına sahip (çoklu oturum açma) iLMS için açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme iLMS ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir iLMS çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden iLMS ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ilms-from-the-gallery"></a>Galeriden iLMS ekleme
Azure AD iLMS tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden iLMS eklemeniz gerekir.

**Galeriden iLMS eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **iLMS**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. Sonuçlar panelinde seçin **iLMS**, ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı iLMS sınayın.

Tekli çalışmaya oturum için Azure AD iLMS karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının iLMS ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** iLMS içinde.

Yapılandırma ve Azure AD çoklu oturum açma iLMS ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir iLMS test kullanıcısı oluşturma](#creating-an-ilms-test-user)**  - bir Britta Simon karşılık gelen her, Azure AD gösterimine bağlı iLMS içinde olması.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma iLMS uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile iLMS yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **iLMS** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. Üzerinde **iLMS etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, yapıştırma **tanımlayıcısı** değeri, kopyalamanız **hizmet sağlayıcısı** iLMS Yönetim Portalı'nda SAML ayarları bölümü.

    b. İçinde **yanıt URL'si** metin kutusuna, Yapıştır **uç noktası (URL)** , kopyalamanız değeri **hizmet sağlayıcısı** SAML ayarlarının aşağıdaki düzeni sahip iLMS Yönetim Portalı'nda bölümünü`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

    >[!Note]
    >Bu '123456' tanımlayıcısının bir örnek değeridir.

4. Denetleme **Göster Gelişmiş URL ayarları**, uygulamada yapılandırmak istiyorsanız **SP** modunda başlatılan:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, Yapıştır **uç noktası (URL)** , kopyalamanız değeri **hizmet sağlayıcısı** olarak iLMS Yönetim Portalı'nda SAML ayarları bölümü`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`     

5. JIT sağlama etkinleştirmek için belirli bir biçimde SAML onaylar iLMS uygulama bekler. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/4.png)
    
    Oluşturma **departman, bölge** ve **bölme** öznitelikleri ve iLMS içinde bu özniteliklerin adını ekleyin. Yukarıda gösterilen bu öznitelikler gereklidir.  

    > [!NOTE] 
    > Etkinleştirmek sahip **Un-recognized kullanıcı hesabı oluşturma** bu öznitelikleri eşlemek için iLMS içinde. Yönergeleri izleyerek [burada](http://support.inspiredelearning.com/customer/portal/articles/2204526) öznitelikleri yapılandırması hakkında bir fikir edinmek için.

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, yukarıdaki resimde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik adı | Öznitelik değeri |
    | ---------------| --------------- |    
    | bölme | User.Department |
    | Bölge | User.State |
    | Bölüm | User.jobtitle |

    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklatın **Tamam**

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. Farklı web tarayıcısı penceresinde oturum açın, **iLMS Yönetici portalı** yönetici olarak.

10. Tıklatın **SSO:SAML** altında **ayarları** sekmesini SAML Ayarları'nı açın ve aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/1.png) 

    a. Genişletme **hizmet sağlayıcısı** bölümü ve kopyalama **tanımlayıcısı** ve **uç noktası (URL)** değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/2.png) 

    b. Altında **kimlik sağlayıcısı** 'yi tıklatın **meta verileri içeri aktarma**.
    
    c. Seçin **meta veri** Azure Portalı'ndan ' ndan indirilen dosya **SAML imzalama sertifikası** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. JIT kaldırmak için iLMS hesapları oluşturmak için sağlama etkinleştirmek isteyip istemediğinizi-kullanıcıların tanıyacağı, aşağıdaki adımları izleyin:
        
       - Denetleme **beklemediğiniz tanınan bir kullanıcı hesabı oluşturma**.
       
       ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  Azure AD iLMS öznitelikleri ile harita öznitelikleri. Öznitelik sütununda öznitelik adı veya varsayılan değeri belirtin.

    e. Git **iş kuralları** sekmesinde ve aşağıdaki adımları gerçekleştirin: 
        
       ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/5.png)

       - Denetleme **Un-recognized bölgeler oluşturmak, bölümler ve Departmanlar** bölgeler, bölümler ve çoklu oturum açma, aynı anda zaten var olmadığından departmanlar oluşturmak için.
        
       - Denetleme **güncelleştirme kullanıcı profili sırasında oturum açma** kullanıcının profilini her çoklu oturum açma ile güncelleştirilip güncelleştirilmediğini belirtmek için. 
        
       - Varsa **"Güncelleştirme boş değerler için olmayan zorunlu alanlar, kullanıcı profili"** seçeneği seçiliyse, oturum açma üzerine boş isteğe bağlı profili alanları da bu alanlar için boş değerler içermesini kullanıcının iLMS profilini neden.
        
       - Denetleme **hata bildirim e-posta Gönder** ve hata bildirim e-posta almak istediğiniz kullanıcının e-posta girin.

11. ' I tıklatın **kaydetmek** düğmesini tıklatarak ayarları kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
    
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-an-ilms-test-user"></a>Bir iLMS test kullanıcısı oluşturma

Uygulama, süresi kullanıcı sağlama ve kimlik doğrulama kullanıcılar uygulamada otomatik olarak oluşturulduktan sonra hemen destekler. JIT çalışır, tıkladıysanız **Un-recognized kullanıcı hesabı oluşturma** iLMS Yönetici portalı SAML yapılandırma ayarı sırasında onay kutusu.

Bir kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:

1. İLMS şirket sitenize yönetici olarak oturum açın.

2. Tıklatın **"Kullanıcı kaydetme"** altında **kullanıcılar** sekmesini açmak için **kullanıcı Kaydet** sayfası. 
   
   ![Çalışanı ekleyin](./media/active-directory-saas-ilms-tutorial/3.png)

3. Üzerinde **"Kullanıcı kaydetme"** sayfasında, aşağıdaki adımları gerçekleştirin.

    ![Çalışanı ekleyin](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    a. İçinde **ad** metin kutusuna, ilk tür adı Britta.
   
    b. İçinde **Soyadı** metin kutusuna, soyadını Simon yazın.

    c. İçinde **e-posta kimliği** metin kutusuna, Britta Simon hesabı e-posta adresini yazın.

    d. İçinde **bölge** açılan listesinde, bölge için bir değer seçin.

    e. İçinde **bölme** açılan listesinde, bölme için değer seçin.

    f. İçinde **departmanı** açılan listesinde, departman için değer seçin.

    g. **Kaydet** düğmesine tıklayın.

    > [!NOTE] 
    > Seçerek kullanıcı kayıt posta gönderebilir **kayıt posta Gönder** onay kutusu.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta iLMS kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**İLMS için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **iLMS**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli iLMS parçasında tıklattığınızda, otomatik olarak iLMS uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

