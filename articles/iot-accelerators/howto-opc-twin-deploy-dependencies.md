---
title: OPC İkizi bulut bağımlılıkları azure'da dağıtma | Microsoft Docs
description: OPC İkizi Azure bağımlılıkları dağıtma
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ae9f2b05bfc6ea6315022d04c8d267d916cf282e
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59491995"
---
# <a name="deploying-dependencies-for-local-development"></a>Yerel geliştirme için bağımlılıklar dağıtma

Bu makalede, yerel geliştirme ve hata ayıklama yapmak için yalnızca Azure Platform hizmetlerini ihtiyaç dağıtılacağı açıklanmaktadır.   Sonunda, yerel geliştirme ve hata ayıklama için gereken her şeyi içeren dağıtılan bir kaynak grubu gerekir.

## <a name="deploy-azure-platform-services"></a>Azure platform hizmetlerini dağıtma

1. PowerShell sahip olduğunuzdan emin olun ve [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0) yüklü uzantıları.  Bir komut istemi veya terminal açın ve çalıştırın:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Kaynak grubu, dağıtımınız için bir ad atamak için yönergeleri izleyin.  Betik yalnızca bağımlılıkları Azure aboneliğinizi ve olmayan mikro hizmetler, bu kaynak grubuna dağıtır.  Betik, bir uygulamayı Azure Active Directory'de de kaydeder.  Bu, OAUTH tabanlı kimlik doğrulamasını desteklemek için gereklidir.  Dağıtım birkaç dakika sürebilir.

3. Betik tamamlandıktan sonra .env dosyasında kaydetmeyi seçebilirsiniz.  .Env ortam dosyası tüm hizmetleri ve araçları, geliştirme makinenizde çalıştırmak istediğiniz yapılandırma dosyasıdır.  

## <a name="troubleshooting-deployment-failures"></a>Dağıtım sorunlarını giderme

### <a name="resource-group-name"></a>Kaynak grubu adı

Bir kısa ve basit bir kaynak grubu adı kullandığınızdan emin olun.  Ad, uyması gereken ad kaynaklarla şekilde kaynak adlandırma gereksinimlerini için de kullanılır.  

### <a name="azure-active-directory-aad-registration"></a>Azure Active Directory (AAD) kaydı

Dağıtım betiği, AAD uygulamaları Azure Active Directory'ye kaydetmeniz dener.  Seçilen AAD kiracısı haklarınızı bağlı olarak, bu başarısız olabilir.   3 seçenek vardır:

1. Kiracılar listesinden bir AAD kiracısı seçtiyseniz, betik yeniden başlatın ve farklı bir listeden seçin.
2. Alternatif olarak, özel bir AAD kiracısı dağıtın, betiği yeniden başlat ve kullanılacağını seçin.
3. Kimlik doğrulaması olmadan devam edin.  Mikro hizmetlerinizi yerel olarak çalıştırıyorsanız bu yana bu kabul edilebilir, ancak üretim ortamlarında taklit değil.  

## <a name="next-steps"></a>Sonraki adımlar

Mevcut bir projeyi OPC İkizi Hizmetleri başarıyla dağıtıldı, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC İkizi modülleri dağıtma hakkında bilgi edinin](howto-opc-twin-deploy-modules.md)
