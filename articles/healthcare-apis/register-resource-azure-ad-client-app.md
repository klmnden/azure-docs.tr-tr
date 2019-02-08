---
title: Azure Active Directory'de - Azure API FHIR için bir kaynak uygulamayı kaydetme
description: Bu makalede, bir kaynak uygulaması Azure Active Directory'ye kaydetmeniz açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: e8305c5a69fa3fda29f4f1292b7faa59f8ec3608
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870155"
---
# <a name="register-a-resource-application-in-azure-active-directory"></a>Kaynak uygulaması Azure Active Directory'de Kaydettir

Bu makalede, bir kaynak (veya API) kaydetme öğreneceksiniz Azure Active Directory'de uygulama. Kaynak uygulaması FHIR sunucunun kendisini API, bir Azure Active Directory temsilidir ve istemci uygulamaları, kimlik doğrulaması yapılırken kaynağa erişim talep edebilir. Kaynak olarak da bilinen uygulamasıdır *İzleyici* OAuth dilinde içinde.

## <a name="app-registrations-in-azure-portal"></a>Azure portalında uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

2. İçinde **Azure Active Directory** dikey penceresinde **uygulama kayıtları (Önizleme)**:

    ![Azure Portalı'nı tıklatın. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Tıklayın **yeni kayıt**.

## <a name="add-a-new-application-registration"></a>Yeni bir uygulama kaydı ekleme

Yeni uygulamanın ayrıntılarını doldurun. Görüntü adı için belirli gereksinimler yoktur, ancak FHIR sunucunun URI'sini ayarı bulmayı kolaylaştırır:

![Yeni uygulama kaydı](media/how-to-aad/portal-aad-register-new-app-registration-NAME.png)

## <a name="set-identifier-uri-and-define-scopes"></a>Tanımlayıcı URI'Sİ'ı ayarlama ve kapsamları tanımlama

Bir kaynak uygulama tanımlayıcı URI'si hangi istemcilerin kaynağa erişim isteğinde bulunurken kullanabilirsiniz (uygulama kimliği URI'si) sahiptir. Bu değer doldurur `aud` erişim belirtecini talep. Bu URI FHIR sunucunuzun URI şu şekilde ayarlamanızı öneririz. FHIR uygulamalarında akıllı için varsayılır *İzleyici* FHIR sunucusunun URI'dir.

1. Tıklayın **bir API'yi kullanıma sunmak**

2. Tıklayın **ayarlamak** yanındaki *uygulama kimliği URI'si*.

3. ' % S'tanımlayıcısı URI girin ve tıklayın **Kaydet**. İyi bir tanımlayıcı URI'si FHIR sunucunuzun URI olacaktır.

4. Tıklayın **"kapsam" Ekle** ve sizin için API tanımlamak istediğiniz herhangi bir kapsam ekleyin. Azure AD şu anda izin vermiyor eğik çizgi (`/`) kapsamın adları. Kullanmanızı öneririz `$` yerine. Bir kapsam ister `patient/*.read` olacaktır `patient$*.read`.

    ![Hedef kitleniz ve kapsamı](media/how-to-aad/portal-aad-register-new-app-registration-AUD-SCOPE.png)

## <a name="define-application-roles"></a>Uygulama rolleri tanımlama

Azure API FHIR ve OSS FHIR sunucunun Azure kullanımı için [Azure Active Directory uygulama rolleri](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) rol tabanlı erişim denetimi için. Hangi rollerin FHIR sunucu API'niz için kullanılabilir olması gerektiğini tanımlamak için kaynak uygulamanın açın [bildirim](https://docs.microsoft.com/azure/active-directory/active-directory-application-manifest/):

1. Tıklayın **bildirim**:

    ![Uygulama Rolleri](media/how-to-aad/portal-aad-register-new-app-registration-APP-ROLES.png)

2. İçinde `appRoles` özelliği, kullanıcılar veya uygulamalar olmasını istediğiniz rolleri Ekle:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User",
          "Application"
        ],
        "description": "FHIR Server Administrators",
        "displayName": "admin",
        "id": "1b4f816e-5eaf-48b9-8613-7923830595ad",
        "isEnabled": true,
        "value": "admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "description": "Users who can read",
        "displayName": "reader",
        "id": "c20e145e-5459-4a6c-a074-b942bbd4cfe1",
        "isEnabled": true,
        "value": "reader"
      }
    ],
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir kaynak uygulaması Azure Active Directory'ye kaydetmeniz öğrendiniz. Ardından, azure'da bir FHIR API dağıtın.
 
>[!div class="nextstepaction"]
>[Açık kaynak FHIR sunucusu dağıtma](fhir-oss-powershell-quickstart.md)