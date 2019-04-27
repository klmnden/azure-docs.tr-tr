---
title: Azure'da konuşma Hizmetleri ile bir özel konuşma tanıma uç noktası oluşturma | Microsoft Docs
description: Azure konuşma Hizmetleri kullanarak özel bir konuşmayı metne uç oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 03/12/2018
ms.author: panosper
ms.openlocfilehash: 1f7a84d187ba6279caad4926d54bfc56254152af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60539035"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Özel bir konuşmayı metne dönüştürme uç noktası oluşturma

Özel akustik modeller veya dil modellerini oluşturduktan sonra özel bir konuşmayı metne uç noktasına dağıtabilirsiniz.

## <a name="create-an-endpoint"></a>Uç nokta oluşturma
Yeni özel uç nokta oluşturmak için Seç **uç noktaları** üzerinde **özel konuşma** sayfanın üstündeki menü. Bu eylem açılır **uç noktaları** geçerli özel uç noktalar tablosunu içeren sayfa. Uç nokta henüz oluşturmadıysanız, boş bir tablodur. Geçerli yerel ayar, tablo başlığında gösterilir.

Farklı bir dil için bir dağıtım oluşturmak için Seç **yerel ayarını Değiştir**. Desteklenen diller hakkında daha fazla bilgi için.

Yeni bir uç nokta oluşturmak için Seç **Yeni Oluştur**. İçinde **Create Endpoınt** bölmesinde, bilgi girin **adı** ve **açıklama** özel dağıtımınızın kutuları.

İçinde **abonelik** birleşik giriş kutusunda, kullanmak istediğiniz aboneliği seçin. S0 (Ücretli) aboneliğiniz varsa, eş zamanlı istek seçebilirsiniz.

İçerik günlüğü açıp anahtarlanır de seçebilirsiniz. Diğer bir deyişle, uç nokta trafiği saklanıyor olsun, seçeneğini belirliyoruz. Seçilmezse, trafiği depolama gizlenir. Günlüğe kaydedilen tüm içeriği bulabilirsiniz indirme bağlantıları uç nokta altında Ayrıntıları görünümü ->

İçinde **akustik Model** listesinde, kullanmak istediğiniz akustik model seçin ve **dil modeli** listesinde, istediğiniz dil modelini seçin. Akustik ve dil modellerini seçenekleri, her zaman temel Microsoft modelleri ekleyin. Birleşimler ve temel modele seçimini sınırlar. Temel konuşma modelleri ile arama karışımı ve temel modele dikte olamaz.

> [!NOTE]
> Kullanım koşulları ve onay kutusunu seçerek fiyatlandırma bilgileri kabul emin olun.
>

Akustik ve dil Modellerinizi seçtikten sonra seçin **Oluştur**. Bu eylem, döndürür **dağıtımları** sayfası. Tablo, artık yeni uç noktanıza karşılık gelen bir giriş içerir. Bunu oluşturulurken uç noktasının durumu geçerli durumunu yansıtır. Bu özel Modellerinizi ile yeni bir uç noktayı örneklemek için en fazla 30 dakika sürebilir. Dağıtım durumu olduğunda *tam*, uç noktayı kullanıma hazırdır.

Dağıtıma hazır olduğunda, bir bağlantı uç noktası adı olur. Bağlantıyı seçerek görüntüler **uç nokta bilgileri** ya da bir HTTP isteği ile kullanılacak özel uç noktanıza URL'lerini veya Microsoft Bilişsel hizmetler konuşma istemcisi kullanan web yuvalarını kitaplığını, görüntüleyen sayfa.

## <a name="next-steps"></a>Sonraki adımlar

Diğer öğreticiler için bkz:
- [Konuşma Tanıma Hizmetleri deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Özel akustik model oluşturma](how-to-customize-acoustic-models.md)
- [Özel dil modeli oluşturma](how-to-customize-language-model.md)
