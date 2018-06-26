---
title: Sunucuları Azure yığında barındırma MySQL | Microsoft Docs
description: MySQL bağdaştırıcısı kaynak Sağlayıcısı sağlama MySQL örnekleri ekleme
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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 5522eb1b8b0398aeb6f1b0dd8578b906880b4e89
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938507"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısı için barındırma sunucuları ekleme

İçinde bir sanal makine (VM) MySQL örneğinde barındırabilir [Azure yığın](azure-stack-poc.md), veya bir VM'de Azure yığın ortamınız dışındaki, MySQL kaynak sağlayıcı örneğine bağlayabilirsiniz sürece.

## <a name="connect-to-a-mysql-hosting-server"></a>Bir MySQL barındırma sunucusuna bağlan

Sistem Yöneticisi ayrıcalıklarına sahip bir hesabın kimlik bilgilerini sağlayın. Bir barındırma sunucusu eklemek için aşağıdaki adımları izleyin:

1. Azure yığın işleci portalında hizmet yönetici olarak oturum açın
2. Seçin **daha fazla hizmet**.
3. Seçin **yönetim KAYNAKLARININ** > **sunucuları barındırma MySQL** > **+ Ekle**. Bu açılır **barındırma MySQL Server Ekle** iletişim kutusunda, aşağıdaki ekran görüntüsünde gösterilen.

   ![Bir barındırma sunucusu yapılandırın](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. MySQL Server örneğinizi bağlantı ayrıntılarını sağlayın.

   * İçin **MySQL barındırma sunucusu adı**, tam etki alanı adı (FQDN) veya geçerli bir IPv4 adresi belirtin. Kısa VM adını kullanmayın.
   * Böylece belirtmek zorunda varsayılan MySQL örneği, sağlanmayan **boyut, barındırma sunucusunun GB cinsinden**. Veritabanı sunucusu kapasite yakın bir boyutu girin.
   * İçin varsayılan ayar tutmak **abonelik**.
   * İçin **kaynak grubu**, yeni bir tane oluşturun veya varolan bir grubu kullanın.

   > [!NOTE]
   > MySQL örneğine Kiracı ve Azure Resource Manager yönetici tarafından erişilebiliyorsa, kaynak sağlayıcısı denetiminde koyabilirsiniz. Ancak, MySQL örneğine **gerekir** özel olarak kaynak sağlayıcısı ayrılamadı.

5. Seçin **SKU'ları** açmak için **oluşturma SKU** iletişim.

   ![Bir MySQL SKU oluşturma](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   SKU **adı** kullanıcılar kendi veritabanlarını uygun SKU dağıtabilmek için SKU özelliklerini yansıtmalıdır.

   >[!IMPORTANT]
   >Özel karakterler, boşluklar ve dönemleri dahil olmak üzere desteklenmeyen **adı** veya **katmanı** oluşturduğunuzda bir SKU için MySQL kaynak sağlayıcısı.

6. Seçin **Tamam** SKU oluşturmak için.
7. Altında **barındırma MySQL Server Ekle**seçin **oluşturma**.

Sunucu eklemek gibi bunları hizmet teklifleri ayırt etmek için bir yeni veya var olan SKU'ya atayın. Örneğin, artan veritabanı ve otomatik yedeklemeler sağlayan bir MySQL Kurumsal örneği olabilir. Kuruluşunuzdaki farklı Departmanlar için bu yüksek performanslı sunucu ayırabilirsiniz.

## <a name="increase-backend-database-capacity"></a>Arka uç veritabanı kapasiteyi artırın

Arka uç veritabanı kapasitesine daha fazla MySQL sunucuları Azure yığın portalında dağıtarak artırabilir. Bu sunucular için yeni veya varolan bir SKU ekleyin. Varolan bir SKU'ya bir sunucu eklerseniz, sunucu özellikleri SKU diğer sunucular ile aynı olduğundan emin olun.

## <a name="make-mysql-database-servers-available-to-your-users"></a>MySQL veritabanı sunucuları kullanıcılarınızın kullanımına sunun

Planları ve MySQL veritabanı sunucuları kullanıcılar tarafından kullanılabilmesini sağlamak için teklifleri oluşturun. Microsoft.MySqlAdapter hizmet planına ekleyin ve varsayılan kota ekleyin veya yeni bir kota oluşturun.

![Planları ve veritabanları için teklifleri oluştur](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="next-steps"></a>Sonraki adımlar

[Bir MySQL veritabanı oluşturma](azure-stack-mysql-resource-provider-databases.md)