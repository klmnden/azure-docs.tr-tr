---
title: Uygulama proxy'si tanımlama bilgisi ayarları - Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) erişim uygulama proxy'si aracılığıyla şirket için erişim ve oturum tanımlama bilgileri vardır. Bu makalede, tanımlama bilgisi ayarları yapılandırmak ve kullanmak nasıl bulabilirsiniz.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: concept
ms.date: 01/16/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: 6bd28ba3c7f94a3eb398d0819f1d8df42a765923
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55960023"
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

<!---

## View current cookie settings - PowerShell

To see the current cookie settings for the application, use this PowerShell command:  

```PowerShell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## Set cookie settings - PowerShell

In the following PowerShell commands, ```<ObjectId>``` is the ObjectId of the application. 

**Http-Only Cookie** 

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Secure Cookie**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Persistent Cookies**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```

-->
