---
title: Bir alt ağ Azure NetApp dosyaları temsilci | Microsoft Docs
description: Bir alt ağ Azure NetApp dosyaları temsilci açıklar.
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
ms.date: 11/13/2018
ms.author: b-juche
ms.openlocfilehash: 6c1a6bf4e7042c28239f57af6b39c0822b63b5e8
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57768085"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Azure NetApp Files için bir alt ağı temsilci olarak belirleme 

Bir alt ağ Azure NetApp dosyaları devretmeniz gerekir.   Bir birim oluşturduğunuzda, temsilci alt ağ belirtmenize gerek.

## <a name="about-this-task"></a>Bu görev hakkında
* Bir/24 varsayılan olarak yeni bir alt ağ oluşturmak için Sihirbazı için 251 kullanılabilir IP adreslerini sağlayan ağ maskesi. Bir/28'i kullanarak 16 kullanılabilir IP adreslerini sağlayan ağ maskesi hizmeti için yeterlidir.
* Bir ağ güvenlik grubu atamak veya hizmet temsilcisi alt ağdaki uç noktası değiştirilemez. Bunun yapılması, alt ağ temsilci başarısız olmasına neden olur.
* Her Azure sanal ağı (Vnet), yalnızca bir alt ağ, Azure için NetApp dosyaları atanabilir.
* Eşlenen sanal ağdan bir birime erişimi şu anda desteklenmiyor.

## <a name="steps"></a>Adımlar 
1.  Git **sanal ağlar** dikey penceresinden Azure portalı ve Azure NetApp dosyaları için kullanmak istediğiniz sanal ağı seçin.    

1. Seçin **alt ağlar** sanal ağ dikey penceresinde ve **+ alt ağ** düğmesi. 

1. Alt ağ Ekle sayfası aşağıdaki gerekli alanları tamamlayarak Azure NetApp dosyaları için kullanılacak yeni bir alt ağ oluşturun:
    * **Ad**: Alt ağ adı belirtin.
    * **Adres aralığı**: IP adres aralığını belirtin.
    * **Alt ağ temsilci**: Seçin **Microsoft.NetApp/volumes**. 

      ![Alt ağ temsilcisi](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Ayrıca oluşturabilir ve bir alt ağ temsilci olduğunda, [birim oluşturmak için Azure NetApp dosyaları](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Sonraki adımlar  
* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
* [Azure Hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


