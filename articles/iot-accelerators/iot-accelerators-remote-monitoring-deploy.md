---
title: Uzaktan izleme çözümü - Azure dağıtma | Microsoft Docs
description: Bu öğretici, Uzaktan izleme Çözüm Hızlandırıcısı azureiotsuite.com gelen sağlama gösterir.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 30d0b0b0250563cf09a50e8165ad9fb1047fd570
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="deploy-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme Çözüm Hızlandırıcısı dağıtma

Bu öğretici, Uzaktan izleme Çözüm Hızlandırıcısı sağlama gösterir. Azureiotsuite.com çözümden dağıtın. Bu seçenek bakın hakkında bilgi edinmek için CLI kullanarak çözüm de dağıtabilirsiniz [Çözüm Hızlandırıcısı komut satırından dağıtma](iot-accelerators-remote-monitoring-deploy-cli.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çözüm Hızlandırıcısı yapılandırın
> * Çözüm Hızlandırıcısı dağıtma
> * Çözüm Hızlandırıcısı oturum açın

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğinizin olması gerekir.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-solution-accelerator"></a>Çözüm Hızlandırıcısı dağıtma

Azure aboneliğinize Çözüm Hızlandırıcısı dağıtmadan önce bazı yapılandırma seçenekleri seçmeniz gerekir:

1. Oturum [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) Azure hesabı kimlik bilgilerinizi kullanarak.

1. Tıklatın **şimdi deneyin** üzerinde **Uzaktan izleme** döşeme.

    ![Uzaktan izleme seçin](./media/iot-accelerators-remote-monitoring-deploy/remotemonitoring.png)

1. Üzerinde **oluşturma Uzaktan izleme çözümü** want bir **çözüm adı** , Uzaktan izleme Çözüm Hızlandırıcısı için.

1. Seçin bir **temel** veya **standart** dağıtım. Nasıl çalıştığını öğrenin veya Tanıtımı çalıştırmak, seçmek için çözüm dağıtıyorsanız **temel** maliyetleri en aza indirmek için seçeneği.

1. Ya da seçin **Java** veya **.NET** dili olarak. Tüm mikro Java veya .NET uygulamaları kullanılabilir.

1. Gözden geçirme **çözüm ayrıntılarını** paneli yapılandırma seçenekleriniz hakkında daha fazla bilgi için.

1. Çözümü hazırlarken kullanmak istediğiniz **Abonelik** ve **Bölge** seçimini yapın.

1. Hazırlama işlemini başlatmak için **Çözümü Oluştur**'a tıklayın. Bu işlem genellikle çalıştırmak için birkaç dakika sürer:

    ![Uzaktan izleme çözümü ayrıntıları](./media/iot-accelerators-remote-monitoring-deploy/createform.png)

Sorun giderme bilgileri için bkz: [bir dağıtımı başarısız olduğunda yapılması gerekenler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) GitHub deposunda.

## <a name="sign-in-to-the-solution-accelerator"></a>Çözüm Hızlandırıcısı oturum açın

Sağlama işlemi tamamlandıktan sonra Uzaktan izleme Çözüm Hızlandırıcısı oturum açın.

1. Üzerinde **sağlanan çözümleri** sayfasında, yeni Uzaktan izleme çözümünüz seçin:

    ![Yeni çözümü seçme](./media/iot-accelerators-remote-monitoring-deploy/choosenew.png)

1. Uzaktan izleme çözümünüzü görünür panelinde ilgili bilgileri görüntüleyin. Seçin **çözüm Panosu** Uzaktan izleme çözümünüz için bağlanmak için.

    > [!NOTE]
    > İle işiniz bittiğinde, bu panelinden Uzaktan izleme çözümünüz silebilirsiniz.

    ![Çözüm paneli](./media/iot-accelerators-remote-monitoring-deploy/solutionpanel.png)

1. Uzaktan izleme çözüm panosunu tarayıcınızda görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Çözüm Hızlandırıcısı yapılandırın
> * Çözüm Hızlandırıcısı dağıtma
> * Çözüm Hızlandırıcısı oturum açın

Uzaktan izleme çözümü dağıttıysanız, sonraki adım olacak [çözüm Panosu özelliklerini keşfedin](./../iot-suite/iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->