---
title: Oturum açma kimlik doğrulaması sırasında - Azure Active Directory kullanıcı adı arama | Microsoft Docs
description: Nasıl ekran Mesajlaşma yansıtır kullanıcı adı arama oturum açma sırasında
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/10/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d82c1909a28de694925a12e9b801361cad1f36d2
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59494714"
---
# <a name="home-realm-discovery-for-azure-active-directory-sign-in-pages"></a>Azure Active Directory oturum açma sayfaları için giriş bölgesi bulma

Yer açmak için yeni kimlik doğrulama yöntemleri ve kullanılabilirliği geliştirmek için Azure Active Directory (Azure AD) oturum açma davranışı değişir. Oturum açma sırasında Azure AD kullanıcı kimlik doğrulaması gereken yere belirler. Azure AD oturum açma sayfasında girilen kullanıcı adı için kuruluş ve kullanıcı ayarlarını okuyarak akıllı kararlar yapar. FIDO 2.0 gibi ek kimlik bilgileri sağlayan bir adım bir parola ücretsiz gelecekteki doğrultusunda budur.

## <a name="home-realm-discovery-behavior"></a>Giriş bölgesi bulma davranışı

Tarihsel olarak, giriş bölgesi bulmayı, bazı eski uygulamalar için bir giriş bölgesi bulma İlkesi veya oturum açma sırasında sağlanan etki alanı tarafından yönetilen. Örneğin, bizim bulma davranışını bir Azure Active Directory kullanıcısı kullanıcı adı yanlış yazılmış olabilir ancak, kuruluşun kimlik bilgisi koleksiyonu ekranında hala gelecek. Bu durum, kullanıcının kuruluşunuzun etki alanı adı "contoso.com" doğru sağlar oluşur. Bu davranış, tek bir kullanıcı deneyimleri özelleştirmek ayrıntı düzeyi izin vermez.

Geniş bir kimlik bilgilerini destekler ve kullanılabilirliğini artırmak için Azure Active Directory kullanıcı adı arama davranışı oturum açma işlemi sırasında artık güncelleştirilir. Yeni davranış, Kiracı ve kullanıcı düzeyi ayarlarına göre oturum açma sayfasında girilen kullanıcı adının okuyarak akıllı kararlar verir. Bunu mümkün hale getirmek için Azure Active Directory oturum açma sayfasında girilen kullanıcı adı, belirtilen etki alanında yok veya kullanıcı kimlik bilgilerini sağlamak üzere yeniden yönlendiren görmek için denetler.

Bu iş ek bir avantaj geliştirilmiş hatadır Mesajlaşma. Yalnızca Azure Active Directory Kullanıcıları destekleyen bir uygulama için oturum açarken Mesajlaşma geliştirilmiş hata bazı örnekleri aşağıda verilmiştir.

1. Kullanıcı adı yanlış yazmış veya kullanıcı adı henüz Azure AD'ye eşitlenmiş değil:
  
    ![Kullanıcı adı yanlış yazmış veya bulunamadı](./media/signin-realm-discovery/typo-username.png)
  
2. Etki alanı adı yanlış yazmış:
  
    ![etki alanı adı yanlış yazmış veya bulunamadı](./media/signin-realm-discovery/typo-domain.png)
  
3. Kullanıcı, bilinen bir tüketici etki alanı ile oturum açmanız çalışır:
  
    ![bir bilinen bir tüketici etki alanı ile oturum açın](./media/signin-realm-discovery/consumer-domain.png)
  
4. Parola yanlış yazmış ancak kullanıcı adı doğru olur:  
  
    ![iyi bir kullanıcı adıyla parola yanlış yazılan](./media/signin-realm-discovery/incorrect-password.png)
  
> [!IMPORTANT]
> Bu özellik eski etki alanı düzeyinde ana bölge Federasyon zorlamak için bulma bağlı olan Federasyon etki alanları üzerinde bir etkisi olabilir. Ne zaman Federasyon etki alanı desteği eklenecektir hakkında güncelleştirmeler almak için bkz: [giriş bölgesi bulmayı Microsoft 365 Hizmetleri için oturum açma sırasında](https://blogs.azure.net/updates/Admin/Blogs/29/Posts/3170). Sırada, bazı kuruluşlar, etki alanı adlarını yönlendirir çünkü kullanıcılar şu anda kuruluşun etki alanı uç noktası için Azure Active Directory'de mevcut değil, ancak uygun etki alanı adını içeren bir kullanıcı adıyla oturum etkinleştirmelerini eğitim. Yeni oturum açma davranışı Bu izin vermez. Kullanıcı adını düzeltmek için kullanıcı bilgilendirilir ve Azure Active Directory'de mevcut olmayan bir kullanıcı adı oturum açmaya izin verilmez.
>
> Eski davranışı bağımlı uygulamalar varsa, siz veya Kurumunuz için kuruluş yöneticileri çalışan oturum açma ve kimlik doğrulaması belgeleri güncelleştirmek ve Azure Active Directory kullanıcı adı oturum açmak için kullanılacak çalışanların eğitmek için önemlidir.
  
Yeni davranış ile ilgili endişeleriniz varsa, konusundaki yorumlara bırakın **geri bildirim** bu makalenin.  

## <a name="next-steps"></a>Sonraki adımlar

[Oturum açma markalamayı özelleştirme](../fundamentals/add-custom-domain.md)