---
title: 'Öğretici: Oracle bulut altyapısı konsolu ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Oracle bulut altyapısı Konsolu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: f045fe19-11f8-4ccf-a3eb-8495fdc8716f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 456c984e577e3427ce8cd62d6f63987118f2c8ed
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164147"
---
# <a name="tutorial-integrate-oracle-cloud-infrastructure-console-with-azure-active-directory"></a>Öğretici: Oracle bulut altyapısı konsol Azure Active Directory ile tümleştirme

Bu öğreticide, Oracle bulut altyapısı konsol Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Oracle bulut altyapısı Konsolu Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Oracle bulut altyapısı konsol erişimi, Azure AD'de denetler.
* Otomatik olarak Oracle bulut altyapısı konsola kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Oracle bulut altyapısı konsol çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin. Oracle bulut altyapısı Konsolu destekler **SP** SSO başlattı.

## <a name="adding-oracle-cloud-infrastructure-console-from-the-gallery"></a>Oracle bulut altyapısı konsol galeri ekleme

Azure AD'de Oracle bulut altyapısı konsolunun tümleştirmesini yapılandırmak için Oracle bulut altyapısı konsol Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **Oracle bulut altyapısı konsol** arama kutusuna.
1. Seçin **Oracle bulut altyapısı konsol** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO Oracle bulut altyapısını kullanarak adlı bir test Kullanıcı konsolu ile test etme **b Simon**. Çalışmak SSO için Oracle bulut altyapısı konsolunda bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO Oracle bulut altyapısı konsolu ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. **[Oracle bulut altyapısı konsolunu yapılandırmak](#configure-oracle-cloud-infrastructure-console)**  uygulama tarafında SSO ayarlarını yapılandırmak için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  Azure AD çoklu oturum açma b Simon ile test etmek için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  Azure AD çoklu oturum açmayı kullanmak b Simon etkinleştirmek için.
1. **[Oracle bulut altyapısı konsol test kullanıcısı oluşturma](#create-oracle-cloud-infrastructure-console-test-user)**  kullanıcı Azure AD gösterimini bağlı Oracle bulut altyapısı konsolunda bir karşılığı b simon'un sağlamak için.
1. **[Test SSO](#test-sso)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **Oracle bulut altyapısı konsol** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **tek oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** sayfasında, aşağıdaki alanlar için değerleri girin:

   > [!NOTE]
   > Hizmet sağlayıcısı meta veri dosyasından erişmenizi sağlayacak **yapılandırma Oracle bulut altyapısı konsol çoklu oturum açma** öğreticinin bölümünde.
    
   1. Tıklayın **meta veri dosyasını karşıya yükleme**.

   1. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

   1. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL'si** değerlerini alma otomatik olarak doldurulmuş **temel SAML yapılandırma** bölümünde metin.
    
      > [!NOTE]
      > Varsa **tanımlayıcı** ve **yanıt URL'si** değerlerin değil otomatik polulated alın ve ardından değerleri ihtiyacınıza göre el ile girin.

      İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://console.<REGIONNAME>.oraclecloud.com/`

      > [!NOTE]
      > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Oracle bulut altyapısı konsol istemcisi Destek ekibine](https://www.oracle.com/support/advanced-customer-support/products/cloud.html) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde, bulma **Federasyon meta verileri XML** seçip **indirin** sertifikayı indirin ve bilgisayarınıza kaydedin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Oracle bulut altyapısı konsol uygulaması, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekliyor. Aşağıdaki ekran görüntüsünde, varsayılan öznitelikler listesinde gösterilmiştir. Tıklayın **Düzenle** kullanıcı öznitelikleri iletişim kutusunu açmak için simge.

   ![image](common/edit-attribute.png)

1. Yukarıdaki için Ayrıca, Oracle bulut altyapısı konsol uygulaması SAML yanıtta geçirilecek birkaç daha fazla öznitelik bekliyor. İçinde **kullanıcı öznitelikleri ve talepler** bölümünde **grup talepleri (Önizleme)**  iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

   1. Tıklayın **kalem** yanındaki **ad tanımlayıcı değeri**.

   1. Seçin **kalıcı** olarak **belirleyin adı tanımlayıcı biçimi**.
 
   1. **Kaydet**’e tıklayın.

      ![image](./media/oracle-cloud-tutorial/config07.png)
    
      ![image](./media/oracle-cloud-tutorial/config11.png)

   1. Tıklayın **kalem** yanındaki **grupları döndürülen talebi**.

   1. Seçin **güvenlik grupları** radyo listeden.

   1. Seçin **kaynak özniteliği** , **Grup Kimliği**.

   1. Denetleme **grup talebi adını özelleştirme**.

   1. İçinde **adı** metin kutusunda, **groupName**.

   1. İçinde **Namespace (isteğe bağlı)** metin kutusunda, `https://auth.oraclecloud.com/saml/claims`.

   1. **Kaydet**’e tıklayın.

      ![image](./media/oracle-cloud-tutorial/config08.png)

1. Üzerinde **Oracle bulut altyapısı Konsolu ayarlama** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

   ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-oracle-cloud-infrastructure-console"></a>Oracle bulut altyapısı Konsolu yapılandırma

1. Farklı bir web tarayıcı penceresinde Oracle bulut altyapısı konsolunu yönetici olarak oturum açın.

1. Menü sol tarafında tıklayın ve tıklayarak **kimlik** gidin **Federasyon**.

   ![Yapılandırma](./media/oracle-cloud-tutorial/config01.png)

1. Kaydet **hizmet sağlayıcısı meta veri dosyası** tıklayarak **bu belgeyi indirme** bağlamak ve içine yükleyin **temel SAML yapılandırma** Azure portalının bölümüne ve ardından tıklayarak **kimlik sağlayıcısı Ekle**.

   ![Yapılandırma](./media/oracle-cloud-tutorial/config02.png)

1. Üzerinde **kimlik sağlayıcısı Ekle** açılır penceresinde, aşağıdaki adımları gerçekleştirin:

   ![Yapılandırma](./media/oracle-cloud-tutorial/config03.png)

   1. İçinde **adı** metin kutusunda, adınızı girin.

   1. İçinde **açıklama** metin kutusunda, açıklamanızı girin.

   1. Seçin **MICROSOFT ACTIVE DIRECTORY FEDERASYON hizmeti (ADFS) veya SAML 2.0 UYUMLU bir kimlik SAĞLAYICISI** olarak **türü**.

   1. Tıklayın **Gözat** Azure portalından indirdiğiniz Federasyon meta verileri XML yüklenecek.

   1. Tıklayın **devam** ve **kimlik sağlayıcısını Düzenle** bölümünde aşağıdaki adımları gerçekleştirin:

      ![Yapılandırma](./media/oracle-cloud-tutorial/config09.png)

   1. İçin **kimlik SAĞLAYICISI GRUBUNU** alanları, Azure portalında ayarladığınız grup kimliği ve grup adını girin. Grup içinde karşılık gelen grubuyla eşlenmesi gereken **OCI grubu** alan.

   1. Azure portalında ve organizasyon gereksinimlerinize kurulumunuza birden çok grup eşleyebilirsiniz. Tıklayarak **+ Ekle eşlemesi** sayıda grubu eklemek için.

   1. **Gönder**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı b Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B. Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin, `B. Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Oracle bulut altyapısı konsoluna erişim vererek Azure çoklu oturum açmayı kullanmak b Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **Oracle bulut altyapısı konsol**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

   ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **b Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-oracle-cloud-infrastructure-console-test-user"></a>Oracle bulut altyapısı konsol test kullanıcısı oluşturma

 Oracle bulut altyapısı Konsolu tam zamanında sağlama, varsayılan olarak olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı erişimi ve kullanıcı oluşturmak için de gerek girişimi sırasında oluşturulan değil.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Oracle bulut altyapısı konsol kutucuğu seçtiğinizde, Oracle bulut altyapısı Konsolu oturum açma sayfasına yönlendirilirsiniz. Seçin **kimlik SAĞLAYICISI** tıklayın ve açılan menüden **devam** oturum açmak için aşağıda gösterildiği gibi. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

![Yapılandırma](./media/oracle-cloud-tutorial/config10.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
