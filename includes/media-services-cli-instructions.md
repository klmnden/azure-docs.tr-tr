---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/25/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: b335cf996de41f4eea15af1899a0c6654c2f679f
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104992"
---
## <a name="open-cli-shell"></a>Açık CLI Kabuğu

Kullanılacak önerilen [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) CLI komutları yürütmek için. **Cloud Shell** bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz, etkileşimli bir kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır. Kodu kopyalayın, Cloud Shell'de yapıştırın ve sonra çalıştırmak için Enter tuşuna basın, Kopyala düğmesini seçmeniz yeterlidir. Cloud Shell’i açmanın birkaç yolu vardır:

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Oturum Aç

(Bulutta veya yerel olarak), CLI Kabuğu çalıştırma kullanmaya başlamak için `az login` Azure ile bir bağlantı oluşturmak için.

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, tarayıcı sayfasını açın ve ayrıldıktan sonra bir yetkilendirme kodu girmek için komut satırında yönergeleri için ihtiyaç duyduğunuz https://aka.ms/devicelogin tarayıcınızda.

### <a name="specify-location-of-files"></a>Dosyalarının konumunu belirtin

Birçok medya Hizmetleri CLI komutları dosya adında bir parametre olarak geçirmenize izin verin. 

Kullanıyorsanız **Azure Cloud Shell**, dosyanızı karşıya **Azure Cloud Shell**. Kabuk pencerenin üst kısmındaki karşıya yükleme/indirme dosyaları düğmesini bulabilirsiniz. Ardından, böyle bir dosya başvurusu: `@{FileName}.` 

![Dosyaları karşıya yükleme]

Azure CLI'yı yerel olarak kullanıyorsanız, tüm dosya yolunu belirtin. Örneğin, `@c:\tracks.json`.


[Dosyaları karşıya yükleme]: ./media/media-services-cli/upload-download-files.png
