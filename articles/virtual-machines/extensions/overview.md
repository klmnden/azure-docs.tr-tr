---
title: Azure sanal makine uzantıları ve özellikleri | Microsoft Docs
description: Hangi Azure VM uzantıları anre Azure sanal makinelerle kullanmayı öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
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
ms.author: danis
ms.openlocfilehash: 01178995dbf9203082a6250ef256522bc1101e57
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Azure sanal makine uzantıları ve özellikleri
Azure Vm'lerinde dağıtım sonrası yapılandırma ve Otomasyon görevlerini sağlayan küçük uygulamalar Azure sanal makine (VM) uzantıları, var olan görüntüleri kullanın ve ardından özel iş dışı alma, dağıtımlarınızın bir parçası olarak özelleştirin görüntü oluşturma.

Azure platformu VM yapılandırması, izleme, güvenlik ve yardımcı programı uygulamaları aralığı birçok uzantıları barındırır. Yayımcılar bir uygulamayı yap, sonra uzantı sarmalayın ve tüm yapmanız gereken şekilde zorunlu parametreleri sağlamak yükleme basitleştirin. 

 Uzantı depo uygulamada yoksa, birinci ve üçüncü taraf uzantıları büyük seçimine olduğu sonra özel betik uzantısı VM kendi betikleri ve komutları ile yapılandırın.

Uzantılar için kullanılan anahtar senaryo örnekleri:
* VM yapılandırması VM yapılandırma aracıları yüklemek ve VM'yi yapılandırmak için (İstenen durum yapılandırması) Powershell DSC, Chef, Puppet ve özel betik uzantılarının kullanabilirsiniz. 
* Symantec, Sıfırla gibi AV ürünler.
* Qualys, Rapid7, HPE gibi VM Güvenlik Açığı aracı.
* VM ve DynaTrace, Azure Ağ İzleyicisi, Site24x7 ve Stackify gibi araçları izleme uygulaması.

Uzantıları yeni VM Dağıtım ile birlikte. Örneğin, uygulamaları VM sağlama üzerinde yapılandırma daha büyük bir dağıtımın bir parçası olması veya tüm desteklenen uzantısı işletilen sistemleri dağıtım sonrası karşı çalıştırın.

## <a name="how-can-i-find-what-extensions-are-available"></a>Nasıl hangi uzantıları kullanılabilir bulabilirim?
Kullanılabilir uzantılar Uzantılar altında Portalı'nda VM dikey penceresinde görüntüleyebilirsiniz, küçük bir miktar yalnızca, bu temsil eder tam listesi için CLI araçlarını kullanın, bkz: [Linux VM uzantıları keşfetme](features-linux.md) ve [ Windows için VM uzantıları keşfetme](features-windows.md).

## <a name="how-can-i-install-an-extension"></a>Uzantı nasıl yükleyebilir miyim?
Azure VM uzantıları, Azure CLI 2.0, Azure PowerShell, Azure Resource Manager şablonları ve Azure portalını kullanarak yönetilebilir. Uzantı denemek için Azure portal, select özel betik uzantısının gidin, sonra bir komut geçirmek / komut dosyası ve Uzantıları'nı çalıştırın.

Eklenen için aynı uzantı istiyorsanız, portal CLI ya da Resource Manager şablonu tarafından farklı uzantı belgelerine gibi bakın [Windows özel betik uzantısının](custom-script-windows.md) ve [Linux özel betik uzantısının](custom-script-linux.md).

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Uzantı uygulama yaşam döngüsü nasıl yönetebilirim?
Doğrudan yüklemek veya uzantısını silmek için bir VM öğesine bağlanmak gerekmez. Azure uzantısı uygulama yaşam döngüsü VM dışında yönetilen ve Azure platformuyla doğrudan tümleşik olarak da uzantısının tümleşik durumunu alın.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Başka bir şey t hakkında uzantıları düşünüyorsunuz?
Uzantıları uygulamaları yüklemek, var olan tüm uygulamalar gibi bazı gereksinimler vardır uzantıları için desteklenen Windows ve Linux işletim sistemleri listesi verilmiştir ve Azure VM aracıları yüklenmiş olması gerekir. Bazı bağımsız VM uzantısı uygulamalar için bir bitiş noktasına erişim gibi kendi ortam önkoşulları olabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Linux aracısı ve uzantılar nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure VM uzantıları ve Linux için özellikleri](features-linux.md).
* Windows Konuk aracısı ve uzantılar nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure VM uzantıları ve Linux için özellikleri](features-windows.md).  
* Windows Konuk Aracısı'nı yüklemek için bkz: [Azure Windows sanal makine Aracısı genel bakış ](agent-windows.md).  
* Linux Aracısı'nı yüklemek için bkz: [Azure Linux sanal makine Aracısı genel bakış ](agent-linux.md).  

