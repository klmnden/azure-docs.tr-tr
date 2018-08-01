---
title: Sertifikalarla Azure AD Raporlama API’sini kullanarak veri alma | Microsoft Docs
description: Kullanıcı müdahalesi olmadan dizinlerden veri almak üzere sertifika kimlik bilgileriyle Azure AD Raporlama API’sini kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 44ec19721ba59898915f6547231fb586cb44eb30
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389761"
---
# <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Sertifikalarla Azure Active Directory raporlama API’sini kullanarak veri alma

[Azure Active Directory (Azure AD) raporlama API'leri](active-directory-reporting-api-getting-started-azure-portal.md), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz. Azure AD raporlama API'sini kullanıcı müdahalesi olmadan erişmek istiyorsanız, erişiminizi sertifikalarını kullanacak şekilde yapılandırabilirsiniz.

Bu, aşağıdaki adımları içerir:

1. [Önkoşulları yükle](#install-prerequisites)
2. [Uygulamanızdaki sertifikayı kaydetme](#register-the-certificate-in-your-app)
3. [MS Graph API'si için bir erişim belirteci alma](#get-an-access-token-for-ms-graph-api)
4. [MS Graph API uç noktaları sorgulama](#query-the-ms-graph-api-endpoints)


## <a name="install-prerequisites"></a>Ön koşulları yükleme

1. İlk olarak, tamamladığınızdan emin olun [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](active-directory-reporting-api-prerequisites-azure-portal.md). 

2. İndirme ve yükleme yönergeleri izleyerek, Azure AD Powershell V2 [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md)

3. Gelen MSCloudIDUtils yükleme [PowerShellGallery - MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Bu modül aşağıdaki çeşitli yardımcı program cmdlet'lerini sağlar:
    - ADAL kimlik doğrulaması için gereken kitaplıkları
    - ADAL kullanarak kullanıcı, uygulama anahtarları ve sertifikalardan erişim belirteçleri
    - Disk belleğine alınmış Graph API işleme sonuçları

4. İlk kez çalıştırma modülü ise **yükleme MSCloudIdUtilsModule** Kurulumu tamamlamak için Aksi takdirde, yalnızca kullanarak içeri aktarabilirsiniz **Import-Module** Powershell komutu.

Oturumunuz şu ekran gibi görünmelidir:

  ![Windows PowerShell](./media/active-directory-reporting-api-with-certificates/module-install.png)

## <a name="register-the-certificate-in-your-app"></a>Uygulamanızdaki sertifikayı kaydetme

1. İlk olarak, uygulama kayıt sayfasına gidin. Giderek bunu yapabilirsiniz [Azure portalında](https://portal.azure.com)a **Azure Active Directory**, ardından **uygulama kayıtları** ve uygulamanızı listeden seçerek. 

2. Ardından, adımları [sertifikanızı Azure AD'ye kaydetme](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-certificate-credentials#register-your-certificate-with-azure-ad) uygulama için. 

3. Uygulama kimliği ve uygulamanızla birlikte yalnızca kayıtlı sertifikanın parmak izini unutmayın. Portalda, uygulama sayfanızdan parmak izi bulmak için Git **ayarları** tıklatıp **anahtarları**. Parmak izi altında olur **ortak anahtarları** listesi.

  
## <a name="get-an-access-token-for-ms-graph-api"></a>MS Graph API'si için bir erişim belirteci alma

MS Graph API'si için bir erişim belirteci almak için kullanın **Get-MSCloudIdMSGraphAccessTokenFromCert** MSCloudIdUtils PowerShell modülünden cmdlet'i. 

>[!NOTE]
>Uygulama Kimliğini (ClientID olarak da bilinir) yanı sıra, sertifikanın sertifika parmak izini (CurrentUser veya LocalMachine sertifika deposuna) bilgisayarınızın sertifika deposunda yüklü özel anahtarla kullanmanız gerekir.
>

 ![Azure portal](./media/active-directory-reporting-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Graph API'sini çağırmak için erişim belirteci kullanma

Artık, Graph API'sini sorgulamak için Powershell betiğinizde erişim belirteci kullanabilirsiniz. Aşağıda bir örnek kullanarak **Invoke-MSCloudIdMSGraphQuery** oturum açma bilgilerini veya diectoryAudits uç nokta Numaralandırılacak MSCloudIDUtils cmdlet'inden. Bu cmdlet, çok sayfalı sonuçları işer ve ardından PowerShell işlem hattına gönderir.

### <a name="query-the-directoryaudits-endpoint"></a>Sorgu DirectoryAudits uç noktası
 ![Azure portal](./media/active-directory-reporting-api-with-certificates/query-directoryAudits.png)

 ### <a name="query-the-signins-endpoint"></a>Sorgu oturum açma bilgilerini uç noktası
 ![Azure portal](./media/active-directory-reporting-api-with-certificates/query-signins.png)

Artık bu veri bir CSV'ye göndermeye ve bir SIEM sistemine kaydetmeye seçebilirsiniz. Ayrıca, betiğinizi zamanlanmış bir göreve kaydırarak, uygulama anahtarlarını kaynak kodda depolamak zorunda kalmadan kiracınızdan Azure AD verilerini düzenli olarak alabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [Raporlama API'leriyle ilgili ilk izlenim elde edin](active-directory-reporting-api-getting-started-azure-portal.md)
* [Denetim API'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma etkinliği raporunu API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



