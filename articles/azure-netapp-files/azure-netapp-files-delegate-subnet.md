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
ms.date: 03/25/2019
ms.author: b-juche
ms.openlocfilehash: fd8e380ad68b86b9ffd0f1e40efde8bdadfb19c5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64711826"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Azure NetApp Files için bir alt ağı temsilci olarak belirleme 

Bir alt ağ Azure NetApp dosyaları devretmeniz gerekir.   Bir birim oluşturduğunuzda, temsilci alt ağ belirtmenize gerek.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Bir/24 varsayılan olarak yeni bir alt ağ oluşturmak için Sihirbazı için 251 kullanılabilir IP adreslerini sağlayan ağ maskesi. Bir/28'i kullanarak 16 kullanılabilir IP adreslerini sağlayan ağ maskesi hizmeti için yeterlidir.
* Her Azure sanal ağı (Vnet), yalnızca bir alt ağ, Azure için NetApp dosyaları atanabilir.
* Bir ağ güvenlik grubu atamak veya hizmet temsilcisi alt ağdaki uç noktası değiştirilemez. Bunun yapılması, alt ağ temsilci başarısız olmasına neden olur.
* Genel olarak eşlenmiş sanal ağdan bir birime erişimi şu anda desteklenmiyor.
* Oluşturma [kullanıcı tanımlı özel yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) adresine sahip VM ağlarındaki Azure için NetApp dosyaları temsilci bir alt ağ ön eki (hedef) desteklenmiyor. Bunun yapılması, VM bağlantısı etkiler.

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


