---
title: DNS kayıt kümeleri ve Azure DNS kayıtlarını yönetme
description: Azure DNS, DNS kayıt kümeleri ve kayıtları etki alanınızı barındırmaya olduğunda yönetme olanağı sağlar.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 10/6/2018
ms.author: victorh
ms.openlocfilehash: 891adfacde6e46b1d8fe8e2f6b5fb39c90ce27a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61296479"
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a>DNS kayıtlarını yönetme ve Azure portalını kullanarak kayıt kümeleri

Bu makalede, kayıt kümeleri ve kayıtları DNS bölgenizin Azure portalını kullanarak yönetme işlemini göstermektedir.

Tek tek DNS kayıtlarını DNS kayıt kümeleri arasındaki farkı anlamak önemlidir. Kayıt kümesi aynı türdeyse ve aynı ada sahip bir bölgedeki kayıtları koleksiyonudur. Daha fazla bilgi için [oluşturma DNS kayıt kümeleri ve Azure portalını kullanarak kayıtları](dns-getstarted-create-recordset-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Yeni kayıt kümesi ve kaydı oluşturma

Azure portalında bir kayıt oluşturmak için bkz: [Azure portalını kullanarak oluşturma DNS kayıtlarını](dns-getstarted-create-recordset-portal.md).

## <a name="view-a-record-set"></a>Kayıt kümesi görüntüleyin

1. Azure portalında Git **DNS bölgesi** dikey penceresi.
2. Kayıt kümesi için arama yapın ve seçin. Bu, kayıt kümesinin özelliklerini açar.

    ![Kayıt kümesi Ara](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-to-a-record-set"></a>Yeni bir kayıt için bir kayıt kümesi Ekle

En fazla 20 kayıt herhangi bir kayıt kümesine ekleyebilirsiniz. Kayıt kümesi iki özdeş kaydı içeremez. Boş bir kayıt kümeleri (sıfır kayıtlarla) oluşturulabilir, ancak Azure DNS ad sunucularında görünmez. Bir kayıt türünün CNAME kaydı kümeleri en fazla içerebilir.

1. Üzerinde **kayıt kümesinin özellikleri** dikey DNS bölgeniz için bir kayıt olarak eklemek istediğiniz kayıt kümesi tıklayın.

    ![Kayıt kümesi seçin](./media/dns-operations-recordsets-portal/selectset500.png)

2. Kaydın alanları doldurarak özellikler kümesi belirtin.

    ![Bir kaydı ekleme](./media/dns-operations-recordsets-portal/addrecord500.png)

3. Tıklayın **Kaydet** ayarlarınızı kaydetmek için dikey pencerenin üst. Ardından dikey penceresini kapatın.
4. Üst köşede bulunan kayıt kaydediyor görürsünüz.

    ![Kayıt kümesi kaydediliyor](./media/dns-operations-recordsets-portal/saving150.png)

Kayıt kaydedildi sonra değerleri **DNS bölgesi** dikey penceresinde yeni kayıt yansıtacaktır.

## <a name="update-a-record"></a>Bir kaydı güncelleştirme

Var olan bir kayıt kümesine kayıt güncelleştirdiğinizde güncelleştirebilirsiniz alanları çalıştığınız kayıt türü bağlıdır.

1. Üzerinde **kayıt kümesinin özellikleri** dikey penceresinde kayıt kümenizi, kayıt arayın.
2. Kaydı değiştirin. Bir kaydı değiştirdiğinizde, kayıt için kullanılabilen ayarları değiştirebilirsiniz. Aşağıdaki örnekte, **IP adresi** alanı seçilmiştir ve değiştirilen sürecinde IP adresidir.

    ![Bir kaydı değiştirme](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Tıklayın **Kaydet** ayarlarınızı kaydetmek için dikey pencerenin üst. Sağ üst köşedeki kaydı kaydedilen bir bildirim görürsünüz.

    ![Kayıt kümesi kaydedildi](./media/dns-operations-recordsets-portal/saved150.png)

Kayıt kaydedildikten sonra kayıt için değerleri ayarlayın **DNS bölgesi** dikey penceresinde, güncelleştirilen kaydı yansıtacaktır.

## <a name="remove-a-record-from-a-record-set"></a>Kayıt kümesinden bir kaydı Kaldır

Kayıt kümesindeki kayıtları kaldırmak için Azure portalını kullanabilirsiniz. Kayıt kümesindeki en son kaydını kaldırma kayıt kümesi silmeyeceğini unutmayın.

1. Üzerinde **kayıt kümesinin özellikleri** dikey penceresinde kayıt kümenizi, kayıt arayın.
2. Kaldırmak istediğiniz kaydı'nı tıklatın. Ardından **Kaldır**.

    ![Bir kaydı Kaldır](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Tıklayın **Kaydet** ayarlarınızı kaydetmek için dikey pencerenin üst.
4. Kaydı kaldırıldıktan sonra kayıt üzerinde değerlerini **DNS bölgesi** dikey penceresinde temizleme yansıtacaktır.

## <a name="delete"></a>Bir kayıt kümesini Sil

1. Üzerinde **kayıt kümesinin özellikleri** dikey penceresinde kayıt kümenizi tıklayın **Sil**.

    ![Bir kayıt kümesini Sil](./media/dns-operations-recordsets-portal/deleterecordset500.PNG)

2. Kayıt kümesi silmek istiyorsanız soran bir ileti görüntülenir.
3. Adı silin ve ardından istediğiniz kayıt kümesi eşleştiğini doğrulayın **Evet**.
4. Üzerinde **DNS bölgesi** dikey penceresinde, kayıt kümesi artık görünür olduğundan emin olun.

## <a name="work-with-ns-and-soa-records"></a>NS ve SOA kayıtlarının ile çalışma

Otomatik olarak oluşturulan NS ve SOA kayıtlarının diğer kayıt türleri farklı şekilde yönetilir.

### <a name="modify-soa-records"></a>SOA kayıtları değiştirme

Ekleyemez veya otomatik olarak oluşturulan SOA kaydı bölgenin tepesinde kümesi kayıtları kaldırmak (name = "\@"). Ancak, herhangi bir parametre içinde SOA kaydını ("ana" dışında) değiştirebilirsiniz ve TTL kaydı ayarlayın.

### <a name="modify-ns-records-at-the-zone-apex"></a>Bölgenin tepesindeki NS kayıtlarını değiştirme

NS kayıt bölge tepesinde kümesi her DNS bölge ile otomatik olarak oluşturulur. Bu bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Ek ad sunucuları bu NS kayıt için birden fazla DNS sağlayıcısı ile ortak barındırma etki alanlarını destekleyecek şekilde ayarlama, ekleyebilirsiniz. Bu kayıt kümesi için meta veriler ve TTL de değiştirebilirsiniz. Ancak, kaldırın veya önceden doldurulmuş Azure DNS ad sunucularını değiştirin.

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerli olduğunu unutmayın. (Alt bölgeleri temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümeleri, kısıtlama değiştirilebilir.

### <a name="delete-soa-or-ns-record-sets"></a>SOA veya NS kayıt kümelerini silin

SOA silemezsiniz ve NS kayıt kümeleri bölgenin tepesinde (ad = "\@") oluşturulan otomatik olarak bölge oluşturulduğunda. Bölge sildiğinizde otomatik olarak silinir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure DNS hakkında daha fazla bilgi için bkz. [Azure DNS'ye genel bakış](dns-overview.md).
* DNS otomatikleştirme hakkında daha fazla bilgi için bkz. [oluşturma DNS bölgeleri ve .NET SDK kullanarak kayıt kümelerini](dns-sdk.md).
* Ters DNS kayıtlarını hakkında daha fazla bilgi için bkz: [geriye doğru DNS ve Azure desteği'na genel bakış](dns-reverse-dns-overview.md).
* Azure DNS diğer ad kayıtlarını hakkında daha fazla bilgi için bkz. [Azure DNS diğer ad kayıtlarını genel bakış](dns-alias.md).
