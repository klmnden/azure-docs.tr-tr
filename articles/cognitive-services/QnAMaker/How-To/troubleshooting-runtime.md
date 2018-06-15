---
title: QnAMaker sorunlarını giderme | Microsoft Docs
titleSuffix: Azure
description: QnAMaker çalışma zamanı ile ilgili sorunları giderme
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 06/04/2018
ms.author: saneppal
ms.openlocfilehash: 23847c81a39ea392b6e64575406c9dc338b852de
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356037"
---
# <a name="qnamaker-troubleshooting"></a>QnAMaker sorunlarını giderme
Kullanıcının Azure hesabında barındırılan bileşenlerinin QnAMaker oluşur. Hata ayıklama QnAMaker Azure kaynaklarını yönetmek veya QnAMaker destek ekibi kendi kurulumu hakkında ek bilgi sağlamak kullanıcıların gerektirebilir.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>En son QnAMaker çalışma zamanı güncelleştirmelerini alma
QnAMaker çalışma zamanı parçasıdır Azure App Service ne zaman dağıtılan, [QnAMaker hizmet oluşturma](./set-up-qnamaker-service-azure.md) Azure portalında. Güncelleştirmeleri düzenli aralıklarla çalışma zamanına yapılır. QnAMaker kurulumunuzu uygulamak için en son güncelleştirmeleri uygulamak için uygulama hizmetini yeniden başlatmanız gerekir.
1. (Kaynak grubu) QnAMaker hizmetinize Git [Azure portalı](https://portal.azure.com)

    ![QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Uygulama hizmeti tıklatın ve genel bakış bölümünde açın

     ![QnAMaker uygulama hizmeti](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Uygulama hizmeti yeniden başlatın. Birkaç saniye içinde tamamlanmalıdır. Bu QnAMaker hizmette, aşağı akış uygulamaları/aracılarını yapı Not son kullanıcılara bu yeniden başlatma süresi sırasında kullanılamaz.

    ![QnAMaker appservice yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>QnAMaker hizmeti ana bilgisayar adını alma
QnAMaker hizmeti ana bilgisayar adı QnAMaker desteği veya UserVoice başvurduğunuzda hata ayıklama amacıyla yararlıdır. Ana bilgisayar adı, bu formu URL'sidir: https://*{hostname}*. azurewebsites.net.
    
1. (Kaynak grubu) QnAMaker hizmetinize Git [Azure portalı](https://portal.azure.com)

    ![QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Uygulama hizmeti

     ![QnAMaker uygulama hizmeti](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Ana bilgisayar adı URL genel bakış bölümünde kullanılabilir

    ![QnAMaker ana bilgisayar adı](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnAMaker API kullanın](./upgrade-qnamaker-service.md)
