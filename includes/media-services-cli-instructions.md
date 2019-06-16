---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/28/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: db431d7815cfcc006563bd6da438154ef77ae6e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814780"
---
## <a name="cli-shell"></a>CLI Kabuğu

Kullanılacak önerilen [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) CLI komutları yürütmek için. **Cloud Shell** bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz, etkileşimli bir kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır. Bu, çalışma biçiminize en uygun kabuk deneyimini seçme esnekliği sağlar. Linux kullanıcıları Bash deneyimini, Windows kullanıcıları ise PowerShell’i tercih edebilir.

CLI'yi yerel olarak da yükleyebilirsiniz. Bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) platformunuza yönelik yönergeler.

### <a name="sign-in"></a>Oturum aç

CLI'ın yerel bir yüklemesini kullanarak Azure'da oturum açma gerektirir. Bu adım, Azure Cloud Shell için gerekli değildir. Oturum açmak için `az login` komutu.

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, tarayıcı sayfasını açın ve ayrıldıktan sonra bir yetkilendirme kodu girmek için komut satırında yönergeleri için ihtiyaç duyduğunuz https://aka.ms/devicelogin tarayıcınızda.

### <a name="specify-location-of-files"></a>Dosyalarının konumunu belirtin

Birçok medya Hizmetleri CLI komutları dosya adında bir parametre olarak geçirmenize izin verin. Kullanıyorsanız **Cloud Shell**, (Bash veya PowerShell kullanarak), clouddrive için dosyanızı karşıya yükleyebilirsiniz. 

![Dosyaları karşıya yükleme]

Yerel bir CLI kullanmanıza veya **Cloud Shell**, işletim sistemi ya da bulut kullanmakta olduğunuz Kabuk (Bash veya PowerShell) göre dosya yolu belirtmeniz gerekir. Aşağıda bazı örnekler verilmiştir:

(Tüm işletim sistemi) dosyasının göreli yolu

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Linux/Mac ve Windows işletim sistemi mutlak dosya yolu

* `@ "/usr/home/mytestfile.json"`
*   `@"c:\tmp\user\mytestfile.json"`

Kullanım `{file}` komut dosyasının yolu için soran durumunda. Örneğin, `az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json`. <br/> Kullanım `@{file}` belirtilen dosya yüklemek için komut yayınlanıyorsa. Örneğin, `az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json`.

[Dosyaları karşıya yükleme]: ./media/media-services-cli/upload-download-files.png
