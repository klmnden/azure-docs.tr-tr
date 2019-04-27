---
title: Veri akışı eşleme Azure veri fabrikası oluşturma
description: Veri akışı eşleme Azure veri fabrikası oluşturma
author: WenJason
ms.author: v-jay
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
origin.date: 02/12/2019
ms.date: 04/22/2019
ms.openlocfilehash: bb6ae9f97d681625218118b8adca116de1c0fb21
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60883763"
---
# <a name="create-azure-data-factory-data-flow"></a>Azure Data Factory veri akışı oluşturma

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Eşleme veri akışları'nda ADF gerekli kodlamaya gerek kalmadan ölçekli verileri dönüştürmek için bir yol sağlar. Bir dizi dönüşümleri oluşturarak bir veri dönüşüm işi veri akışı Tasarımcısı'nda tasarlayabilirsiniz. Kaynak dönüşümleri veri dönüştürme adımı tarafından izlenen herhangi bir sayıda başlayın. Ardından, sonuçlarınızı yerleşmesi bir hedef havuz ile veri akışını tamamlayın.

İlk olarak Azure portalından yeni V2 veri Fabrikasına oluşturarak başlayın. Yeni Fabrika oluşturduktan sonra Data Factory kullanıcı arabirimini başlatmak için "Yazar ve İzleyici" kutucuğuna tıklayın.

![Veri akışı seçenekleri](media/data-flow/v2dataflowportal.png "veri akışı oluşturma")

Data Factory kullanıcı Arabiriminde eklendiğinde, örnek veri akışları kullanabilirsiniz. Örnekler ADF şablonu galerisinden kullanılabilir. ADF, "İşlem hattı şablondan" oluşturma ve şablon Galeriden veri akışı kategoriyi seçin.

![Veri akışı seçenekleri](media/data-flow/template.png "veri akışı oluşturma")

Azure Blob Depolama hesap bilgilerinizi girmeniz istenir.

![Veri akışı seçenekleri](media/data-flow/template2.png "veri akışı Oluştur 2")

[Bu örnekler için kullanılan verileri burada bulunabilir](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Örnek verileri indirme ve örnekleri çalıştırabilmeniz için dosyaları, Azure Blob Depolama hesaplarında depolayın.

## <a name="create-new-data-flow"></a>Yeni veri akışı oluşturma

Veri akışları oluşturmak için ADF Arabiriminde Kaynağı Oluştur "artı işaretini" düğmesini kullanın

![Veri akışı seçenekleri](media/data-flow/newresource.png "yeni kaynak")

## <a name="next-steps"></a>Sonraki adımlar

Veri dönüşümünüzü oluşturmaya başlamak bir [kaynak dönüştürme](data-flow-source.md).
