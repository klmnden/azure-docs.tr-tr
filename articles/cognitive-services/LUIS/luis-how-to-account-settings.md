---
title: Hesap ayarlarınızda LUIS yönetme | Microsoft Docs
description: Hesap ayarlarınızı yönetmek için LUIS Web sitesi kullanın.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2018
ms.author: v-geberr
ms.openlocfilehash: 1f22112a38bf32af03ffaf0493db16839b3fe794
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749972"
---
# <a name="manage-your-luis-account"></a>LUIS hesabınızı yönetme
İki anahtar bilgi HLUIShesabı için kullanıcı hesabı ve geliştirme anahtar parçalarıdır. Oturum açma bilgilerini, yönetilen [account.microsoft.com](https://account.microsoft.com). Geliştirme anahtarınızı gelen yönetilen [LUIS][LUIS] Web sitesi **ayarları** sayfası. 

## <a name="authoring-key"></a>Anahtar yazma

Bu tek, bölgeye özgü yazma anahtar üzerinde **ayarları** , tüm uygulamalardan Yazar olanak tanır, sayfa [LUIS][LUIS] Web sitesi yanı sıra [ API'ları yazma](https://aka.ms/luis-authoring-api). Kolaylık, geliştirme anahtar yapmak için izin verilen bir [sınırlı](luis-boundaries.md) uç nokta sayısı her ay sorgular. 

![LUIS Ayarları sayfası](./media/luis-how-to-account-settings/account-settings.png)

Geliştirme anahtar, bir ortak çalışanı listelenen tüm uygulamaların yanı sıra size ait olan tüm uygulamalar için kullanılır.

## <a name="authoring-key-regions"></a>Anahtar bölgeler yazma
Geliştirme özel anahtarıdır [bölge yazma](luis-reference-regions.md#publishing-regions). Bu anahtar, farklı bir bölgede çalışmaz. 

## <a name="reset-authoring-key"></a>Geliştirme anahtarını Sıfırla
Geliştirme anahtarınızı aşılıp aşılmadığını anahtarını sıfırlayın. Bu anahtar, tüm uygulamalarınızda sıfırlamak [LUIS] Web sitesi. Uygulamalarınızı geliştirme API'leri aracılığıyla yazarsanız, değerini değiştirmeye ihtiyaç `Ocp-Apim-Subscription-Key` yeni anahtarı. 

## <a name="delete-account"></a>Hesabı sil
Bkz: [veri depolama ve Temizleme](luis-concept-data-storage.md#accounts) hesabınızı sildiğinizde, hangi verilerin silinmiş hakkında bilgi. 

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory Kiracı Kullanıcı
LUIS standart Azure Active Directory (Azure AD) onay akışı kullanır. 

Kiracı yönetici, doğrudan Azure AD'de LUIS kullanmak için verilen erişmesi gereken kullanıcı ile çalışması gerekir. 

İlk olarak, kullanıcı HLUISimzalar ve yönetici onayı gerek açılan iletişim görür. Kullanıcı, devam etmeden önce Kiracı yönetici iletişim kurar. 

İkinci olarak, Kiracı yönetici HLUISimzalar ve bir onay akış açılan iletişim görür. Yönetici kullanıcı için izin vermesi gerekir iletişim budur. Yönetici izni kabul ettiğinde, kullanıcı ile LUIS devam edebilirsiniz.

Kiracı yönetici LUIS için oturum açacak değil, yönetici erişebilir [onayı](https://account.activedirectory.windowsazure.com/r#/applications) LUIS için. 

![Uygulama Web sitesi tarafından Azure active directory izni](./media/luis-how-to-account-settings/tenant-permissions.png)

Kiracı yönetici yalnızca belirli kullanıcıların LUIS kullanmasını isterse, bunun için başvuruda [kimliği blogu](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Ortak Çalışanlar için birden çok e-posta ile kullanıcı hesapları
Ortak Çalışanlar bir HALUK uygulamasına eklerseniz, bir ortak çalışanı HALUK kullanmak için bir ortak çalışanı gerekli tam e-posta adresi belirlersiniz. Azure Active Directory (Azure AD) birbirinin yerine kullanılan birden fazla e-posta hesabına sahip tek bir kullanıcı sağlar, ancak HALUK ortak çalışanı'nın listesinde belirtilen e-posta adresi oturum oturum kullanıcının gerektirir. 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek, [anahtar yazma](luis-concept-keys.md#authoring-key). 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
