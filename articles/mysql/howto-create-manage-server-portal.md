---
title: Oluşturma ve Azure portalını kullanarak MySQL sunucusu için Azure veritabanı'nı yönetme
description: Bu makalede nasıl hızla bir MySQL sunucusu için yeni Azure veritabanı oluşturabilir ve Azure portalını kullanarak sunucuyu yönetmek açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 6d6f24475497382dd9e04d3335fb89d6f0bdd514
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459563"
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Oluşturma ve Azure portalını kullanarak MySQL sunucusu için Azure veritabanı'nı yönetme
Bu konu, yeni bir Azure veritabanını MySQL sunucusu için hızlı bir şekilde nasıl oluşturacağınızı açıklar. Ayrıca Azure portalını kullanarak sunucuyu yönetme hakkında bilgi içerir. Sunucu Yönetimi Sunucusu ayrıntıları ve parola sıfırlama için kaynakların ölçeklendirilmesi ve sunucuyu silmek veritabanları görüntüleme içerir.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
"Demosunucum." adlı MySQL sunucusu için Azure veritabanı oluşturmak için aşağıdaki adımları izleyin

1. Tıklayın **kaynak Oluştur** düğmesi Azure portalının sol alt köşesinde bulunan.

2. Yeni sayfada seçin **veritabanları**ve veritabanı sayfasında, ardından **MySQL için Azure veritabanı**.

    > MySQL sunucusu için Azure veritabanı, tanımlı bir dizi ile oluşturulan [işlem ve depolama](./concepts-pricing-tiers.md) kaynakları. Veritabanı, MySQL için Azure veritabanı ve Azure kaynak grubu içinde oluşturulur.

   ![Yeni-sunucusu oluştur](./media/howto-create-manage-server-portal/create-new-server.png)

3. Form MySQL için Azure veritabanı aşağıdaki bilgileri kullanarak doldurun:

    | **Form Alanı** | **Alan Açıklaması** |
    |----------------|-----------------------|
    | *Sunucu adı* | demosunucum (sunucu adı genel olarak benzersiz) |
    | *Abonelik* | mysubscription (aşağı açılan menüden seçim) |
    | *Kaynak grubu* | myresourcegroup (yeni bir kaynak grubu oluşturun veya var olanı kullanın) |
    | *Kaynak seçme* | Boş (boş bir MySQL sunucusu Oluştur) |
    | *Sunucu yöneticisi oturum açma bilgileri* | myadmin (yönetici hesabı adını ayarlayın) |
    | *Parola* | Yönetici hesabı parolasını ayarlayın |
    | *Parolayı onayla* | yönetici hesabı parolasını onaylayın |
    | *Konum* | Güneydoğu Asya (Kuzey Avrupa ve Batı ABD arasında seçim) |
    | *Sürüm* | 5.7 (MySQL server sürümü için Azure veritabanı'i seçin) |

4. Tıklayın **fiyatlandırma katmanı** yeni sunucunuz için Hizmet katmanını ve performans düzeyini belirtmek için. Seçin **genel amaçlı** sekmesi. *5. Nesil*, *2 sanal çekirdek*, *5 GB* ve *7 gün*; **İşlem Nesli**, **Sanal Çekirdek**, **Depolama** ve **Yedekleme Bekletme Dönemi** için varsayılan değerlerdir. Bu kaydırıcıları olduğu gibi bırakabilirsiniz. Coğrafi olarak yedekli depolamada sunucu yedeklerinizi etkinleştirmek için, **Fazladan Yedek Seçenekleri**’nde **Coğrafi Olarak Yedeklemeli**’yi seçin.

   ![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Sunucuyu sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer.

    > Seçin **panoya Sabitle** dağıtımlarınızın kolayca izlenmesini sağlamak için seçeneği.

## <a name="update-an-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı'nı güncelleştirin
Kullanıcı, yeni sunucuya sağlandıktan sonra yönetici parolası sıfırlanıyor ve sanal çekirdek ve depolama değiştirerek sunucunun artırma veya azaltma gibi mevcut sunucu yapılandırmak için birkaç seçenek vardır.

### <a name="change-the-administrator-user-password"></a>Yönetici kullanıcı parolası değiştirme
1. Sunucudan **genel bakış**, tıklayın **parolayı Sıfırla** parola sıfırlama penceresi gösterilecek.

   ![genel bakış](./media/howto-create-manage-server-portal/overview.png)

2. Yeni bir parola girin ve parolayı penceresinde gösterildiği gibi doğrulayın:

   ![Parolayı Sıfırla](./media/howto-create-manage-server-portal/reset-password.png)

3. Tıklayın **Tamam** yeni parolayı kaydetmek için.

### <a name="scale-vcores-updown"></a>Yukarı/Aşağı ölçeklendirme Vcore

1. Tıklayarak **fiyatlandırma katmanı**altında bulunan **ayarları**.

2. Değişiklik **sanal çekirdek** kaydırıcı, istenen değere taşıyarak ayarlama.

    ![ölçeklendirin](./media/howto-create-manage-server-portal/scale-compute.png)

3. Değişiklikleri kaydetmek için **Tamam**'a tıklayın.

### <a name="scale-storage-up"></a>Depolamayı ayarlama

1. Tıklayarak **fiyatlandırma katmanı**altında bulunan **ayarları**.

2. Değişiklik **depolama** kaydırıcı, istenen değere taşıyarak ayarlama.

    ![Depolama ölçek](./media/howto-create-manage-server-portal/scale-storage.png)

3. Değişiklikleri kaydetmek için **Tamam**'a tıklayın.

## <a name="delete-an-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı Sil

1. Sunucudan **genel bakış**, tıklayın **Sil** silme onayı istemini açmak için düğmeyi.

    ![delete](./media/howto-create-manage-server-portal/delete.png)

2. Çift onay giriş kutusuna sunucunun adını yazın.

    ![Silmeyi Onayla](./media/howto-create-manage-server-portal/confirm.png)

3. Tıklayın **Sil** sunucu silme işlemini onaylamanız düğmesi. "MySQL sunucusu başarıyla silindi" pop için kadar bekleyin bildirim çubuğunda görünür.

## <a name="list-the-azure-database-for-mysql-databases"></a>MySQL veritabanları için Azure veritabanı listesi
Sunucudan **genel bakış**, alttaki kutucuğuna veritabanı görene kadar aşağı kaydırın. Sunucudaki tüm veritabanları, tabloda listelenmiştir.

   ![show-veritabanları](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı ayrıntılarını göster
Tıklayarak **özellikleri**altında bulunan **ayarları** sunucu hakkındaki ayrıntılı bilgileri görüntülemek için.

![properties](./media/howto-create-manage-server-portal/properties.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Azure portalını kullanarak MySQL sunucusu için Azure veritabanı oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)