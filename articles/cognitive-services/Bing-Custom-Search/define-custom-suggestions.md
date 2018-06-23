---
title: 'Bing özel arama: Özel otomatik öneri önerileri tanımlama | Microsoft Docs'
description: Özel öneriler özel otomatik öneri yapılandırmayı açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: a41b4e5b6c268ec68488c6764d4192cf8d2345a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352847"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Özel autosuggest deneyiminizi yapılandırın
Uygun düzeyde özel aranacak abone (bkz [sayfaları fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), özel arama deneyiminiz yapılan arama önerileri özelleştirebilirsiniz. Özel Autosuggest kullanıcı sağlayan bir kısmi sorgu dizesine dayalı önerilen sorguların listesini döndürür. Özel otomatik öneri ile arama deneyiminizi ilgili özel arama önerileri sağlar. Yalnızca özel öneriler dönmek mi, yoksa Bing önerileri de içerecek şekilde belirtin. Bing önerileri eklerseniz, özel öneriler önce Bing önerileri görünür. Bing önerileri özel arama örneğinizi bağlamına kısıtlanır.

## <a name="configure-custom-autosuggest"></a>Özel yapılandırma otomatik öneri
Özel otomatik öneri özel arama Örneğiniz için yapılandırmak için aşağıdaki yönergeleri kullanın.

1.  Oturum [özel arama](https://customsearch.ai).
2.  Özel arama örneği tıklatın. Örnek oluşturmak için bkz: [ilk Bing özel arama örneğinizi oluşturmak](quick-start.md).
3.  Tıklatın **Autosuggest** sekmesi.

## <a name="enable-bing-suggestions"></a>Bing önerilerini etkinleştirmek
Bing önerilerini etkinleştirmek için geçiş **otomatik Bing önerileri** kaydırıcı üzerinde konumuna. Kaydırıcı mavi olur.

## <a name="add-suggestions"></a>Öneriler ekleme
Bir öneri eklemek için metin kutusuna girin. Enter tuşuna basın veya tıklatın **+** simgesi. Özel öneriler herhangi bir dilde olabilir ve Bing önerileri önce görüntülenir.

## <a name="upload-suggestions"></a>Öneriler karşıya yükle
Bir dosyadan öneriler listesi karşıya yükleyebilirsiniz. Her bir öneri ayrı bir satıra getirin. Karşıya yükleme simgesine tıklayın ve dosyanızı seçin.

## <a name="remove-suggestions"></a>Öneriler Kaldır
Bir öneri kaldırmak için kaldırmak istediğiniz öneri yanındaki Kaldır simgesine tıklayın.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >Özel otomatik öneri yapılandırma değişikliklerinin etkili olması için 24 saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel öneriler al](./get-custom-suggestions.md)
- [Özel örneğiniz arama](./search-your-custom-view.md)
- [Yapılandırma ve özel barındırılan kullanıcı Arabirimi kullanma](./hosted-ui.md)