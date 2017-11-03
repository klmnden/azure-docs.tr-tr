---
title: "Oluşturma ve Azure veritabanı MySQL sunucusu için Azure portalını kullanarak yönetme | Microsoft Docs"
description: "Bu makalede nasıl hızlı bir şekilde MySQL sunucusu için yeni bir Azure veritabanı oluşturabilir ve Azure Portalı'nı kullanarak sunucu yönetimi açıklanmaktadır."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6e9c541aac1241b6af0e4a58f5591d46f9a98c40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Oluşturma ve Azure veritabanı MySQL sunucusu için Azure portalını kullanarak yönetme
Bu konu, yeni bir Azure veritabanı MySQL sunucusu için hızlı bir şekilde nasıl oluşturabileceğinizi açıklar. Ayrıca Azure portalını kullanarak sunucuyu yönetme hakkında bilgi içerir. Sunucu ayrıntıları ve parola sıfırlama ve sunucuyu silmek veritabanlarının görüntüleme sunucu yönetimi içerir.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL server "mysqlserver4demo." adlı bir Azure veritabanı oluşturmak için aşağıdaki adımları izleyin

1. Tıklatın **yeni** düğme Azure portalında sol üst köşesinde bulunan.

2. Yeni sayfasında seçin **veritabanları**ve veritabanlarını sayfasında, ardından **Azure veritabanı için MySQL**.

    > MySQL sunucusu için bir Azure veritabanı tanımlanan bir dizi ile oluşturulan [işlem ve depolama](./concepts-compute-unit-and-storage.md) kaynakları. Veritabanı, MySQL sunucusu için bir Azure veritabanında, bir Azure kaynak grubu içinde oluşturulur.

   ![Yeni-sunucusu oluştur](./media/howto-create-manage-server-portal/create-new-server.png)

3. Aşağıdaki bilgileri kullanarak Azure veritabanı için MySQL formu doldurun:

    | **Form Alanı** | **Alan Açıklaması** |
    |----------------|-----------------------|
    | *Sunucu adı* | Azure mysql (sunucu adı genel olarak benzersiz) |
    | *Abonelik* | MySQLaaS (seçin açılır menüden) |
    | *Kaynak grubu* | myresource (yeni bir kaynak grubu oluşturun veya var olanı kullanırsınız) |
    | *Sunucu yöneticisi oturum açma bilgileri* | myadmin (yönetici hesabı adını ayarlayın) |
    | *Parola* | Kurulum yönetici hesabı parolası |
    | *Parolayı onayla* | yönetici hesabı parolasını onaylayın |
    | *Konum* | Kuzey Avrupa (Kuzey Avrupa ve Batı ABD arasında seçim) |
    | *Sürüm* | 5.6 (Azure veritabanı için MySQL sunucusu sürümü seçin) |

4. Tıklatın **fiyatlandırma katmanı** yeni sunucunuzu Hizmet katmanını ve performans düzeyini belirtmek için. İşlem birimi 50'le 100 arasında standart katmanındaki 200 ile 100 arasındaki temel katmanındaki yapılandırılabilir ve depolama dahil miktarına bağlı olarak eklenebilir. Bu nasıl yapılır kılavuzu için şimdi bir 50 işlem birimi ve 50 GB'ı seçin. Tıklatın **Tamam** seçiminizi kaydetmek için.

   ![oluşturma sunucu-fiyatlandırma-katmanı](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Sunucuyu sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer.

    > Seçin **panoya Sabitle** dağıtımlarınızı kolay izlenmesini izin vermek için seçeneği.
    > [!NOTE]
    > Depolama için 1000 GB temel katmanındaki ve standart katmanındaki 10.000 GB desteklenir ancak genel önizlemesi için maksimum depolama 1000 GB geçici olarak sınırlıdır.</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure veritabanını güncelleştirme
Yeni Sunucu sağlandıktan sonra kullanıcı varolan sunucusu düzenlemek için iki seçenek vardır: Yönetici parolasını sıfırla veya işlem birimleri değiştirerek sunucu yukarı veya aşağı ölçeklendirin.

### <a name="change-the-administrator-user-password"></a>Yönetici kullanıcı parolasını değiştirme
1. Sunucu üzerindeki **genel bakış** dikey penceresinde tıklatın **parola sıfırlama** bir parola giriş ve onay penceresi doldurmak için.

2. Yeni bir parola girin ve gösterildiği gibi penceresinde parolayı onaylayın:

   ![Parola sıfırlama](./media/howto-create-manage-server-portal/reset-password.png)

3. Tıklatın **Tamam** yeni parolayı kaydetmek için.

### <a name="scale-updown-by-changing-compute-units"></a>Ölçek yukarı/aşağı işlem birimleri değiştirerek

1. Sunucu dikey altında **ayarları**, tıklatın **fiyatlandırma katmanı** MySQL sunucusu için Azure veritabanı için fiyatlandırma katmanı dikey penceresini açmak için.

2. Adım 4'te izleyin **MySQL sunucusu için bir Azure veritabanı oluşturma** işlem birimleri aynı fiyatlandırma katmanını değiştirmek için.

## <a name="delete-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure veritabanını silin

1. Sunucu üzerindeki **genel bakış** dikey penceresinde tıklatın **silmek** silme onayı dikey penceresini açmak için komut düğmesi.

2. Giriş kutusuna çift onay dikey doğru sunucu adını yazın.

3. Tıklatın **silmek** silme eylemi ve ardından "silme başarılı" bekle açılır bildirim çubuğunda görünmesini onaylamak için yeniden düğmesi.

## <a name="list-the-azure-database-for-mysql-databases"></a>MySQL veritabanları için Azure veritabanı listesi
Sunucu üzerindeki **genel bakış** dikey penceresinde sayfanın alt döşeme veritabanı görene kadar aşağı kaydırın. Tüm veritabanları tabloda listelenmiştir. Tıklatın **silmek** komut düğmesi silme onayı dikey penceresini açın.

   ![Veritabanlarını göster](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure veritabanının ayrıntılarını göster
Sunucu dikey altında **ayarları**, tıklatın **özellikleri** açmak için **özellikleri** dikey ve sonra görünümü tüm hakkında ayrıntılı bilgi sunucu.

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Azure veritabanı için Azure portalını kullanarak MySQL sunucusu oluşturun.](./quickstart-create-mysql-server-database-using-azure-portal.md)