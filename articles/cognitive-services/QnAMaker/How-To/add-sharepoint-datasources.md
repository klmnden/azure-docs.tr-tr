---
title: SharePoint dosyalarını - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Sorular ve Active Directory ile güvenli hale yanıtlar ile Bilgi Bankası zenginleştirmek için bilgi bankanızı güvenli Sharepoint veri kaynakları ekleyin.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/05/2019
ms.author: tulasim
ms.openlocfilehash: d02c3fbb762e83584a9ea277ef8d5cc15b391f37
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792583"
---
# <a name="add-a-secured-sharepoint-data-source-to-your-knowledge-base"></a>Bilgi Bankası'na güvenli Sharepoint veri kaynağı Ekle

Sorular ve Active Directory ile güvenli hale yanıtlar ile Bilgi Bankası zenginleştirmek için bilgi bankanızı güvenli Sharepoint veri kaynakları ekleyin. 

Soru-cevap Oluşturucu Yöneticisi olarak, Bilgi Bankası için güvenli bir Sharepoint belge eklediğinizde, soru-cevap Oluşturucu için Active Directory izin istemesi gerekir. Bu izin, erişim için Sharepoint, soru-cevap Oluşturucu Active Directory Yöneticisi'nden verildikten sonra yeniden verilmesi gerekmez. Aynı Sharepoint kaynak ise her sonraki belge ayrıca Bilgi Bankası için yetkilendirme gerekli değildir. 

Soru-cevap Oluşturucu Bilgi Bankası Yöneticisi Active Directory Yöneticisi değilse, bu işlemi tamamlamak için Active Directory Yöneticisi ile iletişim kurması gerekir.

## <a name="add-supported-file-types-to-knowledge-base"></a>Bilgi Bankası'na desteklenen dosya türleri Ekle

Tüm soru-cevap Oluşturucu tarafından desteklenen ekleyebilirsiniz [dosya türleri](../Concepts/data-sources-supported.md) bilgi bankanızı bir Sharepoint sunucusundan. Vermek zorunda kalabilirsiniz [izinleri](#permissions) dosya kaynağı sağlanıyorsa.

1. Sharepoint sunucusundan dosyanın üç nokta menüsünü seçin `...`.
1. Dosyanın URL'sini kopyalayın.

    ![Sonra URL'yi kopyalayarak dosyanın üç nokta menüsünü seçerek Sharepoint dosya URL'yi alın.](../media/add-sharepoint-datasources/get-sharepoint-file-url.png)

1. Soru-cevap Oluşturucu Portalı'nda üzerinde **ayarları** sayfasında [URL'sini ekleyin](edit-knowledge-base.md#add-datasource) Bilgi Bankası için. 

## <a name="permissions"></a>İzinler

İzinleri verme, bir Bilgi Bankası'na bir Sharepoint sunucusuna güvenli bir dosya eklendiğinde gerçekleşir. Sharepoint nasıl ayarladığınıza bağlı olarak yukarı ve bu gerektirebilir dosya ekleme kişinin izinleri:

* ek adımlar - dosya ekleme kişi gereken tüm izinlere sahiptir.
* her ikisi de adımlarla [Bilgi Bankası Yöneticisi](#knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal) ve [Active Directory Yöneticisi](#active-directory-manager-grant-file-read-access-to-qna-maker).

Aşağıda listelenen adımları bakın. 

### <a name="knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal"></a>Bilgi Bankası Yöneticisi: soru-cevap Oluşturucu Portalı'nda Sharepoint veri kaynağı ekleme

Zaman **soru-cevap Oluşturucu manager** güvenli bir Sharepoint belge, bir Active Directory Yöneticisi'ni tamamlamak için gereken izin isteği Bilgi Bankası manager başlatırken bir Bilgi Bankası'na ekler.

Bir Active Directory hesabı için kimlik doğrulaması için bir açılır pencere isteği başlar. 

![Hesabınızda kimlik doğrulaması](../media/add-sharepoint-datasources/authenticate-user-account.png)

Soru-cevap Oluşturucu yöneticisi hesabı seçtikten sonra Active Directory Yöneticisi soru-cevap Oluşturucu, Sharepoint kaynak (soru-cevap Oluşturucu Yöneticisi değil) uygulama erişime izin vermek gerektiğini bir uyarı alırsınız. Her Sharepoint kaynak, ancak bu kaynağa her belgede Bunu yapmak Active Directory Yöneticisi gerekir. 

### <a name="active-directory-manager-grant-file-read-access-to-qna-maker"></a>Active directory Yöneticisi: dosya okuma yetkisi vermek için soru-cevap Oluşturucu

Active Directory Yöneticisi'ni (soru-cevap Oluşturucu Yöneticisi değil), soru-cevap Oluşturucu'ı seçerek Sharepoint kaynağa erişmek için erişim gerektiğinde [bu bağlantıyı](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=Files.Read%20Files.Read.All%20Sites.Read.All%20User.Read%20User.ReadBasic.All%20profile%20openid%20email&client_id=c2c11949-e9bb-4035-bda8-59542eb907a6&redirect_uri=https%3A%2F%2Fwww.qnamaker.ai%3A%2FCreate&state=68) soru-cevap Oluşturucu portalı Sharepoint enterprise uygulamanın dosya okuma yetkisi vermek için izinler. 

![Azure Active Directory Yöneticisi etkileşimli olarak izin verir](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)

<!--
The Active Directory manager must grant QnA Maker access either by application name, `QnAMakerPortalSharepoint`, or by application ID, `c2c11949-e9bb-4035-bda8-59542eb907a6`. 
-->
<!--
### Grant access from the interactive pop-up window 

The Active Directory manager will get a pop-up window requesting permissions to the `QnAMakerPortalSharepoint` app. The pop-up window includes the QnA Maker Manager email address that initiated the request, an `App Info` link to learn more about **QnAMakerPortalSharepoint**, and a list of permissions requested. Select **Accept** to provide those permissions. 

![Azure Active Directory manager grants permission interactively](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)
-->
<!--

### Grant access from the App Registrations list

1. The Active Directory manager signs in to the Azure portal and opens **[App registrations list](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade)**. 

1. Search for and select the **QnAMakerPortalSharepoint** app. Change the second filter box from **My apps** to **All apps**. The app information will open on the right side.

    ![Select QnA Maker app in App registrations list](../media/add-sharepoint-datasources/select-qna-maker-app-in-app-registrations.png)

1. Select **Settings**.

    [![Select Settings in the right-side blade](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png)](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png#lightbox)

1. Under **API access**, select **Required permissions**. 

    ![Select 'Settings', then under 'API access', select 'Required permission'](../media/add-sharepoint-datasources/select-required-permissions-in-settings-blade.png)

1. Do not change any settings in the **Enable Access** window. Select **Grant Permission**. 

    [![Under 'Grant Permission', select 'Yes'](../media/add-sharepoint-datasources/grant-app-required-permissions.png)](../media/add-sharepoint-datasources/grant-app-required-permissions.png#lightbox)

1. Select **YES** in the pop-up confirmation windows. 

    ![Grant required permissions](../media/add-sharepoint-datasources/grant-required-permissions.png)
-->
### <a name="grant-access-from-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim Merkezi'nden erişim izni ver

1. Active Directory Yöneticisi, Azure portalında oturum açtığında ve açılır  **[kurumsal uygulamalar](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps)**. 

1. Arama `QnAMakerPortalSharepoint` soru-cevap Oluşturucu uygulamayı seçin. 

    [![Kurumsal uygulamalar listesinde QnAMakerPortalSharepoint arayın](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png)](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png#lightbox)

1. Altında **güvenlik**Git **izinleri**. Seçin **kuruluş için yönetici onayı vermek**. 

    [![Kimliği doğrulanmış kullanıcı için Active Directory Yöneticisi seçin](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png)](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png#lightbox)

1. İzinleri için Active Directory izinlerini vermek için bir oturum açma hesabı seçin. 


  
<!--

## Add Sharepoint data source with APIs

You need to get the Sharepoint file's URI before adding it to QnA Maker. 

## Get Sharepoint File URI

Use the following steps to transform the Sharepoint URL into a sharing token.

1. Encode the URL using [base64](https://en.wikipedia.org/wiki/Base64). 

1. Convert the base64-encoded result to an unpadded base64url format with the following character changes. 

    * Remove the equal character, `=` from the end of the value. 
    * Replace `/` with `_`. 
    * Replace `+` with `-`. 
    * Append `u!` to be beginning of the string. 

1. Sign in to Graph explorer and run the following query, where `sharedURL` is ...:

    ```
    https://graph.microsoft.com/v1.0/shares/<sharedURL>/driveitem
    ```

    Get the **@microsoft.graph.downloadUrl** and use this as `fileuri` in the QnA Maker APIs.

### Add or update a Sharepoint File URI to your knowledge base

Use the **@microsoft.graph.downloadUrl** from the previous section as the `fileuri` in the QnA Maker API for [adding a knowledge base](https://go.microsoft.com/fwlink/?linkid=2092179) or [updating a knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). The following fields are mandatory: name, fileuri, filename, source.

```
{
    "name": "Knowledge base name",
    "files": [
        {
            "fileUri": "<@microsoft.graph.downloadURL>",
            "fileName": "filename.xlsx",
            "source": "<sharepoint link>"
        }
    ],
    "urls": [],
    "users": [],
    "hostUrl": "",
    "qnaList": []
}
```



## Remove QnA Maker app from Sharepoint authorization

1. Use the steps in the previous section to find the Qna Maker app in the Active Directory admin center. 
1. When you select the **QnAMakerPortalSharepoint**, select **Overview**. 
1. Select **Delete** to remove permissions. 

-->

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankanızı üzerinde işbirliği yapın](collaborate-knowledge-base.md)