---
title: Yapılandırma ve Azure DevTest Labs'de ortak ortamlardaki kullanma | Microsoft Docs
description: Yapılandırma ve Azure DevTest Labs'de ortak ortamlardaki kullanma hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2018
ms.author: spelluru
ms.openlocfilehash: 2cd6998c7ac11638ead67fde384bdf4599692781
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201792"
---
# <a name="configure-and-use-public-environments-in-azure-devtest-labs"></a>Yapılandırma ve Azure DevTest Labs'de ortak ortamlardaki kullanma
Azure DevTest Labs sahip bir [Azure Resource Manager şablonları, ortak depo](https://github.com/Azure/azure-devtestlab/tree/master/Environments) kendiniz bir dış GitHub kaynağına bağlanmak zorunda kalmadan ortamlar oluşturmak için kullanabileceğiniz. Bu depo, Azure Web Apps, Service Fabric kümesi ve SharePoint grubu ortamı geliştirme gibi sık kullanılan şablonları içerir. Bu özellik, oluşturduğunuz her bir laboratuvar için gelen ortak depo yapıtları benzer. Ortam depo labs PaaS kaynaklar için bir yumuşak Başlarken deneyimini ile sağlamak için en düşük giriş parametrelerini içeren önceden yazılmış ortam şablonlarını kullanmaya hızlıca başlayın sağlar. 

## <a name="configuring-public-environments"></a>Ortak ortamlardaki yapılandırma
Laboratuvar sahibi olarak, ortam genel deponun Laboratuvar oluşturma sırasında laboratuvarınız için etkinleştirebilirsiniz. Laboratuvarınız için ortak ortamlardaki etkinleştirmek için seçin **üzerinde** için **ortak ortamlardaki** bir laboratuvar oluşturma sırasında alan. 

![Yeni bir laboratuvara yönelik genel ortam etkinleştir](media/devtest-lab-configure-use-public-environments/enable-public-environment-new-lab.png)


Ortam genel deponun mevcut Laboratuvar için etkin değil. El ile depoda şablonları kullanacak şekilde etkinleştirin. Resource Manager şablonları kullanılarak oluşturulan Laboratuvar için depo de varsayılan olarak devre dışıdır.

Etkinleştirmek/ortak ortamlardaki laboratuvarınız için devre dışı ve ayrıca yalnızca belirli ortamlara kullanılabilir Laboratuvar kullanıcıları için aşağıdaki adımları kullanarak hale getirebilirsiniz: 

1. Seçin **yapılandırması ve ilkelerini** laboratuvarınız için. 
2. İçinde **sanal makine TABANLARI** bölümünden **ortak ortamlardaki**.
3. Laboratuvar için ortak ortamlardaki etkinleştirmek için seçin **Evet**. Aksi takdirde seçin **Hayır**. 
4. Ortak ortamlardaki etkinleştirilirse, depodaki tüm ortamları varsayılan etkindir. Laboratuvar kullanıcılarınız için kullanılabilir yapmak için bir ortam da seçebilirsiniz. 

![Ortak ortamlardaki sayfası](media/devtest-lab-configure-use-public-environments/public-environments-page.png)

## <a name="use-environment-templates-as-a-lab-user"></a>Bir laboratuvar kullanıcı olarak ortam şablonlarını kullanma
Bir laboratuvar kullanıcı olarak yeni bir ortam ortam şablonlarını etkin listesinden yalnızca seçerek oluşturabileceğiniz **+ Ekle** Laboratuvar sayfasında araç çubuğundan. Listenin üst kısmındaki Laboratuvar yöneticinize etkin ortak ortamlardaki şablonları tabanları listesini içerir.

![Genel ortam şablonlarını](media/devtest-lab-configure-use-public-environments/public-environment-templates.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu depo sık kullanılan ve yararlı Resource Manager şablonları kendi ekleme katkıda bulunmak istediğiniz bir açık kaynaklı bir depodur. Katkıda bulunmak için yalnızca karşı deponun çekme isteği gönderin.  
