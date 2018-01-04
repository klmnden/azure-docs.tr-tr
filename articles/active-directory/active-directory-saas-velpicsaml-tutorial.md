---
title: "Öğretici: Azure Active Directory Tümleştirme Velpic SAML ile | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Velpic SAML arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: e4f7e4c9e960450f0024cd7ca35bd3808d31ee19
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Öğretici: Azure Active Directory Tümleştirme Velpic SAML ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Velpic SAML tümleştirme öğrenin.

Azure AD ile Velpic SAML tümleştirme ile aşağıdaki avantajları sağlar:

- Velpic SAML erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Velpic SAML açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure Yönetim Portalı'nı yönetme

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme Velpic SAML ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Velpic SAML çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Bu gerekli olmadığı sürece, üretim ortamınızın kullanmamanız gerekir.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Velpic SAML ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-velpic-saml-from-the-gallery"></a>Galeriden Velpic SAML ekleme
Azure AD Velpic SAML tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Velpic SAML eklemeniz gerekir.

**Galeriden Velpic SAML eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **Ekle** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Velpic SAML**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. Sonuçlar panelinde seçin **Velpic SAML**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırmanız ve Velpic SAML ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD Velpic SAML karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Velpic SAML ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Velpic SAML içinde.

Yapılandırma ve Azure AD çoklu oturum açma Velpic SAML ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Velpic SAML test kullanıcısı oluşturma](#creating-a-velpic-saml-test-user)**  - Britta Simon, karşılık gelen her, Azure AD gösterimine bağlı Velpic SAML sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure Yönetim Portalı'nda etkinleştirin ve çoklu oturum açma Velpic SAML uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Velpic SAML ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Velpic SAML** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda, olarak **modu** seçin **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. Ayrıntılar girin **Velpic SAML etki alanı ve URL'leri** bölüm -

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. İçinde **oturum açma URL'si** metin değeri olarak yazın:`https://<sub-domain>.velpicsaml.net`

    b. İçinde **tanımlayıcısı** metin kutusuna, Yapıştır **'Çoklu oturum açma URL'si'** değeri`https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Oturum açma URL'si Velpic SAML ekibi tarafından sağlanacak ve tanımlayıcı değeri Velpic SAML tarafında SSO eklentisi yapılandırırken kullanılabilecek unutmayın. Bu değer Velpic SAML uygulama sayfadan kopyalayın ve burada yapıştırın gerekir.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. Velpic SAML yapılandırma bölümünde Velpic SAML yapılandırma oturum açma penceresini açmak için Yapılandır'ı tıklatın. SAML varlık kimliği hızlı başvuru bölümünden kopyalayın.

7. Farklı web tarayıcısı penceresinde Velpic SAML şirket sitenize yönetici olarak oturum açın.

8. Tıklayın **Yönet** sekmesinde ve Git **tümleştirme** tıklayın gereken bölüm **eklentileri** oturum açmak için yeni eklenti oluşturmak için düğmesi.

    ![Eklentisi](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. Tıklayın **'Eklenti ekleme'** düğmesi.
    
    ![Eklentisi](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. Tıklayın **SAML** döşeme eklentisi ekleyin sayfasında.
    
    ![Eklentisi](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. Yeni SAML eklentisini adını girin ve tıklayın **'Ekle'** düğmesi.

    ![Eklentisi](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. Ayrıntılar aşağıdaki gibi girin:

    ![Eklentisi](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    a. İçinde **adı** metin kutusuna, SAML eklentisini adını yazın.

    b. İçinde **veren URL'si** metin kutusuna, Yapıştır **SAML varlık kimliği** , kopyalanan **yapılandırma oturum açma** Azure portalının penceresi.

    c. İçinde **sağlayıcısı meta verileri yapılandırma** Azure Portalı'ndan indirilen meta veri XML dosyasını karşıya yükleyin.

    d. Yalnızca zaman etkinleştirerek sağlama SAML etkinleştirmeyi seçebilirsiniz **'Otomatik oluştur yeni kullanıcılar'** onay kutusu. Bir kullanıcı Velpic yok ve bu bayrak etkin değil, Azure oturum açma başarısız olur. Bayrak etkinleştirilirse kullanıcı otomatik olarak Velpic oturum açma sırasında sağlanacak. 

    e. Kopya **tek oturum açma URL'si** metinden kutusuna ve Azure portalında yapıştırın.
    
    f. **Kaydet** düğmesine tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure Yönetim Portalı'nda bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcıların listesini görüntülemek için.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="creating-a-velpic-saml-test-user"></a>Velpic SAML test kullanıcısı oluşturma

Bu adım yalnızca zaman sağlama kullanıcı uygulamayı desteklediği genellikle gerekli değildir. Otomatik kullanıcı sağlamayı etkinleştirilmemişse, el ile kullanıcı oluşturma aşağıda açıklandığı gibi yapılabilir.

Velpic SAML şirket sitenizin yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:
    
1. Yönet sekmesini tıklatın ve kullanıcıların bölümüne gidin, sonra kullanıcı eklemek için yeni düğmesine tıklayın.

    ![Kullanıcı Ekle](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. Üzerinde **"Yeni kullanıcı oluştur"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin.

    ![kullanıcı](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    a. İçinde **ad** metin kutusuna, Britta Simon ilk türünün adı.

    b. İçinde **Soyadı** metin kutusuna, Britta Simon son adını yazın.

    c. İçinde **kullanıcı adı** metin kutusuna, Britta Simon kullanıcı adını yazın.

    d. İçinde **e-posta** metin kutusuna, Britta Simon hesabı e-posta adresini yazın.

    e. Bilgi geri kalanı isteğe bağlıdır, gerekirse doldurabilirsiniz.
    
    f. **KAYDET**'e tıklayın.  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Velpic SAML kendi erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Britta Simon Velpic SAML atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Velpic SAML**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

1. Erişim paneli Velpic SAML parçasında tıkladığınızda, oturum açma sayfasına Velpic SAML uygulamasının almanız gerekir. Görmeniz gerekir **'Günlük oturum Azure AD'** oturum açma sayfası düğmesinde.

    ![Eklentisi](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. Tıklayın **'Günlük oturum Azure AD'** düğmesini Velpic için Azure AD hesabınızı kullanarak oturum açın.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

