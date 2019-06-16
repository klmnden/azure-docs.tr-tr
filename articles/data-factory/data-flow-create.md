---
title: Veri akışı eşleme Azure veri fabrikası oluşturma
description: Bir Azure Data Factory eşleme veri akışı oluşturma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 366ed60534544ebbf889e2f72fe703f9b821f871
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65235661"
---
# <a name="create-azure-data-factory-data-flow"></a>Azure Data Factory veri akışı oluşturma

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Eşleme veri akışları'nda ADF gerekli kodlamaya gerek kalmadan ölçekli verileri dönüştürmek için bir yol sağlar. Bir dizi dönüşümleri oluşturarak bir veri dönüşüm işi veri akışı Tasarımcısı'nda tasarlayabilirsiniz. Kaynak dönüşümleri veri dönüştürme adımı tarafından izlenen herhangi bir sayıda başlayın. Ardından, sonuçlarınızı yerleşmesi bir hedef havuz ile veri akışını tamamlayın.

İlk olarak Azure portalından yeni V2 veri Fabrikasına oluşturarak başlayın. Yeni Fabrika oluşturduktan sonra Data Factory kullanıcı arabirimini başlatmak için "Yazar ve İzleyici" kutucuğuna tıklayın.

![Veri akışı seçenekleri](media/data-flow/v2portal.png "veri akışı oluşturma")

Data Factory kullanıcı Arabiriminde eklendiğinde, örnek veri akışları kullanabilirsiniz. Örnekler ADF şablonu galerisinden kullanılabilir. ADF, "İşlem hattı şablondan" oluşturma ve şablon Galeriden veri akışı kategoriyi seçin.

![Veri akışı seçenekleri](media/data-flow/template.png "veri akışı oluşturma")

Azure Blob Depolama hesap bilgilerinizi girmeniz istenir.

![Veri akışı seçenekleri](media/data-flow/template2.png "veri akışı Oluştur 2")

[Bu örnekler için kullanılan verileri burada bulunabilir](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Örnek verileri indirme ve örnekleri çalıştırabilmeniz için dosyaları, Azure Blob Depolama hesaplarında depolayın.

## <a name="create-new-data-flow"></a>Yeni veri akışı oluşturma

Veri akışları oluşturmak için ADF Arabiriminde Kaynağı Oluştur "artı işaretini" düğmesini kullanın.

![Veri akışı seçenekleri](media/data-flow/newresource.png "yeni kaynak")

## <a name="next-steps"></a>Sonraki adımlar

Veri dönüşümünüzü oluşturmaya başlamak bir [kaynak dönüştürme](data-flow-source.md).
