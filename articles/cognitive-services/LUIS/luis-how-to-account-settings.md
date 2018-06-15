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
ms.openlocfilehash: 25989d07b7d879ac68283ee56a7ccb0c07e09623
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356061"
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

Kiracı yönetici LUIS için oturum açacak değil, yönetici erişebilir [onayı](https://account.activedirectory.windowsazure.com/Consent.aspx?ClientID=65920ba3-ab61-4a9b-9b10-505e5ce61b58) LUIS için. 

Kiracı yönetici yalnızca belirli kullanıcıların LUIS kullanmasını isterse, bunun için başvuruda [kimliği blogu](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek, [anahtar yazma](luis-concept-keys.md#authoring-key). 

[LUIS]: luis-reference-regions.md
