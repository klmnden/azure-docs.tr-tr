---
title: Azure Data Factory eşleme veri akışı parametreleri
description: Data factory işlem hatlarını eşleme veri akışından Parametreleştirme öğrenin
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: af5f421cc3802f3a7ad44bb294f5066c32569f8b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082891"
---
# <a name="mapping-data-flow-parameters"></a>Veri akışı parametreleri eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Data factory'de akışlar, veri eşleme parametrelerinin kullanımını destekler. Parametreler, ifadeleri ardından kullanabilirsiniz, veri akış tanımı içinde tanımlayabilirsiniz. Parametreleri, ardından arama işlem hattı yürütme veri akışı etkinliği aracılığıyla tarafından ayarlanabilir. Değerleri, veri akışında etkinlik ifadeleri ayarlamak üzere kullanmak için üç seçeneğiniz vardır:

* Dinamik bir değer ayarlamak için işlem hattı denetim akışı ifadesi dili kullanın
* Dinamik bir değer ayarlamak için veri akışı ifade dili kullanın
* Statik değişmez değer ayarlamak için her iki ifade dili kullanın

Veri akışlarınızı genel amaçlı, esnek ve yeniden kullanılabilir hale getirmek için bu özelliği kullanın. Veri akış ayarları ve bu parametreleri ifadelerle parametreleştirebilirsiniz.

> [!NOTE]
> İşlem hattı denetim akışı ifadeleri kullanmak için veri akışı parametresi dize türünde olmalıdır.

* Bir yürütme veri akışı etkinliğini işlem hattı tuvaline ekleyin.
* Veri akışınız parametreleri varsa her parametre, parametre değerini girin yanındaki metin kutusuna tıklayın, parametreleri tab.* * kullanılabilir parametrelerin listesini görürsünüz.
* Parametre ifadesi aracılığıyla işlem hattı denetim akışı ifadesi dili veya veri akışı ifadeleri oluşturmayı seçebilirsiniz.

![Veri akışı parametre 3](media/data-flow/params3.png "veri akışı parametre 3")

## <a name="create-parameters-in-data-flow"></a>Parametreler veri akışı oluşturma

![Veri akışı parametreleri 1](media/data-flow/params1.png "veri akışı parametreleri 1")

Veri akışınızı parametre eklemek için genel özellikleri görmek için veri akışı tuvalinin boş bölümü temel tıklayın. Ayarlar bölmesinde parametreleri olarak adlandırılan bir sekme görürsünüz. Veri akışınızı değerler geçirerek işlem hattından ayarlanabilir yeni parametreleri oluşturmak için yeni düğmesine tıklayın. Bir parametre adını girin ve her parametre için veri türünü seçin.

Veri akışı deyimleri içinde ardışık düzen tarafından ayarlanan değerleri kullanarak parametrelerini kullanabilir. Parametreleri $ ile başlayın ve sabittir. Ayrıca, Parametreler sekmesi altında ifade oluşturucu içinde kullanılabilir parametrelerin listesini bulabilirsiniz. Parametreler için yeni değerleri devredemezsiniz rağmen bu değerler, ifade içinde kullanabilirsiniz.

![Veri akışı parametre 2](media/data-flow/params2.png "veri akışı parametre 2")

## <a name="set-data-flow-parameters-from-pipeline"></a>Veri akışı parametrelerini işlem hattından

Parametrelerle veri akışınızı oluşturduktan sonra yürütme veri akışı etkinliği ile bir işlem hattından bu veri akışı artık yürütebilir. Bu etkinlik, işlem hattı tasarım tuvale ekledikten sonra akış parametrelerinin sekmesinde ayarı etkinliğin parametrelerinde kullanılabilir verileri ile sunulur.

![Veri akışı parametreleri ifade dili](media/data-flow/params4.png "veri akışı parametreleri ifade dili")

Doldurma parametre değerleri metin kutusundaki tıkladığınızda, veri akışı ifade Oluşturucu ile sunulur. Burada, herhangi bir ifade veya parametresinin veri türü eşleşen istediğiniz değişmez değerler girebilirsiniz. İfade oluşturucu ve veri akışı ifadesi bir sabit dize örnekler aşağıda verilmiştir:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Ardından, parametre veri türü bir dize ise, bir işlem hattı ya da bir veri akışı ifadesi girin seçebilirsiniz. İşlem hattı ifade tercih ederseniz, bunun yerine ile işlem hattı ifade bölmesi sunulur. İçinde dize ilişkilendirme söz dizimini kullanarak işlem hattı işlevleri eklediğinizden emin olun ' @{<expression>}', örneğin:

```'@{pipeline().RunId}'```

![Veri akışı parametreleri örnek](media/data-flow/params5.png "veri akışı parametre örneği")

## <a name="next-steps"></a>Sonraki adımlar

* [Yürütme veri akışı etkinliği](control-flow-execute-data-flow-activity.md)
* [Denetim akışı ifadeleri](control-flow-expression-language-functions.md)
