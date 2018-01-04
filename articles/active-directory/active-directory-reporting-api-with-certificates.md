---
title: "Sertifikalarla Azure AD Raporlama API’sini kullanarak veri alma | Microsoft Docs"
description: "Kullanıcı müdahalesi olmadan dizinlerden veri almak üzere sertifika kimlik bilgileriyle Azure AD Raporlama API’sini kullanmayı açıklar."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/08/2017
ms.author: ramical
ms.openlocfilehash: 4900e47084256ad6c85886f7ba363399678da9aa
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="get-data-using-the-azure-ad-reporting-api-with-certificates"></a>Sertifikalarla Azure AD Raporlama API’sini kullanarak veri alma
Bu makalede, kullanıcı müdahalesi olmadan dizinlerden veri almak üzere sertifika kimlik bilgileriyle Azure AD Raporlama API’sini kullanma işlemi ele alınmaktadır. 

## <a name="use-the-azure-ad-reporting-api"></a>Azure AD Raporlama API’sini kullanma 
Azure AD Raporlama API'si aşağıdaki adımları tamamlamanızı gerektirir:
 *  Ön koşulları yükleme
 *  Uygulamanızdaki sertifikayı ayarlama
 *  Bir erişim belirteci alma
 *  Graph API'sini çağırmak için erişim belirteci kullanma

Kaynak kodu hakkında bilgi için bkz. [Rapor API Modülünden Yararlanma](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils). 

### <a name="install-prerequisites"></a>Ön koşulları yükleme
Azure AD PowerShell V2 ve AzureADUtils modülünün yüklü olması gerekir.

1. [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md) içindeki yönergeleri izleyerek Azure AD PowerShell V2’yi indirip yükleyin.
2. [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1) dizininden Azure AD Yardımcı Programları modülünü indirin. 
  Bu modül aşağıdaki çeşitli yardımcı program cmdlet'lerini sağlar:
   * Nuget kullanarak en son ADAL sürümü
   * ADAL kullanarak kullanıcı, uygulama anahtarları ve sertifikalardan erişim belirteçleri
   * Disk belleğine alınmış Graph API işleme sonuçları

**Azure AD Yardımcı Programlar modülünü yüklemek için:**

1. Yardımcı programlar modülünün kaydedileceği dizini oluşturun (örneğin, c:\azureAD) ve modülü GitHub’dan indirin.
2. Bir PowerShell oturumu açın ve yeni oluşturduğunuz dizine gidin. 
3. Modülü içeri aktarın ve Install-AzureADUtilsModule cmdlet’ini kullanarak PowerShell modülüne yükleyin. 

Oturum şu ekran gibi görünmelidir:

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-the-certificate-in-your-app"></a>Uygulamanızdaki sertifikayı ayarlama
1. Zaten bir uygulamanız varsa, Azure portalından uygulamanın nesne kimliğini alın. 

  ![Azure portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Bir PowerShell oturumu açın ve Connect-AzureAD cmdlet'ini kullanarak Azure AD’ye bağlanın.

  ![Azure portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. AzureADUtils içindeki New-AzureADApplicationCertificateCredential cmdlet’ini kullanarak bir sertifika kimlik bilgisi ekleyin. 

>[!Note]
>Daha önce yakaladığınız uygulama Nesne Kimliğinin yanı sıra sertifika nesnesini (Cert: sürücüsünü kullanarak alın) belirtmeniz gerekir.
>


  ![Azure portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a>Bir erişim belirteci alma

Erişim belirteci almak için AzureADUtils içindeki Get-AzureADGraphAPIAccessTokenFromCert cmdlet’ini kullanın. 

>[!NOTE]
>Son bölümde kullandığınız Nesne Kimliği yerine Uygulama Kimliğini kullanmanız gerekir.
>

 ![Azure portal](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-the-access-token-to-call-the-graph-api"></a>Graph API'sini çağırmak için erişim belirteci kullanma

Şimdi komut dosyasını oluşturabilirsiniz. AzureADUtils içindeki Invoke-AzureADGraphAPIQuery cmdlet’inin bir örneği aşağıda verilmiştir. Bu cmdlet, çok sayfalı sonuçları işer ve ardından PowerShell işlem hattına gönderir. 

 ![Azure portal](./media/active-directory-report-api-with-certificates/script-completed.png)

Artık bir CSV'ye göndermeye ve bir SIEM sistemine kaydetmeye hazırsınız. Ayrıca, betiğinizi zamanlanmış bir göreve kaydırarak, uygulama anahtarlarını kaynak kodda depolamak zorunda kalmadan kiracınızdan Azure AD verilerini düzenli olarak alabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
[Azure kimlik yönetimi ile ilgili temel bilgiler](https://docs.microsoft.com/azure/active-directory/fundamentals-identity)<br>



