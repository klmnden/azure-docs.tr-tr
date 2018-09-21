---
title: Azure Stack için uygulamalar geliştirin | Microsoft Docs
description: Prototip oluşturma uygulamaları Azure Stack için geliştirme konuları
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 3be22e7f8e69ded8ccc8956cc7fd7c6d71fe5fa1
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497748"
---
# <a name="develop-for-azure-stack"></a>Azure Stack için geliştirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bir Azure Stack ortama erişiminiz yoksa bile uygulamalar bugün geliştirmeye başlayabilirsiniz. Azure Stack, veri merkezinizde çalışan Microsoft Azure hizmetleri sunar çünkü benzer araçları ve işlemleri Azure ile olduğu gibi Azure Stack karşı geliştirmek için kullanabilirsiniz. 

## <a name="development-considerations"></a>Geliştirme konuları

Bazı hazırlık ve aşağıdaki konulardaki yönergeleri kullanarak, bir Azure Stack ortamına öykünmek için Azure'ı kullanabilirsiniz.

* Azure'da, Azure Stack için dağıtılabilen bir Azure Resource Manager şablonları oluşturabilirsiniz. Bkz: [şablonu konuları](azure-stack-develop-templates.md) taşınabilirliği sağlamak için şablon geliştirme konusunda yönergeler için.
* Hizmet kullanılabilirliği ve hizmet sürümü oluşturma Azure ve Azure Stack arasında farklılıklar vardır. Kullanabileceğiniz [Azure Stack ilke modülü](azure-stack-policy-module.md) nedir kullanılabilir Azure Stack'te Azure hizmet kullanılabilirliği ve kaynak türlerini kısıtlamak için. Hizmetleri sınırlamak için uygulamalarınızı Azure Stack için kullanılabilen hizmetler kullandığını sağlar.
* [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates) göstermek için Azure ve Azure Stack dağıtılabilir şablonlarını nasıl geliştireceğinizi yaygın senaryo örnekleri.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack geliştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Resource Manager şablonu en iyi uygulamaları](azure-stack-develop-templates.md)
- [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates)