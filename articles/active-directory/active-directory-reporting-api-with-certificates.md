---
title: Sertifikalarla Azure AD Raporlama API’sini kullanarak veri alma | Microsoft Docs
description: Kullanıcı müdahalesi olmadan dizinlerden veri almak üzere sertifika kimlik bilgileriyle Azure AD Raporlama API’sini kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/07/2018
ms.author: ramical
ms.openlocfilehash: 54e661284c539b835089e858ba7b5e0016e89a83
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Sertifikalarla Azure Active Directory raporlama API’sini kullanarak veri alma

[Azure Active Directory (Azure AD) raporlama API'leri](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Kullanıcı müdahalesi olmadan Azure AD Raporlama API'sine erişmek istiyorsanız, erişiminizi sertifika kullanacak şekilde yapılandırabilirsiniz

Bu makalede:

- Sertifikaları kullanarak Azure AD raporlama API'sine erişmeniz için gereken adımlar sağlanır.
- [Azure Active Directory raporlama API'sine erişim önkoşullarını](active-directory-reporting-api-prerequisites-azure-portal.md) tamamladığınız varsayılır. 


Sertifikalarla raporlama API'sine erişmek için aşağıdaki işlemleri yapmalısınız:

1. Önkoşulları yükleme
2. Uygulamanızdaki sertifikayı ayarlama 
3. İzinleri verme
4. Bir erişim belirteci alma




Kaynak kodu hakkında bilgi için bkz. [Rapor API Modülünden Yararlanma](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils). 

## <a name="install-prerequisites"></a>Ön koşulları yükleme

Azure AD PowerShell V2 ve AzureADUtils modülünün yüklü olması gerekir.

1. [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md) içindeki yönergeleri izleyerek Azure AD PowerShell V2’yi indirip yükleyin.

2. [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1) dizininden Azure AD Yardımcı Programları modülünü indirin. 
  Bu modül aşağıdaki çeşitli yardımcı program cmdlet'lerini sağlar:
    - Nuget kullanarak en son ADAL sürümü
    - ADAL kullanarak kullanıcı, uygulama anahtarları ve sertifikalardan erişim belirteçleri
    - Disk belleğine alınmış Graph API işleme sonuçları

**Azure AD Yardımcı Programlar modülünü yüklemek için:**

1. Yardımcı programlar modülünün kaydedileceği dizini oluşturun (örneğin, c:\azureAD) ve modülü GitHub’dan indirin.
2. Bir PowerShell oturumu açın ve yeni oluşturduğunuz dizine gidin. 
3. Modülü içeri aktarın ve Install-AzureADUtilsModule cmdlet’ini kullanarak PowerShell modülüne yükleyin. 

Oturum şu ekran gibi görünmelidir:

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

## <a name="set-the-certificate-in-your-app"></a>Uygulamanızdaki sertifikayı ayarlama

**Uygulamanızdaki sertifikayı ayarlamak için:**

1. Azure Portal'dan uygulamanızın [Nesne Kimliğini alın](active-directory-reporting-api-prerequisites-azure-portal.md#get-your-applications-client-id). 

  ![Azure portalına](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Bir PowerShell oturumu açın ve Connect-AzureAD cmdlet'ini kullanarak Azure AD’ye bağlanın.

  ![Azure portalına](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. AzureADUtils içindeki New-AzureADApplicationCertificateCredential cmdlet’ini kullanarak bir sertifika kimlik bilgisi ekleyin. 

>[!Note]
>Daha önce yakaladığınız uygulama Nesne Kimliğinin yanı sıra sertifika nesnesini (Cert: sürücüsünü kullanarak alın) belirtmeniz gerekir.
>


  ![Azure portalına](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Erişim belirteci almak için AzureADUtils içindeki **Get-AzureADGraphAPIAccessTokenFromCert** cmdlet’ini kullanın. 

>[!NOTE]
>Son bölümde kullandığınız Nesne Kimliği yerine Uygulama Kimliğini kullanmanız gerekir.
>

 ![Azure portalına](./media/active-directory-report-api-with-certificates/application-id.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Graph API'sini çağırmak için erişim belirteci kullanma

Şimdi betiği oluşturabilirsiniz. Aşağıda, AzureADUtils içindeki **Invoke-AzureADGraphAPIQuery** cmdlet’inin nasıl kullanıldığını gösteren bir örnek yer alır. Bu cmdlet, çok sayfalı sonuçları işer ve ardından PowerShell işlem hattına gönderir. 

 ![Azure portalına](./media/active-directory-report-api-with-certificates/script-completed.png)

Artık bir CSV'ye göndermeye ve bir SIEM sistemine kaydetmeye hazırsınız. Ayrıca, betiğinizi zamanlanmış bir göreve kaydırarak, uygulama anahtarlarını kaynak kodda depolamak zorunda kalmadan kiracınızdan Azure AD verilerini düzenli olarak alabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Raporlama API'leriyle ilgili ilk izlenim elde edin](active-directory-reporting-api-getting-started-azure-portal.md#explore)

- [Kendi çözümünüzü oluşturun](active-directory-reporting-api-getting-started-azure-portal.md#customize)




