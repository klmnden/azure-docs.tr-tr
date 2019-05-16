---
title: Hızlı Başlangıç - Azure NetApp dosyaları yedekleme ve bir NFS birimi oluşturun | Microsoft Docs
description: Hızlı Başlangıç - hızlı bir şekilde Azure NetApp dosyalarını ayarlayın ve birim oluşturmak nasıl açıklar.
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
ms.date: 4/16/2019
ms.author: b-juche
ms.openlocfilehash: 2bcd8163cb3c6071812d4d247b5b333edcfc89e5
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523020"
---
# <a name="quickstart-set-up-azure-netapp-files-and-create-an-nfs-volume"></a>Hızlı Başlangıç: Azure NetApp Files’ı ayarlama ve NFS birimi oluşturma 

Bu makalede Azure NetApp dosyaları hızlı bir şekilde ve birim oluşturma gösterilmektedir. 

Bu hızlı başlangıçta, aşağıdaki öğeleri ayarlar:

- Azure NetApp dosya ve NetApp kaynak Sağlayıcısı kaydı
- NetApp hesabı
- Kapasitesi havuzu
- Azure NetApp dosyaları için bir NFS birimi

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce 

> [!IMPORTANT] 
> Azure NetApp dosyaları hizmetine erişim verilmesi gerekir.  Hizmet erişim istemek için bkz: [Azure NetApp dosyaları waitlist gönderim sayfasını](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u).  Devam etmeden NetApp dosyaları Azure ekibinden bir resmi onay e-posta için beklemeniz gerekir. 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Azure NetApp dosya ve NetApp kaynak sağlayıcısı için kaydolun

1. Azure portalında, sağ üst köşesinde Azure Cloud Shell simgesine tıklayın.

    ![Azure Cloud Shell simgesi](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell-window.png)

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde ve isterseniz, kaynak grubunu silebilirsiniz. Bir kaynak grubu silme işlemi geri alınamaz bir eylemdir.  

> [!IMPORTANT]
> Kaynak grupları içindeki tüm kaynaklar kalıcı olarak silinir ve geri alınamaz. 

1. Azure portalında arama kutusuna **Azure NetApp dosyaları** seçip **Azure NetApp dosyaları** görünen listeden.

2. Abonelikler listesinde, silmek istediğiniz kaynak grubunu (myRG1) tıklayın. 

    ![Kaynak gruplarına gidin](../media/azure-netapp-files/azure-netapp-files-azure-navigate-to-resource-groups.png)


3. Kaynak grubu sayfasındaki tıklayın **kaynak grubunu Sil**.

    ![Kaynak grubunu sil](../media/azure-netapp-files/azure-netapp-files-azure-delete-resource-group.png) 

    Bir pencere açılır ve kaynak grubuyla birlikte silinecek kaynaklar hakkında bir uyarı görüntüler.

4. Kaynak grubunu ve tüm kaynaklarla birlikte kalıcı olarak silmek ve ardından istediğinizi onaylamak için kaynak grubu (myRG1) adını **Sil**.

    ![Kaynak grubunu sil](../media/azure-netapp-files/azure-netapp-files-azure-confirm-resource-group-deletion.png ) 

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Azure NetApp dosyaları aracılığıyla birimleri yönetme](azure-netapp-files-manage-volumes.md)  
