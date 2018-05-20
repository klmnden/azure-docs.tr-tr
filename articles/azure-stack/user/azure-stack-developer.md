---
title: Uygulama geliştirmek için Azure yığın | Microsoft Docs
description: Azure yığında prototipi oluşturulurken uygulamalar için geliştirme konuları
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a1b5a90ca40ce2b19186220344b22ec0ae77e34b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="develop-for-azure-stack"></a>Azure Stack için geliştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir Azure yığın ortamına erişimi yoksa bile uygulamaları Bugün, geliştirme başlayabiliriz. Azure yığını, veri merkezinizde çalışan bir Microsoft Azure hizmetleri sunan olduğundan, Azure ile olduğu gibi Azure yığın karşı geliştirmek için benzer araçlarını ve işlemlerini kullanabilirsiniz. Bazı hazırlıklar ve aşağıdaki konulardaki yönergeleri kullanarak, bir Azure yığın ortamını benzetmek için Azure kullanabilirsiniz.

* Azure'da, Azure yığınına dağıtılabilir Azure Resource Manager şablonları oluşturabilirsiniz. Bkz: [şablonu konuları](azure-stack-develop-templates.md) taşınabilirlik emin olmak için şablonlar geliştirme konusunda yönergeler için.
* Hizmet kullanılabilirliği ve hizmet sürümü oluşturma Azure yığını ile Azure arasındaki farklılıklar vardır. Kullanabileceğiniz [Azure yığın ilke modülü](azure-stack-policy-module.md) Azure yığınında kullanılabilir olanlarla Azure hizmet kullanılabilirliği ve kaynak türlerini kısıtlamak üzere. Hizmetleri sınırlama, uygulamalarınızın Azure yığını tarafından kullanılabilen hizmetler kullandığını sağlar.
* [Azure yığın hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates) Azure ve Azure yığın dağıtılan şablonlarını nasıl geliştireceğinizi Göster yaygın senaryo örnek verilmiştir.
