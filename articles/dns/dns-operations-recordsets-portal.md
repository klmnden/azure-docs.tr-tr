---
title: "DNS kayıt kümelerini ve Azure DNS kayıtlarını yönetme | Microsoft Docs"
description: "Azure DNS etki alanınızı barındırmaya olduğunda DNS kayıt kümelerini ve kayıtları yönetme yeteneği sağlar."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ed44a1-7bfe-454f-964e-922ad978264a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 001b80ccba43beab44f6a598f820df65a85a345f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a>DNS kayıtlarını yönetme ve Azure portalını kullanarak kayıt kümeleri

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Bu makalede Azure portalını kullanarak kayıt kümeleri ve kayıtlar için DNS bölgenizi yönetme gösterilmektedir.

DNS kayıt kümelerini ve DNS kayıtları tek tek arasındaki farkı anlamak önemlidir. Kayıt kümesi, aynı ada sahip ve aynı türde olan bir bölgedeki kayıtları koleksiyonudur. Daha fazla bilgi için bkz: [oluşturma DNS kayıt kümelerini ve kayıtları Azure portalını kullanarak](dns-getstarted-create-recordset-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Yeni kayıt kümesi ve kayıt oluşturma

Bir kayıt Azure portalında kümesi oluşturmak için bkz: [Azure portalını kullanarak oluşturmak DNS kayıtlarını](dns-getstarted-create-recordset-portal.md).

## <a name="view-a-record-set"></a>Bir kayıt kümesini görüntüleme

1. Azure portalında Git **DNS bölgesi** dikey.
2. Kayıt kümesi için arama yapın ve seçin. Bu kayıt kümesinin özelliklerini açar.

    ![Kayıt kümesi arayın](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-to-a-record-set"></a>Bir kayıt kümesine yeni bir kayıt ekleme

Herhangi bir kayıt kümesi en fazla 20 kayıt ekleyebilirsiniz. Kayıt kümesi iki özdeş kaydı içeremez. Boş kaydı kümeleri (sıfır kayıtlarla) oluşturulabilir ancak Azure DNS ad sunucuları üzerinde görünmez. CNAME türündeki kayıt kümesi en fazla bir kayıt içerebilir.

1. Üzerinde **kayıt kümesinin özelliklerini** DNS bölgenizi dikey penceresinde, bir kayıt eklemek istediğiniz kayıt kümesi'ı tıklatın.

    ![Kayıt kümesi seçin](./media/dns-operations-recordsets-portal/selectset500.png)

2. Alanları doldurarak kaydı özellikler kümesi belirtin.

    ![Kayıt ekleme](./media/dns-operations-recordsets-portal/addrecord500.png)

3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için dikey pencerenin üstündeki. Ardından dikey penceresini kapatın.
4. Köşede kaydı kaydediyor görürsünüz.

    ![Kayıt kümesi kaydediliyor](./media/dns-operations-recordsets-portal/saving150.png)

Kayıt kaydedildi sonra değerleri **DNS bölgesi** dikey yeni kayıttaki yansıtacak.

## <a name="update-a-record"></a>Bir kaydı güncelleştirme

Varolan bir kayıt kümesini kaydında güncelleştirdiğinizde güncelleştirebilirsiniz alanları çalıştığınız kayıt türü bağlıdır.

1. Üzerinde **kayıt kümesinin özelliklerini** dikey penceresinde, kayıt kümesi için arama kaydı için.
2. Kayıt değiştirin. Bir kayıt değiştirdiğinizde, kayıt için kullanılabilen ayarları değiştirebilirsiniz. Aşağıdaki örnekte, **IP adresi** alanı seçilmişse ve değiştirilen sürecinde IP adresidir.

    ![Bir kayıt değiştirme](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için dikey pencerenin üstündeki. Sağ üst köşedeki kaydı kaydedilmiş bildirim görürsünüz.

    ![Kayıt kümesi kaydedildi](./media/dns-operations-recordsets-portal/saved150.png)

Kayıt kaydedildikten sonra kayıt için değerleri ayarlamak **DNS bölgesi** dikey penceresinde, güncelleştirilmiş kayıt yansıtacak.

## <a name="remove-a-record-from-a-record-set"></a>Bir kayıt kümesinden bir kaydı kaldırma

Azure portalı, kayıt kümesindeki kayıtları kaldırmak için kullanabilirsiniz. Kayıt kümesindeki son kaydı kaldırma kayıt kümesi silmez unutmayın.

1. Üzerinde **kayıt kümesinin özelliklerini** dikey penceresinde, kayıt kümesi için arama kaydı için.
2. Kaldırmak istediğiniz kaydı tıklatın. Ardından **kaldırmak**.

    ![Bir kaydı kaldırma](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için dikey pencerenin üstündeki.
4. Kayıt kaldırıldıktan sonra kaydında değerlerini **DNS bölgesi** dikey kaldırma yansıtacak.

## <a name="delete"></a>Bir kayıt kümesini Sil

1. Üzerinde **kayıt kümesinin özelliklerini** kayıt kümeniz için dikey tıklayın **silmek**.

    ![Bir kayıt kümesini Sil](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Kayıt kümesi silmek istiyorsanız soran bir ileti görüntülenir.
3. Ad, silme ve ardından istediğiniz kayıt kümesi ile eşleştiğini doğrulayın **Evet**.
4. Üzerinde **DNS bölgesi** dikey penceresinde, kayıt kümesi artık görünür olduğunu doğrulayın.

## <a name="work-with-ns-and-soa-records"></a>NS ve SOA kayıtları ile çalışma

Otomatik olarak oluşturulan NS ve SOA kayıtları diğer kayıt türünden farklı şekilde yönetilir.

### <a name="modify-soa-records"></a>SOA kayıtları değiştirme

Ekleme veya kayıtları otomatik olarak oluşturulan SOA kayıt bölgenin tepesinde kümesi kaldırın (name = "@"). Ancak, SOA kaydı (dışında "ana bilgisayar") içinde parametrelerinden herhangi birini değiştirmek ve TTL kaydı ayarlayın.

### <a name="modify-ns-records-at-the-zone-apex"></a>Bölge tepesinde NS kayıtları değiştirme

NS kayıt bölgenin tepesinde kümesi her DNS bölge ile otomatik olarak oluşturulur. Bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Birden fazla DNS sağlayıcınız ile birlikte barındırma etki alanlarını destekleyecek şekilde bu NS kaydının sunucularına ayarlayın ek ad ekleyebilirsiniz. Bu kayıt kümesi için meta verileri ve TTL de değiştirebilirsiniz. Ancak, kaldırmak veya önceden doldurulmuş haldedir Azure DNS ad sunucuları değiştirin.

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerli olduğunu unutmayın. (Alt bölgelere temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümelerini kısıtlama değiştirilebilir.

### <a name="delete-soa-or-ns-record-sets"></a>SOA veya NS kayıt kümelerini Sil

SOA silemezsiniz ve NS kayıt kümeleri bölgenin tepesinde (ad = "@"), otomatik olarak oluşturulur bölge oluşturulduğunda. Bölge sildiğinizde, bunlar otomatik olarak silinir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure DNS hakkında daha fazla bilgi için bkz: [Azure DNS'ye genel bakış](dns-overview.md).
* DNS otomatikleştirme hakkında daha fazla bilgi için bkz: [oluşturma DNS bölgeleri ve .NET SDK kullanarak kayıt kümelerine](dns-sdk.md).
* Geriye doğru DNS kayıtları hakkında daha fazla bilgi için bkz: [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md).
