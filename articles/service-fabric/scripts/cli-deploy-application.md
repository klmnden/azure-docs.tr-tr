---
title: "Azure Service Fabric CLI betik örnek dağıtma"
description: "Bir uygulamayı Azure Service Fabric CLI kullanarak bir Azure Service Fabric kümesi dağıtma"
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 2bbf689820a92cfa01b26fdbacb4526ade8956ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Service Fabric kümesi bir uygulamayı dağıtma

Bu örnek betik bir uygulama paketi bir küme görüntü deposuna kopyalar, kümede uygulama türü kaydeder ve uygulama türünden uygulama örneğini oluşturur. Varsayılan hizmetlerin de şu anda oluşturulur.

Gerekirse, yükleme [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

İşiniz bittiğinde, [kaldırmak](cli-remove-application.md) komut dosyası, uygulamayı kaldırmak için kullanılabilir. Kaldırma komut dosyası uygulama örneği siler, uygulama türü kaydını siler ve uygulama paketi görüntü deposundan kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [Service Fabric CLI belgelerine](../service-fabric-cli.md).

Azure Service Fabric için ek hizmet doku CLI örnek bulunabilir [Service Fabric CLI örnekleri](../samples-cli.md).
