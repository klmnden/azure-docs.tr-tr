---
title: Sertifikalarla Azure AD raporlama API'sini kullanarak öğretici verileri alma | Microsoft Docs
description: Bu öğretici, kullanıcı müdahalesi olmadan dizinlerden veri almak üzere sertifika kimlik bilgileriyle Azure AD raporlama API'sini kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 5c54af76fc1e145ea062c6bcb37f354a7de94781
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364185"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Öğretici: Azure Active Directory'ı sertifikalarla raporlama API'sini kullanarak veri alma

[Azure Active Directory (Azure AD) raporlama API'leri](concept-reporting-api.md), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz. Azure AD raporlama API'sini kullanıcı müdahalesi olmadan erişmek istiyorsanız, erişiminizi sertifikalarını kullanacak şekilde yapılandırmanız gerekir.

Bu öğreticide, bir test sertifikası oluşturma ve raporlama için MS Graph API'sine erişmek için kullanma konusunda bilgi edinin. Bir üretim ortamında test sertifikası kullanımı önerilmemektedir. 

## <a name="prerequisites"></a>Önkoşullar

1. İlk olarak [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](howto-configure-prerequisites-for-reporting-api.md). 

2. İndirme ve yükleme [Azure AD PowerShell V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md).

3. Yükleme [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Bu modül aşağıdaki çeşitli yardımcı program cmdlet'lerini sağlar:
    - ADAL kimlik doğrulaması için gereken kitaplıkları
    - ADAL kullanarak kullanıcı, uygulama anahtarları ve sertifikalardan erişim belirteçleri
    - Disk belleğine alınmış Graph API işleme sonuçları

4. İlk kez çalıştırma modülü ise **yükleme MSCloudIdUtilsModule**, aksi takdirde kullanarak içe aktarın **Import-Module** Powershell komutu.

Oturumunuz şu ekran gibi görünmelidir:

  ![Windows PowerShell](./media/tutorial-access-api-with-certificates/module-install.png)
  
## <a name="create-a-test-certificate"></a>Bir test sertifikası oluştur

1. Kullanım **New-SelfSignedCertificate** bir test sertifikası oluşturmak için Powershell komutu.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

2. Kullanım **sertifika verme** bir sertifika dosyasına aktarmak için komutu.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="register-the-certificate-in-your-app"></a>Uygulamanızdaki sertifikayı kaydetme

1. Gidin [Azure portalında](https://portal.azure.com)seçin **Azure Active Directory**, ardından **uygulama kayıtları** ve uygulamanızı listeden seçin. 

2. Seçin **ayarları** > **anahtarları** seçip **ortak anahtarı karşıya**.

3. Önceki adımdaki sertifika dosyasını seçin ve seçin **Kaydet**. 

4. Uygulama kimliği ve uygulamanızla birlikte yalnızca kayıtlı sertifikanın parmak izini unutmayın. Portalda, uygulama sayfanızdan parmak izi bulmak için Git **ayarları** tıklatıp **anahtarları**. Parmak izi altında olur **ortak anahtarları** listesi.

5. Uygulama bildirimi satır içi bildirim düzenleyicisinde açın ve Değiştir *keyCredentials* aşağıdaki şemayı kullanarak yeni sertifika bilgisi özelliği. 

   ```
   "keyCredentials": [
        {
            "customKeyIdentifier": "$base64Thumbprint", //base64 encoding of the certificate hash
            "keyId": "$keyid", //GUID to identify the key in the manifest
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value":  "$base64Value" //base64 encoding of the certificate raw data
        }
    ]
   ```

6. Bildirim kaydedin. 
  
## <a name="get-an-access-token-for-ms-graph-api"></a>MS Graph API'si için bir erişim belirteci alma

1. Kullanım **Get-MSCloudIdMSGraphAccessTokenFromCert** uygulama kimliği ve önceki adımda alınan parmak izini geçirme MSCloudIdUtils PowerShell modülünden cmdlet'i. 

 ![Azure portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Graph API'sini çağırmak için erişim belirteci kullanma

1. Artık, Graph API'sini sorgulamak için Powershell betiğinizde erişim belirteci kullanabilirsiniz. Kullanım **Invoke-MSCloudIdMSGraphQuery** oturum açma bilgilerini ve directoryAudits uç nokta Numaralandırılacak MSCloudIDUtils cmdlet'inden. Bu cmdlet, çok sayfalı sonuçları işer ve sonuçları PowerShell işlem hattına gönderir.

2. Denetim günlüklerini almak için directoryAudits uç noktasını sorgulayın. 
 ![Azure portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

3. Oturum açma günlükleri almak için oturum açma bilgilerini uç noktasını sorgulayın.
 ![Azure portal](./media/tutorial-access-api-with-certificates/query-signins.png)

4. Artık bu veri bir CSV'ye göndermeye ve bir SIEM sistemine kaydetmeye seçebilirsiniz. Ayrıca, betiğinizi zamanlanmış bir göreve kaydırarak, uygulama anahtarlarını kaynak kodda depolamak zorunda kalmadan kiracınızdan Azure AD verilerini düzenli olarak alabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Raporlama API'leriyle ilgili ilk izlenim elde edin](concept-reporting-api.md)
* [Denetim API'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma etkinliği raporunu API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



