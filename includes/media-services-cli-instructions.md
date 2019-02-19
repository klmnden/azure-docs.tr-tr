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
ms.openlocfilehash: c13ba8ab42d949e5752bfefd9b792b14a5a5ce59
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56408213"
---
## <a name="cli-shell"></a>CLI Kabuğu

Kullanılacak önerilen [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) CLI komutları yürütmek için. **Cloud Shell** bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz, etkileşimli bir kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır. Bu, çalışma biçiminize en uygun kabuk deneyimini seçme esnekliği sağlar. Linux kullanıcıları Bash deneyimini, Windows kullanıcıları ise PowerShell’i tercih edebilir.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Oturum Aç

(Bulutta veya yerel olarak), CLI Kabuğu çalıştırma kullanmaya başlamak için `az login` Azure ile bir bağlantı oluşturmak için.

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


[Dosyaları karşıya yükleme]: ./media/media-services-cli/upload-download-files.png
