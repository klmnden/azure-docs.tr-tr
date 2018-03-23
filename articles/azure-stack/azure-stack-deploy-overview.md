---
title: Azure yığın Geliştirme Seti değerlendirmek | Microsoft Docs
description: Değerlendirme amacıyla Azure yığın Geliştirme Seti dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/22/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: 1deabcf64b3fbf3cbc89232c340a8882cd2591e8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Hızlı Başlangıç: Azure yığın Geliştirme Seti değerlendir
[Azure yığın Geliştirme Seti (ASDK)](.\asdk\asdk-what-is.md) değerlendirmek ve Azure yığın özelliklerin ve hizmetler için dağıtabileceğiniz bir test ve geliştirme ortamıdır. ASDK ile çalışmaya başlamak için ana bilgisayar donanımını hazırlayın ve (yükleme birkaç saat sürer) bazı kodlar çalıştırmak gerekir. Bundan sonra yönetici ve kullanıcı portalı için Azure yığın kullanmaya başlamak için oturum açabilir.

## <a name="prerequisites"></a>Önkoşullar 
ASDK yüklemeden önce Geliştirme Seti (Geliştirme Seti ana bilgisayarı) barındıracak bilgisayarı hazırlamanız gerekir. Geliştirme Seti ana bilgisayarı, en düşük donanım, yazılım ve ağ gereksinimleri karşılaması gerekir. 

Ayrıca Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanma arasında seçmek kimliği çözümü olarak dağıtımınız için gerekir. 

Geliştirme Seti konak en düşük donanım gereksinimlerini karşıladığından ve yükleme işlemini sorunsuz çalışır dağıtımınıza başlamadan önce yapılan kimlik çözümü kararınızı yaptığınız emin olun. 

**[ASDK dağıtım planlama konuları gözden geçirin](.\asdk\asdk-deploy-considerations.md)**

> [!TIP]
> Kullanabileceğiniz [Azure yığın dağıtım gereksinimlerini denetleyin aracı](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınız tüm gereksinimleri karşıladığını onaylamak için Geliştirme Seti ana bilgisayarda işletim sisteminin yükledikten sonra.

## <a name="download-and-extract-the-deployment-package"></a>Karşıdan yükleme ve dağıtım paketi ayıklayın
Geliştirme Seti ana bilgisayarınız ASDK yüklemek için temel gereksinimleri karşıladığından emin olduktan sonra yüklemek ve ASDK dağıtım paketi ayıklamak için sonraki adım olacaktır. Dağıtım paketi, önyüklenebilir bir işletim sistemi ve Azure yığın yükleme dosyalarını içeren bir sanal sabit sürücü Cloudbuilder.vhdx dosyası içerir.

Dağıtım paketi Geliştirme Seti ana bilgisayara veya başka bir bilgisayara yükleyebilirsiniz. Başka bir bilgisayar kullanılarak Geliştirme Seti ana bilgisayar için donanım gereksinimlerini azaltmaya yardımcı olacak ayıklanan dağıtım dosyaları 60 GB boş disk alanı alın.

**[İndirmeyi ve ayıklamayı Azure yığın Geliştirme Seti (ASDK)](.\asdk\asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti ana bilgisayarı hazırla
Ana bilgisayara ASDK yükleyebilmek için önce ortamı hazırlanmalıdır ve sistem VHD'den önyükleme için yapılandırılmış. Geliştirme Seti ana bilgisayar hazır olduktan sonra ASDK dağıtım başlayabilmesi CloudBuilder.vhdx sanal makine sabit sürücüdeki önyüklenir.

**[ASDK ana bilgisayarı hazırla](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Ana bilgisayara ASDK yükleyin
Geliştirme Seti ana bilgisayar hazırladıktan sonra ASDK CloudBuilder.vhdx görüntüsüne dağıtılabilir. ASDK yükleme ve asdk installer.ps1 PowerShell komut dosyası çalıştırma veya tamamen gelen sağlanan bir grafik kullanıcı arabirimi (GUI) kullanılarak dağıtılabilir [komut satırı](.\asdk\asdk-deploy-powershell.md). 

> [!NOTE]
> İsteğe bağlı olarak, ana bilgisayar CloudBuilder.vhdx önyüklendikten sonra yapılandırabileceğiniz [Azure yığın telemetri ayarlarını](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *önce* ASDK yükleme.


**[Azure yığın Geliştirme Seti (ASDK) yükleyin](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Dağıtım sonrası yapılandırmalar gerçekleştir
ASDK yükledikten sonra birkaç önerilen yükleme sonrası denetler ve vardır, yapılması gereken yapılandırma değişikliklerini. Kurulumunuzu doğrulamak için test AzureStack cmdlet'ini kullanarak başarıyla yüklendi ve Azure yığın PowerShell ve GitHub Araçları'nı yükleyin. 

Azure AD kullanan dağıtımlar sonra Azure yığın yönetici ve Kiracı portalı etkinleştirmeniz gerekir. Bu etkinleştirme, Azure yığın portalı ve Azure Resource Manager (onay sayfasında listelenen) doğru izinler tüm kullanıcılar için dizin vermenizi izin.

Ayrıca, Değerlendirme dönemi sona ermeden önce Geliştirme Seti ana bilgisayar için parola süresi sona ermiyor emin olmak için parola süresi dolma ilkesini sıfırlaması gerekir.

> [!NOTE]
> İsteğe bağlı olarak da yapılandırabilirsiniz [Azure yığın telemetri ayarlarını](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *sonra* ASDK yükleme.

**[POST ASDK dağıtım görevleri](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Azure ile kaydedin
Yapabilmeniz Azure yığın Azure ile kaydetmeniz gerekir [Azure Market öğesi karşıdan](.\asdk\asdk-marketplace-item.md) Azure yığınına.

**[Azure ile Azure yığın kaydedin](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Bu adımları tamamladıktan sonra hem bir geliştirme seti ortamını gerekir [yönetici](https://adminportal.local.azurestack.external) ve [kullanıcı](https://portal.local.azurestack.external) portalları. 
