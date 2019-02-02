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
ms.openlocfilehash: 06651b06ae84934c16e9f1ac9f604abda8b65615
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55648603"
---
## <a name="open-cli-shell"></a>Açık CLI Kabuğu

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
