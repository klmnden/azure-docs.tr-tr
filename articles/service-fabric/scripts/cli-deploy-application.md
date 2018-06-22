---
title: Azure Service Fabric CLI (sfctl) Betik Dağıtma Örneği
description: Bir uygulamayı Azure Service Fabric CLI’sini kullanarak bir Azure Service Fabric kümesine dağıtma
services: service-fabric
documentationcenter: ''
author: Thraka
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 04/16/2018
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 7e2bd212e88b3efcf7f3aad3ef1555b3fc48ef1b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641739"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Service Fabric kümesine uygulama dağıtma

Bu örnek betik bir uygulama paketini küme görüntü deposuna kopyalar, kümede uygulama türünü kaydeder ve uygulama türünden bir uygulama örneği oluşturur. Aynı anda tüm varsayılan hizmetler de oluşturulur.

Gerekirse [Service Fabric CLI](../service-fabric-cli.md)'sını da yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

İşiniz bittiğinde, [kaldırma](cli-remove-application.md) betiği uygulamayı kaldırmak için kullanılabilir. Kaldırma betiği uygulama örneğini siler, uygulama türünün kaydını siler ve uygulama paketini görüntü deposundan kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Service Fabric CLI belgeleri](../service-fabric-cli.md).

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
