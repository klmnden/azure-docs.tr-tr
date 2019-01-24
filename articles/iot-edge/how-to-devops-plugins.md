---
title: Jenkins eklentisi - Azure IOT Edge ile CI/CD etkinleştirme | Microsoft Docs
description: Jenkins için Azure IOT Edge uzantısı, var olan DevOps çözümünüzü IOT Edge devlopment ve dağıtım görevlerini tümleştirmenize olanak tanır.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 016d1c5d389cf1b9e82194e9d273863da1138d2b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830378"
---
# <a name="integrate-azure-iot-edge-with-jenkins-pipelines"></a>Jenkins işlem hatlarında Azure IOT Edge tümleştirin

Azure IOT Edge, Azure DevOps ve Azure DevOps projeleri için yerleşik destek içerir, ancak ayrıca Jenkins ile DevOps işlevselliği genişletmek için bir eklenti sağlar. [Jenkins](https://jenkins.io/) eklentileri geliştirme ve dağıtım projeleri, IOT Edge dahil olmak üzere birçok türlerini desteklemek için kullandığı bir açık kaynak Otomasyon sunucusudur. 

Jenkins için Azure IOT Edge eklentisi, sürekli tümleştirme ve sürekli dağıtım odaklanır. Modülleri oluşturur ve kapsayıcı kayıt defterinizde kendi kapsayıcı görüntülerini gönderim bir derleme ve gönderme işlem hattı oluşturabilirsiniz. Ardından, IOT Edge cihazlarınıza modülleri dağıtan bir yayın işlem hattı oluşturun. 

IOT Edge eklentisi için Jenkins kullanmaya başlamadan önce Azure'da ve kapsayıcı kayıt defteri, kapsayıcı görüntüleri tutmak için IOT hub'ı gerekir. Eklenti dağıtımlar için IOT Edge cihazları oluşturabilmeniz IOT hub'ınıza Jenkins katkıda bulunan izinleri vermek için bir Azure hizmet sorumlusu kullanın. 

Başlamak hazırsanız yükleme bulabilir ve ayrıntılarını [Jenkinx için Azure IOT Edge eklentisi](https://plugins.jenkins.io/azure-iot-edge).