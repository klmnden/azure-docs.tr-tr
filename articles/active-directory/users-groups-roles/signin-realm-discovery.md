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
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b6846c5f907c41db16e99883be7041a68357586
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608784"
---
# <a name="home-realm-discovery-for-azure-active-directory-sign-in-pages"></a>Azure Active Directory oturum açma sayfaları için giriş bölgesi bulma

Yeni kimlik doğrulaması yöntemlerine yer açmak ve kullanılabilirliği geliştirmek için Azure Active Directory (Azure AD) oturum açma davranışımızı değiştiriyoruz. Oturum açma işlemi sırasında Azure AD kullanıcının nerede kimlik doğrulaması yapması gerektiğini belirler. Azure AD, oturum açma sayfasında girilen kullanıcı adının kuruluş ve kullanıcı ayarlarını okuyarak akıllı kararlar alır. Bu, FIDO 2.0 gibi ek kimlik bilgilerinin kullanılmasına imkan tanıyan parolasız bir geleceğe doğru atılan bir adım.

## <a name="home-realm-discovery-behavior"></a>Giriş bölgesi bulma davranışı

Tarihsel olarak, giriş bölgesi bulmayı, bazı eski uygulamalar için bir giriş bölgesi bulma İlkesi veya oturum açma sırasında sağlanan etki alanı tarafından yönetilen. Örneğin, bizim bulma davranışını bir Azure Active Directory kullanıcısı kullanıcı adı yanlış yazılmış olabilir ancak, kuruluşun kimlik bilgisi koleksiyonu ekranında hala gelecek. Bu durum, kullanıcının kuruluşunuzun etki alanı adı "contoso.com" doğru sağlar oluşur. Bu davranış, bireysel kullanıcı için deneyimlerin özelleştirilmesi ayrıntı düzeyine izin vermez.

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
> Bu özellik eski etki alanı düzeyinde ana bölge Federasyon zorlamak için bulma bağlı olan Federasyon etki alanları üzerinde bir etkisi olabilir. Ne zaman Federasyon etki alanı desteği eklenecektir hakkında güncelleştirmeler almak için bkz: [giriş bölgesi bulmayı Microsoft 365 Hizmetleri için oturum açma sırasında](https://azure.microsoft.com/en-us/updates/signin-hrd/). Sırada, bazı kuruluşlar, etki alanı adlarını yönlendirir çünkü kullanıcılar şu anda kuruluşun etki alanı uç noktası için Azure Active Directory'de mevcut değil, ancak uygun etki alanı adını içeren bir kullanıcı adıyla oturum etkinleştirmelerini eğitim. Yeni oturum açma davranışı Bu izin vermez. Kullanıcı adını düzeltmek için kullanıcı bilgilendirilir ve Azure Active Directory'de mevcut olmayan bir kullanıcı adı oturum açmaya izin verilmez.
>
> Eski davranışı bağımlı uygulamalar varsa, siz veya Kurumunuz için kuruluş yöneticileri çalışan oturum açma ve kimlik doğrulaması belgeleri güncelleştirmek ve Azure Active Directory kullanıcı adı oturum açmak için kullanılacak çalışanların eğitmek için önemlidir.
  
Yeni davranış ile ilgili endişeleriniz varsa, konusundaki yorumlara bırakın **geri bildirim** bu makalenin.  

## <a name="next-steps"></a>Sonraki adımlar

[Oturum açma markalamayı özelleştirme](../fundamentals/add-custom-domain.md)
