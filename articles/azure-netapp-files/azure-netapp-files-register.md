---
title: Kaydetmek için Azure NetApp dosyaları | Microsoft Docs
description: Azure NetApp dosyaları kullanmak üzere kaydolmanız açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: b-juche
ms.openlocfilehash: fbe0b82008d7b15332c4e2cd62c49c611f20fe89
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794706"
---
# <a name="register-for-azure-netapp-files"></a>Azure NetApp Files için kaydolma

> [!IMPORTANT] 
> Azure NetApp dosyaları kaynak sağlayıcısı kaydetmeden önce bir e-posta, hizmete erişim verilmiş onaylayan NetApp dosyaları Azure ekibinden aldığınız gerekir. 

Bu makalede, hizmeti kullanmaya başlayabilmesi için Azure NetApp dosyaları kaydetme öğrenin.

## <a name="waitlist"></a>Hizmete erişmek için bir waitlist isteği gönderin

1. Azure NetApp dosya hizmeti üzerinden erişmek için waitlist talebinizi [Azure NetApp dosyaları waitlist gönderim sayfasını](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u). 

    Waitlist kaydolma hemen hizmeti erişimi garanti etmez. 

2. Bir resmi onay e-posta Azure NetApp dosyaları ekibinin diğer görevlerle devam etmeden önce bekleyin. 

## <a name="resource-provider"></a>NetApp kaynak sağlayıcısını kaydetme

Hizmeti kullanmak için Azure için NetApp dosyaları Azure kaynak sağlayıcısını kaydetmeniz gerekir.

> [!NOTE] 
> Başarılı bir şekilde Erişim hizmeti için bile verilmeden NetApp kaynak sağlayıcısını kaydedin mümkün olacaktır. Ancak, erişim yetkisi olmadan herhangi bir Azure portalı veya API isteği NetApp hesabı veya diğer Azure NetApp dosyaları kaynak oluşturmak için şu hatayla reddedilir:  
>
> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"NotFound","message":"{\r\n \"error\": {\r\n \"code\": \"InvalidResourceType\",\r\n \"message\": \"The resource type could not be found in the namespace 'Microsoft.NetApp' for api version '2017-08-15'.\"\r\n }\r\n}"}]}`


1. Azure portalında, sağ üst köşesinde Azure Cloud Shell simgesine tıklayın:

      ![Azure Cloud Shell simgesi](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Azure hesabınızda birden fazla aboneliğiniz varsa, Güvenilenler listesinde Azure NetApp dosyaları için bir tane seçin:
    
        az account set --subscription <subscriptionId>

3. Azure Cloud Shell konsolunda aboneliğinizi izin verilenler listesinde olduğunu doğrulamak için aşağıdaki komutu girin:
    
        az feature list | grep NetApp

   Komut çıktısı aşağıdaki gibidir:
   
       "id": "/subscriptions/<SubID>/providers/Microsoft.Features/providers/Microsoft.NetApp/features/publicPreviewADC",  
       "name": "Microsoft.NetApp/publicPreviewADC" 
       
   `<SubID>` Abonelik kimliğinizi olduğu

    Özellik adı görmüyorsanız `Microsoft.NetApp/publicPreviewADC`, hizmetine erişiminiz yok. Bu adımda durdurun. Yönergeleri [hizmete erişim için waitlist talebinizi](#waitlist) devam etmeden önce hizmet erişim istemek için. 

4. Azure Cloud Shell konsolunda Azure kaynak sağlayıcısını kaydetmek için aşağıdaki komutu girin: 
    
        az provider register --namespace Microsoft.NetApp --wait

   `--wait` Parametresi kaydı tamamlamak beklenecek konsol bildirir. Kayıt işleminin tamamlanması biraz zaman alabilir.

5. Azure Cloud Shell konsolunda Azure kaynak sağlayıcısına kayıtlı olduğunu doğrulamak için aşağıdaki komutu girin: 
    
        az provider show --namespace Microsoft.NetApp

   Komut çıktısı aşağıdaki gibidir:
   
        {
        "id": "/subscriptions/<SubID>/providers/Microsoft.NetApp",
        "namespace": "Microsoft.NetApp", 
        "registrationState": "Registered", 
        "resourceTypes": […. 

   `<SubID>` Abonelik kimliğinizi olduğu  `state` Parametre değeri gösteren `Registered`.

6. Azure portalından tıklayın **abonelikleri** dikey penceresi.
7. Abonelik dikey penceresinde, abonelik kimliğinizi'a tıklayın. 
8. Aboneliğin ayarlarında tıklayın **kaynak sağlayıcıları** Microsoft.NetApp sağlayıcısı kayıtlı durumu gösterdiğini doğrulayın: 

      ![Kayıtlı Microsoft.NetApp](../media/azure-netapp-files/azure-netapp-files-registered-resource-providers.png)


## <a name="next-steps"></a>Sonraki adımlar

[NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)