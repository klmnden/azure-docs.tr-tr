---
title: Kopyalayın veya Azure Data Factory bir veri fabrikasında kopyalama | Microsoft Docs
description: Kopyalayın veya Azure Data Factory bir veri fabrikasında kopyalama hakkında bilgi edinin
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: douglasl
ms.openlocfilehash: c62581447cd395bd48a787fa7dc89659d5172486
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192301"
---
# <a name="copy-or-clone-a-data-factory-in-azure-data-factory"></a>Kopyalayın veya Azure Data Factory bir veri fabrikasında kopyalama

Bu makalede, kopyalama veya Azure Data Factory bir veri fabrikasında kopyalama açıklar.

## <a name="use-cases-for-cloning-a-data-factory"></a>Data factory kopyalama için kullanım örnekleri

İçine kopyalayın veya veri fabrikası kopyalamak faydalı durumlarda bazıları şunlardır:

-   **Kaynakları yeniden adlandırma**. Azure, yeniden adlandırma kaynakları desteklemiyor. Veri Fabrikası yeniden adlandırmak isterseniz, farklı bir ada sahip data factory kopyalama ve var olan bir silin.

-   **Değişiklikleri hata ayıklama** , hata ayıklama özelliklerini yeterli değildir. Bazen değişikliklerinizi test etmek için ana bir uygulamadan önce farklı bir fabrikada değişikliklerinizi test isteyebilirsiniz. Çoğu senaryoda, hata ayıklama kullanabilirsiniz. Tetikleyiciler, değişiklikleri ancak, bir tetikleyici, değişikliklerinizi nasıl davranacağını gibi otomatik olarak çağrılan veya bir zaman penceresi kolayca etmeden test edilebilir olmayabilir. Bu durumlarda, Fabrika kopyalama ve var. yaptığınız değişiklikleri uygulanıyor mantıklı hale getirir. Azure Data Factory öncelikle çalıştırmaları sayısına göre ücretlendirilen olduğundan, ikinci Fabrika ek ücretlerine neden olmaz.

## <a name="how-to-clone-a-data-factory"></a>Veri Fabrikası kopyalamak nasıl

1. Azure portalında Data Factory kullanıcı Arabiriminde tüm yüküne veri fabrikanızın fabrikanızı kopyaladığınızda değiştirmek istediğiniz herhangi bir değeri değiştirmenize olanak sağlayan bir parametre dosyasıyla birlikte bir Resource Manager şablonunu dışarı aktarmanıza olanak tanır.

1. Bir önkoşul olarak hedef data factory'nizi Azure portalından oluşturmanız gerekir.

1. Portaldan yayımladığınız her zaman GIT modunda iseniz, fabrikasının Resource Manager şablonu GIT deposunun adf_publish dalındaki kaydedilir.

1. Diğer senaryolar için Resource Manager şablonu tıklayarak indirilebilir **dışarı Resource Manager şablonu** Portal'daki düğmesi.

1. Resource Manager şablonu indirdikten sonra standart Resource Manager şablonu dağıtım yöntemleri dağıtabilirsiniz.

1. Güvenlik nedenleriyle, oluşturulan bir Resource Manager şablonu bağlı hizmetler için parolalar gibi herhangi bir gizli bilgi içermiyor. Sonuç olarak, bu parolaları dağıtım parametreleri olarak sağlamanız gerekir. Parametreleri sağlayarak arzu değil ise, bağlantı dizeleri ve bağlı hizmetler, parolalar Azure Key Vault'tan edinmek zorunda.

## <a name="next-steps"></a>Sonraki adımlar

Azure portalında bir veri fabrikası oluşturmaya yönelik yönergeleri gözden [Azure Data Factory UI kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md).