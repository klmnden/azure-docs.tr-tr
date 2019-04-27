---
title: Sürüm oluşturma
titleSuffix: Language Understanding - Azure Cognitive Services
description: Geleneksel programlama sürümlerinde sürümlerinde LUIS, benzer. Her sürüm, uygulamanın zaman içinde bir anlık görüntüdür. Uygulamada bir değişiklik yapmadan önce yeni bir sürümünü oluşturun. Unpeel denemek için tam uygulamasına ve uygulamanın amacını ve konuşma önceki bir duruma geri dönmek kolaydır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: diberry
ms.openlocfilehash: 9da79e5b744f8ba70c0e265f0d1f0126b37eba49
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60509688"
---
# <a name="understand-how-and-when-to-use-a-luis-version"></a>Nasıl ve ne zaman anlamak LUIS sürümünü kullanmak için

Geleneksel programlama sürümlerinde sürümlerinde LUIS, benzer. Her sürüm, uygulamanın zaman içinde bir anlık görüntüdür. Uygulamada bir değişiklik yapmadan önce yeni bir sürümünü oluşturun. Tam sürüme geri dönün ve ardından hedefleri ve önceki bir duruma konuşma kaldırmaya kolaydır.

İle aynı uygulamanın farklı modelleri oluşturma [sürümleri](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>Sürüm kimliği
Sürüm kimliği basamak karakterlerinden oluşur veya '.' ve 10 karakterden uzun olamaz.

## <a name="initial-version"></a>İlk sürümü
Başlangıç sürümü (0,1) Varsayılan active sürümüdür. 

## <a name="active-version"></a>Etkin sürümü
İçin [bir sürümünü ayarlama](luis-how-to-manage-versions.md#set-active-version) etkin, şu anda düzenlenebilir ve test oluşturucusunu [LUIS](luis-reference-regions.md) Web sitesi. Bir sürüm kendi verilerine erişim, test etmek ve yayımlamak için güncelleştirmeleri de yapmak için etkin olarak ayarlayın.

Şu anda etkin sürümünün adı uygulama adından sonra üst, sol bölmede görüntülenir. 

[![Etkin sürümünü Değiştir](./media/luis-concept-version/version-in-nav-bar-inline.png)](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Sürümleri ve yayımlama yuvaları
Sizin için ya da bir aşama ve ürün yuvaları yayımlayın. Her yuva, farklı bir sürüm veya aynı sürüme sahip olabilir. Bu, robotlar veya diğer LUIS uygulamalar için kullanılabilir uç noktası aracılığıyla model sürümleri arasındaki değişiklikleri doğrulamak için kullanışlıdır. 

## <a name="clone-a-version"></a>Sürümü Kopyala
Mevcut bir sürümünün bir kopyasını oluşturun ve yeni bir sürüm olarak kaydetmek için bir sürüm kopyalayın. Mevcut sürümde aynı içerik yeni sürüm için başlangıç noktası olarak kullanmak için bir sürüm kopyalayın. Bir sürüm kopyalama sonra yeni sürümü haline gelir **etkin** sürümü. 

## <a name="import-and-export-a-version"></a>İçeri ve dışarı aktarma sürümü
Uygulama düzeyinde bir sürümü içeri aktarabilirsiniz. Bu sürüm, etkin sürümü haline gelir ve sürüm kimliği uygulama dosyasını "VersionID" özelliğinde kullanılan. Ayrıca, mevcut uygulamalarla sürüm düzeyinde içeri aktarabilirsiniz. Yeni sürümü etkin sürümü haline gelir. 

Uygulama düzeyinde bir sürüm dışa aktarabilirsiniz veya bir sürüm sürüm düzeyinde dışarı aktarabilirsiniz. Tek fark, uygulama düzeyinde verilen sürüm sırada sürüm düzeyinde şu anda etkin olan açıklamadır, herhangi bir sürümü üzerinde dışarı aktarmak için seçebileceğiniz **[ayarları](luis-how-to-manage-versions.md)** sayfası. 

İçeri aktarıldıktan sonra uygulamayı retrained için dışarı aktarılan dosyayı makine öğrenilen bilgileri içermiyor. Dışarı aktarılan dosyayı ortak çalışanlar içermiyor--sürümü yeni bir uygulamaya içeri aktarıldıktan sonra bu geri eklemeniz gerekir.

## <a name="export-each-version-as-app-backup"></a>Her sürüm uygulama yedek olarak dışarı aktarma
LUIS uygulamanızı yedekleme için her bir sürümü üzerinde dışarı aktarma **[ayarları](luis-how-to-manage-versions.md)** sayfası.

## <a name="delete-a-version"></a>Bir sürüm Sil
Ayarları sayfasında sürümler listeden etkin sürümü hariç tüm sürümler silebilirsiniz. 

## <a name="version-availability-at-the-endpoint"></a>Uç nokta kullanılabilirliğine sürümü
Eğitilen sürümleri uygulamanızı otomatik olarak kullanılabilir olmayan [uç nokta](luis-glossary.md#endpoint). Yapmanız gerekenler [yayımlama](luis-how-to-publish-app.md) veya bir sürüm, uygulama uç noktada kullanılabilir olması için sırayla yeniden yayımlayın. Yayımlayabilir **hazırlama** ve **üretim**, uç noktada kullanılabilir uygulama en fazla iki sürümü vermiş. Daha fazla kullanılabilir bir uç noktada uygulama sürümlerini gerekiyorsa, sürüm dışarı aktarma ve yeni bir uygulamaya yeniden içeri aktarın gerekir. Yeni uygulama, bir farklı uygulama kimliği vardır.

## <a name="collaborators"></a>Ortak çalışanlar
Sahibi ve tüm [ortak çalışanlar](luis-how-to-collaborate.md) uygulamanın tüm sürümler için tam erişime sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

Eklemeyi öğrenmek [sürüm](luis-how-to-manage-versions.md) uygulama ayarları sayfasında. 

Tasarlamayı öğrenin [hedefleri](luis-concept-intent.md) modeline.
