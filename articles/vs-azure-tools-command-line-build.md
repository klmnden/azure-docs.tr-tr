---
title: Azure komut satırı derleme | Microsoft Docs
description: Azure komut satırı derleme
services: visual-studio-online
author: ghogen
manager: douge
assetId: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/05/2017
ms.author: ghogen
ms.openlocfilehash: 7d0138abb07aea46ad8d0069c87964b393347dcf
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31791435"
---
# <a name="building-azure-projects-from-the-command-line"></a>Komut satırından Azure projeler derleme
Microsoft Build Engine (MSBuild) kullanarak, Visual Studio değil yüklendiği yapı Laboratuvar ortamlarında ürünleri oluşturabilir. MSBuild genişletilebilir ve Microsoft tarafından tam olarak desteklenen proje dosyaları için bir XML biçimi kullanır. MSBuild dosya biçimini kullanarak, hangi öğeler olmalıdır açıklayabilirsiniz bir veya daha fazla platformlar ve yapılandırmaları için oluşturulmuştur.

MSBuild komut satırında çalıştırabilirsiniz ve bu konuda, bu yaklaşımı açıklanmaktadır. Komut satırında özellikleri ayarlayarak, belirli yapılandırmaları bir proje oluşturabilirsiniz. Benzer şekilde, MSBuild derlemeler hedefleri tanımlayabilirsiniz. Komut satırı parametreleri ve MSBuild hakkında daha fazla bilgi için bkz: [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>MSBuild parametreleri
MSBuild ile çalıştırmak için bir paket oluşturmak için en basit yolu olan `/t:Publish` seçeneği. Varsayılan olarak, bu komut bir dizin projenin kök klasörüne göre gibi oluşturur `<ProjectDirectory>\bin\Configuration\app.publish\`. Bir Azure projesi derlerken, iki dosya oluşturulur: Paket dosyası kendisi ve eşlik eden yapılandırma dosyası:

* Paket dosyası (`project.cspkg`)
* Yapılandırma dosyası (`ServiceConfiguration.TargetProfile.cscfg`)

Varsayılan olarak, her Azure projesi yerel (hata ayıklama) yapılar için bir hizmet yapılandırma dosyası ve başka bir bulut (hazırlık veya üretim) derlemeleri içerir. Ancak, ekleyebilir veya gerektiği gibi hizmet yapılandırma dosyalarını kaldırabilirsiniz. Visual Studio içinde bir paket oluşturduğunuzda, hangi hizmet yapılandırma dosyasında paketini içerecek şekilde istenir. MSBuild kullanarak bir paket oluşturduğunuzda, yerel hizmet yapılandırma dosyası varsayılan olarak dahil edilir. Farklı bir hizmet yapılandırma dosyası içerecek şekilde `TargetProfile` MSBuild komut özelliği (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Yapılandırma dosyalarını ve saklı paket için alternatif bir dizin kullanmak istiyorsanız, yolu kullanarak ayarlamak `/p:PublishDir=Directory\` sondaki eğik çizgi ayırıcı dahil olmak üzere seçeneği.

## <a name="next-steps"></a>Sonraki adımlar
Paket oluşturulduktan sonra Azure'a dağıtabilirsiniz.