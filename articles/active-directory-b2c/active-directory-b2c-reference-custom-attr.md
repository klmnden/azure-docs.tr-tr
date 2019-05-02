---
title: Azure Active Directory B2C'de özel öznitelikleri tanımlamak | Microsoft Docs
description: Azure Active Directory B2C, müşterilerinizin hakkında bilgi toplamak için uygulamanız için özel öznitelikler tanımlayabilirsiniz.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 4debd5408abda7e6ae7a7ebf2210fb69210582ae
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687921"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Özel öznitelikler Azure Active Directory B2C'de tanımlayın

 Her müşteriye dönük uygulama, bilgilerin toplanması gerekir için benzersiz gereksinimleri vardır. Azure Active Directory (Azure AD) B2C kiracınızı, verilen ad, Soyadı, şehir ve posta kodu gibi özniteliklerde depolanan bilgileri yerleşik bir dizi ile birlikte gelir. Azure AD B2C ile her bir müşteri hesapta depolanan öznitelik kümesini genişletebilirsiniz. 
 
 Özel öznitelikler oluşturabilirsiniz [Azure portalında](https://portal.azure.com/) ve kaydolma kullanıcı akışları, kaydolma veya oturum açma kullanıcı akışları veya profil düzenleme kullanıcı akışları kullanın. Ayrıca okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API'si](active-directory-b2c-devquickstarts-graph-dotnet.md). Azure AD B2C, özel öznitelikler kullanma [Azure AD Graph API Directory şema uzantıları](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

## <a name="create-a-custom-attribute"></a>Özel bir öznitelik oluşturma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
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

Özel öznitelik listesinde kullanılabilir **kullanıcı öznitelikleri** ve kullanıcı akışlarınızda kullanın. Yalnızca hiçbir kullanıcı akışında kullanılan, ilk kez oluşturulan ve listesine değil eklediğinizde, özel bir öznitelik olan **kullanıcı öznitelikleri**.

## <a name="use-a-custom-attribute-in-your-user-flow"></a>Özel bir öznitelik kullanıcı akışınızı kullanın

1. Azure AD B2C kiracınızı seçin **kullanıcı akışları**.
2. Açmak için ilke (örneğin, "B2C_1_SignupSignin") seçin. 
4. Seçin **kullanıcı öznitelikleri** ve sonra özel öznitelik (örneğin, "ShoeSize") seçin. **Kaydet**’e tıklayın.
5. Seçin **uygulama taleplerini** ve özel özniteliği seçin. 
6. **Kaydet**’e tıklayın.

Kullanabileceğiniz **kullanıcı akışı çalıştırma** müşteri deneyimini doğrulamak için kullanıcı akışı özelliği. Görmelisiniz **ShoeSize** öznitelikler listesinde kaydolma yolculuğu sırasında toplanan ve uygulamanıza geri gönderilen belirteçte bakın.

