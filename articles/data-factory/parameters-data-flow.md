---
title: Azure veri fabrikası veri akışı parametre eşleme
description: Data factory işlem hatlarını eşleme veri akışından Parametreleştirme öğrenin
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 0a7140f70db78c8511f3c4da00b2f9c11c368163
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477677"
---
# <a name="mapping-data-flow-parameters"></a>Veri akışı parametreleri eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Eşleme veri akışları'nda Azure Data Factory parametrelerinin kullanımını destekler. Parametreler, ifadeleri ardından kullanabilirsiniz, veri akış tanımı içinde tanımlayabilirsiniz. Parametre değerlerini, arama işlem hattı yürütme veri akışı etkinliği aracılığıyla tarafından ayarlanabilir. Değerleri, veri akışında etkinlik ifadeleri ayarlamak için üç seçeneğiniz vardır:

* Dinamik bir değer ayarlamak için işlem hattı denetim akışı ifadesi dili kullanın
* Dinamik bir değer ayarlamak için veri akışı ifade dili kullanın
* Statik değişmez değer ayarlamak için her iki ifade dili kullanın

Veri akışlarınızı genel amaçlı, esnek ve yeniden kullanılabilir hale getirmek için bu özelliği kullanın. Veri akış ayarları ve bu parametreleri ifadelerle parametreleştirebilirsiniz.

> [!NOTE]
> İşlem hattı denetim akışı ifadeleri kullanmak için veri akışı parametresi dize türünde olmalıdır.

## <a name="create-parameters-in-mapping-data-flow"></a>Veri akış eşleme parametreleri oluşturma

Veri akışınızı parametre eklemek için genel özellikleri görmek için veri akışı tuvalinin boş bölümü temel tıklayın. Ayarlar bölmesinde, 'Parameters' adlı bir sekme görürsünüz. Yeni bir parametre oluşturmak için 'New' düğmesine tıklayın. Her parametre için bir ad atamanız gerekir, bir türü seçin ve isteğe bağlı olarak bir varsayılan değere ayarlayın.

![Veri akışı parametreleri oluşturma](media/data-flow/create-params.png "parametreleri veri akışı oluşturma")

Parametreleri içindeki herhangi bir veri akışı ifade yararlanılabilir. Parametreleri $ ile başlayın ve sabittir. 'Parameters' sekmesi altındaki ifade oluşturucu içinde kullanılabilir parametrelerin listesini bulabilirsiniz.

![Veri akışı parametre ifadesi](media/data-flow/parameter-expression.png "veri akışı parametre ifadesi")

## <a name="use-parameters-in-your-data-flow"></a>Veri akışınızı parametreleri kullanma

* İçinde dönüştürme ifadeleri parametre değerlerini kullanabilirsiniz. Parametreler sekmesi altında parametre listesi ifade Oluşturucu'da bulabilirsiniz. ![Veri akışı parametrelerini](media/data-flow/params9.png "kullanım veri akışı parametreleri")

* Parametreler, dinamik değerler için kaynak yapılandırın ve dönüştürme ayarlarını havuz için de kullanılır. Yapılandırılabilir alanları içine tıkladığınızda görünür "dinamik contect Ekle" bağlantısını görürsünüz. Orada tıklamak sizi, dinamik değerler kullanılacak parametreleri kullanabileceğiniz bir ifade oluşturucusu götürür. ![Veri akışı dinamik içerik](media/data-flow/params6.png "veri akışı dinamik içerik")

## <a name="set-mapping-data-flow-parameters-from-pipeline"></a>Ardışık düzen tarafından veri akışı eşleme parametrelerini ayarla

Parametrelerle veri akışınızı oluşturduktan sonra yürütme veri akışı etkinliği ile bir işlem hattından yürütebilir. Etkinlik, işlem hattı tuvaline ekledikten sonra 'Parameters' sekmesinde etkinliğin akışını parametrelerinde kullanılabilir verilerle sunulur.

![Bir veri akışı parametre ayarı](media/data-flow/parameter-assign.png "bir veri akışı parametre ayarı")

Parametre değerlerini ayarlamak için metin kutusunun tıkladığınızda, parametre veri türü dize ise, bir işlem hattı ya da bir veri akışı ifadesi girin seçebilirsiniz. İşlem hattı ifade seçerseniz, işlem hattı ifade Bölmesi ile sunulur. İçinde dize ilişkilendirme söz dizimini kullanarak işlem hattı işlevleri eklediğinizden emin olun ' @{<expression>}', örneğin:

```'@{pipeline().RunId}'```

Parametre türü dizesi değilse, her zaman veri akışı ifade Oluşturucu ile sunulur. Burada, herhangi bir ifade veya, istediğiniz parametrenin veri türüyle eşleşen değişmez değerler girebilirsiniz. İfade oluşturucu ve veri akışı ifadesi bir sabit dize örnekler aşağıda verilmiştir:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Her eşleme veri akışı ardışık düzen ve veri akışı ifade parametreleri herhangi bir birleşimi olabilir. 

![Veri akışı parametreleri örnek](media/data-flow/parameter-example.png "veri akışı parametre örneği")



## <a name="next-steps"></a>Sonraki adımlar
* [Yürütme veri akışı etkinliği](control-flow-execute-data-flow-activity.md)
* [Denetim akışı ifadeleri](control-flow-expression-language-functions.md)
