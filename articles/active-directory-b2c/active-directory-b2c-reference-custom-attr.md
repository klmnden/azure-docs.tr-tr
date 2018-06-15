---
title: Azure Active Directory B2C özel öznitelikleri | Microsoft Docs
description: Özel öznitelikler Azure Active Directory B2C'de tüketicileriniz hakkında bilgi toplamak için nasıl kullanılacağını.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 61931be8e50cdc3132e36a63a2fdb059d62ba947
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711884"
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: tüketicileriniz hakkında bilgi toplamak için özel öznitelikler kullanın.
Azure Active Directory (Azure AD) B2C dizininize bilgi (öznitelikler) yerleşik bir dizi birlikte gelir: verilen ad, Soyadı, şehir, posta kodu ve diğer öznitelikleri. Bununla birlikte, her tüketiciye yönelik uygulama tüketici toplamak için benzersiz gereksinimleri ne öznitelikleri üzerinde vardır. Azure AD B2C ile her tüketici hesapta depolanan öznitelikler kümesi genişletebilirsiniz. Özel öznitelikler oluşturabileceğiniz [Azure portal](https://portal.azure.com/) ve aşağıda gösterildiği gibi kaydolma ilkelerinizi kullanın. Aynı zamanda okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Özel öznitelikler kullanın [Azure AD Graph API Directory şema uzantıları](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).
> 
> 

## <a name="create-a-custom-attribute"></a>Özel bir öznitelik oluşturma
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Tıklatın **kullanıcı öznitelikleri**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Sağlayan bir **adı** özel öznitelik (örneğin, "ShoeSize") ve isteğe bağlı olarak bir **açıklama**. **Oluştur**’a tıklayın.
   
   > [!NOTE]
   > Yalnızca "Dize", "Mantıksal" ve "Int" **veri türleri** şu anda kullanılabilir.
   > 
   > 

Özel öznitelik listesinde kullanılabilir **kullanıcı öznitelikleri**ve kaydolma ilkelerinizi kullanmak için.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Kaydolma ilkenizde özel bir öznitelik kullanın
1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. **Kaydolma ilkeleri**’ne tıklayın.
3. Açmak için kayıt ilkesi (örneğin, "B2C_1_SiUp") tıklayın. Tıklatın **Düzenle** dikey pencerenin üstündeki.
4. Tıklatın **kaydolma özniteliklerini** ve özel öznitelik (örneğin, "ShoeSize") seçin. **Tamam**’a tıklayın.
5. Tıklatın **uygulama talepleri** ve özel özniteliği seçin. **Tamam**’a tıklayın.
6. Tıklatın **kaydetmek** dikey pencerenin üstündeki.

Müşteri Deneyimi doğrulamak için ilkeyi "Şimdi Çalıştır" özelliğini kullanabilirsiniz. Şimdi "ShoeSize" tüketici kaydolma sırasında toplanan özniteliklerinin listesini görmek ve uygulamanıza geri gönderilen belirteçte bkz gerekir.

## <a name="notes"></a>Notlar
* Kayıt ilkeleri ile birlikte özel öznitelikler de oturum açma veya kaydolma ilkeleri ve profil düzenleme ilkeleri kullanılabilir.
* Özel özniteliklerin bilinen bir sınırlama yoktur. Yalnızca ilk defa herhangi bir ilkede kullanıldığında oluşturulan ve listesine değil eklendiğinde olan **kullanıcı öznitelikleri**.

