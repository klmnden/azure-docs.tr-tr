---
title: Azure Active Directory B2C özel öznitelikleri | Microsoft Docs
description: Tüketicileriniz hakkında bilgi toplamak için Azure Active Directory B2C'de özel öznitelikler kullanma
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 893dfbae96d2cfea01b1f281f888e9281bf582f9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441925"
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: tüketicileriniz hakkında bilgi toplamak için özel öznitelikler kullanma
Azure Active Directory (Azure AD) B2C dizininizi yerleşik bir bilgi (öznitelikler) kümesi ile birlikte gelir: verilen ad, Soyadı, şehir, posta kodu ve diğer öznitelikleri. Ancak, her tüketiciye yönelik uygulama tüketicilerden toplamak için benzersiz gereksinimleri ne öznitelikleri üzerinde vardır. Azure AD B2C ile her bir tüketici hesapta depolanan öznitelik kümesini genişletebilirsiniz. Özel öznitelikler oluşturabilirsiniz [Azure portalında](https://portal.azure.com/) ve kaydolma ilkelerinizi, aşağıda gösterildiği gibi kullanın. Ayrıca okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API'si](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Özel öznitelikler kullanın [Azure AD Graph API Directory şema uzantıları](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).
> 
> 

## <a name="create-a-custom-attribute"></a>Özel bir öznitelik oluşturma
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Tıklayın **kullanıcı öznitelikleri**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Sağlayan bir **adı** özel öznitelik (örneğin, "ShoeSize") için ve isteğe bağlı olarak bir **açıklama**. **Oluştur**’a tıklayın.
   
   > [!NOTE]
   > Yalnızca "Dize", "Boole" ve "Int" **veri türleri** şu anda kullanılabilir.
   > 
   > 

Özel öznitelik listesinde kullanılabilir **kullanıcı öznitelikleri**ve kaydolma ilkelerini kullanın.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Kaydolma ilkenizde özel bir öznitelik kullanın
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. **Kaydolma ilkeleri**’ne tıklayın.
3. Açmak için kaydolma ilkenizde (örneğin, "B2C_1_SiUp") tıklayın. Tıklayın **Düzenle** dikey penceresinin üstünde.
4. Tıklayın **kaydolma özniteliklerini** ve özel bir öznitelik (örneğin, "ShoeSize") seçin. **Tamam**’a tıklayın.
5. Tıklayın **uygulama taleplerini** ve özel özniteliği seçin. **Tamam**’a tıklayın.
6. Tıklayın **Kaydet** dikey penceresinin üstünde.

Yönelik tüketici deneyimi doğrulamak için ilkeyi "Şimdi Çalıştır" özelliğini kullanabilirsiniz. Artık "ShoeSize" tüketici kaydolma sırasında toplanan özniteliklerin listesini görmek ve uygulamanıza geri gönderilen belirteçte bkz gerekir.

## <a name="notes"></a>Notlar
* Kaydolma ilkeleri ile birlikte özel öznitelikler de oturum açma veya kaydolma ilkeleri'ni ve profil düzenleme ilkeleri kullanılabilir.
* Özel özniteliklerin bilinen bir sınırlama yoktur. Yalnızca ilk defa herhangi bir ilkede kullanıldığında oluşturulan ve listesine değil eklediğinizde olan **kullanıcı öznitelikleri**.

