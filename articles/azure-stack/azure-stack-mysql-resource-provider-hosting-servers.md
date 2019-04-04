---
title: Barındırma sunucuları Azure Stack'te MySQL | Microsoft Docs
description: MySQL bağdaştırıcısı kaynak sağlayıcısı aracılığıyla sağlama için MySQL örnekleri ekleme
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: 8cffcc938a247a2b08ff53b128560e1ab5e1653a
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499786"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcı barındırma sunucuları ekleme

İçinde bir MySQL server örneğini bir sanal makine'de (VM) barındıran konak [Azure Stack](azure-stack-poc.md), veya MySQL kaynak sağlayıcı örneği bağlanabilir sürece, Azure Stack ortamınıza dışında bir VM'de.

> [!NOTE]
> MySQL kaynak sağlayıcısı, barındırma MySQL Server Faturalanabilir, kullanıcı aboneliklerini oluşturulması sırasında varsayılan sağlayıcı abonelikte oluşturulmalıdır. Kaynak sağlayıcısı sunucunun kullanıcı veritabanını barındırmak için kullanılmamalıdır.

MySQL sürüm 5.6, 5.7 ve 8.0, barındırma sunucuları için kullanılabilir. MySQL RP caching_sha2_password kimlik doğrulamasını desteklemez; Bu, sonraki sürümde eklenecek. MySQL 8.0 sunucuları mysql_native_password kullanmak için yapılandırılmalıdır. MariaDB de desteklenir.

## <a name="connect-to-a-mysql-hosting-server"></a>Bir MySQL barındırma sunucusuna bağlan

Sistem Yöneticisi ayrıcalıklarına sahip bir hesabın kimlik bilgilerini sağlayın. Bir barındırma sunucusu eklemek için aşağıdaki adımları izleyin:

1. Bir Hizmet Yöneticisi Azure Stack operatörü portalında oturum açın
2. **Tüm Hizmetler**’i seçin.
3. Altında **yönetim kaynakları** kategorisi seçin **barındırma MySQL Server** > **+ Ekle**. Bu açılır **barındırma MySQL Server Ekle** iletişim kutusunda, aşağıdaki ekran görüntüsünde gösterilen.

   ![Bir barındırma sunucusunu yapılandırma](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. MySQL Server örneğinizi bağlantı ayrıntılarını sağlayın.

   * İçin **MySQL barındırma sunucusunun adını**, tam etki alanı adı (FQDN) veya geçerli bir IPv4 adresi sağlayın. Kısa VM adını kullanmayın.
   * Varsayılan yönetici **kullanıcıadı** Bitnami MySQL Azure Stack marketteki görüntüleri için *kök*. 
   * Kök bilmiyorsanız **parola**, bkz: [Bitnami belgeleri](https://docs.bitnami.com/azure/faq/#how-to-find-application-credentials) nasıl edinileceğini öğrenmek için. 
   * Belirtmek zorunda müşteri varsayılan bir MySQL örneği sağlanmayan **boyut, barındırma sunucusunun GB cinsinden**. Veritabanı sunucusu kapasitesini yakın bir boyutu girin.
   * Varsayılan ayarı tutun **abonelik**.
   * İçin **kaynak grubu**, yeni bir tane oluşturun veya varolan bir grubu kullanın.

   > [!NOTE]
   > MySQL örneği Kiracı ve yönetici Azure Resource Manager tarafından erişilebilen, kaynak sağlayıcısı denetiminde koyabilirsiniz. Ancak, MySQL örneği **gerekir** kaynak sağlayıcıya özel olarak ayrılmış.

5. Seçin **SKU'ları** açmak için **oluşturma SKU** iletişim.

   ![Bir MySQL SKU oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   SKU **adı** kullanıcıların veritabanlarını ve uygun SKU için ucunuzun sku'sunun özelliklerini yansıtmalıdır.

6. Seçin **Tamam** SKU oluşturma.
   > [!NOTE]
   > SKU'ları portalda görünür olması için bir saat sürebilir. SKU dağıtılmış ve çalışır olana kadar bir veritabanı oluşturulamıyor.

7. Altında **barındırma MySQL Server Ekle**seçin **Oluştur**.

Sunucuları eklerken, bunları hizmet teklifleri ayırt etmek için yeni veya mevcut SKU için atayın. Örneğin, artan veritabanı ve otomatik yedeklemeler sağlayan bir MySQL Kurumsal örneğine sahip olabilirsiniz. Kuruluşunuzda farklı Departmanlar için bu yüksek performanslı sunucu ayırabilirsiniz.

## <a name="security-considerations-for-mysql"></a>MySQL için güvenlik konuları

Aşağıdaki bilgiler, RP ve MySQL barındırma sunucuları için geçerlidir:

* Tüm barındırma sunucuları TLS 1.2 kullanarak iletişimi için yapılandırıldığından emin olun. Bkz: [şifrelenmiş bağlantıları kullanmak için MySQL yapılandırma](https://dev.mysql.com/doc/refman/5.7/en/using-encrypted-connections.html).
* Görevlendirmek [saydam veri şifrelemesi](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-data-encryption.html).
* MySQL RP caching_sha2_password kimlik doğrulamasını desteklemez.

## <a name="increase-backend-database-capacity"></a>Arka uç veritabanınızın kapasitesini artırmanız

Daha fazla MySQL sunucusu Azure Stack portalında dağıtarak arka uç veritabanınızın kapasitesini artırabilir. Bu sunucular için yeni veya mevcut bir SKU ekleyin. Mevcut bir SKU için bir sunucu eklerseniz, sunucu özelliklerini SKU diğer sunucular ile aynı olduğundan emin olun.

## <a name="sku-notes"></a>SKU notları
Kapasite ve performans gibi SKU sunucuların özelliklerini tanımlayan bir SKU adı kullanın. Ad ve uygun SKU için kendi veritabanları dağıtma kullanıcılara yardımcı olmak için yardımcı işlevi görür. Örneğin, hizmet teklifleri aşağıdaki özelliklerle ayırt etmek için SKU adları kullanabilirsiniz:
  
* yüksek kapasite
* yüksek performans
* yüksek kullanılabilirlik

En iyi uygulama, bir SKU tüm barındırma sunucuları aynı kaynak ve performansın özelliklere sahip olmalıdır.

SKU'ları, belirli kullanıcılara veya gruplara atanamaz.

Bir SKU düzenlemek için şuraya gidin: **tüm hizmetleri** > **MySQL bağdaştırıcısı** > **SKU'ları**. SKU'ları değiştirmek, gerekli değişiklikleri yapın ve tıklayın seçin **Kaydet** değişiklikleri kaydedin. 

Artık gerekli olmadığında bir SKU silmek için Git **tüm hizmetleri** > **MySQL bağdaştırıcısı** > **SKU'ları**. SKU adı sağ tıklayıp **Sil** silmek için.

> [!IMPORTANT]
> Bu Kullanıcı Portalı'nda kullanılabilir olması yeni SKU'lara bir saate kadar sürebilir.

## <a name="make-mysql-database-servers-available-to-your-users"></a>MySQL veritabanı sunucularını, kullanıcılar için kullanılabilir yap

Planlar ve teklifler MySQL veritabanı sunucularını kullanıcıları için kullanılabilir hale getirmek oluşturun. Microsoft.MySqlAdapter hizmet planına ekleyin ve bir yeni kota oluştur. MySQL veritabanlarının boyutunu sınırlama izin vermez.

> [!IMPORTANT]
> Uygulamanın, Kullanıcı Portalı'nda veya değiştirilmiş bir kota uygulanmadan önce kullanılabilir olması için yeni kotalar iki saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL veritabanı oluşturma](azure-stack-mysql-resource-provider-databases.md)