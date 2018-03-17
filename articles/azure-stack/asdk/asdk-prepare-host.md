---
title: "Azure yığın Geliştirme Seti (ASDK) ana bilgisayar hazırlama | Microsoft Docs"
description: "Azure yığın Geliştirme Seti (ASDK) ana bilgisayar ASDK yüklemesine hazırlanmak açıklar."
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
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 8b1a6298ab32dc364aa1543e4a8d5db47b02a098
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="prepare-the-asdk-host-computer"></a>ASDK ana bilgisayarı hazırla
Ana bilgisayara ASDK yükleyebilmek için önce ASDK ortamı yüklemesi için hazırlanması gerekir. Geliştirme Seti ana bilgisayar hazırlandığınızda ASDK dağıtımına başlamak için CloudBuilder.vhdx sanal makine sabit sürücüden önyükleme yapmaz.

## <a name="prepare-the-development-kit-host-computer"></a>Geliştirme Seti ana bilgisayarı hazırla
Ana bilgisayara ASDK yükleyebilmek için önce ASDK ana bilgisayar ortamı hazırlanmalıdır.
1. Geliştirme Seti ana bilgisayarınız için yerel yönetici olarak oturum açın.
2. CloudBuilder.vhdx dosya C:\ sürücüsü (C:\CloudBuilder.vhdx) kök dizinine taşındığından emin olun.
3. Geliştirme seti yükleyicisini (asdk-installer.ps1) indirin için aşağıdaki betiği çalıştırın [Azure yığın GitHub araçları deposu](https://github.com/Azure/AzureStack-Tools) için **C:\AzureStack_Installer** klasöründe, Geliştirme Seti ana bilgisayarı:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Yükseltilmiş bir PowerShell konsolundan Başlat **C:\AzureStack_Installer\asdk-installer.ps1** komut dosyası ve ardından **hazırlama ortamı**.

    ![](media/asdk-prepare-host/1.PNG) 

5. Üzerinde **seçin Cloudbuilder vhdx** yükleyici, bulun ve seçin sayfasında **cloudbuilder.vhdx** yüklenip içinde ayıklanan dosya [önceki adımları](asdk-download.md). Bu sayfada, isteğe bağlı olarak, etkinleştirebilirsiniz **sürücüleri ekleme** Geliştirme Seti ana bilgisayara ek sürücüler eklemeniz gerekiyorsa, kutuyu. **İleri**’ye tıklayın.  

    ![](media/asdk-prepare-host/2.PNG)

6. Üzerinde **isteğe bağlı ayarlar** sayfasında, yerel yönetici sağlayın ve ardından hesap bilgileri ve Geliştirme Seti konak bilgisayar için **sonraki**. Ayrıca, aşağıdaki isteğe bağlı ayarlar için değerleri sağlayabilirsiniz:
  - **ComputerName**: Bu seçenek Geliştirme Seti ana bilgisayar adını ayarlar. Ad FQDN gereksinimlere uygun olmalıdır ve 15 karakter veya daha az olmalıdır. Varsayılan Windows tarafından oluşturulan bir rastgele bilgisayar adıdır.
  - **Saat dilimi**: Geliştirme Seti ana bilgisayar için saat dilimini ayarlar. Varsayılan değer (UTC-8:00) Pasifik Saati (ABD ve Kanada).
  - **Statik IP yapılandırması**: dağıtımınızı statik bir IP adresi kullanacak şekilde ayarlar. Aksi takdirde, yükleyici cloudbuilder.vhx yeniden başlatıldığında, ağ arabirimleri DHCP ile yapılandırılır.

    ![](media/asdk-prepare-host/3.PNG)

  > [!IMPORTANT]
  > Bu adımda yerel yönetici kimlik bilgileri sağlamazsanız, Geliştirme Seti kurulumunun bir parçası olarak bilgisayar yeniden başlatıldıktan sonra doğrudan ya da barındırmak için KVM erişim gerekir.

7. Bir statik IP yapılandırması önceki adımda seçtiğiniz istiyorsanız, artık gerekir:
    - Bir ağ bağdaştırıcısı seçin. Tıklamadan önce bağdaştırıcıya bağlanabilir emin olun **sonraki**.
    - Olduğundan emin olun **IP adresi**, **ağ geçidi**, ve **DNS** değerleri doğru olduğundan ve ardından **sonraki**.
13. Tıklatın **sonraki** hazırlama işlemini başlatmak için.
14. Zaman hazırlık gösterdiği **tamamlandı**, tıklatın **sonraki**.
15. Tıklatın **artık yeniden** cloudbuilder.vhdx Geliştirme Seti ana bilgisayarı önyüklemek için ve [dağıtım işlemine devam etmek](asdk-install.md).

    ![](media/asdk-prepare-host/4.PNG)


## <a name="next-steps"></a>Sonraki adımlar
[ASDK yükleyin](asdk-install.md)