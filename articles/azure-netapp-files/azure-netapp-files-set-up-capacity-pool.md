---
title: Azure NetApp Files için kapasite havuzunu ayarlama | Microsoft Docs
description: İçinde birimleri oluşturabileceğiniz bir kapasite havuzunu ayarlama işlemi açıklanır.
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
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 55a1d16ce1617ecf7bc28c7c62de8557ceeea311
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53412920"
---
# <a name="set-up-a-capacity-pool"></a>Kapasite havuzunu ayarlama
Kapasite havuzu ayarlamak, içinde birim oluşturmanıza olanak tanır.  

## <a name="before-you-begin"></a>Başlamadan önce 
Önceden bir NetApp hesabınız olması gerekir.   

[NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Adımlar 

1. NetApp hesabınızın yönetim dikey penceresine gidin ve gezinti bölmesindeki **Kapasite havuzları**’nı seçin.

2. Yeni kapasite havuzu oluşturmak için **+ Havuz ekle**’ye tıklayın.   
    Yeni Kapasite Havuzu penceresi görüntülenir.

3. Yeni kapasite havuzuyla ilgili aşağıdaki bilgileri sağlayın:  
  * **Ad**  
    Kapasite havuzunun adını belirtin.  
    Kapasite havuzu adı her NetApp hesabı için benzersiz olmalıdır.

  * **Hizmet düzeyi**   
    Bu alan, kapasite havuzunun hedef performansını gösterir.  
    Şu anda yalnızca Premium hizmet düzeyi kullanılabilir. 

  *  **Boyut**     
      Satın aldığınız kapasite havuzunun boyutunu belirtin.        
      Kapasite havuzunun boyutu en az 4 TiB’dir. Havuzunuzu 4 TiB’nin katları olan büyüklüklerde oluşturabilirsiniz.   
      
      ![Yeni kapasite havuzu](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. **Tamam** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar 

[Temsilci bir alt ağ Azure NetApp dosyaları](azure-netapp-files-delegate-subnet.md)


