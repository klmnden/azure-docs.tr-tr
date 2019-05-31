---
title: Bir çalışma alanı ve proje nedir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Bir çalışma alanı oluşturma ve özel çeviri sisteminizi oluşturmak için bir çalışma alanıdır. Bir çalışma alanı, birden çok proje, modelleri ve belgeleri içerebilir. Proje modeli, belgeler ve test için bir sarmalayıcıdır. Her proje, otomatik olarak doğru dil çiftiniz olması için bu çalışma alanına yüklenen tüm belgeleri içerir.
services: cognitive-services
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: conceptual
ms.openlocfilehash: 312688ddccd6a65745b919e4c396a95f40a58d3e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66382329"
---
# <a name="what-is-a-custom-translator-workspace"></a>Özel Translator çalışma alanı nedir?

Bir çalışma alanı oluşturma ve özel çeviri sisteminizi oluşturmak için bir çalışma alanıdır. Bir çalışma alanı, birden çok proje, modelleri ve belgeleri içerebilir. İçinde özel Translator yaptığı tüm işler, içinde belirli bir çalışma alanıdır.

Çalışma alanı, size ve çalışma alanınıza davet kişiler özeldir. Uninvited kişilerin çalışma alanınızı içeriğindeki erişiminiz yok. Çalışma alanınıza ister ve değiştirmek veya dilediğiniz zaman kişilerin erişimlerini kaldırmanız olarak birçok kişi davet edebilirsiniz. Ayrıca, yeni bir çalışma alanı oluşturabilirsiniz. Varsayılan olarak, herhangi bir proje veya diğer çalışma alanlarınızı içinde olan belgeleri bir çalışma alanı içermez.

## <a name="what-is-a-custom-translator-project"></a>Bir özel Translator projesi nedir?

Proje modeli, belgeler ve test için bir sarmalayıcıdır. Her proje, otomatik olarak doğru dil çiftiniz olması için bu çalışma alanına yüklenen tüm belgeleri içerir. Örneğin, hem bir İngilizce, İspanyolca projesine hem de İngilizce projesine bir İspanyolca varsa, aynı belge her iki projeyi de dahil edilir. Her proje sorgularken kullanılan ilişkili CategoryID sahip [V3 API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl) çevirileri için. CategoryID özel Translator ile oluşturulan özelleştirilmiş bir sistemden çevirileri almak için kullanılan parametredir.

## <a name="project-categories"></a>Proje kategorileri

Kategori, projeniz için – terminoloji ve stil kullanmak istediğiniz alanı – etki alanı tanımlar. Belgelerinizi için en uygun kategoriyi seçin. Bazı durumlarda, seçtiğiniz kategorisi doğrudan özel Translator davranışını etkiler.

İki temel modelleri sahibiz. Genel ve teknoloji değildirler. Kategori **teknoloji** olan seçili teknolojisi temel modelleri kullanılır. Diğer kategori seçimi için genel temel modelleri kullanılır. Teknoloji temel modeli iyi teknoloji etki alanında yok ancak cümle için çeviri kullanılan teknoloji etki alanı içinde 'un altına düşersek yoksa Düşük Kalite gösterir. Müşteriler yalnızca cümleler teknoloji etki alanı içinde kesinlikle düşersek kategori teknoloji seçmek için öneririz.

Aynı çalışma alanında, farklı kategorilerdeki aynı dil çifti projelerde oluşturabilirsiniz. Özel Translator kategorisi ve aynı dil çifti ile yinelenen bir proje oluşturulmasını engeller. Projenize bir etiketi uygulamak, bu kısıtlama önlemek sağlar. Projenize benzersiz bir etiket projelerinizde CategoryID yansıtılır ekleme olarak birden çok istemciyi için çevirisi sistemleri oluşturmakta olduğunuz sürece etiketleri kullanmayın.

## <a name="project-labels"></a>Proje etiketleri

Özel Translator, projenize bir proje etiket atamanıza olanak tanır. Kategori ve aynı dil çifti ile birden çok proje arasında proje etiketi ayırır. En iyi uygulama, kullanmaktan projesi gerekli etiketleri sürece.

Proje etiket CategoryID bir parçası olarak kullanılır. Proje etiket unset bırakılır ya da projeler arasında kümedeki, ardından aynı kategoriye projeleri ve *farklı* dil çiftleri aynı CategoryID paylaşmak. Bu yaklaşım, siz veya müşteriniz her proje için benzersiz olan bir CategoryID hakkında endişelenmeden metin çevirmeni API'sini kullanarak diller arasında geçiş yapmanıza izin verdiğinden avantajlıdır.

Örneğin, teknoloji etki alanından İngilizce Fransızca ve Fransızca için İngilizce çevirileri sağlamak isterseniz miyim iki proje oluşturursunuz: biri İngilizce -\> Fransızca ve Fransızca için-\> İngilizce. Ben, her ikisi için aynı kategoride (teknolojisi) belirtin ve proje etiketi boş bırakın. My CategoryID değiştirmek zorunda kalmadan miyim İngilizce ve Fransızca çevirileri için API'yi sorgulayabilir için iki proje için CategoryID eşleşir.

Müşteriler arasında ayırt etmek için bir proje etiketi'i kullanarak, bir dil hizmet sağlayıcısıysanız ve aynı kategoride ve dil çiftini koruyan farklı modelleri ile birden fazla müşteriye hizmet istiyorsanız akıllıca karar olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [eğitim ve model](training-and-model.md) bilmek, bir çeviri modeline etkili bir şekilde oluşturma.
