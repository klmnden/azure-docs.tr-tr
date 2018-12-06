---
title: Azure Stack geliştirme Seti'ni değerlendirme | Microsoft Docs
description: Değerlendirme amacıyla Azure Stack geliştirme Seti'ni dağıtmayı öğrenin.
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
ms.date: 11/05/2018
ms.author: jeffgilb
ms.custom: mvc
ms.reviewer: misainat
ms.openlocfilehash: 44fed3311234e1a64cb46c3403f39a9e269d189b
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956939"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Hızlı Başlangıç: Azure Stack geliştirme Seti'ni değerlendir

[Azure Stack geliştirme Seti'ni (ASDK)](./asdk/asdk-what-is.md) değerlendirmek ve Azure Stack özelliklerini ve hizmetler için dağıtabileceğiniz bir test ve geliştirme ortamıdır. ASDK ile çalışmaya başlamak için ana bilgisayar donanımını hazırlayın ve ardından (yükleme birkaç saat sürer) bazı komut dosyaları çalıştırmak gerekir. Bundan sonra yönetici veya kullanıcı portalı için Azure Stack kullanmaya başlamak için oturum açabilir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="asdk-host-computer-requirements"></a>ASDK ana bilgisayar gereksinimleri

ASDK yüklemeden önce Geliştirme Seti barındıracak bilgisayarı hazırlamanız gerekir. Donanım, Yazılım Geliştirme Seti ana bilgisayar karşılaması gerekir ve ağ gereksinimleri açıklanmıştır  **[ASDK dağıtım planlama konuları gözden](./asdk/asdk-deploy-considerations.md)**.

> [!TIP]
> Kullanabileceğiniz [Azure Stack dağıtım gereksinimleri kontrol aracı](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) donanımınızın tüm gereksinimleri karşıladığından emin doğrulamak için Geliştirme Seti ana bilgisayarda işletim sistemi yüklendikten sonra.

### <a name="account-requirements"></a>Hesap gereksinimleri

Ayrıca Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanma arasında seçim kimlik çözümü dağıtımınız için gerekir. Hesap gereksinimleri gözden  **[dağıtım planlama konuları](./asdk/asdk-deploy-considerations.md#account-requirements)**

## <a name="download-and-extract-the-deployment-package"></a>İndirin ve ayıklayın dağıtım paketi

Geliştirme Seti ana bilgisayarınız hazırlama sonra indirin ve ASDK dağıtım paketini ayıklayın. Dağıtım paketi, önyüklenebilir bir işletim sistemi sanal sabit disk (VHD) olan Cloudbuilder.vhdx dosyası ve Azure Stack yükleme dosyalarını içerir.

Dağıtım paketi Geliştirme Seti ana bilgisayara veya başka bir bilgisayara yükleyebilirsiniz. Başka bir bilgisayar kullanarak Geliştirme Seti konakta depolama gereksinimlerini azaltmak yardımcı olabilmemiz için ayıklanan dağıtım dosyaları 60 GB boş disk alanı yararlanın.

**[İndirin ve Azure Stack geliştirme Seti'ni (ASDK) ayıklayın](./asdk/asdk-download.md)**

## <a name="prepare-the-host-computer"></a>Ana bilgisayar hazırlama

ASDK yükleyebilmek için önce ana bilgisayar ortamının hazırlanması gerekir ve sistem geliştirme Seti'nden VHD önyükleme için yapılandırılmış. Konağı yeniden başlattığınızda, CloudBuilder.vhdx önyüklenir ve ASDK dağıtmaya başlayabilir.

**[ASDK ana bilgisayarını hazırlayın](./asdk/asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Ana bilgisayara ASDK yükleyin

Ana bilgisayar VHD'den önyüklemesinin ardından Cloudbuilder sanal ortama Geliştirme Seti dağıtabilirsiniz. Asdk installer.ps1 PowerShell betiğini çalıştırarak ya da sağlanan grafik kullanıcı arabirimi (GUI) kullanarak ASDK dağıtabileceğiniz [PowerShell komut satırı](./asdk/asdk-deploy-powershell.md)

> [!NOTE]
> Konak Cloudbuilder.vhdx görüntüsünden önyükleme sonra yapılandırma seçeneğiniz bulunur [Azure Stack telemetri ayarlarını](./asdk/asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *önce* ASDK yükleme.

**[Azure Stack geliştirme Seti'ni (ASDK) yükleyin](./asdk/asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Dağıtım sonrası yapılandırmaları gerçekleştirin

ASDK yükledikten sonra var. birkaç önerilen yükleme sonrası denetimleri ve yapılması gereken yapılandırma değişiklikleri

**Araçlar**

Azure Stack PowerShell ile GitHub araçlarını yüklemek ve test AzureStack cmdlet'ini kullanarak yüklemenizin başarılı denetleyin.

**Kimlik çözümü**

Azure AD kullanan bir dağıtım için Azure Stack yönetici ve Kiracı portalı etkinleştirmeniz gerekir. Bu etkinleştirme Azure Stack portal ve Azure Resource Manager (onay sayfasında listelenmiştir) doğru tüm kullanıcılar için izinleri dizinin vermenizi toplanmasına onay verir.

**Parola kullanım süresi**

Değerlendirme dönemi bitmeden Geliştirme Seti konak için parola süresi sona ermiyor emin olmak için parola süresi dolma ilkesini sıfırlamalısınız.

> [!NOTE]
> Ayrıca yapılandırma seçeneğiniz de vardır [Azure Stack telemetri ayarlarını](./asdk/asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *sonra* ASDK yükleme.

**[POST ASDK dağıtım görevleri](./asdk/asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Azure ile kaydedin

Olan Azure Stack Azure ile kaydetmelisiniz [Azure Market öğelerini indirme](./asdk/asdk-marketplace-item.md) Azure Stack için.

**[Azure Stack Azure ile kaydedin](./asdk/asdk-register.md)**

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler! Bu hızlı başlangıçtaki adımları tamamlayarak ASDK ortam ile sahip bir [yönetici](https://adminportal.local.azurestack.external) portalı ve [kullanıcı](https://portal.local.azurestack.external) portalı.
