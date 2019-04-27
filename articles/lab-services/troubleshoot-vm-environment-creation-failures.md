---
title: VM ve ortam oluşturma hataları Azure DevTest Labs sorunlarını giderme | Microsoft Docs
description: Sanal makine (VM) ve Azure DevTest labs'deki ortam oluşturma hataları sorunlarını gidermeyi öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.openlocfilehash: a653a785e99619c3e256613d6a4d2c7592f54c8c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60848510"
---
# <a name="troubleshoot-virtual-machine-vm-and-environment-creation-failures-in-azure-devtest-labs"></a>Sanal makine (VM) ve Azure DevTest labs'deki ortam oluşturma hataları giderme
DevTest Labs, makine adı geçersiz veya ilgili Laboratuvar ilkeyi ihlal Eğer uyarılar sağlar. Bazı durumlarda, kırmızı gördüğünüz `X` yanındaki Laboratuvarınızı bildiren bir sorun oluştu VM veya ortam durumu.  Bu makalede, altta yatan sorunu bulup neyse, gelecekte sorundan kurtulmak için kullanabileceğiniz birkaç püf noktaları sağlar.

## <a name="portal-notifications"></a>Portal bildirimleri
Azure portalını kullanıyorsanız, bakmak için ilk yerdir **bildirimler panelinde**.  Bildirimler panelinde, tıklayarak ana komut çubuğunda kullanılabilir **zil simgesine**, VM veya ortam oluşturma Laboratuvar veya başarılı olup size bildirir.  Bir hata oluştuğunda, oluşturma hatası ile ilişkili hata iletisini görürsünüz. Ayrıntıları genellikle daha fazla sorunu gidermenize yardımcı olacak bilgiler sağlar. Aşağıdaki örnekte, sanal makine oluşturma, çekirdek bitmesi nedeniyle başarısız oldu. Ayrıntılı ileti sorunu düzeltin ve çekirdek kota artışı isteğinde anlatır.

![Azure portal bildirimi](./media/troubleshoot-vm-environment-creation-failures/portal-notification.png)


## <a name="activity-logs"></a>Etkinlik günlükleri
Süre, VM veya ortam oluşturmayı denediğinizde bir hata araştırdığınızı etkinlik günlüklerine bakın. Bu bölümde sanal makineler ve ortamlar için günlükleri nasıl gösterir.

## <a name="activity-logs-for-virtual-machines"></a>Sanal makineler için etkinlik günlükleri

1. Laboratuvarınız için giriş sayfasında başlatmak için VM'yi seçin **sanal makine** sayfası.
2. Üzerinde **sanal makine** sayfasında **izleme** sol menüsünde, select bölümünü **etkinlik günlüğü** VM ile ilişkili tüm günlükleri görmek için.
3. Etkinlik günlüğü öğelerinde, başarısız olan işlemi seçin. Genellikle, başarısız olan işlemi olarak da adlandırılır `Write Virtualmachines`.
4. Sağ bölmede, JSON sekmesine gidin. Günlük JSON görünümünde ayrıntılarını görürsünüz.

    ![Bir VM için etkinlik günlüğü](./media/troubleshoot-vm-environment-creation-failures/vm-activity-log.png)
5. Bulana kadar JSON günlük Ara `statusMessage` özelliği. Bu, daha ayrıntılı bilgi edinmek ve ana hata iletisi varsa sağlar. Aşağıdaki JSON, bu makalenin önceki bölümlerinde görülen bir teklif çekirdek aşıldı örneğin hatadır.

    ```json
    "properties": {
        "statusCode": "Conflict",
        "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"OperationNotAllowed\",\"message\":\"Operation results in exceeding quota limits of Core. Maximum allowed: 100, Current in use: 100, Additional requested: 8. Please read more about quota increase at http://aka.ms/corequotaincrease.\"}]}}",
    },
    ```

## <a name="activity-log-for-an-environment"></a>Bir ortam için etkinlik günlüğü

Bir ortam oluşturmak için Etkinlik günlüğünü görmek için aşağıdaki adımları izleyin:

1. Laboratuvarınız için giriş sayfasında, seçin **yapılandırması ve ilkelerini** sol menüsünde.
2. üzerinde **yapılandırması ve ilkelerini** sayfasında **etkinlik günlüklerini** menüsünde.
3. Hata günlüğü etkinlik listesinde arayın ve seçin.
4. Sağ bölmedeki JSON sekmesine geçin ve Ara **statusMessage**.

    ![Ortam etkinlik günlüğü](./media/troubleshoot-vm-environment-creation-failures/envirionment-activity-log.png)

## <a name="resource-manager-template-deployment-logs"></a>Resource Manager şablonu dağıtım günlükleri
Ortam ya da sanal makineyi Otomasyon oluşturulmuş olsa bile, hata bilgilerini aramak için bir son bağlantısı yoktur. Azure Resource Manager şablonu dağıtım günlüğü olmasıdır. Bir laboratuar kaynağı Otomasyon oluşturulduğunda, genellikle bir Azure Resource Manager şablon dağıtımı yapılır. Bkz:[ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) örnek Azure Resource Manager şablonları, DevTest Labs kaynakları oluşturmak için.

Laboratuvar şablonu dağıtım günlüklerini görmek için aşağıdaki adımları izleyin:

1. Sayfanın Laboratuvar bulunduğu kaynak grubunun başlatın.
2. Seçin **dağıtımları** sol menüsünde **ayarları**.
3. Dağıtımları başarısız durumundaki arayın ve seçin.
4. Üzerinde **dağıtım** sayfasında **işlem ayrıntıları** başarısız olan işlem için bağlantı.
5. İçinde başarısız olan işlem hakkında bilgi **işlem ayrıntıları** penceresi.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [yapıt hatalarını giderme](devtest-lab-troubleshoot-artifact-failure.md)