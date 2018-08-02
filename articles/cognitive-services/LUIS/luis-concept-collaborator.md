---
title: LUIS uygulama işbirliği - Azure'ı Anlama | Microsoft Docs
description: LUIS uygulamaları tek bir sahibi ve isteğe bağlı ortak çalışanlar gerektirir.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: fe5e35c2dcb08cdff9d92142558cf8d7ec81c36c
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399580"
---
# <a name="collaborating"></a>İşbirliği yapma

LUIS uygulama yazmak için bir grup izin vermek için işbirliği sağlar.

## <a name="luis-account"></a>LUIS hesabı
Tek bir ilişkili LUIS hesabıdır [Microsoft Live](https://login.live.com/) hesabı. Her LUIS hesap ücretsiz verilen [anahtar yazma](luis-concept-keys.md#authoring-key) hesap erişimi olan tüm LUIS uygulamaları yazmak için kullanın. 

Bir LUIS hesabı birçok LUIS uygulaması olabilir.

Bkz: [Azure Active Directory Kiracı Kullanıcı](luis-how-to-collaborate.md#azure-active-directory-tenant-user) Active Directory kullanıcı hesapları hakkında daha fazla bilgi edinmek için. 

## <a name="luis-app-owner"></a>LUIS uygulama sahibi
Bir uygulamayı oluşturan sahibi hesaptır. Her uygulamanın tek bir sahip vardır. Uygulama sahibi listelendiğinden  **[ayarları](luis-how-to-collaborate.md)**. Bu uygulamayı silebilirsiniz hesabıdır. Uç nokta kota aylık sınırı %75 ulaştığında e-posta alırsa hesabı da budur. 

## <a name="transfer-ownership"></a>Sahipliği devretme
LUIS, ancak herhangi bir ortak çalışanı uygulamayı dışarı aktarabilir ve ardından içeri aktararak uygulama oluşturma, sahipliğin aktarılması sağlamaz. Yeni uygulama farklı bir uygulama kimliği olan unutmayın Eğitim, yayımlanan yeni uygulama gereksinimlerini ve kullanılan yeni uç nokta.

## <a name="luis-app-collaborators"></a>LUIS uygulaması ortak çalışanlar
Uygulamanın sahibi ortak çalışanlar için uygulama ekleyebilirsiniz. Uygulamada ortak çalışan kişinin e-posta adresi eklemek sahibe ihtiyacı  **[ayarları](luis-how-to-collaborate.md)**. Ortak çalışan uygulamanın tam erişime sahip olur. Ortak çalışan uygulamayı silerse, uygulama ortak çalışanı'nın hesabından kaldırıldı, ancak kendi hesabında kalır. 

Birden fazla uygulama ortak çalışanlarla paylaşmak istiyorsanız, her uygulamanın eklendi ve çalışan kişinin e-posta gerekir. 

## <a name="managing-multiple-authors"></a>Birden çok yazarlar yönetme
[LUIS](luis-reference-regions.md#luis-website) Web sitesi olmayan şu anda teklif işlem düzeyinde yazma. Temel bir sürümden bağımsız sürümler üzerinde çalışmak yazarlar izin verebilirsiniz. İki farklı yöntemleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Aynı uygulama içinde birden çok sürümlerini yönetme
Başlayın [kopyalama](luis-how-to-manage-versions.md#clone-a-version), her yazar için bir temel sürüm. 

Her geliştirici kendi uygulama sürümüne değişiklik yapar. Her geliştirici modeliyle memnun olduğunda, yeni sürümleri JSON dosyasına dışarı aktarın.  

Değişiklikler için karşılaştırılabilir JSON biçimli dosyaları dışarı aktarılan uygulamalardır. Yeni sürümü tek bir JSON dosyası oluşturmak için dosyaları birleştirin. Değişiklik **VersionID** yeni birleştirilmiş sürüm belirtmek için JSON özelliği. Bu sürüm, özgün uygulamaya aktarma. 

Bu yöntem bir etkin sürüm, bir aşama sürümü ve bir yayımlanmış sürümüne sahip olmanızı sağlar. Sonuçları etkileşimli test bölmesinde üç sürümde karşılaştırabilirsiniz.

### <a name="manage-multiple-versions-as-apps"></a>Birden çok sürümü uygulamaları yönetme
[Dışarı aktarma](luis-how-to-manage-versions.md#export-version) temel sürüm. Her geliştirici sürümünü alır. Uygulamayı alır kişi sürüm sahibidir. Bunların ne zaman yapılır dışarı aktarma sürümü uygulama değiştirme. 

Değişiklikler için temel dışarı aktarma ile karşılaştırılabilir JSON biçimli dosyaları dışarı aktarılan uygulamalardır. Yeni sürümü tek bir JSON dosyası oluşturmak için dosyaları birleştirin. Değişiklik **VersionID** yeni birleştirilmiş sürüm belirtmek için JSON özelliği. Bu sürüm, özgün uygulamaya aktarma.

## <a name="next-steps"></a>Sonraki adımlar

Anlamak [sürüm](luis-concept-version.md) kavramları. 

Bkz: [uygulama ayarları](luis-how-to-collaborate.md) LUIS uygulamanızda ortak çalışanlar yönetme hakkında bilgi edinmek için.

Bkz: [e-posta erişim listesine eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) yazma API'leri ile.