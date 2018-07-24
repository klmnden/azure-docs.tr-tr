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
ms.openlocfilehash: 0e9203f5b4e2a9043e242b804c82017cf6fc3ee1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010812"
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

4. **Tamam**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar 

1. [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
2. [Birim için dışarı aktarma ilkesini yapılandırma (isteğe bağlı)](azure-netapp-files-configure-export-policy.md)

