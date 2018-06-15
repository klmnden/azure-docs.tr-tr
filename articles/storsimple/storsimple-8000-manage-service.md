---
title: Azure StorSimple cihaz Yöneticisi Hizmeti'ni dağıtma | Microsoft Docs
description: Oluşturma ve Azure portalında StorSimple cihaz Yöneticisi hizmetini silmek açıklar ve hizmet kayıt anahtarını yönetme açıklar.
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
ms.openlocfilehash: d6010b7ff03689588251a9649eecb412bf9f3a8d
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012752"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>StorSimple 8000 serisi cihazlar için StorSimple cihaz Yöneticisi hizmeti dağıtma

## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple cihazını bağlanır. Hizmeti oluşturduktan sonra böylece yönetici yükünü en aza tek, merkezi bir konumdan StorSimple cihaz Yöneticisi hizmetine bağlanan tüm cihazları yönetmek için kullanabilirsiniz.

Bu öğretici oluşturma, silme, hizmetinin geçişini ve hizmet kayıt anahtarı yönetimi için gereken adımları açıklar. Bu makalede yer alan bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. StorSimple sanal dizileri hakkında daha fazla bilgi için Git [, StorSimple sanal dizisi için StorSimple Aygıt Yöneticisi'ni hizmet dağıtma](storsimple-virtual-array-manage-service.md).

> [!NOTE]
> -  Azure Portalı'nı güncelleştirme 5.0 veya üstünü çalıştıran cihazları destekler. Cihazınız güncel değilse, güncelleştirme 5 hemen yükleme. Daha fazla bilgi için Git [yükleme güncelleştirme 5](storsimple-8000-install-update-5.md). 
> - Bir StorSimple bulut uygulaması (8010/8020) kullanıyorsanız, bulut uygulaması güncelleştirilemiyor. En son sürümünü yazılım güncelleştirme 5.0 ile yeni bir bulut uygulaması oluşturun ve sonra oluşturulan yeni bulut uygulaması yük için kullanın. 
> - Güncelleştirme 4.0 veya önceki sürümleri çalıştıran tüm cihazlara yaşar [yönetim işlevleri azaltılmış](storsimple-8000-manage-service.md#supported-operations-on-devices-running-versions-prior-to-update-5.0). 

## <a name="create-a-service"></a>Hizmet oluşturma
StorSimple cihaz Yöneticisi hizmet oluşturmak için sahip olmanız gerekir:

* Bir aboneliği bir kurumsal anlaşma ile
* Etkin bir Microsoft Azure depolama hesabı
* Erişim yönetimi için kullanılan faturalama bilgileri

Yalnızca bir kurumsal anlaşma ile aboneliklerine izin verilir. Hizmet oluşturduğunuzda, varsayılan depolama hesabı oluşturmak seçebilirsiniz.

Tek bir hizmet birden çok cihazları yönetebilirsiniz. Ancak, bir aygıt birden fazla hizmet yayılamaz. Büyük bir kuruluş farklı Aboneliklerde, kuruluşlar veya bile Dağıtım konumları ile çalışmak için birden fazla hizmet örneği olabilir. 

> [!NOTE]
> StorSimple 8000 serisi cihazlar ve StorSimple sanal dizileri yönetmek için StorSimple cihaz Yöneticisi hizmeti ayrı ayrı örnekleri gerekir.

Bir hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Her StorSimple cihaz Yöneticisi hizmeti için aşağıdaki öznitelikler mevcuttur:

* **Ad** – oluşturulduğunda StorSimple cihaz Yöneticisi hizmetinize atanmış adı. **Hizmet adı, hizmet oluşturulduktan sonra değiştirilemez. Bu, aynı zamanda cihazları, birimler, birim kapsayıcıları ve Azure portalında yeniden adlandırılamaz yedekleme ilkeleri gibi diğer varlıklar için de geçerlidir.**
* **Durum** – olabilir hizmetinin durumunu **etkin**, **oluşturma**, veya **çevrimiçi**.
* **Konum** –, StorSimple cihazı dağıtılacak coğrafi konum.
* **Abonelik** – hizmetiniz ile ilişkili fatura abonelik.

## <a name="delete-a-service"></a>Bir hizmeti silin

Bir hizmet silmeden önce hiçbir bağlı aygıtlar, kullandığınızdan emin olun. Hizmet kullanımdaysa bağlı aygıtları devre dışı bırakın. Devre dışı bırakma işlemi cihaz ile hizmet arasında bağlantı sever ancak bulutta cihaz verileri koruma.

> [!IMPORTANT]
> Hizmet silindikten sonra işlemi geri alınamaz. Hizmet tarafından kullanılan herhangi bir aygıtı başka bir hizmetle kullanılabilmesi için önce fabrika ayarlarına sıfırlanması gerekir. Bu senaryoda, cihaz olarak yapılandırma üzerinde yerel veriler kaybolur.

Bir hizmeti silmek için aşağıdaki adımları gerçekleştirin.

### <a name="to-delete-a-service"></a>Bir hizmeti silmek için

1. Silmek istediğiniz hizmet arayın. Tıklatın **kaynakları** simgesi ve aramak için uygun koşulları girin. Arama sonuçlarında, silmek istediğiniz hizmete tıklayın.

    ![Arama hizmeti silmek için](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Bu sizi, StorSimple cihaz Yöneticisi hizmeti dikey penceresine götürür. **Sil**'e tıklayın.

    ![Hizmeti sil](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Tıklatın **Evet** onay bildirim. Hizmetin silinmesi birkaç dakika sürebilir.

    ![Silme işlemini onaylama](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>Hizmet kayıt anahtarı alma

Bir hizmeti başarıyla oluşturduktan sonra StorSimple Cihazınızı hizmete kaydolmak gerekir. İlk StorSimple Cihazınızı kaydetmek için hizmet kayıt anahtarı gerekir. Mevcut bir StorSimple hizmetiyle ek cihazlar kaydetmek için kayıt anahtarını ve (ilk cihazda kayıt sırasında oluşturulan) hizmeti veri şifreleme anahtarı gerekir. Hizmet verileri şifreleme anahtarı hakkında daha fazla bilgi için bkz: [StorSimple güvenlik](storsimple-8000-security.md). Kayıt anahtarını erişerek alabileceğiniz **anahtarları** StorSimple Aygıt Yöneticisi'ni dikey penceresinde.

Hizmet kayıt anahtarını almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Hizmet kayıt anahtarını güvenli bir yerde tutun. Bu hizmetiyle ek cihazlar kaydetmek için hizmet verileri şifreleme anahtarı, yanı sıra, bu anahtar gerekir. Hizmet kayıt anahtarı aldıktan sonra Windows PowerShell aracılığıyla cihazınızın StorSimple arabirimi için yapılandırmanız gerekir.

Bu kayıt anahtarı kullanma hakkında daha fazla bilgi için bkz: [adım 3: yapılandırmak ve storsimple için Windows PowerShell üzerinden cihazı](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturma
Hizmet kayıt anahtarını anahtar döndürmeyi gerçekleştirmek için gerekli olduğunu veya hizmet yöneticilerinin listesini değiştirilmişse yeniden oluşturulması gerekir. Anahtarı yeniden oluşturmak, yeni anahtar yalnızca sonraki cihazları kaydetmek için kullanılır. Bu işlem tarafından zaten kayıtlı olan cihazları etkilenmez.

Hizmet kayıt anahtarını yeniden oluşturmak için aşağıdaki adımları gerçekleştirin.

### <a name="to-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturmak için
1. İçinde **StorSimple Aygıt Yöneticisi'ni** dikey penceresinde, Git **Yönetim &gt;**  **anahtarları**.
    
    ![Anahtarlar dikey penceresi](./media/storsimple-8000-manage-service/regenregkey2.png)

2. İçinde **anahtarları** dikey penceresinde tıklatın **yeniden**.

    ![Yeniden tıklatın](./media/storsimple-8000-manage-service/regenregkey3.png)
3. İçinde **üretme hizmet kayıt anahtarını** dikey penceresinde, gözden geçirme eylem gerekli olduğunda anahtarları yeniden oluşturulur. Bu hizmetle kaydedilen tüm sonraki cihazlar yeni kayıt anahtarını kullanın. Tıklatın **yeniden** onaylamak için. Yeniden oluşturma işlemi tamamlandıktan sonra size bildirilir.

    ![Yeniden onaylayın](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Yeni bir hizmet kayıt anahtarı görüntülenir.

5. Bu anahtarı kopyalayın ve bu hizmeti ile yeni aygıtları kaydetme için kaydedin.



## <a name="change-the-service-data-encryption-key"></a>Değişiklik hizmeti veri şifreleme anahtarı
Hizmet veri şifreleme anahtarları, StorSimple Yöneticisi hizmetiniz StorSimple cihaza gönderilen depolama hesabının kimlik bilgilerini gibi gizli müşteri verilerini şifrelemek için kullanılır. BT kuruluşunuzun depolama cihazlarında bir anahtar döndürme ilke varsa bu anahtarlar düzenli olarak değiştirmeniz gerekir. Anahtar değişim işlemini bağlı olarak biraz farklı olabilir bir tek veya birden çok cihazı StorSimple Yöneticisi hizmeti tarafından yönetilen yoktur. Daha fazla bilgi için Git [StorSimple güvenlik ve veri koruması](storsimple-8000-security.md).

Hizmet verileri şifreleme anahtarı değiştirme 3 adımlı bir işlemdir:

1. Azure Resource Manager için Windows PowerShell komut dosyalarını kullanarak hizmet verileri şifreleme anahtarı değiştirmek için bir aygıt yetkilendirin.
2. StorSimple için Windows PowerShell kullanarak hizmet verileri şifreleme anahtarı değişikliği başlatır.
3. Birden çok StorSimple cihazınız varsa, diğer cihazlarda hizmet verileri şifreleme anahtarı güncelleştirin.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>1. adım: hizmet verileri şifreleme anahtarı değiştirmek için bir aygıt yetkilendirmek için kullanım Windows PowerShell Betiği
Genellikle, cihaz yönetici Hizmet Yöneticisi hizmeti veri şifreleme anahtarları değiştirmek için bir aygıt yetkilendirmek ister. Hizmet Yöneticisi, ardından anahtarı değiştirmek için aygıtı yetkilendirecek.

Bu adım, Azure Resource Manager temelli komut dosyası kullanılarak gerçekleştirilir. Hizmet Yöneticisi yetki verilmesi uygun bir aygıtı seçebilirsiniz. Cihaz sonra hizmet verileri şifreleme anahtar değiştirme işlemini başlatmak için yetkili. 

Komut dosyası kullanma hakkında daha fazla bilgi için Git [Authorize ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Hangi cihazların hizmeti veri şifreleme anahtarları değiştirme yetkisi?
Hizmet verileri şifreleme anahtarı değişiklikleri başlatmak için yetkili önce bir aygıt aşağıdaki ölçütleri karşılamalıdır:

* Aygıt hizmeti veri şifreleme anahtarı değişiklik yetkilendirme için uygun olması için çevrimiçi olması gerekir.
* Anahtar değişikliği başlatılmaz, 30 dakika sonra yeniden aynı aygıt yetki verebilir.
* Anahtar değişikliği daha önceden yetkili aygıt tarafından başlatılmamış olması koşuluyla farklı bir cihaz yetki verebilir. Yeni cihaz yetkilendirildikten sonra eski aygıt değişikliği başlatamaz.
* Hizmet verileri şifreleme anahtarı geçiş işlemi sürerken bir aygıtı yetkilendirilemiyor.
* Başkalarının olmamasına karşın bazı hizmetine kayıtlı cihazlar üzerinde şifrelemeyi gezinirken bir aygıtı yetki verebilir. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>2. adım: Hizmet verileri şifreleme anahtarı değişikliğini başlatmak StorSimple için Windows PowerShell'i kullanın
Bu adım, StorSimple arabirimi yetkili StorSimple cihazında için Windows PowerShell'de gerçekleştirilir.

> [!NOTE]
> Anahtar geçişi tamamlanana kadar hiçbir işlem, StorSimple Yöneticisi hizmetiniz Azure portalında gerçekleştirilebilir.


Windows PowerShell arabirimine bağlamak için cihaz seri konsoluna kullanıyorsanız, aşağıdaki adımları gerçekleştirin.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Hizmet verileri şifreleme anahtarı değişikliğini başlatmak için
1. Tam erişimle oturum açmak için 1 seçeneğini belirleyin.
2. Komut istemine yazın:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Cmdlet başarıyla tamamlandıktan sonra yeni bir hizmet verileri şifreleme anahtarı alır. Kopyalayın ve bu anahtarı kullanmak için bu işlem 3. adımında kaydedin. Bu anahtar StorSimple Yöneticisi hizmetine kayıtlı kalan tüm aygıtlar güncelleştirmek için kullanılır.
   
   > [!NOTE]
   > Bu işlem, StorSimple cihaz yetkilendirme dört saat içinde başlatılmalıdır.
   > 
   > 
   
   Bu yeni anahtarı, ardından hizmetle kaydedilen tüm aygıtlara edilmesini hizmetine gönderilir. Bir uyarı sonra hizmet panosunda görüntülenir. Hizmet kayıtlı cihazlarda tüm işlemleri devre dışı bırakır ve cihaz Yöneticisi daha sonra diğer cihazlarda hizmet verileri şifreleme anahtarı güncelleştirmeniz gerekir. Ancak, g/ç (ana bilgisayardan verileri buluta gönderme) kesilmiş olabilir değil.
   
   Hizmete kayıtlı tek bir aygıtta varsa, geçiş işlemi tamamlanmıştır ve sonraki adıma atlayabilirsiniz. Birden çok aygıt hizmete kayıtlı varsa, 3. adıma geçin.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>3. adım: hizmet verileri şifreleme anahtarı başka StorSimple cihazlar üzerinde güncelleştir
StorSimple Yöneticisi hizmetinize kayıtlı birden çok aygıt varsa, bu adımları StorSimple Cihazınızı Windows PowerShell arabiriminde gerçekleştirilmesi gerekir. 2. adımda elde ettiğiniz anahtarı StorSimple Yöneticisi hizmetine kayıtlı tüm kalan StorSimple Cihazınızı güncelleştirmek için kullanılmalıdır.

Hizmet verileri şifreleme aygıtınızda güncelleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-update-the-service-data-encryption-key-on-physical-devices"></a>Hizmet verileri şifreleme anahtarı fiziksel cihazlarda güncelleştirmek için
1. StorSimple için Windows PowerShell konsoluna bağlanmak için kullanın. Tam erişimle oturum açmak için 1 seçeneğini belirleyin.
2. Komut istemine yazın:  `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Makalesinde aldığınız hizmet verileri şifreleme anahtarı sağlayan [2. adım: hizmet verileri şifreleme anahtarı değişikliğini başlatmak StorSimple için Windows PowerShell'i kullanın](#to-initiate-the-service-data-encryption-key-change).

#### <a name="to-update-the-service-data-encryption-key-on-all-the-80108020-cloud-appliances"></a>Hizmet verileri şifreleme anahtarı tüm 8010/8020 bulut cihazları üzerinde güncelleştirmek için
1. Karşıdan yükleme ve Kurulum [güncelleştirme CloudApplianceServiceEncryptionKey.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Update-CloudApplianceServiceEncryptionKey.ps1) PowerShell Betiği. 
2. PowerShell'i açın ve komut istemine yazın:  `Update-CloudApplianceServiceEncryptionKey.ps1 -SubscriptionId [subscription] -TenantId [tenantid] -ResourceGroupName [resource group] -ManagerName [device manager]`

Bu komut dosyası, bu hizmet verileri şifreleme anahtarı Aygıt Yöneticisi'ni altındaki tüm 8010/8020 bulut cihazları ayarlanan güvence altına alır.

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>Operations güncelleştirme 5.0'den önceki sürümleri çalıştıran cihazlarda desteklenir
Azure Portal'da, yalnızca güncelleştirme 5.0 ve üstünü çalıştıran StorSimple cihazlar desteklenir. Eski sürümlerini çalıştıran cihazlar sınırlı bir desteği. Azure portalına geçirdikten sonra hangi işlemleri güncelleştirme 5.0'den önceki sürümleri çalıştıran cihazlarda desteklenir anlamak için aşağıdaki tabloyu kullanın.

| İşlem                                                                                                                       | Desteklenen      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Cihaz kaydetme                                                                                                               | Evet            |
| Cihaz ayarları genel gibi ağ ve güvenlik yapılandırma                                                                | Evet            |
| Tarama, indirme ve güncelleştirmeleri yükle                                                                                             | Evet            |
| Aygıt devre dışı bırakma                                                                                                               | Evet            |
| Cihazı silme                                                                                                                   | Evet            |
| Oluşturma, değiştirme ve bir birim kapsayıcısı Sil                                                                                   | Hayır             |
| Oluşturma, değiştirme ve bir birim Sil                                                                                             | Hayır             |
| Oluşturma, değiştirme ve bir yedekleme ilkesi silme                                                                                      | Hayır             |
| El ile yedekleyin                                                                                                            | Hayır             |
| Zamanlanmış yedekleyin                                                                                                         | Uygulanamaz |
| Bir yedekleme kümesini geri yükleme                                                                                                        | Hayır             |
| Güncelleştirme 3.0 ve sonraki sürümleri çalıştıran bir cihaza kopyalama <br> Kaynak aygıt güncelleştirme 3.0 sürümünü çalıştırıyor.                                | Evet            |
| Update 3. 0'den önceki sürümleri çalıştıran bir cihazda kopyalamak                                                                          | Hayır             |
| Yük devretme kaynak aygıt olarak <br> (güncelleştirme 3.0 çalıştıran bir cihazda güncelleştirme 3.0 için önceki ve sonraki sürümünü çalıştıran bir aygıttan)                                                               | Evet            |
| Hedef aygıt olarak yük devretme <br> (güncelleştirme 3.0 önce yazılım sürümü çalıştıran bir cihaza)                                                                                   | Hayır             |
| Bir uyarı temizleyin                                                                                                                  | Evet            |
| Görünüm yedekleme ilkeleri, yedekleme kataloğu, birimler, birim kapsayıcıları, izleme grafikleri, işleri ve klasik Portalı'nda oluşturulan uyarılar | Evet            |
| Açma veya aygıt denetleyicileri kapatma                                                                                              | Evet            |


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple dağıtım işlemi](storsimple-8000-deployment-walkthrough-u2.md).
* Daha fazla bilgi edinmek [StorSimple depolama hesabınızı yönetme](storsimple-8000-manage-storage-accounts.md).
* Nasıl yapılır hakkında daha fazla bilgi [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).
