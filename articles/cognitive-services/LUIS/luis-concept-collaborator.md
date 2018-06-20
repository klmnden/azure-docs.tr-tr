---
title: HALUK uygulama işbirliği - Azure anlama | Microsoft Docs
description: HALUK uygulamaları tek bir kullanıcıya ve isteğe bağlı ortak çalışanlar gerektirir.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 51b3958f83cd110ace27f6ee42571c05843f5aa2
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265074"
---
# <a name="collaborating"></a>İşbirliği

HALUK işbirliği bir grup kişi bir uygulama yazmak için olanak sağlar.

## <a name="luis-account"></a>HALUK hesabı
Tek bir ilişkili HALUK hesabıdır [Microsoft Live](https://login.live.com/) hesabı. Her HALUK hesap ücretsiz verilen [anahtar yazma](luis-concept-keys.md#authoring-key) tüm HALUK uygulamaları yazmak için kullanılacak hesap erişimi. 

HALUK hesap birçok HALUK uygulamalar olabilir.

## <a name="luis-app-owner"></a>HALUK uygulama sahibi
Bir uygulama oluşturur hesap sahibi değil. Her uygulamanın tek bir sahibi vardır. Sahibi uygulamasında listelendiğinden  **[ayarları](luis-how-to-collaborate.md)**. Uygulamasını silebilir hesap budur. Bu aynı zamanda uç nokta kota aylık sınırı % 75 ulaştığında, e-posta alırsa hesabıdır. 

## <a name="transfer-ownership"></a>Sahipliği devretme
HALUK sahipliği aktarımını sağlamaz, ancak herhangi bir ortak çalışanı uygulama dışarı ve içeri aktararak sonra bir uygulama oluşturun. Yeni uygulama farklı bir uygulama kimliği sahip unutmayın Yeni uygulama gereksinimlerini eğitilmiş, yayımlanan ve kullanılan yeni uç noktası.

## <a name="luis-app-collaborators"></a>HALUK uygulama ortak çalışanlar
Bir uygulama sahip ortak bir uygulama ekleyebilirsiniz. Sahibi uygulamasını ortak çalışanı kişinin e-posta adresi eklemek gereken  **[ayarları](luis-how-to-collaborate.md)**. Ortak çalışanı uygulama tam erişimi vardır. Ortak çalışanı uygulama silerse, uygulama ortak çalışanı'nın hesabından kaldırıldı, ancak sahibinin hesabında kalır. 

Birden fazla uygulama ortak çalışanlarla paylaşmak istiyorsanız, her uygulama eklenen ortak çalışanı kişinin e-posta gerekir. 

## <a name="managing-multiple-authors"></a>Birden çok yazar yönetme
[HALUK] [ LUIS] Web sitesi değil şu anda teklif işlem düzeyindeki yazma. Temel bir sürümünden bağımsız sürümleri üzerinde çalışmak yazarlar izin verebilirsiniz. İki farklı yöntem aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Aynı uygulama içinde birden çok sürümü yönetme
İle başlayan [kopyalama](luis-how-to-manage-versions.md#clone-a-version), her yazar için bir temel sürüm. 

Her geliştirici kendi uygulama sürümüne değişiklikleri yapar. Her yazar modelle memnun olduktan sonra yeni sürümler için JSON dosyalarını aktarın.  

Dışarı aktarılan uygulamaları için değişiklikleri karşılaştırılabilir JSON biçimli dosyaları bulunur. Yeni sürümün tek bir JSON dosyası oluşturmak için dosyaları birleştirin. Değişiklik **VersionID** yeni birleştirilmiş sürüm belirtmek için JSON özelliği. Bu sürüm özgün uygulamaya içeri aktarın. 

Bu yöntem bir etkin sürümü, bir aşama sürüm ve yayımlanmış bir sürüm sahip olmanızı sağlar. Üç sürümleri arasında etkileşimli test bölmesi sonuçlarında karşılaştırabilirsiniz.

### <a name="manage-multiple-versions-as-apps"></a>Birden çok sürümü uygulamaları yönetme
[Dışarı aktarma](luis-how-to-manage-versions.md#export-version) temel sürüm. Her geliştirici sürümünü alır. Uygulama alır kişinin sürüm sahibi. Bunlar bittiğinde uygulama değiştirme verme sürümü. 

Değişiklikleri temel dışarı aktarma ile karşılaştırıldığında JSON biçimli dosyaları dışarı aktarılan uygulamalardır. Yeni sürümün tek bir JSON dosyası oluşturmak için dosyaları birleştirin. Değişiklik **VersionID** yeni birleştirilmiş sürüm belirtmek için JSON özelliği. Bu sürüm özgün uygulamaya içeri aktarın.

## <a name="next-steps"></a>Sonraki adımlar

Anlamak [sürüm](luis-concept-version.md) kavramları. 

Bkz: [uygulama ayarları](luis-how-to-collaborate.md) ortak çalışanlar HALUK uygulamanızda yönetme hakkında bilgi edinmek için.

Bkz: [e-posta erişim listesine eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) geliştirme API'leri ile.

[luis-reference-prebuilt-domains]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-domains
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website