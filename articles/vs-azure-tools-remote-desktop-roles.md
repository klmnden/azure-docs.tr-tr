---
title: "Uzak Masaüstü kullanarak Azure rolleriyle | Microsoft Docs"
description: "Azure rolleri ile Uzak Masaüstü'nü kullanma"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: eab135d10c0d6df8ca72ac47d6804017a998a3d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a>Azure rolleri ile Uzak Masaüstü'nü kullanma
Uzak Masaüstü Hizmetleri ve Azure SDK kullanarak Azure rolleri ve Azure tarafından barındırılan sanal makinelerin erişebilir. Visual Studio'da bir Azure bulut hizmeti projesi Uzak Masaüstü Hizmetleri yapılandırabilirsiniz. Uzak Masaüstü Hizmetleri etkinleştirmek için bir veya daha fazla rolü içeren bir çalışma projesi oluşturun ve Azure'a yayımlayacaksınız.

> [!IMPORTANT]
> Sorun giderme veya yalnızca geliştirme için Azure bir rolü erişim. Her bir sanal makine amacı, diğer istemci uygulamaları çalıştırmayı Azure uygulamanızı, belirli bir rol çalıştırmaktır. Herhangi bir amaçla kullanabileceğiniz bir sanal makineyi barındırmak için Azure kullanmak istiyorsanız, Azure sanal makineleri Sunucu Gezgini'nden erişme bakın.
> 
> 

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Etkinleştirmek ve bir Azure rol için Uzak Masaüstü'nü kullanmak için
1. Çözüm Gezgini'nde, bulut hizmeti projenizi için kısayol menüsünü açın ve ardından **Yayımla**.
   
    **Azure uygulamasını Yayımla** Sihirbazı görünür.
   
    ![Komutu için bir bulut hizmeti projesini Yayımla](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. Ekranın alt kısmındaki **Microsoft Azure yayımlama ayarları** seçin sayfasında **Uzak Masaüstü'nü etkinleştirme** tüm rolleri onay kutusu. 
   
    **Uzak masaüstü yapılandırması** iletişim kutusu görüntülenir.
3. Ekranın alt kısmındaki **uzak masaüstü yapılandırması** iletişim kutusunda, seçin **diğer seçenekler** düğmesi. 
   
    Bu oluşturmak veya Uzak Masaüstü aracılığıyla bağlanırken kimlik bilgilerini şifrelemek için bir sertifika seçin imkan tanıyan bir açılır liste kutusu görüntüler.
4. Aşağı açılan listesinde seçin  **&lt;Oluştur >**, veya mevcut bir listeden seçin. 
   
    Varolan bir sertifikayı seçerseniz, aşağıdaki adımları atlayın.
   
   > [!NOTE]
   > Bir Uzak Masaüstü bağlantısı için gereken sertifikalar, diğer Azure işlemleri için kullandığı sertifikalar farklıdır. Uzaktan erişim sertifikasının özel anahtarı olması gerekir.
   > 
   > 
   
    **Oluşturduğunuz sertifika** iletişim kutusu görüntülenir.
   
   1. Yeni sertifika için kolay bir ad sağlayın ve ardından **Tamam** düğmesi. Yeni sertifika açılır liste kutusunda görüntülenir.
   2. İçinde **uzak masaüstü yapılandırması** iletişim kutusunda, bir kullanıcı adı ve parola sağlayın.
      
       Var olan bir hesap kullanamazsınız. Yönetici yeni hesap için kullanıcı adı olarak belirtmeyin.
      
      > [!NOTE]
      > Parola karmaşıklık gereksinimlerini karşılamıyorsa, kırmızı bir simge yanındaki parola metin kutusu görünür. Parola büyük harf, küçük harfler ve sayılar veya simgeler içermelidir.
      > 
      > 
   3. Hesap sona erecek ve hangi Uzak Masaüstü bağlantıları engellenir sonra bir tarihi seçin.
   4. Gerekli tüm bilgileri sağladığınız sonra tercih **Tamam** düğmesi.
      
       Uzaktan erişim hizmetleri sağlayan birkaç ayarları .cscfg ve .csdef dosyasına eklenir.
5. İçinde **Microsoft Azure yayımlama ayarları** Sihirbazı'nı seçin **Tamam** bulut hizmetiniz yayımlamaya hazır olduğunuzda düğmesine tıklayın.
   
    Yayımlamaya hazır değilseniz seçin **iptal** düğmesi. Yapılandırma ayarları kaydedilir ve daha sonra bulut hizmetinizi yayımlayın.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Uzak Masaüstü'nü kullanarak bir Azure rolüne bağlayın
Bulut hizmetinizde Azure yayımladıktan sonra Azure barındıran sanal makinelerine oturum için Sunucu Gezgini kullanabilirsiniz. 

1. Server Explorer'da genişletin **Azure** düğümünü ve bir bulut hizmeti ve örneklerinin bir listesini görüntülemek için kendi rollerinden birini düğümünü genişletin.
2. Bir örnek düğümü için kısayol menüsünü açın ve ardından **Uzak Masaüstü kullanarak bağlanmak**.
   
    ![Uzak Masaüstü aracılığıyla bağlanma](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Kullanıcı adı ve daha önce oluşturduğunuz parolayı girin. Şimdi uzak oturumunuza günlüğe kaydedilir.

