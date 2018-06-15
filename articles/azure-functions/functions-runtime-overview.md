---
title: Azure işlevleri çalışma zamanına genel bakış | Microsoft Docs
description: Azure işlevleri çalışma zamanı önizlemesi genel bakış
services: functions
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 557f071e2cd8d4f639c881274e6e74a8fb745859
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
ms.locfileid: "26290239"
---
# <a name="azure-functions-runtime-overview"></a>Azure işlevleri çalışma zamanına genel bakış

Azure işlevleri çalışma zamanı programlama modeli şirket içi kolaylık ve esneklik Azure işlevlerinin yararlanacak yeni bir yol sağlar. Azure işlevleri olarak aynı açık kaynak kökleri yerleşik olarak, dağıtılmış bir bulut hizmeti olarak neredeyse aynı geliştirme deneyimi sağlamak için şirket içi Azure işlevleri çalışma zamanı olur.

![Azure işlevleri çalışma zamanı Önizleme portalı][1]

Azure işlevleri çalışma zamanı buluta kaydetmeden önce Azure işlevleri deneyimi bir yol sağlar. Geçirdiğinizde bu şekilde, oluşturacağınız kod varlıklarına sonra sizinle buluta alınabilir.  Toplu işlemler günde çalıştırmak için şirket içi bilgisayarları yedek işlem gücünü kullanma gibi yeni seçeneklerini, çalışma zamanı da açılır. Koşullu başka sistemlere, hem şirket içi ve buluttaki veri göndermek için kuruluşunuz içinde cihazları da kullanabilirsiniz.

Azure işlevleri çalışma zamanı iki parça oluşur:

* Azure işlevleri çalışma zamanı Yönetimi rolü
* Azure işlevleri çalışma zamanı çalışan rolü

## <a name="azure-functions-management-role"></a>Azure işlevleri Yönetimi rolü

Azure işlevleri yönetim rolü işlevleri şirket içi yönetim için bir konak sağlar. Bu rol, aşağıdaki görevleri gerçekleştirir:

* Aynı biri içinde gördüğünüz Azure işlevleri Yönetim Portalı'nın barındırma [Azure portal](https://portal.azure.com). Portal, Azure portalında olduğu gibi aynı şekilde işlevlerinizi geliştirmenize olanak tanıyan tutarlı bir deneyim sağlar.
* İşlevler arasında birden çok işlevleri Worker dağıtma.
* Microsoft Visual Studio, işlevleri doğrudan indirme ve yayımlama profilini içeri aktarma yayımlaması yayımlama bir uç noktası sağlama.

## <a name="azure-functions-worker-role"></a>Azure işlevleri çalışan rolü

Azure işlevleri çalışan rolleri Windows kapsayıcılarında dağıtılan ve olduğunuz nerede işlevi kodunuzu yürütür.  Kuruluşunuz genelinde birden çok çalışan rolleri dağıtabilirsiniz ve bu seçeneği müşteriler hale getirebilir anahtar bir şekilde yedek işlem gücünü kullanabilirsiniz.  Bir yedek işlem birçok kuruluşta bulunduğu örneğidir sürekli açık makineler ancak uzun süreyle kullanılmıyor.

## <a name="minimum-requirements"></a>En düşük gereksinimler

Azure işlevleri çalışma zamanı ile çalışmaya başlamak için bir makineye Windows Server 2016 veya Windows 10 oluşturucuları Update ile bir SQL Server örneği erişimi olması gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

Yükleme [Azure işlevleri çalışma zamanı Önizleme](https://aka.ms/azafrdoc)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
