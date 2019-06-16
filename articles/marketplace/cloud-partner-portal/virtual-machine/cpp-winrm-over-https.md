---
title: Azure için Windows Uzaktan Yönetimi HTTPS üzerinden | Azure Market
description: PowerShell ile uzaktan yönetilebilir, Azure'da barındırılan, Windows tabanlı bir VM yapılandırma açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: pabutler
ms.openlocfilehash: fb661a2705d437d1f40ceebcad7e759c2a78540f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938219"
---
# <a name="windows-remote-management-over-https"></a>HTTPS üzerinden Windows Uzak Yönetimi

Bu bölümde, yönetilen ve PowerShell ile uzaktan dağıtılması için Azure'da barındırılan, Windows tabanlı bir VM yapılandırma açıklanmaktadır.  PowerShell uzaktan iletişimini etkinleştirmek için hedef sanal Makineyi bir Windows Uzaktan Yönetim (WinRM) HTTPS uç noktasını açığa çıkarmalıdır.  PowerShell uzaktan iletişimi hakkında daha fazla bilgi için bkz: [çalıştıran uzak komutları](https://docs.microsoft.com/powershell/scripting/core-powershell/running-remote-commands?view=powershell-6).  WinRM hakkında daha fazla bilgi için bkz: [Windows Uzaktan Yönetimi](https://docs.microsoft.com/windows/desktop/WinRM/portal).

"Klasik" Azure yaklaşımlardan birini kullanarak bir VM oluşturduysanız — Azure Service Manager portalı veya kullanım dışı [Azure Hizmet Yönetimi API'si](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100))— sonra bir WinRM uç noktasıyla otomatik olarak yapılandırılır.  Ancak, bir VM oluşturursanız, aşağıdakilerden herhangi birini kullanarak "modern" Azure yaklaşıyor sonra VM'nizi olacak *değil* , HTTPS üzerinden WinRM için yapılandırılmış.  

- Kullanarak [Azure portalında](https://portal.azure.com/), genellikle bölümünde açıklandığı gibi onaylı bir temel gelen [Azure ile uyumlu bir VHD oluşturun](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-vhd)
- [Azure Resource Manager şablonlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)
- Azure PowerShell veya Azure CLI komut kabuğunu kullanma.  Örnekler için bkz [hızlı başlangıç: PowerShell ile azure'da Windows sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell) ve [hızlı başlangıç: Azure CLI ile Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli).

Bu WinRM uç nokta ayrıca yetiştirme sertifika Araç Seti VM çalıştırmak için açıklandığı gibi gerekli [VM görüntünüzü sertifika](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-certify-vm).

Buna karşılık, genellikle Linux Vm'leri uzaktan kullanarak yönetilen [Azure CLI](https://docs.microsoft.com/cli/azure) veya SSH konsolundan Linux komutları.  Azure için alternatif çeşitli yöntemler sunar [Linux VM'nize betikleri çalıştırma](https://docs.microsoft.com/azure/virtual-machines/linux/run-scripts-in-vm).  Daha karmaşık senaryolarda, Windows veya Linux tabanlı VM'ler için kullanılabilen bir dizi otomasyon ve tümleştirme çözümleri vardır.


## <a name="configure-and-deploy-with-winrm"></a>Yapılandırma ve WinRM ile dağıtma

WinRM uç nokta windows tabanlı bir VM için iki farklı aşamalarında, geliştirme sırasında yapılandırılabilir:

- Oluşturma sırasında - varolan bir VHD'yi bir VM dağıtımı sırasında.  Yeni teklifler için tercih edilen yaklaşım budur.  Bu yaklaşım, sağlanan Azure Resource Manager şablonlarını kullanarak bir Azure sertifikası oluşturulmasını gerektirir ve PowerShell betikleri çalıştırma özelleştirilebilir. 
- Dağıtımdan sonra - Azure'da barındırılan mevcut bir VM üzerinde.  Zaten Azure'da dağıtılan bir VM çözümü varsa ve pencere uzaktan yönetimi etkinleştirmek gereken bu yaklaşımı kullanın.  Bu yaklaşım, Azure portalında ve hedef VM'de bir betik yürütme işlemi el ile yapılan değişiklikler gerektirir. 


## <a name="next-steps"></a>Sonraki adımlar
Yeni bir sanal makine oluşturuyorsanız, WinRM sırasında etkinleştirebilirsiniz [dağıtım kendi vhd'lerden sanal makinenizin](./cpp-deploy-vm-vhd.md).  Aksi takdirde, varolan bir VM'yi WinRM etkinleştirilebilir  
