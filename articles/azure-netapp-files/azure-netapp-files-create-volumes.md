---
title: Azure NetApp Files için birim oluşturma | Microsoft Docs
description: Azure NetApp Files için birim oluşturma işlemi açıklanır.
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
ms.openlocfilehash: 42e475f4da2102bb8b010daec6e6451ba7f13848
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011101"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Azure NetApp Files için birim oluşturma

Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.  Kapasite havuzunda birden fazla birim oluşturabilirsiniz; ancak, birimlerin toplam kapasite kullanımı havuzun boyutunu aşamaz. 

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.  

[Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)

## <a name="steps"></a>Adımlar 
1.  Kapasite Havuzlarını Yönet dikey penceresinden **Birimler** dikey penceresine tıklayın. 
2.  Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Yeni Birim penceresi görüntülenir.

3.  Yeni Birim penceresinde **Oluştur**’a tıklayın ve aşağıdaki alanlar için bilgileri girin:   
    * **Ad**      
        Oluşturmakta olduğunuz birim için ad belirtin.   
        Ad, kaynak grubu içinde benzersiz olmalıdır. En az 3 karakter uzunluğunda olmalıdır.  Tüm alfasayısal karakterler kullanılabilir.

    * **Dosya yolu**  
        Yeni birimin dışarı aktarma yolunu oluşturmak için kullanılacak dosya yolunu belirtin. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.   
        Bağlama hedefi, NFS hizmeti IP adresinin uç noktasıdır. Otomatik olarak oluşturulur.    
        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır.  

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  
        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin. Belirttiğiniz Vnet'te Azure NetApp Files yapılandırılmış olmalıdır. Azure NetApp Files hizmetine yalnızca birimle aynı konumda yer alan Vnet'ten erişilebilir.   

    ![Yeni birim](../media/azure-netapp-files/azure-netapp-files-new-volume.png)

4.  **Tamam**’a tıklayın. 
 
Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar  
[Birim için dışarı aktarma ilkesini yapılandırma (isteğe bağlı)](azure-netapp-files-configure-export-policy.md)

