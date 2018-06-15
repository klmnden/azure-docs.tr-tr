---
title: StorSimple sanal dizinin paylaşımlarını yönetmek | Microsoft Docs
description: StorSimple cihaz Yöneticisi'ni açıklar ve bunu, StorSimple sanal dizisindeki paylaşımlarını yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: e5c62689de36baa175001f5f4f70d87568876ef0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23876274"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-shares-on-the-storsimple-virtual-array"></a>StorSimple sanal dizisindeki paylaşımlarını yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bu öğretici StorSimple cihaz Yöneticisi hizmeti oluşturmak ve StorSimple sanal dizinizi paylaşımlarında yönetmek için nasıl kullanılacağını açıklar.

StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzün bir tek web arabiriminden yönetmenizi sağlayan Azure portalında bir uzantısıdır. Paylaşımları ve birimler yönetmeye ek olarak, StorSimple cihaz Yöneticisi hizmeti görüntülemek ve cihazları yönetmek, uyarıları görüntülemek, yedekleme ilkelerini yönetmek ve yedekleme kataloğu yönetmek için kullanabilirsiniz.

## <a name="share-types"></a>Paylaşım türleri

StorSimple paylaşımları olabilir:

* **Yerel olarak sabitlenmiş**: bu paylaşımlar verileri her zaman dizi kalmasını ve buluta dağıtılmamasını.
* **Katmanlı**: bu paylaşımlar verileri buluta sığdırmaya. Katmanlı bir paylaşımı oluşturduğunuzda, alanın % 10'yaklaşık yerel katmanında sağlanır ve alan % 90'ını bulutta sağlanır. Örneğin, 1 TB paylaşımı sağlanan, 100 GB yerel alanında bulunması ve 900 GB bulutta kullanılabilir zaman veri katmanları. Bu, sırayla cihazda yerel alana çalıştırırsanız, (% 10 yerel katmanda gereken kullanılamaz çünkü), bir katmanlı paylaşımı sağlayamazsınız olduğunu gösterir.

### <a name="provisioned-capacity"></a>Sağlanan kapasite

Her bir paylaşım türü için maksimum sağlanan kapasite için aşağıdaki tabloya bakın.

| **Sınır tanımlayıcısı** | **Sınırı** |
| --- | --- |
| Katmanlı bir paylaşımı en küçük boyut |500 GB |
| Katmanlı bir paylaşımı en büyük boyutu |20 TB |
| Yerel olarak sabitlenmiş bir paylaşımı en küçük boyut |50 GB |
| Yerel olarak sabitlenmiş bir paylaşımı en büyük boyutu |2 TB |

## <a name="the-shares-blade"></a>Paylaşımlar dikey penceresi

**Paylaşımları** StorSimple hizmeti Özet dikey menüsünde belirli bir StorSimple dizi depolama paylaşımları listesini görüntüler ve bunları yönetmenize olanak sağlar.

![Paylaşımlar dikey penceresi](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

Bir paylaşım öznitelikleri bir dizi oluşur:

* **Paylaşım adı** – benzersiz olmalı ve Paylaşım tanımlamanıza yardımcı olan açıklayıcı bir ad.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir paylaşım çevrimdışıysa, paylaşımın kullanıcılar buna erişebilir olmayacaktır.
* **Tür** – paylaşım olup olmadığını belirten **katmanlı** (varsayılan) veya **yerel olarak sabitlenmiş**.
* **Kapasite** – toplam paylaşımında depolanan veri miktarını karşılaştırıldığında kullanılan veri miktarını belirtir.
* **Açıklama** – paylaşım açıklamak yardımcı olan isteğe bağlı bir ayar.
* **İzinleri** -Windows Explorer ile yönetilebilir paylaşımı için NTFS izinleri.
* **Yedekleme** – StorSimple sanal dizisi tüm paylaşımlar yedekleme için otomatik olarak etkinleştirilmiş durumda.

![Paylaşımlar ayrıntıları](./media/storsimple-virtual-array-manage-shares/share-details.png)

Aşağıdaki görevleri gerçekleştirmek için Bu öğreticide yönergeleri kullanın:

* Paylaşım Ekle
* Bir paylaşımı değiştirme
* Bir paylaşım çevrimdışı duruma getirin
* Bir paylaşımı silme

## <a name="add-a-share"></a>Paylaşım Ekle

1. StorSimple hizmeti Özet dikey penceresinden tıklayın **+ Ekle paylaşımı** komut çubuğundan. Bu açılır **Ekle paylaşımı** dikey.

    ![Paylaşım Ekle](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. İçinde **Ekle paylaşımı** dikey penceresinde aşağıdakileri yapın:
   
    1. İçinde **paylaşım adı** alanında, paylaşımınıza için benzersiz bir ad girin. Adı 3 ile 127 karakter içeren bir dize olmalıdır.

    2. İsteğe bağlı bir **açıklama** paylaşımı için. Açıklama, paylaşım sahiplerini tanımlamaya yardımcı olur.

    3. İçinde **türü** açılır listesinde, oluşturulup oluşturulmayacağını belirtin bir **katmanlı** veya **yerel olarak sabitlenmiş** paylaşın. Yerel GARANTİLERİN, düşük gecikme ve yüksek performansın gerektiği iş yükleri için seçin **Paylaşımı'yerel olarak sabitlenmiş**. Diğer tüm veriler için seçin **katmanlı** paylaşın.

    4. İçinde **kapasite** alanında, paylaşımı boyutunu belirtin. Katmanlı bir paylaşımı, 500 GB ile 20 TB arasında olmalıdır ve yerel olarak sabitlenmiş bir paylaşımı, 50 GB ile 2 TB arasında olmalıdır.

    5. İçinde **kümesine varsayılan tam izinleri** alanında, kullanıcı ya da bu paylaşıma erişim grubu için izinleri atayın. Kullanıcı veya kullanıcı grubunun adını belirtin  _john@contoso.com_  biçimi. Bu paylaşımlar erişmek yönetici ayrıcalıkları izin vermek için bir kullanıcı grubu (yerine tek bir kullanıcı) kullanmanızı öneririz. İzinlerini burada atadıktan sonra bu izinleri değiştirmek için dosya Gezgini'ni kullanabilirsiniz.
3. Paylaşımınıza yapılandırma tamamladığınızda tıklatın **oluşturma**. Belirtilen ayarlarla bir paylaşımı oluşturulur ve bir bildirim görürsünüz. Varsayılan olarak, yedekleme paylaşım için etkin.
4. Paylaşım başarıyla oluşturulduğunu doğrulamak için şu adrese gidin **paylaşımları** dikey. Listelenen paylaşımına görmeniz gerekir.
   
    ![Paylaşımı oluşturma başarılı](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Bir paylaşımı değiştirme

Paylaşımın açıklaması değiştirmeniz gerektiğinde bir paylaşımı değiştirin. Paylaşımı oluşturulduktan sonra başka bir paylaşım özellikleri değiştirilebilir.

#### <a name="to-modify-a-share"></a>Bir paylaşım değiştirmek için

1. Gelen **paylaşımları** değiştirmenizi istiyor paylaşımın bulunduğu sanal dizinin StorSimple hizmeti Özet dikey ayarı seçin.
2. **Seçin** geçerli açıklamasını görüntülemek ve değiştirmek için paylaşımı.
3. Tıklayarak yaptığınız değişiklikleri kaydetmek **kaydetmek** komut çubuğu. Belirtilen ayarlarınızı uygulanır ve bir bildirim görürsünüz.
   
    ![ Paylaşım Düzenle](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Bir paylaşım çevrimdışı duruma getirin

Değiştirmek veya silmek planlama yaparken bir paylaşımı çevrimdışına gerekebilir. Bir paylaşım çevrimdışı olduğunda okuma-yazma erişimi için kullanılabilir değildir. Cihaz yanı sıra konak paylaşımı çevrimdışı olması gerekir.

#### <a name="to-take-a-share-offline"></a>Bir paylaşım çevrimdışına almak için

1. Söz konusu paylaşımı çevrimdışı duruma getirmeden önce kullanımda olmadığından emin olun.
2. Paylaşım, aşağıdaki adımları gerçekleştirerek dizi çevrimdışı gerçekleştirin:
   
    1. Gelen **paylaşımları** StorSimple hizmeti Özet dikey ayarı, çevrimdışı duruma getirmenizi istiyor paylaşımın bulunduğu sanal dizinin seçin.

    2. **Seçin** tıklatın ve Paylaşım **...**  (Alternatif olarak bu satırı sağ) ve bağlam menüsünden seçin **çevrimdışına**.
     
        ![Çevrimdışı paylaşımı](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. Bilgileri gözden **çevrimdışına** dikey ve işlemin onaylamış olursunuz. Tıklatın **çevrimdışına** paylaşım çevrimdışı duruma getirmek için. İşlem devam ettiğinden, bir bildirim görürsünüz.

    4. Paylaşım başarıyla çevrimdışı duruma olduğunu doğrulamak için Git **paylaşımları** dikey. Paylaşım durumu çevrimdışı olarak görmeniz gerekir.

## <a name="delete-a-share"></a>Bir paylaşımı silme

> [!IMPORTANT]
> Yalnızca çevrimdışı ise, bir paylaşım silebilirsiniz.


Bir paylaşımı silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-share"></a>Bir paylaşımı silmek için

1. Gelen **paylaşımları** StorSimple hizmeti Özet dikey ayarı, silmek istediğiniz paylaşımı bulunduğu sanal dizinin seçin.
2. **Seçin** tıklatın ve Paylaşım **...**  (Alternatif olarak bu satırı sağ) ve bağlam menüsünden seçin **silmek**.
   
    ![Paylaşımı silin](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Silmek istediğiniz paylaşımı durumunu denetleyin. Silmek istediğiniz paylaşımı çevrimdışı durumda değilse, onu önce çevrimdışına. Adımları [bir paylaşımı çevrimdışına](#take-a-share-offline).
4. İçinde onaylamanız istendiğinde **silmek** dikey penceresinde, onay kabul etmek ve tıklatın **silmek**. Paylaşım şimdi silinecek ve **paylaşımları** dikey paylaşımları Sanal dizi güncelleştirilmiş listesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [bir StorSimple paylaşımı kopyalama](storsimple-virtual-array-clone.md).

