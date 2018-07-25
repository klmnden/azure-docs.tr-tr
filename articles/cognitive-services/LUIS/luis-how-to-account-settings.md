---
title: Hesap ayarlarınızda LUIS yönetme | Microsoft Docs
description: Hesap ayarlarınızı yönetmek için LUIS Web sitesi kullanın.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 963a7f8c196702ea899ddfe31e6187a15eb5f683
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223217"
---
# <a name="manage-account-and-authoring-key"></a>Hesabı ve anahtarı yazma yönetme
İki anahtar bilgi HLUIShesabı için kullanıcı hesabı ve geliştirme anahtar parçalarıdır. Oturum açma bilgilerini, yönetilen [account.microsoft.com](https://account.microsoft.com). Yazma anahtarınızı yönetilen engelle [LUIS](luis-reference-regions.md) Web sitesi **ayarları** sayfası. 

## <a name="authoring-key"></a>Anahtar yazma

Bu tek ve bölgeye özgü yazma üzerinde anahtar **ayarları** sayfasında, tüm uygulamalarınızdan yazmanızı sağlar [LUIS](luis-reference-regions.md) Web sitesi hem de [yazma API'leri](https://aka.ms/luis-authoring-api). Kolaylık, geliştirme anahtar yapmak için izin verilen bir [sınırlı](luis-boundaries.md) uç nokta sayısı, her ay sorgular. 

![LUIS Ayarları sayfası](./media/luis-how-to-account-settings/account-settings.png)

Yazma anahtar, sahip olduğunuz tüm uygulamaları ve bunun yanı sıra ortak çalışan listelenen tüm uygulamalar için kullanılır.

## <a name="authoring-key-regions"></a>Anahtar bölgeleri yazma
Yazma anahtar özeldir [yazma bölgesi](luis-reference-regions.md#publishing-regions). Anahtar, farklı bir bölgede çalışmaz. 

## <a name="reset-authoring-key"></a>Yazma anahtarını Sıfırla
Yazma anahtarınız açığa çıktıysa, anahtar sıfırlayın. Bu anahtar, tüm uygulamalarınızda sıfırlamak [LUIS](luis-reference-regions.md) Web sitesi. Uygulamalarınızı geliştirme API'leri aracılığıyla Yazar değerini değiştirmek gerekirse `Ocp-Apim-Subscription-Key` yeni anahtarı. 

## <a name="delete-account"></a>Hesabı sil
Bkz: [veri depolama ve Temizleme](luis-concept-data-storage.md#accounts) hesabınızı sildiğinizde, hangi verilerin silinir hakkında bilgi için. 

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory Kiracı Kullanıcı
LUIS standart Azure Active Directory (Azure AD) onay akışı kullanır. 

Kiracı yönetici, doğrudan Azure AD'de LUIS kullanmak için verilen erişmesi gereken kullanıcı ile çalışması gerekir. 

İlk olarak, kullanıcı HLUISimzalar ve yönetici onayı gerek açılan iletişim görür. Kullanıcı, Kiracı Yöneticisi devam etmeden önce bağlantı kurar. 

İkinci olarak, Kiracı yönetici HLUISimzalar ve bir onay akış açılan iletişim görür. Yönetici kullanıcı için izin vermek için gereken iletişim budur. Yönetici izni kabul ettiğinde, kullanıcı ile LUIS devam edebilirsiniz.

Kiracı yönetici LUIS için oturum açacak değil, yönetici erişebilir [onayı](https://account.activedirectory.windowsazure.com/r#/applications) LUIS için. 

![Uygulama Web sitesi tarafından Azure active directory izni](./media/luis-how-to-account-settings/tenant-permissions.png)

Kiracı yönetici yalnızca belirli kullanıcıların LUIS kullanmasını isterse, bunun için başvuruda [kimliği blogu](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Ortak Çalışanlar için birden çok e-posta ile kullanıcı hesapları
Bir LUIS uygulaması için ortak çalışanlar eklerseniz, bir ortak çalışanı LUIS ortak çalışan kullanması gereken tam e-posta adresi belirtiyorsunuz. Azure Active Directory (Azure AD) tek bir kullanıcı kullanılabileceğinden, birden fazla e-posta hesabına sahip olmasını sağlar, ancak ortak çalışanı'nın listesinde belirtilen e-posta adresiyle oturum kullanıcının LUIS gerektirir. 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin, [anahtar yazma](luis-concept-keys.md#authoring-key). 

