---
title: Azure Active Directory B2C'de özel öznitelikleri tanımlamak | Microsoft Docs
description: Azure Active Directory B2C, müşterilerinizin hakkında bilgi toplamak için uygulamanız için özel öznitelikler tanımlayabilirsiniz.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d5ef77ab0bbf00d4ddbb05b7a38516e3c3e7d800
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968783"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Özel öznitelikler Azure Active Directory B2C'de tanımlayın

 Her müşteriye dönük uygulama, bilgilerin toplanması gerekir için benzersiz gereksinimleri vardır. Azure Active Directory (Azure AD) B2C kiracınızı, verilen ad, Soyadı, şehir ve posta kodu gibi özniteliklerde depolanan bilgileri yerleşik bir dizi ile birlikte gelir. Azure AD B2C ile her bir müşteri hesapta depolanan öznitelik kümesini genişletebilirsiniz. 
 
 Özel öznitelikler oluşturabilirsiniz [Azure portalında](https://portal.azure.com/) ve kaydolma ilkeleri, oturum açma veya kaydolma ilkeleri'ni veya profil düzenleme ilkeleri kullanın. Ayrıca okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API'si](active-directory-b2c-devquickstarts-graph-dotnet.md). Azure AD B2C, özel öznitelikler kullanma [Azure AD Graph API Directory şema uzantıları](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

## <a name="create-a-custom-attribute"></a>Özel bir öznitelik oluşturma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-reference-custom-attr/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-reference-custom-attr/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kullanıcı öznitelikleri**ve ardından **Ekle**.
5. Sağlayan bir **adı** özel öznitelik (örneğin, "ShoeSize")
6. Seçin bir **veri türü**. Yalnızca **dize**, **Boole**, ve **Int** kullanılabilir.
7. İsteğe bağlı olarak girin bir **açıklama** bilgilendirme amaçlıdır. 
8. **Oluştur**’a tıklayın.

Özel öznitelik listesinde kullanılabilir **kullanıcı öznitelikleri** ve ilkelerini kullanın. Yalnızca ilk defa herhangi bir ilkede kullanıldığında oluşturulan ve listesine değil eklediğinizde, özel bir öznitelik olan **kullanıcı öznitelikleri**.

## <a name="use-a-custom-attribute-in-your-policy"></a>Özel bir öznitelik ilkenizde kullanın

1. Azure AD B2C kiracınızı seçin **oturum açma veya kaydolma ilkeleri'ni**.
2. Açmak için ilke (örneğin, "B2C_1_SignupSignin") seçin. 
3. **Düzenle**’ye tıklayın.
4. Seçin **kaydolma özniteliklerini** ve sonra özel öznitelik (örneğin, "ShoeSize") seçin. **Tamam**’a tıklayın.
5. Seçin **uygulama taleplerini** ve özel özniteliği seçin. **Tamam**’a tıklayın.
6. **Kaydet**’e tıklayın.

Kullanabileceğiniz **Şimdi Çalıştır** müşteri deneyimini doğrulamak için bir ilke özelliği. Görmelisiniz **ShoeSize** öznitelikler listesinde kaydolma yolculuğu sırasında toplanan ve uygulamanıza geri gönderilen belirteçte bakın.

