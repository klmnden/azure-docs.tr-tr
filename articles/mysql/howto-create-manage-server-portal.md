---
title: Oluşturma ve Azure veritabanı MySQL sunucusu için Azure portalını kullanarak yönetme
description: Bu makalede nasıl hızlı bir şekilde MySQL sunucusu için yeni bir Azure veritabanı oluşturabilir ve Azure Portalı'nı kullanarak sunucu yönetimi açıklanmaktadır.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 065eb708a1d80b0eac618bd9039a859db6ef1340
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265593"
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Oluşturma ve Azure veritabanı MySQL sunucusu için Azure portalını kullanarak yönetme
Bu konu, yeni bir Azure veritabanı MySQL sunucusu için hızlı bir şekilde nasıl oluşturabileceğinizi açıklar. Ayrıca Azure portalını kullanarak sunucuyu yönetme hakkında bilgi içerir. Sunucu ayrıntıları ve parola sıfırlama kaynaklarını ölçeklendirme ve sunucuyu silmek veritabanlarının görüntüleme sunucu yönetimi içerir.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL server "mydemoserver." adlı bir Azure veritabanı oluşturmak için aşağıdaki adımları izleyin

1. Tıklatın **kaynak oluşturma** düğme Azure portalında sol üst köşesinde bulunan.

2. Yeni sayfasında seçin **veritabanları**ve veritabanlarını sayfasında, ardından **Azure veritabanı için MySQL**.

    > MySQL sunucusu için bir Azure veritabanı tanımlanan bir dizi ile oluşturulan [işlem ve depolama](./concepts-pricing-tiers.md) kaynakları. Veritabanı, MySQL sunucusu için bir Azure veritabanında, bir Azure kaynak grubu içinde oluşturulur.

   ![Yeni-sunucusu oluştur](./media/howto-create-manage-server-portal/create-new-server.png)

3. Aşağıdaki bilgileri kullanarak Azure veritabanı için MySQL formu doldurun:

    | **Form Alanı** | **Alan Açıklaması** |
    |----------------|-----------------------|
    | *Sunucu adı* | mydemoserver (sunucu adı genel olarak benzersiz) |
    | *Abonelik* | mysubscription (seçin açılır menüden) |
    | *Kaynak grubu* | myresourcegroup (yeni bir kaynak grubu oluşturun veya var olanı kullanırsınız) |
    | *Kaynak seçme* | Boş (boş bir MySQL server oluşturun) |
    | *Sunucu yöneticisi oturum açma bilgileri* | myadmin (yönetici hesabı adını ayarlayın) |
    | *Parola* | Yönetici hesabı parolasını ayarlayın |
    | *Parolayı onayla* | yönetici hesabı parolasını onaylayın |
    | *Konum* | Güneydoğu Asya (Kuzey Avrupa ve Batı ABD arasında seçim) |
    | *Sürüm* | 5.7 (Azure veritabanı için MySQL sunucusu sürümü seçin) |

4. Tıklatın **fiyatlandırma katmanı** yeni sunucunuzu Hizmet katmanını ve performans düzeyini belirtmek için. Seçin **genel amaçlı** sekmesi. *Gen 4*, *2 sanal çekirdek*, *5 GB* ve *7 gün*; **İşlem Nesli**, **Sanal Çekirdek**, **Depolama** ve **Yedekleme Bekletme Dönemi** için varsayılan değerlerdir. Bu kaydırıcıları olduğu gibi bırakabilirsiniz. Coğrafi olarak yedekli depolamada sunucu yedeklerinizi etkinleştirmek için, **Fazladan Yedek Seçenekleri**’nde **Coğrafi Olarak Yedeklemeli**’yi seçin.

   ![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Sunucuyu sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer.

    > Seçin **panoya Sabitle** dağıtımlarınızı kolay izlenmesini izin vermek için seçeneği.

## <a name="update-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure veritabanını güncelleştirme
Yeni Sunucu sağlandıktan sonra kullanıcının yönetici parolasını sıfırlama ve vCore veya depolama değiştirerek sunucu yukarı veya aşağı Ölçeklendirmesi dahil olmak üzere varolan sunucusunu yapılandırmak için birkaç seçenek vardır.

### <a name="change-the-administrator-user-password"></a>Yönetici kullanıcı parolasını değiştirme
1. Sunucudan **genel bakış**, tıklatın **parola sıfırlama** parola sıfırlama penceresi göstermek için.

   ![genel bakış](./media/howto-create-manage-server-portal/overview.png)

2. Yeni bir parola girin ve gösterildiği gibi penceresinde parolayı onaylayın:

   ![Parola sıfırlama](./media/howto-create-manage-server-portal/reset-password.png)

3. Tıklatın **Tamam** yeni parolayı kaydetmek için.

### <a name="scale-vcores-updown"></a>Ölçek vCores yukarı/aşağı

1. Tıklayın **fiyatlandırma katmanı**altında bulunan **ayarları**.

2. Değişiklik **vCore** istenen değeriniz kaydırıcıyı taşıyarak ayarlama.

    ![Ölçek işlem](./media/howto-create-manage-server-portal/scale-compute.png)

3. Değişiklikleri kaydetmek için **Tamam**'a tıklayın.

### <a name="scale-storage-up"></a>Ölçek depolama alanı

1. Tıklayın **fiyatlandırma katmanı**altında bulunan **ayarları**.

2. Değişiklik **depolama** istenen değeriniz kaydırıcıyı taşıyarak ayarlama.

    ![Ölçek depolama](./media/howto-create-manage-server-portal/scale-storage.png)

3. Değişiklikleri kaydetmek için **Tamam**'a tıklayın.

## <a name="delete-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure veritabanını silin

1. Sunucudan **genel bakış**, tıklatın **silmek** Sil onay istemi açmak için düğmeye.

    ![sil](./media/howto-create-manage-server-portal/delete.png)

2. Çift onay giriş kutusuna sunucunun adını yazın.

    ![Onayla Sil](./media/howto-create-manage-server-portal/confirm.png)

3. Tıklatın **silmek** sunucuyu silmek onaylamak için düğmesi. "Başarıyla silindi MySQL server" pop için en fazla bekleme bildirim çubuğunda görüntülenir.

## <a name="list-the-azure-database-for-mysql-databases"></a>MySQL veritabanları için Azure veritabanı listesi
Sunucudan **genel bakış**, alt kısmında döşeme veritabanı görene kadar aşağı kaydırın. Sunucudaki tüm veritabanları tabloda listelenmiştir.

   ![Veritabanlarını göster](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure veritabanının ayrıntılarını göster
Tıklayın **özellikleri**altında bulunan **ayarları** sunucu hakkındaki ayrıntılı bilgileri görüntülemek için.

![properties](./media/howto-create-manage-server-portal/properties.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Azure veritabanı için Azure portalını kullanarak MySQL sunucusu oluşturun.](./quickstart-create-mysql-server-database-using-azure-portal.md)