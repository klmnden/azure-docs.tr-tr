---
title: Bir Azure Machine Learning modeli Azure IOT Edge cihazına dağıtma | Microsoft Docs
description: Bu belgede, Azure Machine Learning modellerini Azure IOT Edge cihazlarına nasıl dağıtılabilir açıklanmaktadır.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: 24d3cf0c4b1a1283e7a6a7f61f0bb23dae7143d5
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43113011"
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Bir Azure Machine Learning modeli Azure IOT Edge cihazına dağıtma

Azure Machine Learning modeli Docker tabanlı web Hizmetleri olarak kapsayıcılı hale. Azure IOT Edge cihazları uzaktan kapsayıcılarının dağıtmanıza olanak sağlar. Bu hizmetler birlikte daha hızlı yanıt süreleri ve veri aktarımı daha az Modellerinizi ucuna çalıştırmak için kullanın. 

Ek betikleri ve yönergeler bulunabilir [Azure IOT Edge için AI Toolkit](http://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Modeli kullanıma hazır hale getirme

Azure IOT Edge modülleri, kapsayıcı görüntüleri üzerinde temel alır. Makine öğrenimi modelinizi IOT Edge cihazına dağıtmak için bir Docker görüntüsü oluşturmanız gerekir.

' Ndaki yönergeleri takip ederek modelinizi kullanıma hazır hale getirme [Azure Machine Learning Model Yönetimi Web hizmeti dağıtımı](model-management-service-deploy.md) modelinizi bir Docker görüntüsü oluşturmak için.

## <a name="deploy-to-azure-iot-edge"></a>Azure IOT Edge için dağıtma

Modelinizi görüntüsünü oluşturduktan sonra tüm Azure IOT Edge cihazına dağıtabilirsiniz. Tüm makine öğrenimi modellerini IOT Edge cihazlarında çalıştırabilirsiniz. 

### <a name="set-up-an-iot-edge-device"></a>Bir IOT Edge cihazı ayarlama

Bir cihazı hazırlamak için Azure IOT Edge belgeleri kullanın. 

1. [Azure IOT Hub ile cihaz kaydetme](../../iot-edge/how-to-register-device-portal.md). Bu işlemlerin çıkış fiziksel Cihazınızı yapılandırmak için kullanabileceğiniz bir bağlantı dizesidir. 
2. IOT Edge çalışma zamanı fiziksel cihazınıza yüklemeniz ve bir bağlantı dizesiyle yapılandırın. Çalışma zamanı yükleyebileceğiniz [Windows](../../iot-edge/how-to-install-iot-edge-windows-with-windows.md) veya [Linux](../../iot-edge/how-to-install-iot-edge-linux.md) cihazlar.  


### <a name="find-the-machine-learning-container-image-location"></a>Machine Learning kapsayıcı görüntüsü konumunu bulma
Machine Learning kapsayıcı görüntünüzü konumunu ihtiyacınız vardır. Kapsayıcı görüntü konumu bulmak için:

1. [Azure portalında](http://portal.azure.com/) oturum açın.
2. İçinde **Azure Container Registry**, incelemek istediğiniz kayıt defteri seçin.
3. Kayıt defterindeki tıklayın **depoları** tüm depolar ve resimlerinin listesini görmek için.

Azure portalındaki kapsayıcı kayıt defterinizin adresindeki bakarken, kapsayıcı kayıt defteri kimlik bilgilerini alın. Bu kimlik bilgilerinin görüntünün özel kayıt defterinizdeki çekebilirsiniz böylece IOT Edge cihazına verilmesi gerekir. 

1. Kapsayıcı kayıt defterinde tıklayın **erişim anahtarları**. 
2. **Etkinleştirme** yönetici kullanıcı, zaten yoksa. 
3. İçin değerleri kaydedin **oturum açma sunucusu**, **kullanıcıadı**, ve **parola**. 

### <a name="deploy-the-container-image-to-your-device"></a>Cihazınız için kapsayıcı görüntüsü dağıtma

Kapsayıcı görüntüsünü ve kapsayıcı kayıt defteri kimlik bilgileri ile makine öğrenimi modelinde IOT Edge cihazınıza dağıtmaya hazırsınız. 

Bölümündeki yönergeleri [dağıtma IOT Edge modülleri Azure portalından](../../iot-edge/how-to-deploy-modules-portal.md) modelinizi IOT Edge Cihazınızda başlatmak için. 











