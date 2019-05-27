---
title: include dosyası
description: include dosyası
author: jonels-msft
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: include
ms.date: 05/14/2019
ms.author: jonels
ms.custom: include file
ms.openlocfilehash: c07e352288d7dc1d0bf198fd74c8baaded3a2d23
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66154486"
---
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-an-azure-database-for-postgresql---hyperscale-citus"></a>-Hiper ölçekli (Citus) PostgreSQL için Azure veritabanı oluşturma

PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **PostgreSQL için Azure Veritabanı**’nı seçin.
3. Dağıtım seçeneği için tıklatın **Oluştur** düğmesini **hiper ölçekli (Citus) sunucu grubu - Önizleme.**
4. Yeni sunucu ayrıntıları formunu aşağıdaki bilgilerle doldurun:
   - Kaynak grubu: tıklayın **Yeni Oluştur** Bu alan için metin kutusunun altında bağlantı. Gibi bir ad girin **myresourcegroup**.
   - Sunucu grubu adı: sunucu alt etki alanı için kullanılacak yeni bir sunucu grubu için benzersiz bir ad girin.
   - Yönetici kullanıcı adı: geçerli bir değer olmasını gerekli **citus**ve değiştirilemez.
   - Parola: olmalıdır en az sekiz karakter uzunluğunda olmalı ve şu kategorilerin üçünden karakterler içermelidir – İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, % vb..)
   - Konum: bunları verilere en hızlı erişim sağlamak için kullanıcılarınıza en yakın konumu kullanın.

   > [!IMPORTANT]
   > Burada belirttiğiniz sunucu yöneticisi parolasını, sunucu ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

5. Tıklayın **yapılandırma sunucusu grubunu**. Ayarları içeren bölüm değiştirmeden bırakın ve tıklayın **Kaydet**.
6. Tıklayın **gözden geçir + Oluştur** ardından **Oluştur** sunucuyu sağlamak için. Sağlama birkaç dakika sürer.
7. Sayfa dağıtımını izlemek için yönlendirir. Canlı durumu değiştiğinde **devam ettiği dağıtımıdır** için **dağıtımınız tamamlandıktan**, tıklayın **çıkışları** sayfasının sol menü öğesi.
8. Çıkış sayfası değeri panoya kopyalamak için yanında bir düğme olan bir düzenleyici ana bilgisayar adı içerir. Daha sonra kullanmak için bu bilgileri kaydedin.

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

Azure sunucu düzeyinde bir güvenlik duvarı (Citus) (Önizleme) hizmeti kullandığı PostgreSQL hiper ölçekli veritabanı. Varsayılan olarak, düzenleyici düğüm ve içinde herhangi bir veritabanına bağlanmasını tüm dış uygulama ve araçların güvenlik duvarı engeller. Biz, belirli bir IP adresi aralığı için güvenlik duvarını açmak üzere bir kural eklemeniz gerekir.

1. Gelen **çıkışları** daha önce kopyaladığınız Düzenleyici düğüm ana bilgisayar bölümü tıklatın geri **genel bakış** menü öğesi.

2. Dağıtımınızın server grubunun adını bulun ve tıklatın. (Sunucu grubu adı olur *değil* son eke sahiptir. Öğeler, sonlanan adlara sahip Örneğin, "-c", "-w0", veya "-w1" sunucu grubu değildir.)

3. Tıklayın **Güvenlik Duvarı** altında **güvenlik** sol menüdeki.

4. Bağlantıya tıklayın **+ geçerli istemci IP adresi için Güvenlik Duvarı Kuralı Ekle**.

5. Son olarak, tıklayın **Kaydet** düğmesi.

   > [!NOTE]
   > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 5432 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
   >

## <a name="connect-to-the-database-using-psql"></a>Psql kullanarak veritabanına bağlanma

PostgreSQL için Azure veritabanı sunucunuza oluşturduğunuzda, adlı varsayılan veritabanı **citus** oluşturulur. Veritabanı sunucunuza bağlanmak için bir bağlantı dizesi ve yönetici parolası gerekir.

1. Bağlantı dizesini edinin. Sunucu grubu sayfasında tıklayın **bağlantı dizeleri** menü öğesi. (Bunun altında olduğunu **ayarları**.) İşaretli dize Bul  **C++ (libpq)**. Bu tür olacaktır:

   ```
   host=hostname.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require
   ```

   Dizeyi kopyalayın. Değiştirmeniz gerekecektir. "{,\_parola}" ile daha önce seçtiğiniz yönetici parolası. Sistem, düz metin parola depolamaz ve bu nedenle, bağlantı dizesinde görüntüleyemez.

2. Yerel bilgisayarınızda bir terminal penceresi açın.

3. İstemde, ile PostgreSQL için Azure veritabanı sunucunuza bağlanın [psql](https://www.postgresql.org/docs/current/app-psql.html) yardımcı programı. Bağlantı dizenizi parolanızı içerdiğinden emin olan tırnak içine geçirin:
   ```bash
   psql "{connection_string}"
   ```

   Örneğin, aşağıdaki komut, sunucu grubu Düzenleyici düğüme yeniden bağlanır **demosunucum**:

   ```bash
   psql "host=mydemoserver-c.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require"
   ```
