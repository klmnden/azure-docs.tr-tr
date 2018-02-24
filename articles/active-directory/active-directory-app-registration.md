---
title: "Azure Active Directory Uygulama kaydı | Microsoft Docs"
description: "Bu makalede, bir uygulamayı Azure Active Directory ile kaydetmek için Azure Portalı'nı kullanmayı açıklar"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mtillman
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: priyamo
ms.reviewer: elisol
ms.openlocfilehash: 8f5d4ba82fcf3c963373b0e90b707a7d86fc0fea
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınızın ile uygulamanızı kaydetme

Uygulamanız Azure Active Directory (Azure AD) Kiracı ile kaydetmek için Azure portalını kullanabilirsiniz. Bu uygulama için bir uygulama kimliği oluşturur ve belirteçleri almak etkinleştirir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD kiracınız, sayfanın sağ üst köşesinde hesabınızı seçerek belirleyin.
3. Sol Gezinti Bölmesi'nde seçin **tüm hizmetleri**, tıklatın **uygulama kayıtlar**, tıklatıp **Ekle**.
4. Komut istemlerini izleyin ve yeni bir uygulama oluşturun. Belirli örnekler web uygulamaları veya yerel uygulamaları için isterseniz, kullanıma bizim [quickstarts](active-directory-developers-guide.md).
  * Web uygulamaları için sağlamak **oturum açma URL'si**, kullanıcılar, örneğin,'burada oturum açabilir, uygulamanızın temel URL olduğu `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Yerel uygulamalar için sağlamaya bir **yeniden yönlendirme URI'si**, hangi belirteci yanıtları döndürmek için Azure AD kullanır. Uygulamanıza özgü bir değer girin, örn. `http://MyFirstAADApp`
5. Azure AD kaydı tamamladıktan sonra uygulamanızı bir benzersiz istemci tanımlayıcısı uygulama kimliği atar.

## <a name="update-application-settings-from-the-azure-portal"></a>Azure Portalı'ndan uygulama ayarlarını güncelleştirme

Azure Portalı'nı kullanarak var olan bir uygulamanın ayarlarını kolayca değiştirebilirsiniz. Örneğin, burada Azure AD belirteci yanıtları sorunları olan bir yanıt URL'si yapılandırmak isteyebilirsiniz. Diğer uygulamalara izinler örneği için yapılandırmak isteyebilirsiniz, uygulamanızın Microsoft grafik API'sine erişim izin vermek için. Tüm uygulama ayarları sayfası üzerinden bunu yapabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD kiracınız, sayfanın sağ üst köşesinde hesabınızı seçerek belirleyin.
3. Sol Gezinti Bölmesi'nde seçin **tüm hizmetleri**, tıklatın **uygulama kayıtlar**ve uygulamanızı listeden seçin.
4. Tıklatın **ayarları** uygulama için ayarları sayfasını açın.
  * **Özellikleri** sayfası, uygulama için genel bilgileri değiştirmenize olanak sağlar. Bu, uygulama adı, oturum açma URL'si ve oturum kapatma URL'sini içerir.
  * **Yanıt URL'leri** sayfa, Azure AD belirteci yanıtları yere gönderir olan bir yanıt URL eklemenize olanak sağlar.
  * **Sahipleri** sayfası uygulama sahipleri eklemenize olanak sağlar.
  * **Gerekli izinleri** sayfası uygulama izinlerini yapılandırmanıza olanak sağlar. Örneğin, Microsoft Graph API erişmek için tıklatın **Ekle** seçip **Microsoft Graph** API seçicide sonra Örneğin gerekli izni vermeyi **dizin verilerini okuma**.
  * **Anahtarları** sayfası, uygulama parolaları eklemenize olanak sağlar. Bunu yapma için kopyaladığınızdan emin hemen oluşturulduktan sonra daha sonra kullanmak gizli yalnızca görüntülenir.

## <a name="use-the-inline-manifest-editor"></a>Satır içi bildirim Düzenleyicisi'ni kullanın

Doğrudan Azure Portalı'nda gösterilmez belirli uygulama özelliklerini değiştirmek için satır içi bildirim Düzenleyicisi'ni kullanabilirsiniz. Örneğin, uygulamanın uygulama kimliği URI'si değiştirmek veya varsayılan yetkilendirme grant kod akışı yerine OAuth2.0 örtük akışını etkinleştirmek için bunu kullanabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD kiracınız, sayfanın sağ üst köşesinde hesabınızı seçerek belirleyin.
3. Sol Gezinti Bölmesi'nde seçin **tüm hizmetleri**, tıklatın **uygulama kayıtlar**ve uygulamanızı listeden seçin.
4. Tıklatın **bildirim** satır içi bildirim Düzenleyicisi'ni açmak için uygulama sayfasından.
5. Doğrudan değişiklik bildirimine ve hazır olduğunuzda kaydedin. Alternatif olarak, bildiriminin en sık kullandığınız düzenleyicide açın ve güncelleştirilmiş bildirimi karşıya yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

1. Kullanıma [Quickstarts](active-directory-developers-guide.md) ayrıntılı talimatları uygulamaları Azure AD kullanarak kimlik doğrulaması gerçekleştirmek için.
2. Tam kod örnekleri listemiz kullanıma [GitHub](https://github.com/azure-samples).
