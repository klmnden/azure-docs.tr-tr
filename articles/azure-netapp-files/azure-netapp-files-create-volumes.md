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
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: b-juche
ms.openlocfilehash: 0d4629651e17c917720196c1e5f6152c50c2a33a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770275"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Azure NetApp Files için birim oluşturma

Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır.  Kapasite havuzunda birden fazla birim oluşturabilirsiniz; ancak, birimlerin toplam kapasite kullanımı havuzun boyutunu aşamaz. 

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasitesi havuzu oluşturmak](azure-netapp-files-set-up-capacity-pool.md)   
Bir alt ağ, Azure için NetApp dosyaları temsilci gerekir.  
[Temsilci bir alt ağ Azure NetApp dosyaları](azure-netapp-files-delegate-subnet.md)


## <a name="steps"></a>Adımlar 
1.  Kapasite Havuzlarını Yönet dikey penceresinden **Birimler** dikey penceresine tıklayın. 
2.  Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Yeni Birim penceresi görüntülenir.

3.  Yeni Birim penceresinde **Oluştur**’a tıklayın ve aşağıdaki alanlar için bilgileri girin:   
    * **Ad**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Ad, kaynak grubu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır.  Tüm alfasayısal karakterler kullanılabilir.

    * **Dosya yolu**  
        Yeni birimin dışarı aktarma yolunu oluşturmak için kullanılacak dosya yolunu belirtin. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.   
     
        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır.  

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  

        Belirttiğiniz sanal ağ, Azure için NetApp dosyaları temsilci bir alt ağ olması gerekir. Azure NetApp dosyaları hizmeti, yalnızca aynı sanal ağda veya bir sanal ağ aynı bölgedeyse Vnet eşlemesi aracılığıyla toplu olarak erişilebilir. Express Route üzerinden şirket içi ağınızdan birimi de erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağ belirtin.  
        Belirttiğiniz alt ağ, Azure için NetApp dosyaları temsilci gerekir. 
        
        Bir alt ağ temsilcisi yok, tıklayabilirsiniz **Yeni Oluştur** sayfasında bir birim oluşturun. Ardından alt ağ oluşturma sayfası alt ağ bilgileri belirtin ve seçin **Microsoft.NetApp/volumes** alt ağın Azure NetApp dosyaları için temsilci. Her bir sanal ağda Azure NetApp dosyaları için yalnızca bir alt ağı atanabilir unutmayın.   
 
        ![Yeni birim](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)


4.  **Tamam** düğmesine tıklayın. 
 
Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar  
* [Birim için dışarı aktarma ilkesini yapılandırma (isteğe bağlı)](azure-netapp-files-configure-export-policy.md)
* [Azure Hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)

