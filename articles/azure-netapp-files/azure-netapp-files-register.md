---
title: Kaydetmek için Azure NetApp dosyaları | Microsoft Docs
description: Azure NetApp dosyaları hizmetine kaydetmek üzere bir istek göndermek nasıl açıklar.
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
ms.date: 01/04/2018
ms.author: b-juche
ms.openlocfilehash: 86c016a5dbcc0d78378e59bc6b3606ddf2c54f64
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60452798"
---
# <a name="register-for-azure-netapp-files"></a>Azure NetApp Files için kaydolma
Azure NetApp dosyaları kullanmadan önce Azure NetApp dosyaları hizmetine kaydetmek için birer istek göndermeniz gerekmektedir.  Kayıttan sonra daha sonra hizmeti kullandığınız kaydedin.

## <a name="request-to-enroll-in-the-service"></a>İstek hizmetine kaydetmek için
Genel Önizleme programına ve Microsoft.NetApp kaynak sağlayıcısına erişmek için izin verilenler listesinde bir parçası olmanız gerekir. Genel Önizleme programına katılma hakkındaki ayrıntılar için bkz. [Azure NetApp Files Genel Önizleme kayıt sayfası](https://aka.ms/nfspublicpreview). 


## <a name="register-the-netapp-resource-provider"></a>NetApp kaynak sağlayıcısını kaydetme

Hizmeti kullanmak için Azure için NetApp dosyaları Azure kaynak sağlayıcısını kaydetmeniz gerekir. 

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