---
title: Service Fabric CLI Betik Örneği - Bir küme üzerinde uygulama güncelleştirme
description: Service Fabric CLI Betik Örneği - Uygulamayı yeni bir sürümle güncelleştirme. Bu örnek ayrıca dağıtılmış bir uygulamayı yeni bitlerle yükseltir.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: aljo
ms.custom: ''
ms.openlocfilehash: 7c6b4b6214f5fc2788827ae34aad4019dad1d8ae
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806972"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Service Fabric kümesine uygulama sertifikası ekleme

Bu örnek betik, mevcut bir uygulamanın yeni bir sürümünü karşıya yükler ve sonra dağıtılmış bir uygulamayı yeni bitlerle yükseltir.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Service Fabric CLI belgeleri](../service-fabric-cli.md).

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
