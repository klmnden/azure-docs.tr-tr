---
title: "Azure yığın Geliştirme Seti dağıtma | Microsoft Docs"
description: "Bu öğreticide, yükleyici komut dosyalarını kullanarak ASDK yükleyin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: de31ed75b79ddb3832e32fad141ea7aef806d4d3
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-deploy-the-asdk-using-the-installer"></a>Öğretici: Yükleyicisi'ni kullanarak ASDK dağıtma
Bu öğreticide, Azure yığın Geliştirme Seti (ASDK) bir üretim dışı ortamda dağıtın. 

ASDK değerlendirmek ve Azure yığın özelliklerin ve hizmetler için dağıtabileceğiniz bir test ve geliştirme ortamıdır. ASDK ile çalışmaya başlamak için ana bilgisayar donanımını hazırlayın ve (yükleme birkaç saat sürer) bazı kodlar çalıştırmak gerekir. Bundan sonra yönetici ve kullanıcı portalı için Azure yığın kullanmaya başlamak için oturum açabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Karşıdan yükleme ve dağıtım paketi ayıklayın
> * Geliştirme Seti ana bilgisayarı hazırla 
> * Ana bilgisayara ASDK yükleyin
> * Dağıtım sonrası yapılandırmalar gerçekleştir
> * Azure ile kaydedin

## <a name="prerequisites"></a>Önkoşullar 
ASDK yüklemeden önce Geliştirme Seti (Geliştirme Seti ana bilgisayarı) barındıracak bilgisayarı hazırlamanız gerekir. Geliştirme Seti ana bilgisayarı, en düşük donanım, yazılım ve ağ gereksinimleri karşılaması gerekir. 

Ayrıca Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanma arasında seçmek kimliği çözümü olarak dağıtımınız için gerekir. 

Geliştirme Seti konak en düşük donanım gereksinimlerini karşıladığından ve yükleme işlemini sorunsuz çalışır dağıtımınıza başlamadan önce yapılan kimlik çözümü kararınızı yaptığınız emin olun. 

**[ASDK dağıtım planlama konuları gözden geçirin](asdk-deploy-considerations.md)**

> [!TIP]
> Kullanabileceğiniz [Azure yığın dağıtım gereksinimlerini denetleyin aracı](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınız tüm gereksinimleri karşıladığını onaylamak için Geliştirme Seti ana bilgisayarda işletim sisteminin yükledikten sonra.

## <a name="download-and-extract-the-deployment-package"></a>Karşıdan yükleme ve dağıtım paketi ayıklayın
Geliştirme Seti ana bilgisayarınız ASDK yüklemek için temel gereksinimleri karşıladığından emin olduktan sonra yüklemek ve ASDK dağıtım paketi ayıklamak için sonraki adım olacaktır. Dağıtım paketi, önyüklenebilir bir işletim sistemi ve Azure yığın yükleme dosyalarını içeren bir sanal sabit sürücü Cloudbuilder.vhdx dosyası içerir.

Dağıtım paketi Geliştirme Seti ana bilgisayara veya başka bir bilgisayara yükleyebilirsiniz. Başka bir bilgisayar kullanılarak Geliştirme Seti ana bilgisayar için donanım gereksinimlerini azaltmaya yardımcı olacak ayıklanan dağıtım dosyaları 60 GB boş disk alanı alın.

**[İndirmeyi ve ayıklamayı Azure yığın Geliştirme Seti (ASDK)](asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti ana bilgisayarı hazırla
Ana bilgisayara ASDK yükleyebilmek için önce ortamı hazırlanmalıdır ve sistem VHD'den önyükleme için yapılandırılmış. Geliştirme Seti ana bilgisayar hazır olduktan sonra ASDK dağıtım başlayabilmesi CloudBuilder.vhdx sanal makine sabit sürücüdeki önyüklenir.

**[ASDK ana bilgisayarı hazırla](asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Ana bilgisayara ASDK yükleyin
Geliştirme Seti ana bilgisayar hazırladıktan sonra ASDK CloudBuilder.vhdx görüntüsüne dağıtılabilir. ASDK yükleme ve asdk installer.ps1 PowerShell komut dosyası çalıştırma veya tamamen gelen sağlanan bir grafik kullanıcı arabirimi (GUI) kullanılarak dağıtılabilir [komut satırı](asdk-deploy-powershell.md). 

> [!NOTE]
> İsteğe bağlı olarak, ana bilgisayar CloudBuilder.vhdx önyüklendikten sonra yapılandırabileceğiniz [Azure yığın telemetri ayarlarını](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *önce* ASDK yükleme.


**[Azure yığın Geliştirme Seti (ASDK) yükleyin](asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Dağıtım sonrası yapılandırmalar gerçekleştir
ASDK yükledikten sonra birkaç önerilen yükleme sonrası denetler ve vardır, yapılması gereken yapılandırma değişikliklerini. Kurulumunuzu doğrulamak için test AzureStack cmdlet'ini kullanarak başarıyla yüklendi ve Azure yığın PowerShell ve GitHub Araçları'nı yükleyin. 

Azure AD kullanan dağıtımlar sonra Azure yığın yönetici ve Kiracı portalı etkinleştirmeniz gerekir. Bu etkinleştirme, Azure yığın portalı ve Azure Resource Manager (onay sayfasında listelenen) doğru izinler tüm kullanıcılar için dizin vermenizi izin.

Ayrıca, Değerlendirme dönemi sona ermeden önce Geliştirme Seti ana bilgisayar için parola süresi sona ermiyor emin olmak için parola süresi dolma ilkesini sıfırlaması gerekir.

> [!NOTE]
> İsteğe bağlı olarak da yapılandırabilirsiniz [Azure yığın telemetri ayarlarını](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *sonra* ASDK yükleme.

**[POST ASDK dağıtım görevleri](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Azure ile kaydedin
Yapabilmeniz Azure yığın Azure ile kaydetmeniz gerekir [Azure Market öğesi karşıdan](asdk-marketplace-item.md) Azure yığınına.

**[Azure ile Azure yığın kaydedin](asdk-register.md)**

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! Bu adımları tamamladıktan sonra hem bir geliştirme seti ortamını gerekir [yönetici](https://adminportal.local.azurestack.external) ve [kullanıcı](https://portal.local.azurestack.external) portalları. 

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Karşıdan yükleme ve dağıtım paketi ayıklayın
> * Geliştirme Seti ana bilgisayarı hazırla 
> * Ana bilgisayara ASDK yükleyin
> * Dağıtım sonrası yapılandırmalar gerçekleştir
> * Azure ile kaydedin

Azure yığın Market öğe ekleme konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Bir Azure yığın Market öğesi ekleme](asdk-marketplace-item.md)




