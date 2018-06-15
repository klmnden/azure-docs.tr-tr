---
title: Azure bulut Hizmetleri'ni Visual Studio ile rollerinde yönetme | Microsoft Docs
description: Rol Ekleme ve Visual Studio ile Azure cloud Services kaldırma öğrenin.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: d0a2148274cd41654eb789772b3ea46bc4ed01aa
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31793979"
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Azure bulut Hizmetleri'ni Visual Studio ile rollerini yönetme
Azure bulut hizmetinizi oluşturduktan sonra yeni rolleri ekleyin veya mevcut rolleri kaldırın. Ayrıca, mevcut bir projeyi alın ve bir role dönüştürün. Örneğin, bir ASP.NET web uygulamasını almak ve bir web rolü olarak belirleyin.

## <a name="adding-a-role-to-an-azure-cloud-service"></a>Bir Azure bulut hizmeti için bir rol ekleme
Aşağıdaki adımlarda Visual Studio'da bir Azure bulut hizmeti projesi için bir web veya çalışan rolü eklerken size kılavuzluk.

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, proje düğümünü genişletin

1. Sağ **rolleri** bağlam menüsünü görüntülemek için düğümü. Bağlam menüsünden seçin **Ekle**, bir var olan web rolü veya çalışan rolü geçerli çözümden seçin veya bir web veya çalışan rolü projesi oluşturun. Ayrıca, bir ASP.NET web uygulaması projesini gibi uygun bir projeyi seçin ve rol projesi ile ilişkilendirebilirsiniz.

    ![Bir Azure bulut hizmeti projesine bir rolü eklemek için menü seçenekleri](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>Bir Azure bulut hizmetinden bir rolünü kaldırma
Aşağıdaki adımlar bir Azure bulut hizmeti projesini Visual Studio'da web veya çalışan rolü kaldırma aracılığıyla yol.

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, proje düğümünü genişletin

1. Genişletme **rolleri** düğümü.

1. İstediğiniz kaldırmak ve bağlam menüsünden seçin düğümünü sağ tıklatın **kaldırmak**. 

    ![Bir Azure bulut hizmeti için bir rolü eklemek için menü seçenekleri](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-to-an-azure-cloud-service-project"></a>Bir rol için bir Azure bulut hizmeti projesi yeniden ekleniyor
Bulut hizmeti projenizden rolü kaldırmak ancak projeye rolü eklemek daha sonra karar verirseniz, yalnızca Rol bildirimi ve uç noktaları ve tanılama bilgileri gibi temel öznitelikler eklenir. Ek kaynaklar veya başvuruları eklenir `ServiceDefinition.csdef` dosya veya `ServiceConfiguration.cscfg` dosyası. Bu bilgileri eklemek istiyorsanız, bu dosyalar onu el ile eklemeniz gerekir.

Örneğin, bir web hizmeti rolü kaldırabilir ve daha sonra geri çözümünüze bu rolü eklemek karar. Bunu yaparsanız, bir hata oluşur. Bu hatayı önlemek için eklemeniz gerekir `<LocalResources>` uygulamasına geri aşağıdaki XML gösterilen öğesinin `ServiceDefinition.csdef` dosya. Ad özniteliği için bir parçası olarak geri projeye eklenen web hizmeti rolü adını kullanmak **<LocalStorage>** öğesi. Bu örnekte, web hizmeti rolü adıdır **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Sonraki adımlar
- [Visual Studio ile rolleri bir Azure bulut hizmeti için yapılandırma](vs-azure-tools-configure-roles-for-cloud-service.md)
