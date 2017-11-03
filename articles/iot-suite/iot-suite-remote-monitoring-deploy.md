---
title: "Uzaktan izleme çözümü - Azure dağıtma | Microsoft Docs"
description: "Bu öğretici, Uzaktan izleme önceden yapılandırılmış çözüm azureiotsuite.com sağlama gösterir."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 08/09/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: babcf20b58af1415e0e658e0a622cb056e34642b
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>Önceden yapılandırılmış Uzaktan izleme çözümü dağıtma

Bu öğretici, önceden yapılandırılmış uzaktan izleme çözümünün nasıl hazırlanacağını gösterir. Azureiotsuite.com çözümden dağıtın. Bu seçenek bakın hakkında bilgi edinmek için CLI kullanarak çözüm de dağıtabilirsiniz [komut satırından önceden yapılandırılmış bir çözüm dağıtma](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploy-a-pcs-from-the-command-line).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önceden yapılandırılmış çözümünüzü yapılandırma
> * Önceden yapılandırılmış çözümü dağıtma
> * Önceden yapılandırılmış çözümü oturum açın

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğinizin olması gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-preconfigured-solution"></a>Önceden yapılandırılmış çözümü dağıtma

Önceden yapılandırılmış çözümü Azure aboneliğinize dağıtmadan önce bazı yapılandırma seçenekleri seçmeniz gerekir:

1. Oturum [azureiotsuite.com](https://www.azureiotsuite.com) Azure kullanarak hesap kimlik bilgilerini ve tıklatın  **+**  bir çözüm oluşturmak için.

1. **Uzaktan izleme** kutucuğunda **Seç**'e tıklayın.

1. Üzerinde **oluşturma Uzaktan izleme çözümü** want bir **çözüm adı** çözüm önceden yapılandırılmış Uzaktan izleme için.

1. Seçin bir **temel** veya **Kurumsal** dağıtım. Nasıl çalıştığını öğrenin veya Tanıtımı çalıştırmak, seçmek için çözüm deplying varsa **temel** maliyetleri en aza indirmek için seçeneği.

1. Ya da seçin **Java** veya **.NET** dili olarak. Tüm mikro Java veya .NET uygulamaları kullanılabilir.

1. Gözden geçirme **çözüm ayrıntılarını** paneli yapılandırma seçenekleriniz hakkında daha fazla bilgi için.

1. Çözümü hazırlarken kullanmak istediğiniz **Abonelik** ve **Bölge** seçimini yapın.

1. Hazırlama işlemini başlatmak için **Çözümü Oluştur**'a tıklayın. Bu işlemin çalışması genellikle birkaç dakika sürer.

Sorun giderme bilgileri için bkz: [bir dağıtımı başarısız olduğunda yapılması gerekenler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) GitHub deposunda.

## <a name="sign-in-to-the-preconfigured-solution"></a>Önceden yapılandırılmış çözümü oturum açın

Sağlama işlemi tamamlandığında, Uzaktan izleme önceden yapılandırılmış çözümünüzün için oturum açın.

1. Üzerinde **sağlanan çözümleri** sayfasında, yeni Uzaktan izleme çözümünüz seçin.

1. Uzaktan izleme çözümünüzü görünür panelinde ilgili bilgileri görüntüleyin. Seçin **çözüm Panosu** Uzaktan izleme çözümünüz için bağlanmak için.

    > [!NOTE]
    > İle işiniz bittiğinde, bu panelinden Uzaktan izleme çözümünüz silebilirsiniz.

1. Uzaktan izleme çözüm panosunu tarayıcınızda görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Önceden yapılandırılmış çözümünüzü yapılandırma
> * Önceden yapılandırılmış çözümü dağıtma
> * Önceden yapılandırılmış çözümü oturum açın

Uzaktan izleme çözümü dağıttıysanız, sonraki adım olacak [çözüm Panosu özelliklerini keşfedin](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->