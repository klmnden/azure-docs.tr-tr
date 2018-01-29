---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Pingboard yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve kullanıcı hesaplarına Pingboard sağlanmasını için Azure Active Directory yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 0492a59852a6a2dd424c98e44969eab1ac4697e8
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="tutorial-configuring-pingboard-for-automatic-user-provisioning"></a>Öğretici: Pingboard otomatik kullanıcı sağlamayı için yapılandırma

Bu öğreticinin amacı otomatik sağlama ve Azure Active Directory'den kullanıcı hesaplarının Pingboard sağlamayı kaldırma özelliklerini gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active Directory kiracısı
*   Pingboard Kiracı [Pro hesabı](https://pingboard.com/pricing) 
*   Pingboard yönetici izinlerine sahip bir kullanıcı hesabı 

> [!NOTE] 
> Azure Active Directory'ı tümleştirme sağlama Pingboard API'sini kullanır (`https://your_domain.pingboard.com/scim/v2`) hesabınıza kullanılabilir olduğu.

## <a name="assigning-users-to-pingboard"></a>Kullanıcılar için Pingboard atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca "Azure Active Directory'de bir uygulama atanmış" kullanıcılar eşitlenir. 

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce kullanıcıların Azure Active Directory'de Pingboard uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Pingboard uygulamanıza atayabilirsiniz:

[Kullanıcı bir kurumsal uygulama atama](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Kullanıcılar için Pingboard atamak için önemli ipuçları

*   Tek bir Azure Active Directory kullanıcı sağlama yapılandırmayı test etmek için Pingboard atamanız önerilir. Ek kullanıcılar daha sonra atanabilir.

## <a name="configuring-user-provisioning-to-pingboard"></a>Kullanıcı için Pingboard sağlama yapılandırma 

Bu bölümde Azure Active Directory'yi Pingboard kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirmek ve azure'da kullanıcı ataması göre Pingboard atanan kullanıcı hesaplarında devre dışı bırak Active Directory.

> [!TIP]
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için Pingboard, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-active-directory"></a>Azure Active Directory'de Pingboard için otomatik olarak bir kullanıcı hesabı sağlama yapılandırmak için:

1)  İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2) Çoklu oturum açma için Pingboard zaten yapılandırdıysanız arama alanı kullanarak Pingboard Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Pingboard** uygulama galerisinde. Seçin **Pingboard** Arama sonuçlarından ve uygulamaları listenize ekleyin.

3)  Pingboard örneğiniz seçin ve ardından **sağlama** sekmesi.

4)  Ayarlama **sağlama modunda** için **otomatik**.

![Pingboard sağlama](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5) Yönetici kimlik bilgileri bölümü altında aşağıdaki adımları gerçekleştirin:

* İçinde **Kiracı URL** metin girin `https://your_domain.pingboard.com/scim/v2` ve şöyledir: your_domain gerçek etki alanı ile değiştirin
* Oturum [Pingboard](https://pingboard.com/) yönetici hesabı kullanarak.
* Eklentileri tıklayın > tümleştirmeler > Azure Active Directory.
* Yapılandırma sekmesinde tıklayın ve **Azure'dan kullanıcı sağlamayı etkinleştirin**.
* Kopya **OAuth taşıyıcı belirteci** alan ve girerek **gizli belirteci** metin kutusu.

6) Azure portalında tıklatın **Bağlantıyı Sına** Azure Active Directory Pingboard uygulamanıza bağlanabilir emin olmak için. Bağlantı başarısız olursa Pingboard hesabınız yönetici izinleri ve deneyin olduğundan **"Test Bağlantısı"** adım yeniden uygulayın.

7) Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve aşağıdaki onay kutusunu işaretleyin.

8) **Kaydet**’e tıklayın. 

9) Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Pingboard**.

10) İçinde **öznitelik eşlemelerini** bölümünde, Pingboard için Azure Active Directory'den eşitlenmesi için kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Pingboard kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Seçin **kaydetmek** düğmesi değişiklikleri uygulayın. Daha fazla bilgi için bkz: [kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md).

11) Azure Active Directory'ı Pingboard için hizmet sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12) Tıklatın **kaydetmek** Pingboard için atanan kullanıcılar ilk eşitleme başlatmak için.

İlk yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler gerçekleştirmek için daha uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Pingboard uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Azure Active Directory'ı günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-enterprise-apps-manage-provisioning.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-pingboard-tutorial.md)
