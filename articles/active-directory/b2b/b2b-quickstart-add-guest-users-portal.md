---
title: 'Hızlı Başlangıç: Azure portal - Azure Active Directory Konuk kullanıcıları ekleme'
description: Bu hızlı başlangıcı kullanarak, Azure AD yöneticilerinin Azure portalda nasıl B2B konuk kullanıcıları ekleyebileceğini öğrenecek ve B2B davet iş akışı boyunca adım adım yol gösterici bilgiler alacaksınız.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4935cc15bf3edeccd6b6ce9da701904a32606db
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60357367"
---
# <a name="quickstart-add-guest-users-to-your-directory-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında dizinine Konuk kullanıcıları eklemek

Herhangi bir kişiyi dizininize konuk kullanıcı olarak ekleyerek onu kuruluşunuzla işbirliği yapması için davet edebilirsiniz. Daha sonra paylaşmak istediğiniz bir uygulamanın doğrudan bağlantısını gönderebilir veya kullanım bağlantısını içeren bir davet e-postası gönderebilirsiniz. Konuk kullanıcılar, kendi iş, okul veya sosyal kimlikleriyle oturum açabilir.

Bu hızlı başlangıçta, Azure AD’ye yeni bir konuk kullanıcı ekler, davet gönderir ve konuk kullanıcının davet kullanım işleminin nasıl göründüğünü görürsünüz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide senaryoyu tamamlamak için şunlar gereklidir:

 - Genel Yönetici rolü gibi kiracı dizininizde kullanıcılar oluşturmanıza olanak sağlayan bir rol veya sınırlı yönetici dizin rollerinden herhangi biri.
 - Kiracı dizininize ekleyebileceğiniz ve test davet e-postasını almak için kullanabileceğiniz geçerli bir e-posta hesabı.

## <a name="add-a-new-guest-user-in-azure-ad"></a>Azure AD’de yeni bir konuk kullanıcı ekleme

1. [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2. Sol bölmede **Azure Active Directory**’yi seçin.
3.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.

    ![Nereye kullanıcılar seçeneğini gösteren ekran görüntüsü](media/quickstart-add-users-portal/quickstart-users-portal-user.png)

4.  **Yeni konuk kullanıcı**’yı seçin.

    ![Yeni Konuk kullanıcı Seç seçeneği nerede gösteren ekran görüntüsü](media/quickstart-add-users-portal/quickstart-users-portal-user-3.png)

5.  **Kullanıcı adı** bölümüne, harici kullanıcının e-posta adresini girin. **Davete kişisel bir mesaj ekleyin** bölümüne bir karşılama mesajı girin. 

    ![Konuk kullanıcı davet iletisi girmek nereye gösteren ekran görüntüsü](media/quickstart-add-users-portal/quickstart-users-portal-user-4.png)

6. Konuk kullanıcıya otomatik olarak daveti göndermek için **Davet Et**’i seçin. **Kullanıcı başarıyla davet edildi** iletisi ile sağ üst kısımda bir bildirim görüntülenir. 
7.  Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.

## <a name="assign-an-app-to-the-guest-user"></a>Konuk kullanıcıya uygulama atama
Test kiracınıza Salesforce uygulamasını ekleyin ve test konuk kullanıcısını uygulamaya atayın.
1.  Azure portalda Azure AD yöneticisi olarak oturum açın.
2.  Sol bölmede **Kurumsal uygulamalar**’ı seçin.
3.  **Yeni uygulama**’yı seçin.
4. **Galeriden ekle** bölümünde **Salesforce** araması yapın ve bunu seçin.

    ![Galeri arama kutusundan Ekle gösteren ekran görüntüsü](media/quickstart-add-users-portal/quickstart-users-portal-select-salesforce.png)
5. **Add (Ekle)** seçeneğini belirleyin.
6. **Yönet** bölümünde **Çoklu oturum açma**’yı seçin ve **Çoklu Oturum Açma Modu** bölümünde **Parola tabanlı Oturum açma**’yı seçin ve **Kaydet**’e tıklayın.
7. **Yönet** bölümünde **Kullanıcılar ve gruplar** > **Kullanıcı ekle** > **Kullanıcılar ve gruplar** seçeneklerini belirleyin.
8. Arama kutusunu kullanarak test kullanıcısını arayın (gerekirse) ve listeden test kullanıcısını seçin. Ardından **Seç**'e tıklayın.
9. **Ata**'yı seçin. 

## <a name="accept-the-invitation"></a>Daveti kabul etme
Şimdi daveti görmek için konuk kullanıcı olarak oturum açın.
1.  Test konuk kullanıcısının e-posta hesabıyla oturum açın.
2.  Gelen kutunuzda “Davetlisiniz” e-postasını bulun.

    ![B2B davet e-postası gösteren ekran görüntüsü](media/quickstart-add-users-portal/quickstart-users-portal-email-small.png)

3.  E-posta gövdesinde **Başlarken**‘i seçin. Tarayıcıda bir **İzinleri gözden geçir** sayfası açılır. 

    ![Gözden geçirme izinleri sayfasını gösteren ekran görüntüsü](media/quickstart-add-users-portal/quickstart-users-portal-accept.png)

4. **Kabul Et**’i seçin. Konuk kullanıcının erişebileceği uygulamaları listeleyen Erişim Paneli açılır.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse test konuk kullanıcısını ve test uygulamasını silin.
1.  Azure portalda Azure AD yöneticisi olarak oturum açın.
2.  Sol bölmede **Azure Active Directory**’yi seçin.
3.  **Yönet** bölümünde **Kurumsal uygulamalar**’ı seçin.
4.  **Salesforce** uygulamasını açın ve **Sil**’i seçin.
5.  Sol bölmede **Azure Active Directory**’yi seçin.
6.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.
7.  Test kullanıcısını seçin ve **Kullanıcıyı sil** seçeneğini belirleyin.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalda bir konuk kullanıcı oluşturdunuz ve uygulamaları paylaşmak için bir davet gönderdiniz. Daha sonra konuk kullanıcının perspektifinden kullanım işlemini görüntülediniz ve uygulamanın konuk kullanıcının Erişim Panelinde görüntülendiğini doğruladınız. İşbirliği yapmak üzere konuk kullanıcılar ekleme hakkında daha fazla bilgi için bkz. [Azure portalda Azure Active Directory B2B işbirliği kullanıcıları ekleme](add-users-administrator.md).
