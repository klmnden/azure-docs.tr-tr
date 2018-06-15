---
title: Gelişmiş Azure yığın değerlendirme görevleri | Microsoft Docs
description: Bu makalede, Gelişmiş Azure yığın değerlendirme görevleri açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: c4bf76aa07ec5025d9e53b5518929199ace27e18
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
ms.locfileid: "29975828"
---
# <a name="advanced-azure-stack-development-kit-evaluation-tasks"></a>Gelişmiş Azure yığın Geliştirme Seti değerlendirme görevleri
Temel Azure yığın Geliştirme Seti (ASDK) hizmet özellikleri ve yetenekleri öğrendikten sonra daha gelişmiş senaryolarını test etmeye göre daha fazla Azure yığın Anlayışınızı deepen. Bu daha gelişmiş değerlendirme görevleri Azure yığın işleci belgelerinde tam olarak belgelenmiştir.

> [!NOTE]
> Birçok işleci görevleri ASDK ve üretim, çok düğümlü Azure yığın dağıtımlar için desteklenirken tüm kullanım senaryoları ASDK dağıtımları için desteklenir. Bkz: [ASDK ve çok düğümlü Azure yığın farklar](asdk-what-is.md#asdk-and-multi-node-azure-stack-differences) daha fazla bilgi için.

## <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme
Azure yığın operatör olarak genellikle teklifleri oluşturma ve kullanıcıları imzalama sorumlu diğer kişilerin yerleştirmek istediğiniz. Örneğin, bir hizmet sağlayıcı barındırıyorsanız, satıcılar müşterileri oturum ve bunları sizin adınıza yönetmek isteyebilirsiniz. Ya da bir kuruluşta merkezi BT grubunun bir parçası değilseniz, kullanıcıları, müdahalesi olmadan oturum yan kuruluşlarının isteyebilirsiniz.

[Azure yığınında teklifleri için temsilci seçme](.\.\azure-stack-delegated-provider.md) erişmek ve doğrudan daha fazla kullanıcı yönetmek olası hale getirerek bu görevleri yardımcı olur. 

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>SQL veritabanları Azure yığın kullanıcılarınızın kullanımına sunun
Bir Azure yığın operatör olarak, kullanıcılarınıza teklifleri oluşturabilirsiniz (kiracılar) bulut yerel uygulamalar, Web siteleri ve iş yükleri kullanabilmeniz için SQL veritabanları oluşturun. Kullanıcılarınız için bu, isteğe bağlı, özel bulut tabanlı veritabanları sağlayarak bunları zaman ve kaynak kaydedebilirsiniz. 

SQL Server Kaynak sağlayıcısı bağdaştırıcıya kullanmak [SQL veritabanları Azure yığın kullanıcılarınız için kullanılabilir hale](.\.\azure-stack-tutorial-sql-server.md) Azure yığınının hizmet olarak. Kaynak sağlayıcısını yükledikten sonra bir veya daha fazla SQL Server örneklerine bağlayın.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web ve API uygulamaları Azure yığın kullanıcılarınızın kullanımına sunun
Bir Azure yığın operatör olarak, kullanıcılarınıza teklifleri oluşturabilirsiniz (kiracılar) Azure işlevleri ve web ve API uygulamaları oluşturun. Kullanıcılarınız için bu isteğe bağlı, bulut tabanlı uygulamalara erişim sağlayarak bunları zaman ve kaynak kaydedebilirsiniz.

Uygulama hizmeti kaynak sağlayıcısı için dağıtmak [web ve API uygulamaları Azure yığın kullanıcılarınızın kullanımına sunun](.\.\azure-stack-tutorial-app-service.md)

## <a name="next-steps"></a>Sonraki adımlar
[Azure tümleşik yığını sistemleriyle hizmetleri sunan hakkında daha fazla bilgi edinin](.\.\azure-stack-offer-services-overview.md)