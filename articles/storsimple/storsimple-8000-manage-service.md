---
title: Azure'da StorSimple cihaz Yöneticisi hizmetini dağıtma | Microsoft Docs
description: Oluşturma ve Azure portalında StorSimple cihaz Yöneticisi hizmetini silme açıklar ve hizmet kayıt anahtarını yönetme işlemi açıklanır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: eb1fe69a7fb99949ac95291c33e76c1a32bf5439
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60506697"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>StorSimple 8000 serisi cihazlar için StorSimple cihaz Yöneticisi hizmetini dağıtma

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure'da çalışan ve birden çok StorSimple aygıtına bağlanır. Hizmeti oluşturduktan sonra böylece yönetici yükünü en aza indirmek için StorSimple cihaz Yöneticisi hizmeti tek, merkezi bir konumdan bağlı olan tüm cihazları yönetmek için kullanabilirsiniz.

Bu öğreticide, oluşturma, silme, geçiş hizmetinin ve hizmet kayıt anahtarı yönetimi için gereken adımlar açıklanmaktadır. Bu makalede yer alan bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. StorSimple sanal dizilerine hakkında daha fazla bilgi için Git [bir StorSimple Virtual Array'iniz için StorSimple cihaz Yöneticisi hizmetini dağıtma](storsimple-virtual-array-manage-service.md).

> [!NOTE]
> -  Azure portalında güncelleştirme 5.0 veya sonraki sürümleri çalıştıran cihazları destekler. Güncelleştirme 5 hemen cihazınız güncel değilse, yükleyin. Daha fazla bilgi için Git [güncelleştirme 5'i yükleme](storsimple-8000-install-update-5.md). 
> - StorSimple bulut Gereci (8010/8020) kullanıyorsanız, bir bulut gerecini güncelleştiremezsiniz. Güncelleştirme 5. 0'ile yeni bir bulut Gereci oluşturmak ve sonra oluşturulan yeni bulut gerecine yük devretme için yazılımın en son sürümünü kullanın. 
> - Güncelleştirme 4.0 veya önceki sürümleri çalıştıran tüm cihazlara sınırlı yönetim işlevselliğine karşılaşırsınız. 

## <a name="create-a-service"></a>Hizmet oluşturma
StorSimple cihaz Yöneticisi hizmeti oluşturmak için sahip olmanız gerekir:

* Bir kurumsal anlaşma kapsamında olan bir abonelik
* Etkin bir Microsoft Azure depolama hesabı
* Erişim yönetimi için kullanılan faturalandırma bilgileri

Yalnızca bir Kurumsal Anlaşma abonelikleri izin verilir. Hizmet oluşturduğunuzda, varsayılan depolama hesabı oluşturmak seçebilirsiniz.

Tek bir hizmet birden çok cihazları yönetebilirsiniz. Ancak, bir cihaz birden fazla hizmet yayılamaz. Büyük bir kuruluş, farklı Aboneliklerde, kuruluşlar veya bile Dağıtım konumları ile çalışmak için birden çok hizmeti örneği olabilir. 

> [!NOTE]
> StorSimple 8000 serisi cihazlar ve StorSimple sanal dizilerini yönetmek için StorSimple cihaz Yöneticisi hizmeti ayrı örneklerini ihtiyacınız vardır.

Bir hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Her bir StorSimple cihaz Yöneticisi hizmeti için şu öznitelikler bulunur:

* **Adı** – oluşturulduğunda, StorSimple cihaz Yöneticisi hizmetinize atanan ad. **Hizmet adı, hizmet oluşturulduktan sonra değiştirilemez. Bu, aynı zamanda cihazları, birimleri, birim kapsayıcıları ve Azure portalında yeniden adlandırılamaz yedekleme ilkeleri gibi diğer varlıklar için de geçerlidir.**
* **Durum** – olabilir hizmetinin durumunu **etkin**, **oluşturma**, veya **çevrimiçi**.
* **Konum** –, StorSimple cihaz dağıtılacağı coğrafi konum.
* **Abonelik** – hizmetiniz ile ilişkili olan faturalama aboneliği.

## <a name="delete-a-service"></a>Bir hizmeti Sil

Bir hizmet silmeden önce bağlı cihaz yok, kullandığınızdan emin olun. Hizmet kullanılıyorsa, bağlı cihazları devre dışı bırakın. Devre dışı bırakma işlemi cihaz ile hizmet arasında bağlantı sever, ancak bulutta cihaz verilerini koruma.

> [!IMPORTANT]
> Hizmet silindikten sonra işlemi geri alınamaz. Hizmeti kullanan herhangi bir CİHAZDAN başka bir hizmet ile kullanılabilmesi için fabrika ayarlarına sıfırlanması gerekiyor. Bu senaryoda, yapılandırmanın yanı sıra, cihaz üzerinde yerel veriler kaybolur.

Bir hizmeti silmek için aşağıdaki adımları gerçekleştirin.

### <a name="to-delete-a-service"></a>Bir hizmeti silmek için

1. Silmek istediğiniz hizmeti arayın. Tıklayın **kaynakları** simgesine ve ardından aramak için uygun bir koşulları girin. Arama sonuçlarında, silmek istediğiniz service'ı tıklatın.

    ![Arama hizmeti silinemedi](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Bu StorSimple cihaz Yöneticisi hizmet dikey penceresine götürür. **Sil**'e tıklayın.

    ![Hizmeti sil](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Tıklayın **Evet** onay bildirimi içinde. Bu hizmetin silinmesi birkaç dakika sürebilir.

    ![Silme işlemini onaylama](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>Hizmet kayıt anahtarı alma

Bir hizmeti başarıyla oluşturduktan sonra StorSimple Cihazınızı hizmetine kaydetmeniz gerekir. İlk StorSimple Cihazınızı kaydetmek için hizmet kayıt anahtarı gerekir. Mevcut bir StorSimple hizmetiyle ek cihazlar kaydetmek için kayıt anahtarını hem de (Bu cihaz üzerinde ilk kayıt sırasında oluşturulan) hizmet veri şifreleme anahtarı gerekir. Hizmet veri şifreleme anahtarı hakkında daha fazla bilgi için bkz: [StorSimple güvenlik](storsimple-8000-security.md). Kayıt anahtarını erişerek alabileceğiniz **anahtarları** , StorSimple cihaz Yöneticisi dikey penceresinde.

Hizmet kayıt anahtarı almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Hizmet kayıt anahtarı güvenli bir yerde saklayın. Bu hizmetiyle ek cihazlar kaydetmek için hizmet veri şifreleme anahtarı, yanı sıra, bu anahtar gerekir. Hizmet kayıt anahtarını aldıktan sonra Windows PowerShell aracılığıyla cihazınızın StorSimple arabirimi için yapılandırmanız gerekir.

Bu kayıt anahtarı kullanma hakkında daha fazla ayrıntı için bkz. [3. adım: Yapılandırma ve StorSimple için Windows PowerShell üzerinden cihazı kaydetme](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluştur
Anahtar döndürme gerçekleştirmek için gerekli olduğunda veya hizmet yöneticilerinin listesini değişmişse bir hizmet kayıt anahtarını yeniden oluşturmak gerekir. Anahtarı yeniden ürettiğinizde, yeni anahtar yalnızca sonraki cihazları kaydetmek için kullanılır. Bu işlem tarafından zaten kayıtlı cihazlar etkilenmez.

Bir hizmet kayıt anahtarını yeniden oluşturmak için aşağıdaki adımları gerçekleştirin.

### <a name="to-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturmak için
1. İçinde **StorSimple cihaz Yöneticisi** dikey penceresinde, Git **Yönetim &gt;**  **anahtarları**.
    
    ![Anahtarlar dikey penceresi](./media/storsimple-8000-manage-service/regenregkey2.png)

2. İçinde **anahtarları** dikey penceresinde tıklayın **yeniden**.

    ![Regenerate tıklayın](./media/storsimple-8000-manage-service/regenregkey3.png)
3. İçinde **Regenerate hizmet kayıt anahtarını** dikey penceresinde, gözden geçirme eylem gerekli olduğunda anahtarları yeniden oluşturulur. Bu hizmete kayıtlı olan tüm sonraki cihazlar yeni kayıt anahtarını kullanın. Tıklayın **yeniden** onaylamak için. Yeniden oluşturma işlemi tamamlandığında size bildirilir.

    ![Regenerate onaylayın](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Yeni bir hizmet kayıt anahtarı görüntülenir.

5. Bu anahtarı kopyalayın ve bu hizmetle yeni cihazları kaydetmek için kaydedin.



## <a name="change-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarı değiştirme
Hizmet veri şifreleme anahtarları için StorSimple cihazı StorSimple Yöneticisi hizmetiniz gönderilen depolama hesabı kimlik bilgileri gibi müşteri gizli verileri şifrelemek için kullanılır. BT kuruluşunuzun depolama cihazlarında bir anahtar döndürme ilke varsa bu anahtarlar düzenli olarak değiştirmeniz gerekir. Anahtar değiştirme işlemi bağlı olarak biraz farklı olabilir. bir tek veya birden çok cihazı StorSimple Yöneticisi hizmeti tarafından yönetilen yoktur. Daha fazla bilgi için Git [StorSimple güvenlik ve veri koruma](storsimple-8000-security.md).

Hizmet veri şifreleme anahtarı değiştirilirken 3 adımlık bir işlemdir:

1. Azure Resource Manager için Windows PowerShell betiklerini kullanarak, hizmet veri şifreleme anahtarını değiştirmek için bir cihaz yetkilendirin.
2. StorSimple için Windows PowerShell kullanarak, hizmet veri şifreleme anahtarı değişikliği başlatın.
3. Birden fazla StorSimple cihazınız varsa, hizmet veri şifreleme anahtarı diğer cihazlarda güncelleştirin.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>1. Adım: Hizmet veri şifreleme anahtarını değiştirmek için bir cihaz son noktanın yetkilendirilmesi için Windows PowerShell betiğini kullanın.
Genellikle, cihaz Yöneticisi hizmet Yöneticisi bir cihazın hizmet veri şifreleme anahtarlarını değiştirmek için yetki verdiğiniz ister. Hizmet Yöneticisi daha sonra anahtarı değiştirmek için cihaz yetkilendirirsiniz.

Bu adım, Azure Resource Manager tabanlı betik kullanarak gerçekleştirilir. Hizmet Yöneticisi yetki verilmesi uygun olan bir cihaz seçebilirsiniz. Cihaz ardından hizmet veri şifreleme anahtar değiştirme işlemini başlatmak için yetkili. 

Komut dosyası kullanma hakkında daha fazla bilgi için Git [ServiceEncryptionRollover.ps1 Yetkilendir](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Hizmet veri şifreleme anahtarlarını değiştirmek için hangi cihazların yetkilendirilebilir?
Bir cihaz, hizmet veri şifreleme anahtarı değişiklikleri başlatmak için yetkilendirilebilir önce aşağıdaki ölçütleri karşılaması gerekir:

* Cihazın hizmet veri şifreleme anahtar değiştirme yetkilendirme için uygun olması için çevrimiçi olması gerekir.
* Anahtar değişikliği başlatılamadı, aynı cihaza 30 dakika sonra tekrar yetki verebilir.
* Anahtar değiştirme ve daha önce yetkili bir cihaz tarafından başlatılmamış koşuluyla farklı bir cihaz yetki verebilir. Yeni cihaz yetkilendirildikten sonra eski cihaz değişikliği başlatamazsınız.
* Hizmet veri şifreleme anahtarı geçiş işlemi devam ederken bir cihaz yetkilendirilemiyor.
* Başkalarının yok ancak bazı hizmete kayıtlı cihazlar şifrelemeyi gezinirken bir cihaz yetki verebilir. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>2. Adım: Hizmet veri şifreleme anahtarı'nı başlatmak StorSimple için Windows PowerShell kullanarak değiştirme
Bu adım, yetkili StorSimple cihazında arabirimi StorSimple için Windows PowerShell'de gerçekleştirilir.

> [!NOTE]
> Anahtar geçişi tamamlanana kadar hiçbir işlem StorSimple Yöneticisi hizmetiniz Azure portalında gerçekleştirilebilir.


Cihaz seri konsoluna bağlanmak için Windows PowerShell arabirimi için kullanıyorsanız, aşağıdaki adımları gerçekleştirin.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Hizmet veri şifreleme anahtar değişikliğini başlatmak için
1. Tam erişimle oturum açmak için 1 seçeneğini belirleyin.
2. Komut istemine şunları yazın:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Cmdlet başarıyla tamamlandıktan sonra yeni bir hizmet veri şifreleme anahtarı alırsınız. Kopyalayın ve bu işlem 3. adımında kullanmak için bu anahtarı kaydedin. Bu anahtar, StorSimple Yöneticisi hizmetine kayıtlı tüm diğer cihazları güncelleştirmek için kullanılır.
   
   > [!NOTE]
   > Bu işlem, bir StorSimple cihaz yetkilendirme sonra dört saat içinde başlatılmalıdır.
   > 
   > 
   
   Bu yeni anahtar, hizmete kayıtlı olan tüm cihazlara gönderilecek hizmete sonra gönderilir. Bir uyarı, ardından hizmet panosunda görünür. Hizmet kayıtlı cihazlarda tüm işlemleri devre dışı bırakır ve cihaz Yöneticisi daha sonra diğer cihazlar üzerinde hizmet veri şifreleme anahtarı güncelleştirmeniz gerekir. Ancak, g/ç (verileri buluta göndermeden ana) açması engellenecek değil.
   
   Hizmete kayıtlı tek bir cihaz varsa, geçiş işlemi tamamlanmıştır ve sonraki adıma atlayabilirsiniz. Birden çok cihaz hizmete kayıtlı varsa, 3. adıma geçin.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>3. Adım: Başka StorSimple cihazlar üzerinde hizmet veri şifreleme anahtarını güncelleştir
Birden çok cihaz için StorSimple Yöneticisi hizmetine kayıtlı varsa bu adımlar StorSimple cihazınızın Windows PowerShell arabiriminde gerçekleştirilmesi gerekir. 2. adımda elde edilen anahtar, StorSimple Yöneticisi hizmetine kayıtlı tüm kalan StorSimple cihaz güncelleştirmek için kullanılmalıdır.

Hizmet veri şifreleme Cihazınızda güncelleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-update-the-service-data-encryption-key-on-physical-devices"></a>Hizmet veri şifreleme anahtarı fiziksel cihazlarda güncelleştirmek için
1. StorSimple için Windows PowerShell konsoluna bağlanmak için kullanın. Tam erişimle oturum açmak için 1 seçeneğini belirleyin.
2. Komut isteminde aşağıdakini yazın:  `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Aldığınız hizmet veri şifreleme anahtarını sağlamalı [2. adım: Hizmet veri şifreleme anahtar değişikliğini başlatmak için StorSimple için Windows PowerShell kullanmak](#to-initiate-the-service-data-encryption-key-change).

#### <a name="to-update-the-service-data-encryption-key-on-all-the-80108020-cloud-appliances"></a>8010/8020 bulut Gereçleri üzerinde hizmet veri şifreleme anahtarını güncelleştirmek için
1. Karşıdan yükleme ve Kurulum [güncelleştirme CloudApplianceServiceEncryptionKey.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Update-CloudApplianceServiceEncryptionKey.ps1) PowerShell Betiği. 
2. PowerShell'i açın ve komut istemine yazın:  `Update-CloudApplianceServiceEncryptionKey.ps1 -SubscriptionId [subscription] -TenantId [tenantid] -ResourceGroupName [resource group] -ManagerName [device manager]`

Bu betik, cihaz Yöneticisi altındaki tüm 8010/8020 bulut Gereçleri, hizmet veri şifreleme anahtarı ayarlanır sağlayacaktır.

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>İşlemleri güncelleştirme 5.0 önceki sürümleri çalıştıran cihazlarda desteklenir
Azure portalında, yalnızca güncelleştirme 5.0 ve üzeri çalıştıran StorSimple cihazlar desteklenir. Eski sürümlerini çalıştıran cihazları sınırlı destek. Azure portalına geçiş yaptıktan sonra hangi işlemleri güncelleştirme 5.0 önceki sürümleri çalıştıran cihazlarda desteklenir anlamak için aşağıdaki tabloyu kullanın.

| İşlem                                                                                                                       | Desteklenen      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Cihaz kaydetme                                                                                                               | Evet            |
| Cihaz ayarları genel gibi ağ ve güvenliği yapılandırma                                                                | Evet            |
| Tarama, indirme ve güncelleştirmeleri yükleyin                                                                                             | Evet            |
| Cihazı devre dışı                                                                                                               | Evet            |
| Cihazı silme                                                                                                                   | Evet            |
| Oluşturabilir, değiştirebilir ve bir birim kapsayıcısını silme                                                                                   | Hayır             |
| Oluşturabilir, değiştirebilir ve bir birimini silme                                                                                             | Hayır             |
| Oluşturabilir, değiştirebilir ve yedekleme ilkesini silme                                                                                      | Hayır             |
| El ile yedekleyin                                                                                                            | Hayır             |
| Zamanlanmış yedekleyin                                                                                                         | Uygulanamaz |
| Bir yedek kümesi ' geri yükleme                                                                                                        | Hayır             |
| Güncelleştirme 3.0 ve üzerini çalıştıran bir cihaza Kopyala <br> Kaynak cihaz güncelleştirme 3.0 sürümünü çalıştırıyor.                                | Evet            |
| Güncelleştirme 3.0 önceki sürümlerini çalıştıran bir cihaza Kopyala                                                                          | Hayır             |
| Kaynak cihaz olarak yük devretme <br> (önceki güncelleştirme 3.0 çalıştıran bir cihazda güncelleştirme 3.0 ve sonraki sürümünü çalıştıran bir CİHAZDAN)                                                               | Evet            |
| Hedef cihaz olarak yük devretme <br> (yazılım sürümü güncelleştirme 3.0 çalıştıran bir cihaz için)                                                                                   | Hayır             |
| Bir uyarıyı temizleyin                                                                                                                  | Evet            |
| Yedekleme ilkelerini, yedekleme kataloğunu, birimler, birim kapsayıcıları, izleme grafiklerini, işleri ve klasik portalda oluşturulan uyarıları görüntüleme | Evet            |
| Açma ve kapatma cihaz denetleyicileri                                                                                              | Evet            |


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple dağıtım işlemi](storsimple-8000-deployment-walkthrough-u2.md).
* Daha fazla bilgi edinin [StorSimple depolama hesabınızı yönetme](storsimple-8000-manage-storage-accounts.md).
* Kullanma hakkında daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).
