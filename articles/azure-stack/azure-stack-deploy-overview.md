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
ms.date: 06/04/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: a0e742ab3ac43cc7977761dd94c9689e3a7c2e0b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235194"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Hızlı Başlangıç: Azure yığın Geliştirme Seti değerlendir

[Azure yığın Geliştirme Seti (ASDK)](.\asdk\asdk-what-is.md) değerlendirmek ve Azure yığın özelliklerin ve hizmetler için dağıtabileceğiniz bir test ve geliştirme ortamıdır. ASDK ile çalışmaya başlamak için ana bilgisayar donanımını hazırlayın ve (yükleme birkaç saat sürer) bazı kodlar çalıştırmak gerekir. Bundan sonra yönetici veya kullanıcı portalı için Azure yığın kullanmaya başlamak için oturum açabilir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="asdk-host-computer-requirements"></a>ASDK ana bilgisayar gereksinimleri

ASDK yüklemeden önce Geliştirme Seti barındıracak bilgisayarı hazırlamanız gerekir. Donanım, Yazılım Geliştirme Seti ana bilgisayar karşılaması gerekir ve ağ gereksinimleri bölümünde açıklanan  **[ASDK dağıtım planlama konuları gözden](.\asdk\asdk-deploy-considerations.md)**.

> [!TIP]
> Kullanabileceğiniz [Azure yığın dağıtım gereksinimlerini denetleyin aracı](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınız tüm gereksinimleri karşıladığını onaylamak için Geliştirme Seti ana bilgisayarda işletim sisteminin yükledikten sonra.

### <a name="account-requirements"></a>Hesap gereksinimleri

Ayrıca Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanma arasında seçmek kimliği çözümü olarak dağıtımınız için gerekir. Hesap gereksinimleri gözden  **[dağıtım planlama konuları](.\asdk\asdk-deploy-considerations.md#account-requirements)**

## <a name="download-and-extract-the-deployment-package"></a>Karşıdan yükleme ve dağıtım paketi ayıklayın

Geliştirme Seti ana bilgisayarınız hazırlama sonra karşıdan yükleyip ASDK dağıtım paketi ayıklayın. Dağıtım paketi ile önyüklenebilir bir işletim sistemi sanal sabit disk (VHD) Cloudbuilder.vhdx dosyasını ve Azure yığın yükleme dosyalarını içerir.

Dağıtım paketi Geliştirme Seti ana bilgisayara veya başka bir bilgisayara yükleyebilirsiniz. Başka bir bilgisayar kullanılarak Geliştirme Seti konaktaki depolama gereksinimlerini azaltmak yardımcı olacak ayıklanan dağıtım dosyaları 60 GB boş disk alanı olması.

**[İndirmeyi ve ayıklamayı Azure yığın Geliştirme Seti (ASDK)](.\asdk\asdk-download.md)**

## <a name="prepare-the-host-computer"></a>Ana bilgisayar hazırlayın

ASDK yükleyebilmeniz için önce ana bilgisayar ortamının hazırlanması gerekir ve sistem önyüklemesi için Geliştirme Seti VHD yapılandırılmış. Konağı yeniden başlattığınızda, CloudBuilder.vhdx önyüklenir ve ASDK dağıtmaya başlatabilirsiniz.

**[ASDK ana bilgisayarı hazırla](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Ana bilgisayara ASDK yükleyin

Ana bilgisayar VHD'den önyükleme sonra Geliştirme Seti Cloudbuilder sanal bir ortama dağıtabilirsiniz. Asdk installer.ps1 PowerShell betiğini çalıştırarak ya da gelen sağlanan grafik kullanıcı arabirimi (GUI) kullanarak ASDK dağıtabilirsiniz [PowerShell komut satırı](.\asdk\asdk-deploy-powershell.md)

> [!NOTE]
> Ana bilgisayar Cloudbuilder.vhdx görüntüden önyükleme sonra yapılandırma seçeneğiniz [Azure yığın telemetri ayarlarını](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *önce* ASDK yükleme.

**[Azure yığın Geliştirme Seti (ASDK) yükleyin](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Dağıtım sonrası yapılandırmalar gerçekleştir

ASDK yükledikten sonra birkaç önerilen yükleme sonrası denetler ve vardır, yapılması gereken yapılandırma değişikliklerini.

**Araçlar**

Azure yığın PowerShell ve GitHub Araçları'nı yüklemek ve test AzureStack cmdlet'ini kullanarak yüklemenizi başarısını denetleyin.

**Kimlik çözümü**

Azure AD kullanan bir dağıtım için Azure yığın yönetici ve Kiracı portalı etkinleştirmeniz gerekir. Bu etkinleştirme, Azure yığın portalı ve Azure Resource Manager (onay sayfasında listelenen) doğru izinler tüm kullanıcılar için dizin vermenizi izin.

**Parola geçerlilik süresi**

Geliştirme Seti konak parolası, Değerlendirme dönemi sona ermeden önce süresi sona ermiyor emin olmak için parola süresi dolma ilkesini sıfırlamanız gerekir.

> [!NOTE]
> Yapılandırma seçeneğiniz de [Azure yığın telemetri ayarlarını](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *sonra* ASDK yükleme.

**[POST ASDK dağıtım görevleri](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Azure ile kaydedin

Yapabilmeniz Azure yığın Azure ile kaydetmeniz gerekir [Azure Market öğesi karşıdan](.\asdk\asdk-marketplace-item.md) Azure yığınına.

**[Azure ile Azure yığın kaydedin](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler! Bu Hızlı Başlangıç'ndaki adımları tamamlayarak bir ASDK ortamıyla sahip bir [yönetici](https://adminportal.local.azurestack.external) portal ve [kullanıcı](https://portal.local.azurestack.external) portal.
