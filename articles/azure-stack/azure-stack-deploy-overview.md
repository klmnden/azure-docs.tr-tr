---
title: "Azure yığın Geliştirme Seti değerlendirmek | Microsoft Docs"
description: "Değerlendirme amacıyla Azure yığın Geliştirme Seti dağıtmayı öğrenin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/22/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: 4ad2e0a91e2fd5023417722fc0a1a6fae93960d0
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Hızlı Başlangıç: Azure yığın Geliştirme Seti değerlendir

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

[Azure yığın Geliştirme Seti](azure-stack-poc.md) değerlendirmek ve Azure yığın özelliklerin ve hizmetler için dağıtabileceğiniz bir test ve geliştirme ortamıdır. Bunu almak için hazır ve çalışır, ortam donanımını hazırlayın ve (Bu işlem birkaç saat sürebilir) bazı kodlar çalıştırmak gerekir. Bundan sonra Azure yığın yönetmek ve teklifleri test etmek için yönetici ve kullanıcı portalı için oturum açabilirsiniz. 

1. [**Donanım, yazılım ve ağ planlama**](azure-stack-deploy.md). Geliştirme Seti (Geliştirme Seti ana bilgisayarı) barındıran bilgisayarın donanım, yazılım ve ağ gereksinimleri karşılaması gerekir. Ayrıca, Azure Active Directory veya Active Directory Federasyon Hizmetleri'ni kullanma arasında seçmeniz gerekir. Yükleme işlemi sorunsuz çalışır dağıtımınıza başlamadan önce bu önkoşulları yerine uyumlu emin olun. 

2. [**Karşıdan yükleme ve dağıtım paketi ayıklayın**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit). Dağıtım paketi Geliştirme Seti ana bilgisayara veya başka bir bilgisayara yükleyebilirsiniz. Başka bir bilgisayar kullanılarak Geliştirme Seti ana bilgisayar için donanım gereksinimlerini azaltmaya yardımcı olacak ayıklanan dağıtım dosyaları 60 GB boş disk alanı alın.

3. [**Geliştirme Seti konak hazırlama** ](azure-stack-run-powershell-script.md) yükleyici kullanarak. Bu adımdan sonra Geliştirme Seti ana bilgisayar (önyüklenebilir bir işletim sistemi ve Azure yığını içeren sanal bir sabit sürücüye yükleme dosyaları) Cloudbuilder.vhdx önyükleme yapmaz.

4. [**Geliştirme Seti dağıtmak** ](azure-stack-run-powershell-script.md) Geliştirme Seti konaktaki.

5. Azure yığın dağıtımınızı Azure Active Directory kullanıyorsa, yapmanız gerekenler [Azure yığın Azure ile kaydedin](azure-stack-register.md) yapabilmeniz [Azure Market öğesi karşıdan](azure-stack-download-azure-marketplace-item.md) Azure yığınına.

Bu adımları tamamladıktan sonra yönetici ve Kullanıcı Portalı ile Geliştirme Seti ortamını sahip olacaksınız. Ardından, şunları yapabilirsiniz [bağlanma ve oturum açma](azure-stack-connect-azure-stack.md) portalı. Daha sonra başlatabilirsiniz kaynak sağlayıcıları dağıtma, oluşturma [sunar](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions)ve Azure yığın doldurmak [Market](azure-stack-marketplace.md).
