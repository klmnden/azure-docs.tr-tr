---
title: "Varsayılan geçici klasör boyutu bir rol için çok küçük | Microsoft Docs"
description: "Sınırlı miktarda alan TEMP klasörü için bir bulut hizmet rolü vardır. Bu makalede alanı tükenmesini önleme bazı öneriler sağlar."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 02a185fbe3603aa7f033ea3d50dc6ad36dd7f8f6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Varsayılan TEMP klasörü boyutu bir bulut hizmeti web/çalışan rolü çok küçük.
Varsayılan geçici dizini bulut hizmeti çalışan veya web rolünün belirli bir noktada tam dönüşebilir 100 MB maksimum boyuta sahiptir. Bu makalede, geçici dizin için alanının tükenmesinden önlemek açıklar.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Alana neden çalıştırılsın mı?
Standart Windows ortam değişkenlerini TEMP ve TMP uygulamanızda çalışan kod için kullanılabilir. TEMP ve TMP 100 MB maksimum boyuta sahip tek bir dizin gelin. Bulut hizmeti yaşam döngüsü bu dizinde depolanan tüm verileri kalıcı değildir; bir bulut hizmetinde rol örnekleri dönüştürülünceye, dizin temizlendi.

## <a name="suggestion-to-fix-the-problem"></a>Sorunu düzeltmek için öneri
Aşağıdaki alternatifleri birini uygulayın:

* Yerel depolama kaynağı yapılandırın ve TEMP veya TMP kullanmak yerine doğrudan erişebilirsiniz. Uygulama içinde çalışan koddan yerel depolama kaynağı erişmek için arama [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) yöntemi.
* Yerel depolama kaynağı yapılandırın ve yerel depolama kaynağı yoluna işaret edecek şekilde TEMP ve TMP dizinleri gelin. Bu değişikliği içinde gerçekleştirilmelidir [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) yöntemi.

Aşağıdaki kod örneğinde hedef dizinleri TEMP ve TMP gelen OnStart yöntemi içinde nasıl değiştirileceğini gösterir:

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Açıklayan bir blog okuma [Azure Web rolü ASP.NET geçici klasörü boyutunu artırmak nasıl](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Daha fazla bilgi görüntülemek [sorun giderme makalelerini](/?tag=top-support-issue&product=cloud-services) bulut Hizmetleri için.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme konusunda bilgi almak için görüntüleyin [Kevin Williamson'ın blog dizisini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
