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
ms.openlocfilehash: 1442f6ccc1d4ec349bb20d302faabd6788ff9253
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
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

1. Oturum [azureiotsuite.com](https://www.azureiotsuite.com) Azure kullanarak hesap kimlik bilgilerini ve tıklatın ** + ** yeni bir çözüm oluşturmak için:

    ![Yeni çözüm oluşturma](media/iot-suite-remote-monitoring-deploy/createnewsolution.png)

1. Tıklatın **seçin** üzerinde **Uzaktan izleme Önizleme** döşeme.

    ![Uzaktan izleme seçin](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. Üzerinde **oluşturma Uzaktan izleme çözümü** want bir **çözüm adı** çözüm önceden yapılandırılmış Uzaktan izleme için.

1. Seçin bir **temel** veya **standart** dağıtım. Nasıl çalıştığını öğrenin veya Tanıtımı çalıştırmak, seçmek için çözüm deplying varsa **temel** maliyetleri en aza indirmek için seçeneği.

1. Ya da seçin **Java** veya **.NET** dili olarak. Tüm mikro Java veya .NET uygulamaları kullanılabilir.

1. Gözden geçirme **çözüm ayrıntılarını** paneli yapılandırma seçenekleriniz hakkında daha fazla bilgi için.

1. Çözümü hazırlarken kullanmak istediğiniz **Abonelik** ve **Bölge** seçimini yapın.

1. Hazırlama işlemini başlatmak için **Çözümü Oluştur**'a tıklayın. Bu işlem genellikle çalıştırmak için birkaç dakika sürer:

    ![Uzaktan izleme çözümü ayrıntıları](media/iot-suite-remote-monitoring-deploy/createform.png)

Sorun giderme bilgileri için bkz: [bir dağıtımı başarısız olduğunda yapılması gerekenler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) GitHub deposunda.

## <a name="sign-in-to-the-preconfigured-solution"></a>Önceden yapılandırılmış çözümü oturum açın

Sağlama işlemi tamamlandığında, Uzaktan izleme önceden yapılandırılmış çözümünüzün için oturum açın.

1. Üzerinde **sağlanan çözümleri** sayfasında, yeni Uzaktan izleme çözümünüz seçin:

    ![Yeni çözümü seçme](media/iot-suite-remote-monitoring-deploy/choosenew.png)

1. Uzaktan izleme çözümünüzü görünür panelinde ilgili bilgileri görüntüleyin. Seçin **çözüm Panosu** Uzaktan izleme çözümünüz için bağlanmak için.

    > [!NOTE]
    > İle işiniz bittiğinde, bu panelinden Uzaktan izleme çözümünüz silebilirsiniz.

    ![Çözüm paneli](media/iot-suite-remote-monitoring-deploy/solutionpanel.png)

1. Uzaktan izleme çözüm panosunu tarayıcınızda görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Önceden yapılandırılmış çözümünüzü yapılandırma
> * Önceden yapılandırılmış çözümü dağıtma
> * Önceden yapılandırılmış çözümü oturum açın

Uzaktan izleme çözümü dağıttıysanız, sonraki adım olacak [çözüm Panosu özelliklerini keşfedin](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->