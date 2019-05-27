---
title: REST API'si ile Azure NetApp dosyaları için geliştirme | Microsoft Docs
description: Azure NetApp dosya REST API'sini kullanmaya başlama işlemini açıklamaktadır.
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
ms.date: 05/17/2019
ms.author: b-juche
ms.openlocfilehash: 996fbcc7c3c9af0da9160216785ecd54840660e8
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957030"
---
# <a name="develop-for-azure-netapp-files-with-rest-api"></a>REST API'si ile Azure NetApp dosyaları için geliştirme 

NetApp hesabı, kapasitesi havuzu, birimleri ve anlık görüntüleri gibi kaynakları üzerinde yapılan HTTP işlemlerinin Azure NetApp dosya hizmeti REST API tanımlar. Bu makale Azure NetApp dosya REST API'sini kullanmaya başlamanıza yardımcı olur.

## <a name="azure-netapp-files-rest-api-specification"></a>Azure NetApp dosya REST API'sini belirtimi

Azure NetApp dosyaları için REST API Belirtimi üzerinden yayımlanan [GitHub](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager):

`https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager`


## <a name="access-the-azure-netapp-files-rest-api"></a>Azure NetApp dosya REST API erişimi  

1. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) , zaten yapmadıysanız.
2. Azure Active Directory'de (Azure AD) bir hizmet sorumlusu oluşturun:
   1. Sahip olduğunuzu doğrulayın [yeterli izinlere](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions).

   1. Azure CLI içinde aşağıdaki komutu girin:  

           az ad sp create-for-rbac --name $YOURSPNAMEGOESHERE--password $YOURGENERATEDPASSWORDGOESHERE

      Komut çıktısı, aşağıdaki örneğe benzer:  

           { 
               "appId": "appIDgoeshere", 
               "displayName": "APPNAME", 
               "name": "http://APPNAME", 
               "password": "supersecretpassword", 
               "tenant": "tenantIDgoeshere" 
           } 

      Komut çıktısı tutun.  İhtiyacınız olacak `appId`, `password`, ve `tenant` değerleri. 

3. Bir OAuth erişim belirteci isteği:

    Bu makaledeki örneklerde cURL kullanın.  Çeşitli API araçları gibi kullanabilirsiniz [Postman](https://www.getpostman.com/), [uyku moduna geçmeme](https://insomnia.rest/), ve [Paw](https://paw.cloud/).  

    Adım 2'deki komut çıktısı aşağıdaki örnekte değişkenleri değiştirin. 

        curl -X POST -d 'grant_type=client_credentials&client_id=[APP_ID]&client_secret=[PASSWORD]&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/[TENANT_ID]/oauth2/token

    Çıktı aşağıdaki örneğe benzer bir erişim belirteci sağlar:

        eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9

    Görüntülenen belirteci 3600 saniye için geçerlidir. Bundan sonra yeni bir belirteç istemeniz gerekir. 
    Belirteç için bir metin düzenleyicisi kaydedin.  Sonraki adım için ihtiyacınız.

4. Bir test çağrısı gönderin ve belirteci, REST API erişimi doğrulamak için şunları içerir:

        curl -X GET -H "Authorization: Bearer [TOKEN]" -H "Content-Type: application/json" https://management.azure.com/subscriptions/[SUBSCRIPTION_ID]/providers/Microsoft.Web/sites?api-version=2016-08-01

## <a name="examples-using-the-api"></a>API kullanımı örnekleri  

Bu makalede, istek için temel aşağıdaki URL'yi kullanır. Bu URL Azure NetApp dosyaları ad alanı kök dizinine işaret eder. 

`https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2017-08-15`

Yerini mi `subID` ve `resourceGroups` aşağıdaki örnekleri değerleri kendi değerlerinizle. 

### <a name="get-request-examples"></a>İstek örnekleri edinin

Aşağıdaki örneklerde gösterildiği gibi bir GET isteği bir abonelikte Azure NetApp dosya sorgu nesnelere kullanabilirsiniz: 

        #get NetApp accounts 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2017-08-15

        #get capacity pools for NetApp account 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools?api-version=2017-08-15

        #get volumes in NetApp account & capacity pool 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes?api-version=2017-08-15

        #get snapshots for a volume 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/VOLUMEGOESHERE/snapshots?api-version=2017-08-15

### <a name="put-request-examples"></a>PUT İsteği örnekleri

Bir PUT isteği olarak aşağıdaki örneklerde gösterildiği Azure NetApp dosyaları yeni nesneler oluşturmak için kullanın. PUT istek gövdesinde JSON biçimli verilerin değişikliklerin içerebilir veya bir dosyayı okuma belirtebilirsiniz. 

        #create a NetApp account  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE?api-version=2017-08-15

        #create a capacity pool  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE?api-version=2017-08-15

        #create a volume  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME?api-version=2017-08-15

        #create a volume snapshot  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME/Snapshots/SNAPNAME?api-version=2017-08-15

### <a name="json-examples"></a>JSON örnekleri

Aşağıdaki örnek, NetApp hesabının nasıl oluşturulacağını gösterir:

    { 
        "name": "MYNETAPPACCOUNT", 
        "type": "Microsoft.NetApp/netAppAccounts", 
        "location": "westus2", 
        "properties": { 
            "name": "MYNETAPPACCOUNT" 
        }
    } 

Aşağıdaki örnek, bir kapasitesi havuzu oluşturma işlemi gösterilmektedir: 

    {
        "name": "MYNETAPPACCOUNT/POOLNAME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools",
        "location": "westus2",
        "properties": {
            "name": "POOLNAME"
            "size": "4398046511104",
            "serviceLevel": "Premium"
        }
    }

Aşağıdaki örnek, yeni bir birim oluşturma işlemi gösterilmektedir: 

    {
        "name": "MYNEWVOLUME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/volumes",
        "location": "westus2",
        "properties": {
            "serviceLevel": "Premium",
            "usageThreshold": "322122547200",
            "creationToken": "MY-FILEPATH",
            "snapshotId": "",
            "subnetId": "/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.Network/virtualNetworks/VNETGOESHERE/subnets/MYDELEGATEDSUBNET.sn"
            }
    }

Aşağıdaki örnek, bir birimin anlık görüntüsünü oluşturma işlemi gösterilmektedir: 

    {
        "name": "apitest2/apiPool01/apiVol01/snap02",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots",
        "location": "westus2",
        "properties": {
            "name": "snap02",
            "fileSystemId": "0168704a-bbec-da81-2c29-503825fe7420"
        }
    }

> [!NOTE] 
> Belirtmeniz gereken `fileSystemId` bir anlık görüntü oluşturmak için.  Alabilirsiniz `fileSystemId` bir birim için bir GET isteğiyle değeri. 

## <a name="next-steps"></a>Sonraki adımlar

[Bkz. Azure NetApp dosya REST API'si başvurusu](https://docs.microsoft.com/rest/api/netapp/)
