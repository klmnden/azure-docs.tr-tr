---
title: Azure IOT çözümleri sitesini - Azure | Microsoft Docs
description: Çözüm hızlandırıcınız dağıtmak için AzureIoTSolutions.com Web sitesi kullanmayı açıklar.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: dobett
ms.openlocfilehash: 87f6b9cef50e4b8c388be835b2aa7bed8177ac4b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447463"
---
# <a name="use-the-azureiotsolutionscom-site-to-deploy-your-solution-accelerator"></a>Çözüm hızlandırıcınız dağıtılacağı azureiotsolutions.com sitesini kullan

Azure IOT Çözüm Hızlandırıcıları Azure aboneliğinizden dağıtabileceğiniz [AzureIoTSolutions.com](https://www.azureiotsolutions.com/Accelerators). Microsoft açık kaynak ve iş ortağı Çözüm Hızlandırıcıları AzureIoTSolutions.com barındırır. Bu çözüm Hızlandırıcıları ile hizalanan [Azure IOT başvuru mimarisi](https://aka.ms/iotrefarchitecture). Site, hızlı bir çözüm Hızlandırıcı bir tanıtım veya üretim ortamı olarak dağıtmak için kullanabilirsiniz.

![AzureIoTSolutions.com](media/iot-accelerators-permissions/iotsolutionscom.png)

> [!TIP]
> Dağıtım işlemi hakkında daha fazla denetime ihtiyacınız varsa, kullanabileceğiniz [çözüm Hızlandırıcısını dağıtmayı CLI](iot-accelerators-remote-monitoring-deploy-cli.md).

Çözüm Hızlandırıcıları aşağıdaki yapılandırmaları dağıtabilirsiniz:

* **Standart**: Bir üretim ortamında geliştirmek için bir genişletilmiş altyapı dağıtımı. Azure Container Service, mikro Hizmetleri birden fazla Azure sanal makinelerine dağıtır. Kubernetes mikro hizmetleri tek tek barındıran Docker kapsayıcılarını düzenler.
* **Temel**: Düşük maliyet bir Tanıtım amaçlı ya da bir sürümünü dağıtımı test etmek için. Tüm mikro hizmetler tek bir Azure sanal makinesine dağıtılır.
* **Yerel**: Test ve geliştirme için yerel makine dağıtım. Bu yaklaşım, mikro hizmetler için yerel bir Docker kapsayıcısı dağıtır ve IOT Hub, Azure Cosmos DB ile buluttaki Azure depolama hizmetlerine bağlar.

Her Çözüm Hızlandırıcıları, IOT Hub, Azure Stream Analytics ve Cosmos DB gibi Azure Hizmetleri farklı bir birleşimini kullanır. Daha fazla bilgi için ziyaret [AzureIoTSolutions.com](https://www.azureiotsolutions.com/Accelerators) ve çözüm Hızlandırıcısını seçin.

## <a name="sign-in-at-azureiotsolutionscom"></a>Azureiotsolutions.com oturum açın

Çözüm Hızlandırıcısını dağıtmadan önce bir Azure aboneliği ile ilişkili kimlik bilgilerini kullanarak AzureIoTSolutions.com oturum açmalısınız. Hesabınız birden fazla Microsoft Azure Active Directory (AD) kiracısı ile ilişkili ise, kullanabileceğiniz **hesap seçimi açılan listesini** için kullanılacak dizini seçin.

Çözüm Hızlandırıcısı dağıtma, kullanıcıları yönetme ve Azure hizmetlerini yönetmek için izinleriniz seçili dizin rolünüze bağlıdır. Çözüm Hızlandırıcıları ile ilgili yaygın Azure AD rolleri şunlardır:

* **Genel yönetici**: Birçok da olabilir [genel Yöneticiler](../active-directory/users-groups-roles/directory-assign-admin-roles.md) Azure AD Kiracı başına:

  * Azure AD kiracısı oluşturduğunuzda varsayılan olarak bu kiracının genel Yöneticisi olursunuz.
  * Genel yönetici, temel ve standart Çözüm Hızlandırıcıları dağıtabilirsiniz.

* **Etki alanı kullanıcısı**: Azure AD kiracısı başına çok sayıda etki alanı kullanıcıları olabilir. Bir etki alanı kullanıcısı temel çözüm Hızlandırıcısını dağıtabilirsiniz.

* **Konuk kullanıcı**: Azure AD kiracısı başına çok sayıda Konuk kullanıcı olabilir. Konuk kullanıcıları Azure AD kiracısında bir çözüm Hızlandırıcı dağıtamazsınız.

Azure AD'de kullanıcıları ve rolleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Active Directory'de kullanıcıları oluşturma](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
* [Uygulamalar için kullanıcı atama](../active-directory/manage-apps/assign-user-or-group-access-portal.md)

## <a name="choose-your-device"></a>Cihazınızı seçin

AzureIoTSolutions.com site bağlantıları [IOT cihaz kataloğu için Azure sertifikalı](https://catalog.azureiotsolutions.com/).

Katalog onaylı IOT donanım cihazlarının IOT çözümünüzü oluşturmaya başlamak için çözüm hızlandırıcılarınız bağlanabilir yüzlerce listeler.

![Cihaz kataloğu](media/iot-accelerators-permissions/devicecatalog.png)

Donanım üreticisi değilseniz tıklayın **bir iş ortağı** IOT programı için sertifikalı Microsoft ile iş ortaklığı hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarından birini denemek için hızlı başlangıçları inceleyin:

* [Uzaktan izleme çözümü deneyin](quickstart-remote-monitoring-deploy.md)
* [Bağlı fabrika çözümünü deneyin](quickstart-connected-factory-deploy.md)
* [Tahmine dayalı bakım çözümünü deneyin](quickstart-predictive-maintenance-deploy.md)
* [Cihaz benzetimi çözümünü deneyin](quickstart-device-simulation-deploy.md)
