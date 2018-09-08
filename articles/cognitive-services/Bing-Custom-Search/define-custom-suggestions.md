---
title: 'Bing özel arama: Özel otomatik öneri önerileri tanımlama | Microsoft Docs'
description: Özel otomatik öneri özel öneriler yapılandırılması açıklanmaktadır
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: e7a62a79bdc2e486fb6bfca34eb4addeba2bde0e
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158322"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Özel otomatik öneri deneyiminizi yapılandırın
Özel arama uygun düzeyde abone (bkz [fiyatlandırma sayfalarına](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), özel arama deneyiminizi yapılan arama önerilerini özelleştirebilirsiniz. Özel otomatik öneri özelliği kullanıcının sağladığı bir kısmi sorgu dizesine göre önerilen sorgular listesi döndürür. Özel otomatik öneri ile ilgili arama deneyiminizi özel arama önerileri sağlar. Yalnızca özel öneriler döndürmek mi, yoksa Bing önerileri de içerecek şekilde belirtin. Bing önerileri eklerseniz, özel öneriler Bing öneriler önce görünür. Öneriler Bing özel arama örneğinizin bağlamına kısıtlanır.

## <a name="configure-custom-autosuggest"></a>Yapılandırma özel otomatik öneri
Özel otomatik öneri özel arama örneğinizin yapılandırmak için aşağıdaki yönergeleri kullanın.

1.  Oturum [özel arama](https://customsearch.ai).
2.  Özel arama örneği tıklayın. Örneği oluşturmak için bkz [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).
3.  Tıklayın **otomatik öneri** sekmesi.

## <a name="enable-bing-suggestions"></a>Bing önerilerini etkinleştirmek
Bing önerilerini etkinleştirmek için geçiş **Bing otomatik öneri** kaydırıcısını açık konuma. Kaydırıcı mavi haline gelir.

## <a name="add-suggestions"></a>Önerileri ekleme
Bir öneri eklemek için metin kutusuna girin. Enter tuşuna basın veya tıklayın **+** simgesi. Özel öneriler, herhangi bir dilde olabilir ve Bing önerileri önce görünür.

## <a name="upload-suggestions"></a>Öneriler karşıya yükleme
Bir dosyadan öneriler listesi karşıya yükleyebilirsiniz. Her öneri, ayrı bir satıra yerleştirin. Karşıya yükleme simgesine tıklayın ve dosyanızı seçin.

## <a name="remove-suggestions"></a>Öneriler Kaldır
Bir öneriyi kaldırmak için kaldırmak istediğiniz öneri yanındaki Kaldır simgesine tıklayın.

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >Bu özel otomatik öneri yapılandırma değişikliklerinin etkinleşmesi 24 saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel öneriler alın](./get-custom-suggestions.md)
- [Özel Örneğinize arama](./search-your-custom-view.md)
- [Yapılandırma ve barındırılan özel kullanıcı Arabirimi kullanma](./hosted-ui.md)