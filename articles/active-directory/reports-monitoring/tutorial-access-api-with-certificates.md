---
title: Sertifikalarla Azure AD raporlama API'sini kullanarak öğretici verileri alma | Microsoft Docs
description: Bu öğretici, kullanıcı müdahalesi olmadan dizinlerden veri almak üzere sertifika kimlik bilgileriyle Azure AD raporlama API'sini kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e006111cce7f53ff87f1c6d60b2a5147da02e1e
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438997"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Öğretici: Sertifikalarla Azure Active Directory raporlama API’sini kullanarak veri alma

[Azure Active Directory (Azure AD) raporlama API'leri](concept-reporting-api.md), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz. Azure AD raporlama API'sini kullanıcı müdahalesi olmadan erişmek istiyorsanız, erişiminizi sertifikalarını kullanacak şekilde yapılandırmanız gerekir.

Bu öğreticide, bir test sertifikası raporlama için MS Graph API'sine erişmek için nasıl kullanılacağını öğrenin. Bir üretim ortamında test sertifikalarınızı kullanımı önerilmemektedir. 

## <a name="prerequisites"></a>Önkoşullar

1. Oturum açma verilerine erişmek için bir premium (ö1/ö2) lisansına sahip bir Azure Active Directory kiracınız olduğundan emin olun. Bkz: [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için. Yükseltme öncesinde tüm etkinlikleri veri yoksa, birkaç gün raporlarda görünmesi için bir premium lisansı yükselttikten sonra verilerin gerektiğine dikkat edin. 

2. Oluşturma veya geçiş için bir kullanıcı hesabında **genel yönetici**, **Güvenlik Yöneticisi**, **güvenlik okuyucusu** veya **rapor okuyucu** Kiracı için rolü. 

3. Tamamlamak [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](howto-configure-prerequisites-for-reporting-api.md). 

4. İndirme ve yükleme [Azure AD PowerShell V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md).

5. Yükleme [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Bu modül aşağıdaki çeşitli yardımcı program cmdlet'lerini sağlar:
    - ADAL kimlik doğrulaması için gereken kitaplıkları
    - ADAL kullanarak kullanıcı, uygulama anahtarları ve sertifikalardan erişim belirteçleri
    - Disk belleğine alınmış Graph API işleme sonuçları

6. İlk kez çalıştırma modülü ise **yükleme MSCloudIdUtilsModule**, aksi takdirde kullanarak içe aktarın **Import-Module** Powershell komutu. Oturumunuz şu ekran gibi görünmelidir: ![Windows Powershell](./media/tutorial-access-api-with-certificates/module-install.png)
  
7. Kullanım **New-SelfSignedCertificate** bir test sertifikası oluşturmak için Powershell komutu.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

8. Kullanım **sertifika verme** bir sertifika dosyasına aktarmak için komutu.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Sertifikalarla Azure Active Directory raporlama API’sini kullanarak veri alma

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
  
7. Artık, bir erişim MS Graph API'si için bu sertifikayı kullanarak belirteci edinebilirsiniz. Kullanım **Get-MSCloudIdMSGraphAccessTokenFromCert** uygulama kimliği ve önceki adımda alınan parmak izini geçirme MSCloudIdUtils PowerShell modülünden cmdlet'i. 

   ![Azure portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

8. Erişim belirteci Powershell betiğinizde Graph API'yi sorgulama için kullanın. Kullanım **Invoke-MSCloudIdMSGraphQuery** oturum açma bilgilerini ve directoryAudits uç nokta Numaralandırılacak MSCloudIDUtils cmdlet'inden. Bu cmdlet, çok sayfalı sonuçları işer ve sonuçları PowerShell işlem hattına gönderir.

9. Denetim günlüklerini almak için directoryAudits uç noktasını sorgulayın. 
   ![Azure portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

10. Oturum açma günlükleri almak için oturum açma bilgilerini uç noktasını sorgulayın.
    ![Azure portal](./media/tutorial-access-api-with-certificates/query-signins.png)

11. Artık bu veri bir CSV'ye göndermeye ve bir SIEM sistemine kaydetmeye seçebilirsiniz. Ayrıca, betiğinizi zamanlanmış bir göreve kaydırarak, uygulama anahtarlarını kaynak kodda depolamak zorunda kalmadan kiracınızdan Azure AD verilerini düzenli olarak alabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Raporlama API'leriyle ilgili ilk izlenim elde edin](concept-reporting-api.md)
* [Denetim API'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma etkinliği raporunu API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
