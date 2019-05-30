---
title: Azure sanal makine uzantıları ve özellikleri | Microsoft Docs
description: Azure VM uzantıları nedir ve bunları Azure sanal makineler ile nasıl kullanacağınızı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: roiyz
ms.openlocfilehash: a35cba0ab7df80596ba1403765980809635c0249
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "60618005"
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Azure sanal makine uzantıları ve özellikleri
Azure sanal makine (VM) uzantıları Azure Vm'leri üzerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sunan küçük uygulamalar, mevcut görüntülerinizi kullanın ve ardından onları, dağıtımlarınızın bir parçası olarak özel faaliyet alma özelleştirin görüntü oluşturuluyor.

Azure platformu, VM yapılandırması, izleme, güvenlik ve yardımcı programı uygulamaları aralığı birçok uzantıları barındırır. Yayımcılar bir uygulama yararlanın, ardından bir uzantı sarmalamak ve yükleme, tek yapmanız gereken, bu nedenle zorunlu parametreler sağlamak basitleştirin. 

 Uzantı deposunda uygulama yoksa, birinci ve üçüncü taraf uzantıları, büyük bir seçim olan ve ardından özel betik uzantısı kullanın ve sanal makinenizin kendi betikleri ve komutları ile yapılandırın.

Uzantılar için kullanılan anahtar senaryo örnekleri:
* VM yapılandırması, VM yapılandırması aracıları yükleme ve sanal makinenize yapılandırmak için (Desired State Configuration) Powershell DSC, Chef, Puppet ve özel betik uzantıları kullanabilirsiniz. 
* AV ürünleri, Symantec, ESET gibi.
* Qualys Rapid7, HPE gibi güvenlik açığı aracı VM.
* VM ve DynaTrace, Azure Ağ İzleyicisi, Site24x7 ve Stackify'i gibi bir araç kullanımı izleme uygulaması.

Uzantılar, yeni bir VM dağıtımı ile gönderilebilir. Örneğin, uygulamaları üzerinde VM sağlama, yapılandırma, daha büyük bir dağıtımın bir parçası olması veya tüm desteklenen uzantısı işletilen sistemleri post dağıtımına yönelik olarak çalıştırılır.

## <a name="how-can-i-find-what-extensions-are-available"></a>Nasıl hangi uzantıların kullanılabilir olduğunu bulabilirsiniz?
Kullanılabilir uzantılar portalında uzantılarının altında VM dikey penceresinde görüntüleyebilirsiniz, bu yalnızca küçük bir miktar, temsil eder tam listesi için CLI araçlarından yararlanın, bkz: [Linux için VM uzantıları bulma](features-linux.md) ve [ Windows için VM uzantıları bulma](features-windows.md).

## <a name="how-can-i-install-an-extension"></a>Bir uzantıyı nasıl yükleyebilirim?
Azure VM uzantıları, Azure CLI, Azure PowerShell, Azure Resource Manager şablonları ve Azure portalı kullanılarak yönetilebilir. Uzantı denemek için Azure portalında, select özel betik uzantısı gidin, ardından bir komuta geçirin / komut dosyası ve Uzantıları'nı çalıştırın.

CLI veya Resource Manager şablonu tarafından portalda eklenen aynı uzantı istiyorsanız, farklı bir uzantı belgeler gibi bkz [Windows özel betik uzantısı](custom-script-windows.md) ve [Linux özel betik uzantısı'nı](custom-script-linux.md).

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Uzantı uygulama yaşam döngüsünü nasıl yönetebilirim?
Doğrudan yüklemek ya da uzantı silmek için bir VM'ye bağlanmak gerekmez. Azure uzantısı uygulama yaşam döngüsünü VM dışında yönetilen ve Azure platformuyla doğrudan tümleşik olarak da uzantı tümleşik durumunu alın.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Başka bir şey miyim hakkında için uzantıları düşünmek?
Uygulama uzantıları yüklemek, var olan tüm uygulamalar gibi var. uzantıları için bazı gereksinimleri, desteklenen Windows ve Linux işletim sistemleri listesi verilmiştir ve Azure VM aracıları yüklü olması gerekir. Bazı tek VM uzantısı uygulamaları gibi bir uç nokta için erişimi kendi ortam önkoşulları olabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Linux aracısı ve uzantıları nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure VM uzantıları ve özellikleri için Linux](features-linux.md).
* Windows Konuk aracısı ve uzantıları nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure VM uzantıları ve özellikleri Windows için](features-windows.md).  
* Windows Konuk aracısı yüklemek için bkz [Azure Windows sanal makine Aracısı genel bakış](agent-windows.md).  
* Linux aracısını yüklemek için bkz: [Azure Linux sanal makine Aracısı genel bakış](agent-linux.md).  

