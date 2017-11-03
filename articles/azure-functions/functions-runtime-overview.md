---
title: "Azure işlevleri çalışma zamanına genel bakış | Microsoft Docs"
description: "Azure işlevleri çalışma zamanı önizlemesi genel bakış"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: cb98d5f2aaa526555820c15ba5a32fb7e78ffc5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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

* Azure işlevleri yönetim olan Portalı'nın, barındırma aynı gördüğünüz [Azure portal](https://portal.azure.com). Bu, Azure portalında gibi aynı şekilde işlevlerinizi geliştirmenize olanak tanır.
* İşlevler arasında birden çok işlevleri Worker dağıtma.
* Microsoft Visual Studio, işlevleri doğrudan yayımlaması yayımlama bir uç noktası sağlama.

## <a name="azure-functions-worker-role"></a>Azure işlevleri çalışan rolü

Azure işlevleri çalışan rolleri Windows kapsayıcılarında dağıtılır ve bu işlev kodunuzu nereye yürütür.  Birden çok çalışan rolleri kuruluşunuz genelinde dağıtabilirsiniz ve müşteriler hale getirebilir anahtar şekilde yedek işlem gücünü kullanın.

## <a name="minimum-requirements"></a>En düşük gereksinimler

Azure işlevleri çalışma zamanı ile çalışmaya başlamak için bir makine olmalıdır **Windows Server 2016 veya Windows 10 oluşturucuları Update** erişimi olan bir **SQL Server** örneği.

## <a name="next-steps"></a>Sonraki Adımlar

Yükleme [Azure işlevleri çalışma zamanı Önizleme](https://aka.ms/azafr)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
