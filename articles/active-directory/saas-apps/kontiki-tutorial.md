---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Kontiki | Microsoft Docs'
description: Azure Active Directory ve Kontiki arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8d5e5413-da4c-40d8-b1d0-f03ecfef030b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bee7454942b9214eeb1253339446df370e20fe01
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65785842"
---
# <a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Öğretici: Kontiki ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Kontiki tümleştirme konusunda bilgi edinin.

Kontiki Azure AD ile tümleştirme, aşağıdaki avantajları sunar:

* Azure AD Kontiki erişimi denetlemek için kullanabilirsiniz.
* Kullanıcıları otomatik olarak Kontiki için kendi Azure AD hesapları (çoklu oturum açma) ile oturum açmanız.
* Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Kontiki yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.
* Çoklu oturum etkin açma Kontiki abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin ve Kontiki Azure AD ile tümleştirme yapılandırın.

Kontiki aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan çoklu oturum açma**
* **Just-ın-time kullanıcı sağlama**

## <a name="add-kontiki-in-the-azure-portal"></a>Azure portalında Kontiki Ekle

Kontiki Azure AD ile tümleştirmek için Kontiki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol menüde **Azure Active Directory**.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Bir uygulama eklemek için seçin **yeni uygulama**.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Kontiki**. Arama sonuçlarında seçin **Kontiki**ve ardından **Ekle**.

    ![Sonuç listesinde Kontiki](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Kontiki adlı bir test kullanıcı tabanlı test **Britta Simon**. Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bağlı bir ilişki içinde Kontiki oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Kontiki ile test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD çoklu oturum açmayı yapılandırın](#configure-azure-ad-single-sign-on)** | Bu özelliği kullanmak olanak sağlar. |
| **[Kontiki çoklu oturum açmayı yapılandırın](#configure-kontiki-single-sign-on)** | Uygulamada çoklu oturum açma ayarları yapılandırır. |
| **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Testleri Azure AD çoklu oturum açma kullanıcı Britta Simon adı. |
| **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)** | Azure AD çoklu oturum açmayı kullanmak Britta Simon sağlar. |
| **[Kontiki test kullanıcısı oluşturma](#create-a-kontiki-test-user)** | Kullanıcı Azure AD gösterimini bağlı Kontiki içinde bir karşılığı Britta simon'un oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırma çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma ile Kontiki Azure portalında yapılandırın.

1. İçinde [Azure portalında](https://portal.azure.com/), **Kontiki** uygulama tümleştirme bölmesinde **çoklu oturum açma**.

    ![Çoklu oturum açma seçeneği yapılandırın](common/select-sso.png)

1. İçinde **tek bir oturum açma yönteminizi seçmeniz** bölmesinde seçin **SAML** veya **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

1. İçinde **yukarı çoklu oturum açma SAML ile ayarlayın** bölmesinde **Düzenle** (açmak için kalem simgesi) **temel SAML yapılandırma** bölmesi.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. İçinde **temel SAML yapılandırma** bölmesinde, **oturum açma URL'si** metin kutusunda, aşağıdaki desenin bir URL girin: `https://<companyname>.mc.eval.kontiki.com`

    ![Kontiki etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    > [!NOTE]
    > İlgili kişi [Kontiki istemci Destek ekibine](https://customersupport.kontiki.com/enterprise/contactsupport.html) kullanmak için doğru değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

1. İçinde **yukarı çoklu oturum açma SAML ile ayarlanmış** bölmesinde, **SAML imzalama sertifikası** bölümünden **indirme** yanındaki **Federasyon meta verileri XML**. Gereksinimlerinize göre bir indirme seçeneğini seçin. Sertifika bilgisayarınıza kaydedin.

    ![Federasyon meta verileri XML sertifika yükleme seçeneği](common/metadataxml.png)

1. İçinde **Kontiki kümesi** bölümünde, gereksinimlerinize göre aşağıdaki URL'ler kopyalayın:

    * Oturum Açma URL'si:
    * Azure AD Tanımlayıcısı
    * Oturum Kapatma URL'si

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

### <a name="configure-kontiki-single-sign-on"></a>Kontiki çoklu oturum açmayı yapılandırın

Çoklu oturum açma Kontiki tarafında yapılandırmak için indirilen Federasyon meta veri XML dosyasını ve için Azure Portalı'ndan kopyaladığınız ilgili URL'leri Gönder [Kontiki Destek ekibine](https://customersupport.kontiki.com/enterprise/contactsupport.html). Oturum açma SAML tek bağlantısı her iki kenarı da düzgün ayarlandığından emin olmak için bunları gönderdiğiniz bilgiler Kontiki Destek ekibine kullanır.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcılar ve tüm kullanıcılar seçenekleri](common/users.png)

1. Seçin **yeni kullanıcı**.

    ![Yeni kullanıcı seçeneği](common/new-user.png)

1. İçinde **kullanıcı** bölmesinde, aşağıdaki adımları tamamlayın:

    1. İçinde **adı** kutusuna **BrittaSimon**.
  
    1. İçinde **kullanıcı adı** kutusuna **brittasimon\@\<your-şirket etki alanı >.\< Uzantı >**. Örneğin, **brittasimon\@contoso.com**.

    1. Seçin **Show parola** onay kutusu. Görüntülenen değer azaltma **parola** kutusu.

    1. **Oluştur**’u seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Filiz Azure çoklu oturum açma kullanabilmeniz için bu bölümde, Britta Simon Kontiki için erişim.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **Kontiki**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde **Kontiki**.

    ![Uygulamalar listesinde Kontiki](common/all-applications.png)

1. Menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**. Ardından **ataması ekleme** bölmesinde **kullanıcılar ve gruplar**.

    ![Ekle atama bölmesi](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** bölmesinde **Britta Simon** kullanıcılar listesinde. **Seç**’i seçin.

1. SAML onaylaması rol değeri de beklediğiniz varsa **rol seçme** bölmesinde, listeden kullanıcı için uygun rolü seçin. **Seç**’i seçin.

1. İçinde **atama Ekle** bölmesinde **atama**.

### <a name="create-a-kontiki-test-user"></a>Kontiki test kullanıcısı oluşturma

Kullanıcı hazırlama Kontiki yapılandırmanız için hiçbir eylem öğesini yoktur. Uygulamalarım portalını kullanarak Kontiki için oturum açmak atanmış bir kullanıcısı çalıştığında Kontiki kullanıcının var olup olmadığını denetler. Kullanıcı hesabı bulursa, Kontiki kullanıcı hesabına otomatik olarak oluşturur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, uygulamalarım portalını kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Çoklu oturum açma, getirdiğinizde seçtiğinizde **Kontiki** uygulamalarım portalında, otomatik olarak Kontiki için oturum açtınız. Uygulamalarım portal hakkında daha fazla bilgi için bkz. [erişim ve kullanım uygulamaları uygulamalarım portalında](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bu makaleleri gözden geçirin:

- [İçin Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'de uygulamalar için çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
