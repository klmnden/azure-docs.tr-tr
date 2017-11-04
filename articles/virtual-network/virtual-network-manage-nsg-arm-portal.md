---
title: "Azure portalını kullanarak Nsg'ler yönetme | Microsoft Docs"
description: "Azure Portalı'nı kullanarak var olan Nsg'ler yönetmeyi öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: e9bcf8a893ff209337f6a5763b631a22f8514e20
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-nsgs-using-the-portal"></a>Nsg'ler Portalı'nı kullanarak yönetme

> [!div class="op_single_selector"]
> * [Portal](virtual-network-manage-nsg-arm-portal.md)
> * [PowerShell](virtual-network-manage-nsg-arm-ps.md)
> * [Azure CLI](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft önerir Resource Manager dağıtım modelini kullanarak yer almaktadır.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Bilgi alma
Varolan Nsg'lerinizi görüntülemek için varolan bir NSG kuralları almak ve hangi kaynakların bir NSG için ilişkili olduğunu öğrenin.

### <a name="view-existing-nsgs"></a>Varolan Nsg'ler görüntüleyin

Bir Abonelikteki tüm mevcut Nsg'ler görüntülemek için aşağıdaki adımları tamamlayın:

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.

2. Tıklatın **Gözat >** > **ağ güvenlik grupları**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Nsg'ler içinde listesini kontrol edin **ağ güvenlik grupları** dikey.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a>Bir kaynak grubunda görünüm Nsg'ler

Nsg'ler içinde listesini görüntülemek için **RG NSG** kaynak grubunda, aşağıdaki adımları tamamlayın:

1. Tıklatın **kaynak grupları >** > **RG NSG** > **...** .

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Gösterildiği gibi kaynak listesinde NSG simgesini görüntüleme öğeleri arar **kaynakları** dikey penceresinde aşağıdaki.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a>Bir NSG için tüm kuralları listesinde

Adlı bir NSG kurallarını görüntülemek için **NSG ön uç**, aşağıdaki adımları tamamlayın:

1. Gelen **ağ güvenlik grupları** dikey penceresinde veya **kaynakları** , yukarıda gösterilen dikey tıklayın **NSG ön uç**.

2. İçinde **ayarları** sekmesini tıklatın, **gelen güvenlik kuralları**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. **Gelen güvenlik kuralları** dikey penceresi aşağıda gösterildiği gibi görüntülenir.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. İçinde **ayarları** sekmesini tıklatın, **giden güvenlik kurallarında** Giden kurallarını görmek için.

    > [!NOTE]
    > Varsayılan kuralları görüntülemek için **varsayılan kuralları** kuralları görüntüler dikey simgesine tıklayın.
    >

### <a name="view-nsgs-associations"></a>Nsg'ler ilişkilendirmelerini görüntülemek

Hangi kaynakları görüntülemek için **NSG ön uç** NSG ilişkilendirmek, aşağıdaki adımları tamamlayın:

1. Gelen **ağ güvenlik grupları** dikey penceresinde veya **kaynakları** , yukarıda gösterilen dikey tıklayın **NSG ön uç**.

2. İçinde **ayarları** sekmesini tıklatın, **alt ağlar** hangi alt ağların NSG'yi ilişkili görüntülemek için.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. İçinde **ayarları** sekmesini tıklatın, **ağ arabirimleri** NIC'ler NSG'yi ilişkili nelerdir görüntülemek için.

## <a name="manage-rules"></a>Kuralları yönetme
Varolan bir NSG kuralları ekleme, mevcut kuralları düzenlemek ve kuralları kaldırın.

### <a name="add-a-rule"></a>Kural ekleme
İzin verme kuralı eklemek için **gelen** bağlantı noktası trafiği **443** için herhangi bir makineden **NSG ön uç** NSG, aşağıdaki adımları tamamlayın:

1. Gelen **ağ güvenlik grupları** dikey penceresinde veya **kaynakları** , yukarıda gösterilen dikey tıklayın **NSG ön uç**.
2. İçinde **ayarları** sekmesini tıklatın, **gelen güvenlik kuralları**.
3. İçinde **gelen güvenlik kuralları** dikey penceresinde tıklatın **Ekle**. Ardından **gelen Güvenlik Kuralı Ekle** dikey penceresinde, aşağıda gösterildiği gibi değerlerini doldurun ve ardından **Tamam**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    Birkaç saniye sonra yeni kuralında fark **gelen güvenlik kuralları** dikey.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Bir kural değiştirme
Öğesinden gelen trafiğe izin vermek için yukarıda oluşturduğunuz kural değiştirmek için **Internet** yalnızca, aşağıdaki adımları tamamlayın:

1. Gelen **ağ güvenlik grupları** dikey penceresinde veya **kaynakları** , yukarıda gösterilen dikey tıklayın **NSG ön uç**.
2. İçinde **ayarları** sekmesinde, yukarıda oluşturduğunuz kural'ı tıklatın.
3. İçinde **izin https** dikey penceresinde, değişiklik **kaynak** aşağıda gösterildiği gibi özelliği ve ardından **kaydetmek**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Kural silme

Yukarıda oluşturduğunuz kural silmek için aşağıdaki adımları tamamlayın:

1. Gelen **ağ güvenlik grupları** dikey penceresinde veya **kaynakları** , yukarıda gösterilen dikey tıklayın **NSG ön uç**.
2. İçinde **ayarları** sekmesinde, yukarıda oluşturduğunuz kural'ı tıklatın.
3. İçinde **izin https** dikey penceresinde tıklatın **silmek**ve ardından **Evet**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>İlişkileri yönetme
Bir NSG'yi alt ağlara ve NIC ilişkilendirebilirsiniz. Bir NSG'yi ilişkili olduğu herhangi bir kaynaktan ilişkisini kaldırın.

### <a name="associate-an-nsg-to-a-nic"></a>Bir NSG'yi bir NIC ilişkilendirme
İlişkilendirilecek **NSG ön uç** NSG'yi **TestNICWeb1** NIC, aşağıdaki adımları tamamlayın:

1. Gelen **ağ güvenlik grupları** dikey penceresinde veya **kaynakları** , yukarıda gösterilen dikey tıklayın **NSG ön uç**.
2. İçinde **ayarları** sekmesini tıklatın, **ağ arabirimleri** > **ilişkilendirmek** > **TestNICWeb1**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Bir NSG'yi bir NIC gelen ilişkilendirmesini Kaldır

İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **TestNICWeb1** NIC, aşağıdaki adımları tamamlayın:

1. Azure portalından tıklatın **kaynak grupları >** > **RG NSG** > **...**   >  **TestNICWeb1**.

2. İçinde **TestNICWeb1** dikey penceresinde tıklatın **değiştirme güvenlik...**   >  **Hiçbiri**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> Bu dikey pencere, varolan bir NSG NIC'ye ilişkilendirmek için de kullanabilirsiniz.
>

### <a name="dissociate-an-nsg-from-a-subnet"></a>Bir NSG'yi bir alt ağdan ilişkilendirmesini Kaldır

İlişkilendirmesini kaldırmak **NSG ön uç** NSG gelen **ön uç** alt ağ, aşağıdaki adımları tamamlayın:

1. Azure portalından tıklatın **kaynak grupları >** > **RG NSG** > **...**   >  **TestVNet**.

2. İçinde **ayarları** dikey penceresinde tıklatın **alt ağlar** > **ön uç** > **ağ güvenlik grubu**  >  **Hiçbiri**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. İçinde **ön uç** dikey penceresinde tıklatın **kaydetmek**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Bir NSG'yi bir alt ağ ilişkilendirme

İlişkilendirilecek **NSG ön uç** NSG'yi **FronEnd** alt yeniden, aşağıdaki adımları tamamlayın:

1. Azure portalından tıklatın **kaynak grupları >** > **RG NSG** > **...**   >  **TestVNet**.
2. İçinde **ayarları** dikey penceresinde tıklatın **alt ağlar** > **ön uç** > **ağ güvenlik grubu** > **NSG ön uç**.
3. İçinde **ön uç** dikey penceresinde tıklatın **kaydetmek**.

> [!NOTE]
> Ayrıca bir NSG bir alt ağa thh NSG's ilişkilendirebilirsiniz **ayarları** dikey.
>

## <a name="delete-an-nsg"></a>Bir NSG'yi Sil
Herhangi bir kaynağa ilişkili olmayan bir NSG'yi yalnızca silebilirsiniz. Bir NSG'yi silmek için aşağıdaki adımları tamamlayın:.

1. Azure portalından tıklatın **kaynak grupları >** > **RG NSG** > **...**   >  **NSG ön uç**.
2. İçinde **ayarları** dikey penceresinde tıklatın **ağ arabirimleri**.
3. Listelenen NIC'ler varsa, NIC tıklayın ve adım 2'de izleyin [bir NSG'yi bir NIC gelen ilişkilendirmesini](#Dissociate-an-NSG-from-a-NIC).
4. 3. adım her NIC için yineleyin
5. İçinde **ayarları** dikey penceresinde tıklatın **alt ağlar**.
6. Listelenen alt ağı varsa, alt ağ'ı tıklatın ve 2 ve 3'te adımları izleyerek [bir NSG bir alt ağdan ilişkilendirmesini](#Dissociate-an-NSG-from-a-subnet).
7. Sola kaydırır **NSG ön uç** dikey penceresinde, ardından **silmek** > **Evet**.

    ![Azure portal - Nsg'ler](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Günlük kaydını etkinleştir](virtual-network-nsg-manage-log.md) Nsg'ler için.
