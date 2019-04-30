---
title: Azure NetApp dosyalarını ve bir NFS birimi oluşturma | Microsoft Docs
description: Hızlı bir şekilde Azure NetApp dosyalarını ayarlayın ve birim oluşturma işlemini açıklamaktadır.
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
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: b-juche
ms.openlocfilehash: 5b9e85da29b3130d5183f577a7d83d164e217310
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63759835"
---
# <a name="set-up-azure-netapp-files-and-create-an-nfs-volume"></a>Azure NetApp dosyalarını ve bir NFS birimini oluşturma 

Bu makalede Azure NetApp dosyaları hızlı bir şekilde ve birim oluşturma gösterilmektedir. 

## <a name="before-you-begin"></a>Başlamadan önce 

Genel Önizleme programına ve Microsoft.NetApp kaynak sağlayıcısına erişmek için izin verilenler listesinde bir parçası olmanız gerekir. Genel Önizleme programına katılma hakkındaki ayrıntılar için bkz. [Azure NetApp Files Genel Önizleme kayıt sayfası](https://aka.ms/nfspublicpreview). 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Azure NetApp dosya ve NetApp kaynak sağlayıcısı için kaydolun

1. Azure portalında, sağ üst köşesinde Azure Cloud Shell simgesine tıklayın.

      ![Azure Cloud Shell simgesi](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Azure NetApp dosyaları için Güvenilenler listesinde aboneliği belirtin:
    
        az account set --subscription <subscriptionId>

3. Azure kaynak sağlayıcısını kaydedin: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    Kayıt işleminin tamamlanması biraz zaman alabilir.

## <a name="create-a-netapp-account"></a>NetApp hesabı oluşturma

1. Azure portalında arama kutusuna **Azure NetApp dosyaları** seçip **Azure NetApp dosyalar (Önizleme)** görünen listeden.

      ![Azure NetApp dosya seçin](../media/azure-netapp-files/azure-netapp-files-select-azure-netapp-files.png)

2. Yeni bir NetApp hesabı oluşturmak için **+ Ekle**'ye tıklayın.

     ![Yeni NetApp hesabı oluşturma](../media/azure-netapp-files/azure-netapp-files-create-new-netapp-account.png)

3. Yeni NetApp hesabı penceresinde aşağıdaki bilgileri sağlayın: 
   1. Girin **myaccount1** hesap adı için. 
   2. Aboneliğinizi seçin.
   3. Seçin **Yeni Oluştur** yeni kaynak grubu oluşturun. Girin **myRG1** için kaynak grubu adı. **Tamam** düğmesine tıklayın. 
   4. Hesap konumunuzu seçin.  

      ![Yeni NetApp hesabı penceresi](../media/azure-netapp-files/azure-netapp-files-new-account-window.png)  

      ![Kaynak grubu penceresi](../media/azure-netapp-files/azure-netapp-files-resource-group-window.png)

4. Tıklayın **Oluştur** yeni NetApp hesabınızı oluşturmak için.

## <a name="set-up-a-capacity-pool"></a>Kapasite havuzunu ayarlama

1. NetApp hesabınızı Azure NetApp dosya yönetimi dikey penceresinden seçin (**myaccount1**).

    ![NetApp hesabı seçin](../media/azure-netapp-files/azure-netapp-files-select-netapp-account.png)  

2. Azure NetApp dosyaları yönetim dikey NetApp hesabınızın **kapasitesi havuzu**.

    ![Kapasitesi havuzu tıklayın](../media/azure-netapp-files/azure-netapp-files-click-capacity-pools.png)  

3. Tıklayın **+ Ekle havuzları**. 

    ![Ekle havuzları'nı tıklatın](../media/azure-netapp-files/azure-netapp-files-click-add-pools.png)  

4. Kapasitesi havuzu için bilgileri sağlayın: 
    1. Girin **mypool1** havuzu adı.
    2. Seçin **Premium** hizmet düzeyi için. 
    3. Belirtin **4 (TiB)** havuz boyutunu olarak. 

5. **Tamam** düğmesine tıklayın.

## <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Azure için NetApp dosyaları bir NFS birimini oluşturma

1. Azure NetApp dosyaları yönetim dikey NetApp hesabınızın **birimleri**.

    ![Birimler'e tıklayın](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. **+ Birim ekle**’ye tıklayın.

    ![Ekle birimler'e tıklayın](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. Oluşturma bir birim penceresi, birim için bilgileri sağlayın: 
   1. Girin **myvol1** birim adı. 
   3. Kapasite havuzunuzu seçin (**mypool1**).
   4. Kota için varsayılan değeri kullanın. 
   5. Sanal ağı altında **Yeni Oluştur** yeni bir Azure sanal ağı (Vnet) oluşturmak için.  Ardından aşağıdaki bilgileri doldurun:
       * Girin **myvnet1** Vnet adı.
       * Örneğin, 10.7.0.0/16 ayarınız için bir adres alanını belirtin
       * Girin **myANFsubnet** alt ağ adı olarak.
       * Örneğin, 10.7.0.0/24 alt ağ adres aralığı belirtin. Diğer kaynaklar ile ayrılmış alt ağında paylaşamaz unutmayın.
       * Seçin **Microsoft.NetApp/volumes** alt temsilci.
       * Tıklayın **Tamam** Vnet oluşturmak için.
   6. Yeni oluşturulan sanal ağ alt ağında seçin (**myvnet1**) temsilci alt ağ.

      ![Bir birim penceresi oluştur](../media/azure-netapp-files/azure-netapp-files-create-volume-window.png)  

      ![Sanal ağ penceresi oluştur](../media/azure-netapp-files/azure-netapp-files-create-virtual-network-window.png)  

4. Tıklayın **Protokolü**, ardından **NFS** protokol türü için toplu olarak.   

    Girin **myfilepath1** birimi için dışarı aktarma yolu oluşturmak için kullanılan dosya yolu. 

    ![Hızlı Başlangıç için NFS Protokolü belirtin](../media/azure-netapp-files/azure-netapp-files-quickstart-protocol-nfs.png)

5. **Gözden geçir ve oluştur**’a tıklayın.

    ![Gözden geçirin ve penceresi oluştur](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Birim bilgileri gözden geçirin ve ardından tıklayın **Oluştur**.  
    Oluşturulan birim birimleri dikey penceresinde görünür.

    ![Oluşturulan birim](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="next-steps"></a>Sonraki adımlar  

* [NetApp dosyaları Azure depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
* [Azure NetApp dosyaları aracılığıyla birimleri yönetme](azure-netapp-files-manage-volumes.md) 
