---
title: Veri akışı eşleme Azure veri fabrikası oluşturma
description: Veri akışı eşleme Azure veri fabrikası oluşturma
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: b706e229bed48c821d5ca772450df320fd7e0b7f
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272139"
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

Veri akışları oluşturmak için ADF Arabiriminde Kaynağı Oluştur "artı işaretini" düğmesini kullanın

![Veri akışı seçenekleri](media/data-flow/newresource.png "yeni kaynak")

