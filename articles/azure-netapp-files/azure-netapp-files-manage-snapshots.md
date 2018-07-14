---
title: Anlık görüntüleri Azure NetApp dosyalarını kullanarak yönetme | Microsoft Docs
description: Bir birim veya geri yükleme için bir isteğe bağlı anlık görüntü, Azure NetApp dosyalarını kullanarak yeni bir birime anlık görüntüden oluşturmayı açıklar.
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
ms.topic: how-to-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 48cb88b9815ba723d93c18caf63f33b50eea850c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009199"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Azure NetApp dosyalarını kullanarak anlık görüntüleri yönetme
Bir birim için bir isteğe bağlı anlık görüntü oluşturmak veya yeni bir birim için bir anlık görüntüden geri yükleme için Azure NetApp dosyaları kullanabilirsiniz.

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Bir birim için bir isteğe bağlı anlık görüntü oluşturma
Yalnızca isteğe bağlı olarak, anlık görüntü oluşturabilirsiniz.  Anlık görüntü ilkeleri şu anda desteklenmemektedir.  
1.  Birim yönetme dikey penceresinden tıklayın **anlık görüntüleri**, ardından **+ Ekle anlık görüntü** bir birim için bir isteğe bağlı anlık görüntüsünü oluşturmak için.

2.  Yeni anlık görüntü penceresinde, oluşturmakta olduğunuz yeni anlık görüntü için bir ad sağlayın.   

3. **Tamam**’a tıklayın. 


## <a name="restore-a-snapshot-to-a-new-volume"></a>Yeni bir birim için bir anlık görüntü geri yükleme
Şu anda yalnızca yeni bir birim için bir anlık görüntü geri yükleyebilirsiniz. 
1. Git **ekran görüntülerini Yönet** dikey penceresinde anlık görüntü listesini görüntülemek için toplu dikey penceresinden. 
2. Geri yüklemek için bir anlık görüntü seçin.  
3. Anlık görüntü adını sağ tıklatıp **yeni bir birime geri** menü seçeneğinden.  

    ![Yeni birim için anlık görüntü geri yükleme](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Yeni birim penceresinde yeni birimi için bilgileri sağlayın:  
    * **Adı**   
        Oluşturmakta olduğunuz birim adını belirtin.  
        
        Adı bir kaynak grubu içinde benzersiz olmalıdır. En az 3 karakter uzunluğunda olmalıdır.  Bunu, herhangi bir alfasayısal karakter kullanabilirsiniz.

    * **Dosya yolu**     
        Yeni birim dışarı aktarma yolu oluşturmak için kullanılan dosya yolu belirtin. Dışarı aktarma yoluna bağlayın ve birime erişmek için kullanılır.   
        
        Bir bağlama hedefi NFS hizmeti IP adresi uç noktadır. Otomatik olarak oluşturulur.   
        
        Dosya yolu adı harf, rakam ve kısa çizgi içerebilir ("-") yalnızca. 16 ve 40 karakter uzunluğunda olmalıdır. 

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alan yeni bir birim oluşturma doğrultusunda kullanabilirsiniz seçilen kapasitesi havuzu kullanılmayan alanı miktarını gösterir. Yeni birim boyutu kullanılabilir kota aşmamalıdır.

    *   **Sanal ağ**  
        İçinden birime erişmek istediğiniz Azure sanal ağı (Vnet) belirtin. 
        
        Belirttiğiniz sanal ağ, Azure NetApp dosyaları yapılandırılmış olması gerekir. Azure NetApp dosyaları hizmet birimi ile aynı konumda olan bir Vnet erişilebilir.  

    ![Geri yüklenen yeni birim](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
    
5. **Tamam**’a tıklayın.   
    Anlık görüntü geri yüklendikten yeni birim birimleri dikey penceresinde görüntülenir.

