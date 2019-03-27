---
title: Uygulama proxy'si tanımlama bilgisi ayarları - Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) erişim uygulama proxy'si aracılığıyla şirket için erişim ve oturum tanımlama bilgileri vardır. Bu makalede, tanımlama bilgisi ayarları yapılandırmak ve kullanmak nasıl bulabilirsiniz.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: celested
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06fd83ee815e9e207c1fa5a1c6767280122c4d0c
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482700"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>Azure Active Directory'de şirket içi uygulamalara erişmek için tanımlama bilgisi ayarları

Azure Active Directory (Azure AD) erişim uygulama proxy'si aracılığıyla şirket için erişim ve oturum tanımlama bilgileri vardır. Uygulama proxy'si tanımlama bilgisi ayarları kullanmayı öğrenin. 

## <a name="what-are-the-cookie-settings"></a>Tanımlama bilgisi ayarları nelerdir?

[Uygulama proxy'si](application-proxy.md) aşağıdaki erişim ve oturum tanımlama bilgisi ayarları kullanır.

| Tanımlama bilgisi ayarı | Varsayılan | Açıklama | Öneriler |
| -------------- | ------- | ----------- | --------------- |
| Yalnızca HTTP Tanımlama Bilgisi Kullan | **Hayır** | **Evet** HTTPOnly bayrağı HTTP yanıt üstbilgileri dahil etmek uygulama proxy'si sağlar. Bu bayrak ek güvenlik avantajları vardır, örneğin, (kopyalama veya tanımlama bilgilerini değiştirme istemci tarafı komut dosyası CSS) engeller.<br></br><br></br>Biz yalnızca HTTP ayarı desteklenen önce uygulama proxy'si şifrelenir ve tanımlama bilgileri değişiklik karşı korumak için güvenli bir SSL kanalı üzerinden aktarılan. | Kullanım **Evet** ek güvenlik avantajları nedeniyle.<br></br><br></br>Kullanım **Hayır** istemciler veya oturum tanımlama bilgisinin erişim gerektiren kullanıcı aracıları. Örneğin, **Hayır** uygulama proxy'si aracılığıyla bir Uzak Masaüstü Ağ Geçidi sunucusuna bağlanan bir RDP veya MTSC istemcinin için.|
| Güvenli Tanımlama Bilgisi Kullan | **Hayır** | **Evet** güvenli dahil etmek uygulama proxy'si sağlayan HTTP yanıt üst bilgilerini bayrağı. Güvenli tanımlama bilgileri, bir TLS güvenli kanal gibi HTTPS üzerinden tanımlama bilgilerini ileterek güvenlik geliştirir. Bu, tanımlama bilgileri nedeniyle iletim tanımlama bilgisinin yetkisiz üçüncü taraflarca düz metin olarak gözlemlenmekte öğesinden engeller. | Kullanım **Evet** ek güvenlik avantajları nedeniyle.|
| Kalıcı Tanımlama Bilgisi Kullan | **Hayır** | **Evet** web tarayıcı kapatıldığında geçmemesi için kendi erişim tanımlama bilgilerini ayarlamak uygulama proxy'si sağlar. Kalıcılık, erişim belirtecinin süresi dolana kadar veya kullanıcının kalıcı tanımlama bilgilerini elle silene kadar sürer. | Kullanım **Hayır** kimliği doğrulanmış kullanıcılar tutulmasına ilişkin güvenlik riski nedeniyle.<br></br><br></br>Yalnızca kullanmanızı öneririz **Evet** tanımlama bilgilerini işlemler arasında paylaşamaz eski uygulamalar için. Kalıcı tanımlama bilgileri kullanmak yerine işlemleri arasında paylaşım tanımlama bilgilerini işlemek için uygulamanızı güncelleştirmeniz daha iyidir. Örneğin, bir kullanıcı bir SharePoint sitesinden Gezgini görünümünde Office belgeleri açmaya izin verecek şekilde kalıcı tanımlama bilgileri gerekebilir. Erişim tanımlama bilgilerini tarayıcı, explorer işlemi ve Office işlem arasında paylaşılmaz kalıcı tanımlama bilgileri bu işlemi başarısız. |

## <a name="set-the-cookie-settings---azure-portal"></a>Azure portalı - tanımlama bilgisi ayarları ayarlayın
Azure portalını kullanarak tanımlama bilgisi ayarları ayarlamak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**.
3. Bir tanımlama bilgisi ayarı etkinleştirmek istediğiniz uygulamayı seçin.
4. Tıklayın **uygulama proxy'si**.
5. Altında **ek ayarlar**, tanımlama bilgisi ayarının **Evet** veya **Hayır**.
6. Tıklayın **Kaydet** yaptığınız değişiklikleri uygulamak için. 

## <a name="view-current-cookie-settings---powershell"></a>Geçerli tanımlama bilgisi ayarları - PowerShell görüntüleyin

Uygulama için geçerli tanımlama bilgisi ayarları görmek için bu PowerShell komutunu kullanın:  

```powershell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## <a name="set-cookie-settings---powershell"></a>Tanımlama bilgisi ayarları - PowerShell ayarlama

Aşağıdaki PowerShell komutlarında ```<ObjectId>``` objectID uygulamanın olduğu. 

**Yalnızca HTTP tanımlama bilgisi** 

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Güvenli bir tanımlama bilgisi**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Kalıcı tanımlama bilgileri**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```
