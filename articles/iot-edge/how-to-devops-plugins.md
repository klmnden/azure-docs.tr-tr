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
ms.openlocfilehash: 173e6ff91acd2ad28d7203b2b5db65e0ee0ecc43
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126350"
---
# <a name="integrate-azure-iot-edge-with-jenkins-pipelines"></a>Jenkins işlem hatlarında Azure IOT Edge tümleştirin

Azure IOT Edge, Azure DevOps ve Azure DevOps projeleri için yerleşik destek içerir, ancak ayrıca Jenkins ile DevOps işlevselliği genişletmek için bir eklenti sağlar. [Jenkins](https://jenkins.io/) eklentileri geliştirme ve dağıtım projeleri, IOT Edge dahil olmak üzere birçok türlerini desteklemek için kullandığı bir açık kaynak Otomasyon sunucusudur. 

Jenkins için Azure IOT Edge eklentisi, sürekli tümleştirme ve sürekli dağıtım odaklanır. Modülleri oluşturur ve kapsayıcı kayıt defterinizde kendi kapsayıcı görüntülerini gönderim bir derleme ve gönderme işlem hattı oluşturabilirsiniz. Ardından, IOT Edge cihazlarınıza modülleri dağıtan bir yayın işlem hattı oluşturun. 

IOT Edge eklentisi için Jenkins kullanmaya başlamadan önce Azure'da ve kapsayıcı kayıt defteri, kapsayıcı görüntüleri tutmak için IOT hub'ı gerekir. Eklenti dağıtımlar için IOT Edge cihazları oluşturabilmeniz IOT hub'ınıza Jenkins katkıda bulunan izinleri vermek için bir Azure hizmet sorumlusu kullanın. 

Başlamak hazırsanız yükleme bulabilir ve ayrıntılarını [Jenkins için Azure IOT Edge eklentisi](https://plugins.jenkins.io/azure-iot-edge).