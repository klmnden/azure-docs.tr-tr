---
title: Anlık görüntüleri Azure NetApp dosyalarını kullanarak yönetme | Microsoft Docs
description: Bir birim veya geri yükleme için anlık görüntüleri, Azure NetApp dosyalarını kullanarak yeni bir birime anlık görüntüden oluşturmayı açıklar.
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
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 01387d0c219c86f33762b9c3fbf9f81cf04b4455
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58880823"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Azure NetApp dosyalarını kullanarak anlık görüntüleri yönetme

Bir birim için bir isteğe bağlı anlık görüntü oluşturmak veya yeni bir birim için bir anlık görüntüden geri yükleme için Azure NetApp dosyaları kullanabilirsiniz.

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Bir birim için bir isteğe bağlı anlık görüntü oluşturma

Yalnızca isteğe bağlı olarak, anlık görüntü oluşturabilirsiniz. Anlık görüntü ilkeleri şu anda desteklenmemektedir.

1.  Birim dikey penceresinden tıklayın **anlık görüntüleri**.

    ![Anlık görüntü gidin](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Tıklayın **+ Ekle anlık görüntü** bir birim için bir isteğe bağlı anlık görüntüsünü oluşturmak için.

    ![Anlık görüntü ekle](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Yeni anlık görüntü penceresinde, oluşturmakta olduğunuz yeni anlık görüntü için bir ad sağlayın.   

    ![Yeni anlık görüntü](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. **Tamam** düğmesine tıklayın. 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Yeni bir birim için bir anlık görüntü geri yükleme

Şu anda yalnızca yeni bir birim için bir anlık görüntü geri yükleyebilirsiniz. 
1. Git **ekran görüntülerini Yönet** dikey penceresinde anlık görüntü listesini görüntülemek için toplu dikey penceresinden. 
2. Geri yüklemek için bir anlık görüntü seçin.  
3. Anlık görüntü adını sağ tıklatıp **yeni bir birime geri** menü seçeneğinden.  

    ![Yeni birim için anlık görüntü geri yükleme](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Yeni birim penceresinde yeni birimi için bilgileri sağlayın:  
    * **Ad**   
        Oluşturmakta olduğunuz birim için ad belirtin.  
        
        Ad, kaynak grubu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır.  Tüm alfasayısal karakterler kullanılabilir.

    * **Dosya yolu**     
        Yeni birimin dışarı aktarma yolunu oluşturmak için kullanılacak dosya yolunu belirtin. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.   
        
        Bağlama hedefi, NFS hizmeti IP adresinin uç noktasıdır. Otomatik olarak oluşturulur.   
        
        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır. 

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.

    *   **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  
        Belirttiğiniz sanal ağ, Azure için NetApp dosyaları temsilci bir alt ağ olması gerekir. Yalnızca aynı sanal ağda veya bir sanal ağ aynı bölgedeyse Vnet eşlemesi aracılığıyla birimi olarak Azure NetApp dosyalara erişebilirsiniz. Express Route üzerinden şirket içi ağınızdan birim erişebilirsiniz. 

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağ belirtin.  
        Belirttiğiniz alt ağ Azure NetApp dosyaları hizmetine temsilci gerekir. Seçerek yeni bir alt ağ oluşturabilirsiniz **Yeni Oluştur** alt ağ alanı altında.  
   <!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
   -->

5. **Tamam** düğmesine tıklayın.   
    Anlık görüntü geri yüklendikten yeni birim birimleri dikey penceresinde görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

[NetApp dosyaları Azure depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)