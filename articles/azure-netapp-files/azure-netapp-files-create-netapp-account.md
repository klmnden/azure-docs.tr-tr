---
title: Azure NetApp Files'a erişim için NetApp hesabı oluşturma | Microsoft Docs
description: Azure NetApp Files'a erişme ve bir kapasite havuzu ayarlayıp birim oluşturabilmek için NetApp hesabı oluşturma işlemleri açıklanır.
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
ms.openlocfilehash: ad8cc550ce69e4dc4c19a569718fa873a65b3620
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010353"
---
# <a name="create-a-netapp-account"></a>NetApp hesabı oluşturma
NetApp hesabı oluşturmak, kapasite havuzu ayarlamanıza ve ardından birim oluşturmanıza olanak tanır. Yeni NetApp hesabını oluşturmak için Azure NetApp Files dikey penceresini kullanırsınız.

## <a name="before-you-begin"></a>Başlamadan önce
Microsoft.NetApp Azure Kaynak Sağlayıcısı'na erişim için izin verilenler listesinde yer almanız ve Azure NetApp Files hizmetini kullanmak için yapılandırılmış olmanız gerekir.  

[Azure NetApp Files Genel Önizleme kayıt sayfası](https://aka.ms/nfspublicpreview). 

## <a name="steps"></a>Adımlar 

1. Önizleme davetinizde önizleme Azure portalı URL'sini bulun ve portalda oturum açın. 
2.  Aşağıdaki yöntemlerden birini kullanarak Azure NetApp Files dikey penceresine ulaşın:  
  * Azure portalı arama kutusunda **Azure NetApp Files** için arama yapın.  
  * Gezintide **Tüm hizmetler**'e tıklayın ve ardından Azure NetApp Files için filtreleyin.  

  Azure NetApp Files dikey penceresinin yanındaki yıldız simgesine tıklayarak bunu "sık kullanılanlara" ekleyebilirsiniz. 

3. Yeni bir NetApp hesabı oluşturmak için **+ Ekle**'ye tıklayın.  
  Yeni NetApp hesabı penceresi görüntülenir.  

4. NetApp hesabınızla ilgili şu bilgileri sağlayın: 
  * **Hesap adı**  
    Abonelik için benzersiz bir ad belirtin.
  *  **Abonelik**  
    Mevcut abonelikleriniz arasından bir abonelik seçin.
  * **Kaynak grubu**   
    Mevcut Kaynak Grubunu kullanın ya da yeni bir tane oluşturun.
  * **Konum**  
    Hesabın ve alt kaynaklarının içinde yer almasını istediğiniz bölgeyi seçin.  
    Şu anda, Azure NetApp Files hizmeti yalnızca ABD Doğu bölgesinde desteklenir.  

    ![Yeni NetApp hesabı](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. **Oluştur**’a tıklayın.     
  Oluşturduğunuz NetApp hesabı artık Azure NetApp Files dikey penceresinde gösterilir. 

## <a name="next-steps"></a>Sonraki adımlar  

1. [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
2. [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
3. [Birim için dışarı aktarma ilkesini yapılandırma (isteğe bağlı)](azure-netapp-files-configure-export-policy.md)
