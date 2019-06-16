---
title: StorSimple 8000 serisi dağıtım sorunlarını giderme | Microsoft Docs
description: StorSimple'ı ilk kez dağıttığınızda oluşan hataları tanılayıp açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: f2b454e812db1eea686f82e92841163f1129b6c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64715224"
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>StorSimple cihaz dağıtım sorunlarını giderme
## <a name="overview"></a>Genel Bakış
Bu makalede, Microsoft Azure StorSimple dağıtımınız için yararlı sorun giderme kılavuzu verilmiştir. Bu, StorSimple yapılandırdığınızda, karşılaşabileceğiniz sorunları gidermenize yardımcı olmak için yaygın sorunlar, olası nedenler ve önerilen adımları açıklar. 

Bu bilgiler, StorSimple 8000 serisi fiziksel cihazının ve StorSimple bulut Gereci için geçerlidir.

> [!NOTE]
> Cihaza ilk kez dağıtmak veya cihaz uygulandığında bunların daha sonra oluşabilir, karşılaşabilecekleri cihaz yapılandırması ile ilgili sorunlar oluşabilir. Bu makalede, ilk kez dağıtım sorunlarını giderme üzerinde odaklanır. İşletimsel bir cihaz sorunlarını gidermek için Git [işletimsel bir cihaz sorunlarını gidermek için Tanılama aracını kullanma](storsimple-8000-diagnostics.md).

Bu makalede, ayrıca StorSimple dağıtımlarının sorunlarını gidermeye yönelik araçlar açıklar ve adım adım sorun giderme bir örnek sağlar.

## <a name="first-time-deployment-issues"></a>İlk kez dağıtım sorunları
Cihazınız ilk kez dağıtırken bir sorunla çalıştırırsanız, aşağıdakileri göz önünde bulundurun:

* Fiziksel bir cihaz gideriyorsanız, donanım yüklendi ve açıklanan şekilde yapılandırılmış emin emin [StorSimple 8100 cihazınızın yükleme](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md).
* Dağıtım önkoşulları denetleyin. Açıklanan tüm bilgilere sahip olduğunuzdan emin olun [dağıtım yapılandırma denetim listesi](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist).
* Sorun açıklanan görmek için StorSimple sürüm notlarını gözden geçirin. Sürüm Notları yükleme bilinen sorunlara yönelik geçici çözümler içerir. 

Cihaz dağıtımı sırasında yaygın yüz ortaya kullanıcıların Kurulum Sihirbazı'nı çalıştırdığınızda ve bunlar StorSimple için Windows PowerShell üzerinden cihazı ne zaman kaydedin verir. (StorSimple için Windows PowerShell kullanarak StorSimple aygıtınızı yapılandırmak ve kaydetmek için kullandığınız. Cihaz kaydı hakkında daha fazla bilgi için bkz. [3. adım: Yapılandırma ve StorSimple için Windows PowerShell aracılığıyla cihazınızın kaydetme](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Aşağıdaki bölümlerde, StorSimple cihaz ilk kez yapılandırırken, karşılaştığınız sorunları çözmenize yardımcı olabilir.

## <a name="first-time-setup-wizard-process"></a>İlk kez Kurulum Sihirbazı işlemi
Aşağıdaki adımlar, Kurulum Sihirbazı işlemi özetler. Ayrıntılı kurulum için bilgi [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

1. Çalıştırma [Invoke-hcssetupwizard'ı](https://technet.microsoft.com/library/dn688135.aspx) kalan aracılığıyla yönlendirecektir Kurulum sihirbazını başlatmak için cmdlet adımları. 
2. Ağ yapılandırma: Kurulum Sihirbazı, StorSimple Cihazınızda veri 0 ağ arabirimi için ağ ayarlarını yapılandırmanıza olanak sağlar. Bu ayarlar aşağıdakileri içerir:
   * Sanal IP (VIP), alt ağ maskesi ve ağ geçidi – [kümesi HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet'ini arka planda gerçekleştirilir. StorSimple cihazı IP adresi, alt ağ maskesi ve ağ geçidi için DATA 0 ağ arabirimindeki yapılandırır.
   * Birincil DNS sunucusu – [kümesi HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet'ini arka planda gerçekleştirilir. StorSimple çözümünüzü için DNS ayarlarını yapılandırır.
   * NTP sunucusu – [kümesi HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet'ini arka planda gerçekleştirilir. StorSimple çözümünüzü NTP sunucu ayarlarını yapılandırır.
   * İsteğe bağlı web proxy – [kümesi HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet'ini arka planda gerçekleştirilir. Bu ayarlar ve StorSimple çözümünüz için web proxy yapılandırmasını sağlar.
3. Parolanızı ayarlayın: sonraki adım, cihaz yönetici parolası ayarlamaktır.
   Cihaz Yöneticisi parolasını cihazınıza oturum açmak için kullanılır. Varsayılan cihaz parolası **Password1**’dir.
        
     > [!IMPORTANT]
     > Parola kaydı önce toplanan, ancak yalnızca cihaz başarıyla kaydettikten sonra uygulanır. Bir parola uygulamak için bir hata varsa, (karmaşıklık gereksinimleri karşılayan) gerekli parolaları toplanan kadar parolayı yeniden belirtmeniz istenir.
     
4. Cihazı kaydedin: Microsoft Azure'da çalışan StorSimple cihaz Yöneticisi hizmeti ile cihaz kaydetmek için son adımdır. Kayıt gerektiren [hizmet kayıt anahtarı alma](storsimple-8000-manage-service.md#get-the-service-registration-key) Azure portalından ve Kurulum Sihirbazı'nda sağlayın. **Cihaz başarıyla kaydedildikten sonra hizmet veri şifreleme anahtarı için sağlanır. Sonraki tüm cihazlara hizmete kaydolmak için gerekli olacağından bu şifreleme anahtarı güvenli bir konuma tuttuğunuzdan emin olun.**

## <a name="common-errors-during-device-deployment"></a>Cihaz dağıtım sırasında sık karşılaşılan hatalar
Aşağıdaki tablolar ne zaman karşılaşabileceğiniz genel hatalar listeler:

* Gerekli ağ ayarlarını yapılandırın.
* İsteğe bağlı web proxy ayarlarını yapılandırın.
* Cihaz Yöneticisi parolasını ayarlayın.
* Cihazı kaydedin.

## <a name="errors-during-the-required-network-settings"></a>Gerekli ağ ayarlarını sırasında hatalar
| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Invoke-hcssetupwizard'ı: Bu komut, yalnızca etkin denetleyicide çalıştırılabilir. |Yapılandırma edilgen denetleyiciye gerçekleştirildi. |Bu komut, etkin denetleyiciden çalıştırın. Daha fazla bilgi için [Cihazınızda etkin bir denetleyiciyi belirleme](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| 2 |Invoke-hcssetupwizard'ı: Cihaz hazır değil. |DATA 0 ağ bağlantısı sorunları vardır. |DATA 0 üzerindeki fiziksel ağ bağlantısını denetleyin. |
| 3 |Invoke-hcssetupwizard'ı: Ağdaki başka bir sistem ile bir IP adresi çakışması (HRESULT özel durum: 0x80070263). |DATA 0 için sağlanan IP zaten başka bir sistem tarafından kullanımda olan. |Kullanımda olmayan bir yeni IP sağlar. |
| 4 |Invoke-hcssetupwizard'ı: Küme kaynağı başarısız oldu. (HRESULT özel durum: 0x800713AE). |Yinelenen VIP. Sağlanan IP zaten kullanılıyor. |Kullanımda olmayan bir yeni IP sağlar. |
| 5 |Invoke-hcssetupwizard'ı: Geçersiz IPv4 adresi. |IP adresi, yanlış bir biçimde sağlanır. |Biçimini denetleyin ve yeniden IP adresinizi sağlayın. Daha fazla bilgi için [IPv4 adresleme][1]. |
| 6 |Invoke-hcssetupwizard'ı: IPv6 adresi geçersiz. |IP adresi, yanlış bir biçimde sağlanır. |Biçimini denetleyin ve yeniden IP adresinizi sağlayın. Daha fazla bilgi için [IPv6 adresleme][2]. |
| 7 |Invoke-hcssetupwizard'ı: Uç nokta eşleyicisinden kullanılabilir daha fazla uç nokta yok. (HRESULT özel durum: 0x800706D9) |Küme işlevselliğini çalışmıyor. |Sonraki adımlar için [Microsoft Desteği](storsimple-8000-contact-microsoft-support.md)'ne başvurun. |

## <a name="errors-during-the-optional-web-proxy-settings"></a>İsteğe bağlı web proxy ayarlarını sırasında hatalar
| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Invoke-hcssetupwizard'ı: Geçersiz parametre (HRESULT özel durum: 0x80070057) |Bir proxy ayarları için sağlanan parametreler geçerli değil. |URI, doğru biçimde sağlanmadı. Aşağıdaki biçimi kullanın: http:// *\<IP adresi veya web proxy sunucusu FQDN'si >* : *\<TCP bağlantı noktası numarası >* |
| 2 |Invoke-hcssetupwizard'ı: RPC sunucusu kullanılamıyor (HRESULT özel durum: 0x800706ba) |Kök nedeni aşağıdakilerden biridir:<ol><li>Küme en fazla değil.</li><li>Edilgen denetleyici etkin denetleyiciyle iletişim kuramıyor ve komutunun pasif denetleyicisinden çalıştırılır.</li></ol> |Kök nedenine bağlı olarak:<ol><li>[Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) kümesi olduğundan emin olmak için.</li><li>Komut etkin denetleyiciden çalıştırın. Pasif denetleyicisinden komutu çalıştırmak istiyorsanız, edilgen denetleyici etkin denetleyiciyle iletişim kurabildiğinden emin olmak gerekir. Şunları yapmanız gerekir [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) varsa bu bağlantı bozuk.</li></ol> |
| 3 |Invoke-hcssetupwizard'ı: RPC çağrısı başarısız oldu (HRESULT özel durum: 0X800706be) |Küme çalışmıyor. |[Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) kümesi olduğundan emin olmak için. |
| 4 |Invoke-hcssetupwizard'ı: Küme kaynağı bulunamadı (HRESULT özel durum: 0x8007138f) |Küme kaynağı bulunamadı. Yüklemenin doğru değildi, bu durum oluşabilir. |Cihazı varsayılan fabrika ayarlarına sıfırlamanız gerekebilir. [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) küme kaynağı oluşturmak için. |
| 5 |Invoke-hcssetupwizard'ı: Küme kaynağı çevrimiçi değil (HRESULT özel durum: 0x8007138c) |Küme kaynaklarını çevrimiçi değil. |Sonraki adımlar için [Microsoft Desteği](storsimple-8000-contact-microsoft-support.md)'ne başvurun. |

## <a name="errors-related-to-device-administrator-password"></a>Cihaz Yöneticisi parolası için ilgili hataları
Varsayılan cihaz Yöneticisi parolası **Password1**. Bu parola süresi dolduktan sonra ilk oturum açma; Bu nedenle, değiştirmek için Kurulum Sihirbazı'nı kullanmanız gerekecektir. Cihaz ilk kez kaydolduğunuzda yeni bir cihaz Yöneticisi parolasını sağlamanız gerekir. 

Parolalarınızı aşağıdaki gereksinimleri karşıladığından emin olun:

* Cihaz Yöneticisi parolanızın 8-15 karakter uzunluğunda olmalıdır.
* Parolalar, 3, 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harfler, sayısal ve özel. 
* Parolanızı son 24 parola ile aynı olamaz.

Buna parolalar, her yıl sona ve yalnızca cihaz başarıyla kaydettikten sonra değiştirilebilir göz önünde bulundurun. Kayıt için herhangi bir nedenle başarısız olursa, parolaları değiştirilmez.

Cihaz Yöneticisi parolası ile ilgili daha fazla bilgi için Git [StorSimple parolanızı değiştirmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-change-passwords.md).

Cihaz yöneticisini ve StorSimple Snapshot Manager parolalarını ayarlanırken bir veya daha fazla şu hatalarla karşılaşabilirsiniz.

| Hayır. | Hata iletisi | Önerilen eylem |
| --- | --- | --- |
| 1 |Parola en fazla uzunluğu aşıyor. |Cihaz Yöneticisi parolanızın 8-15 karakter uzunluğunda olmalıdır. |
| 2 |Parola gerekli uzunluğa karşılamıyor. |Cihaz Yöneticisi parolanızın 8-15 karakter uzunluğunda olmalıdır.|
| 3 |Parola küçük harf karakterler içermelidir. |Parolalar, 3, 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harfler, sayısal ve özel. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 4 |Parola, sayısal karakterler içermelidir. |Parolalar, 3, 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harfler, sayısal ve özel. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 5 |Parola, özel karakterler içermelidir. |Parolalar, 3, 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harfler, sayısal ve özel. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 6 |Parola 3 4 karakter türlerinden birini içermelidir: büyük harf, küçük harfler, sayısal ve özel. |Parolanızı karakter gerekli türlerini içermiyor. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 7 |Parametresi, onay eşleşmiyor. |Parolanızı tüm gereksinimlerini karşıladığından ve onu doğru girildiğinden emin olun. |
| 8 |Parolanızı varsayılan aynı olamaz. |Varsayılan parola *Password1*. İlk kez oturum açtıktan sonra bu parolayı değiştirmeniz gerekir. |
| 9 |Cihaz parolasının girdiğiniz parola eşleşmiyor. Lütfen parolayı yeniden yazın. |Parolayı denetleyin ve yeniden yazın. |

Cihaz kayıtlıysa ancak yalnızca başarılı kayıt sonrasında uygulanır önce parolaları toplanır. Parola kurtarma iş akışı cihazın kayıtlı olması gerekir.

> [!IMPORTANT]
> Geçerli bir parola girişimi başarısız olursa, genel olarak, daha sonra yazılım art arda başarılı olana kadar parola toplamak çalışır. Nadir durumlarda, parola uygulanamaz. Bu durumda, cihazı kaydetmek ve devam edin, ancak parolalar değiştirilmeyecektir. Kayıttan sonra Azure portalında cihaz Yöneticisi parolasını değiştirebilirsiniz.


Azure portalında StorSimple cihaz Yöneticisi hizmeti aracılığıyla parolayı sıfırlayabilirsiniz. Daha fazla bilgi için Git [cihaz Yöneticisi parolasını değiştirme](storsimple-8000-change-passwords.md#change-the-device-administrator-password).

## <a name="errors-during-device-registration"></a>Cihaz kaydı sırasında karşılaşılan hatalar
Microsoft Azure'da çalışan StorSimple cihaz Yöneticisi hizmeti, cihaz kaydetmek için kullanın. Cihaz kaydı sırasında bir veya daha fazla aşağıdaki sorunlarla karşılaşabilirsiniz.

| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |350027\. hata: Cihaz StorSimple cihaz Yöneticisi ile kaydetme başarısız oldu. | |Birkaç dakika bekleyin ve sonra işlemi yeniden deneyin. Sorun devam ederse [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md). |
| 2 |350013\. hata: Cihaz kaydedilirken bir hata oluştu. Bu, hizmet kayıt anahtarının hatalı nedeniyle olabilir. | |Lütfen cihazı doğru hizmet kayıt anahtarıyla yeniden kaydedin. Daha fazla bilgi için [hizmet kayıt anahtarını alın.](storsimple-8000-manage-service.md#get-the-service-registration-key) |
| 3 |350063\. hata: Kimlik doğrulaması için StorSimple cihaz Yöneticisi hizmetine geçirilen ancak kayıt başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. |ACS ile kimlik doğrulaması başarılı ancak hizmete yapılan kayıt çağrı başarısız oldu, bu hata gösterir. Bu işlem sonucunda ara sıra bir sorun olabilir. |Sorunu ederse, lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md). |
| 4 |350049\. hata: Kayıt sırasında hizmete erişilemedi. |Hizmete çağrı yapıldığında, bir web özel durum aldı. Bazı durumlarda, bu işlemi daha sonra yeniden denenerek düzeltilecek. |Lütfen IP adresi ve DNS adını denetleyin ve sonra işlemi yeniden deneyin. Sorun devam ederse [Microsoft Support başvurun.](storsimple-8000-contact-microsoft-support.md) |
| 5 |350031\. hata: Cihaz zaten kayıtlı. | |Hiçbir eylem gerekmiyor. |
| 6 |350016\. hata: Cihaz kaydı başarısız oldu. | |Lütfen kayıt anahtarının doğru olduğundan emin olun. |
| 7 |Invoke-hcssetupwizard'ı: Cihazınız kaydedilirken bir hata oluştu; Bu, hatalı IP adresi veya DNS adı nedeniyle olabilir. Lütfen ağ ayarlarınızı denetleyin ve yeniden deneyin. Sorun devam ederse [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md). (Hata 350050) |Cihazınızın Ağ dışından ping atabilirsiniz emin olun. Kayıt, dış ağ bağlantısı yoksa bu hata ile başarısız olabilir. Bu hata, bir veya daha fazlasını bir birleşimi olabilir:<ul><li>Hatalı IP</li><li>Yanlış alt ağ</li><li>Hatalı ağ geçidi</li><li>DNS ayarları yanlış</li></ul> |Açıklanan adımları izleyerek [adım adım sorun giderme örnek](#step-by-step-storsimple-troubleshooting-example). |
| 8 |Invoke-hcssetupwizard'ı: Geçerli işlem bir [0x1FBE2] iç hizmet hatası nedeniyle başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse lütfen Microsoft Support başvurun. |Bu, hizmeti veya aracı için tüm kullanıcı görünmez hataları oluşturulur, genel bir hatadır. En yaygın nedeni, ACS kimlik doğrulaması başarısız oldu olabilir. Hatanın olası nedeni NTP sunucusu yapılandırılması ile ilgili sorunları vardır ve cihaz üzerinde zaman doğru ayarlanmamış olmasıdır. |(Bir sorun varsa) saati düzeltin ve ardından kayıt işlemi yeniden deneyin. Saat dilimini ayarlamak için Set-HcsSystem - Timezone komutunu kullanırsanız, saat dilimi (örneğin "Pasifik Standart Saati") her sözcüğün büyük harfe Dönüştür.  Bu sorun devam ederse [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) sonraki adımlar için. |
| 9 |Uyarı: Cihaz etkinleştirilemedi. Cihaz yöneticinize ve StorSimple Snapshot Manager parolalar değiştirilmedi. |Kayıt başarısız olursa cihaz yöneticisini ve StorSimple Snapshot Manager parolalar değiştirilmez. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>StorSimple dağıtımlarının sorunlarını gidermeye yönelik Araçlar
StorSimple, StorSimple çözümünüzü gidermek için kullanabileceğiniz çeşitli araçlar içerir. Bunlar:

* Paketler ve cihaz günlükleri destekler.
* Sorun giderme için özel olarak tasarlanmış cmdlet'leri.

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Paketler ve cihaz günlükleri kullanılabilir sorun giderme için destek
Destek Paketi cihaz sorunlarını giderme ile Microsoft Support ekibine yardımcı olabilecek ilgili günlükleri içerir. StorSimple için Windows PowerShell, ardından destek personeli ile paylaşabileceğiniz bir şifrelenmiş destek paketi oluşturmak için kullanabilirsiniz.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Günlükleri veya destek paketinin içeriği görüntülemek için
1. Bölümünde anlatıldığı gibi bir destek paketi oluşturmak için StorSimple için Windows PowerShell kullanmak [oluşturun ve bir destek paketi yönetme](storsimple-8000-create-manage-support-package.md).
2. İndirme [şifre çözme betik](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) istemci bilgisayarınızda yerel olarak.
3. Bunu kullanın [adım adım yordam](storsimple-8000-create-manage-support-package.md#edit-a-support-package) açın ve Destek Paketi şifresini çözmek için.
4. Şifresi çözülmüş destek paketi günlükleri etw/etvx biçimindedir. Bu dosyalar Windows Olay Görüntüleyicisi'nde görüntülemek için aşağıdaki adımları gerçekleştirebilirsiniz:
   
   1. Çalıştırma **eventvwr** Windows istemciniz üzerinde komutu. Bu Olay Görüntüleyicisi'ni başlatır.
   2. İçinde **eylemleri** bölmesinde tıklayın **açık kaydedilmiş günlük** ve işaret (Destek Paketi) biçiminde etvx/etw günlük dosyalarına. Artık dosya görüntüleyebilirsiniz. Dosyayı açtıktan sonra sağ tıklayın ve dosyayı metin olarak kaydedin.
      
      > [!IMPORTANT]
      > Ayrıca **Get-WinEvent** cmdlet'ini bu dosyalar Windows PowerShell'de açın. Daha fazla bilgi için [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) Windows PowerShell cmdlet'i başvuru belgelerinde.
     
5. Günlükleri Olay Görüntüleyicisi'nde açtığınızda, cihaz yapılandırması ile ilgili sorunları içeren aşağıdaki günlüklere bakın:
   
   * hcs_pfconfig/Operational günlüğünü
   * hcs_pfconfig/Config
6. Kurulum Sihirbazı tarafından adlandırılan cmdlet'leri ilgili dizeler için günlük dosyalarında arayın. Bkz: [ilk kez Kurulum Sihirbazı işlemi](#first-time-setup-wizard-process) bu cmdlet'lerin bir listesi.
7. Sorunun nedenini anlamak mümkün değilse, yapabilecekleriniz [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) sonraki adımlar için. İçindeki adımları kullanın [bir destek isteği oluşturma](storsimple-8000-contact-microsoft-support.md#create-a-support-request) , sizinle Microsoft Support için Yardım.

## <a name="cmdlets-available-for-troubleshooting"></a>Sorun giderme için uygun cmdlet'leri
Bağlantı hataları algılamak için aşağıdaki Windows PowerShell cmdlet'lerini kullanın.

* `Get-NetAdapter`: Ağ arabirimleri durumunu algılamak için bu cmdlet'i kullanın.
* `Test-Connection`: İçindeki ve dışındaki ağ ağ bağlantısını denetlemek için bu cmdlet'i kullanın.
* `Test-HcsmConnection`: Başarıyla kayıtlı bir cihazın bağlantısını kontrol etmek için bu cmdlet'i kullanın.
* `Sync-HcsTime`: Cihaz saatini görüntülemek ve NTP sunucusuyla zaman eşitlemeye zorlamak için bu cmdlet'i kullanın.
* `Enable-HcsPing` ve `Disable-HcsPing`: StorSimple Cihazınızda ağ arabirimleri ping ana izin vermek için bu cmdlet'leri kullanın. Varsayılan olarak, StorSimple ağ arabirimleri ping isteklere yanıt vermez.
* `Trace-HcsRoute`: Bu cmdlet, bir rota izleme aracı kullanın. Bir süre son hedefine her yönlendiriciye paketleri gönderir ve ardından her adımdan döndürülen paketlerin göre sonuçları hesaplar. Bu yana `Trace-HcsRoute` hangi yönlendiriciler saptayabilirler veya bağlantıları ağ sorunlara neden herhangi belirli bir yönlendirici veya bağlantı, paket kaybı derecesini gösterir.
* `Get-HcsRoutingTable`: Yerel IP yönlendirme tablosu görüntülemek için bu cmdlet'i kullanın.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Get-NetAdapter cmdlet'iyle sorun giderme
Ağ arabirimleri için bir cihaz ilk kez dağıtım yapılandırdığınızda, cihaz ile hizmet henüz kaydedilmediğinden donanım durumunu StorSimple cihaz Yöneticisi hizmeti kullanıcı Arabirimi kullanılabilir değil. Ayrıca, **donanım sistem durumu** dikey değil her zaman doğru yansıtacak cihaz durumunu özellikle eşitleme hizmeti etkileyen bir sorun varsa. Bu durumda, kullandığınız `Get-NetAdapter` cmdlet'ini sağlığı ve durumu, ağ arabirimleri belirleyin.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Cihazınızda tüm ağ bağdaştırıcılarının bir listesini görmek için
1. StorSimple için Windows PowerShell'i başlatın ve ardından yazın `Get-NetAdapter`. 
2. Çıktısını kullanın `Get-NetAdapter` cmdlet'i ve ağ Arabiriminizin durumunu anlamak için aşağıdaki yönergeleri.
   
   * Arabirim sağlıklı ve etkin ise **İfındex** durumu olarak gösterildiği **yukarı**.
   * Arabirim kötü durumda, ancak (bir ağ kablosu), fiziksel olarak bağlı değil **İfındex** olarak gösterilen **devre dışı bırakılmış**.
   * Arabirim sağlıklı ancak etkin değil ise **İfındex** durumu olarak gösterildiği **NotPresent**.
   * Arabirimi yoksa, bu listede görünmüyor. StorSimple cihaz Yöneticisi hizmeti kullanıcı Arabirimi, başarısız durumda yine de bu arabirimi gösterilir.

Bu cmdlet'in nasıl kullanılacağı hakkında daha fazla bilgi için Git [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter?view=win10-ps) Windows PowerShell cmdlet başvurusu.

Aşağıdaki bölümlerde çıktısını örnekleri Göster `Get-NetAdapter` cmdlet'i.

 Bu örnekler denetleyici 0 edilgen denetleyici oluştu ve şu şekilde yapılandırıldı:

* DATA 0, verileri 1, veri 2 ve DATA 3 ağ arabirimleri cihazda vardı.
* Veri 4 ve 5 veri ağ arabirim kartları mevcut değil; Bu nedenle, çıkış listelenmemiştir.
* DATA 0 etkinleştirildi.

Denetleyici 1 etkin denetleyiciyi oluştu ve şu şekilde yapılandırılmışsa:

* DATA 0, verileri 1, veri 2, veri 3, veri 4 ve veri 5 ağ arabirimleri cihazda vardı.
* DATA 0 etkinleştirildi.

**Örnek çıktı – denetleyici 0**

Denetleyici 0 (Edilgen denetleyici) çıktısını verilmiştir. Veri 1, veri 2 ve DATA 3 bağlı değil. Cihazda mevcut olmadığı için veri 4 ve 5 veri listelenmemiştir.

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Örnek çıktı – Denetleyici 1**

Denetleyici 1 (etkin denetleyiciyi) çıktısını verilmiştir. Yalnızca cihazın ağ arabiriminde yapılandırılan DATA 0 ve çalışma.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent


## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Test-Connection cmdlet'iyle sorun giderme
Kullanabileceğiniz `Test-Connection` StorSimple Cihazınızı dış ağa bağlanıp bağlanamayacaklarını belirlemek için cmdlet'i. DNS de dahil olmak üzere ağ tüm parametreleri Kurulum Sihirbazı'nda doğru yapılandırılmış olması halinde kullanabileceğiniz `Test-Connection` outlook.com gibi ağ dışına bilinen bir adresine ping atmayı cmdlet'i.

Ping devre dışı bırakılırsa, bu cmdlet ile ilgili bağlantı sorunlarını gidermek ping etkinleştirmeniz gerekir.

Çıktısı aşağıdaki örneklere bakın `Test-Connection` cmdlet'i.

> [!NOTE]
> İlk örnekte, cihazın yanlış bir DNS ile yapılandırılır. İkinci örnekte, DNS doğrudur.

**Örnek çıktı – yanlış DNS**

Aşağıdaki örnekte, IPv4 ve IPv6 adresleri için DNS çözümlenmedi gösteren çıktı yok. Bu, dış ağ bağlantısı yok ve doğru DNS @username anlamına gelir.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Örnek çıktı – doğru DNS**

Aşağıdaki örnekte, DNS, DNS doğru şekilde yapılandırıldığını gösteren IPv4 adresini döndürür. Bu, dış ağ bağlantısı olduğunu doğrular.

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Test-HcsmConnection cmdlet'iyle sorun giderme
Kullanım `Test-HcsmConnection` zaten bağlı olan ve StorSimple cihaz Yöneticisi hizmetine kayıtlı bir cihaz için cmdlet'i. Bu cmdlet, kayıtlı bir cihaz ve karşılık gelen StorSimple cihaz Yöneticisi hizmeti arasındaki bağlantının doğrulamanıza yardımcı olur. StorSimple için Windows PowerShell'i temel bu komutu çalıştırabilirsiniz.

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Test-HcsmConnection cmdlet'ini çalıştırmak için
1. Cihaz kayıtlı olduğundan emin olun.
2. Cihaz durumunu kontrol edin. Cihaz, Bakım modunda veya çevrimdışı devre dışı bırakılmışsa aşağıdaki hatalardan birini görebilirsiniz:
   
   * ErrorCode.CiSDeviceDecommissioned – bu cihaz devre dışı olduğunu belirtir.
   * ErrorCode.DeviceNotReady – bu cihaz bakım modunda olduğunu gösterir.
   * ErrorCode.DeviceNotReady – Bu, cihazın çevrimiçi olduğunu gösterir.
3. StorSimple cihaz Yöneticisi hizmetinin çalıştığını doğrulayın (kullanın [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) cmdlet'i). Hizmet çalışmıyorsa şu hatalar görebilirsiniz:
   
   * ErrorCode.CiSApplianceAgentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – Bu, Get-ClusterResource çalıştırdığınızda bir özel durum olduğunu gösterir.
4. Access Control Service (ACS) belirteci denetleyin. Bir web özel durum oluşturursa, ağ geçidi ile ilgili bir sorun, eksik bir ara sunucu kimlik doğrulaması, yanlış bir DNS veya kimlik doğrulama hatası sonucu olabilir. Aşağıdaki hatalar görebilirsiniz:
   
   * ErrorCode.CiSApplianceGateway – bu gösterir HttpStatusCode.BadGateway özel durum: ad Çözümleyici hizmeti konak adı çözümlenemedi.
   * ErrorCode.CiSApplianceProxy – bu gösterir (HTTP durum kodu 407) HttpStatusCode.ProxyAuthenticationRequired özel durum: proxy sunucusu ile istemci kimliğini doğrulayamadı.
   * ErrorCode.CiSApplianceDNSError – bu gösterir WebExceptionStatus.NameResolutionFailure özel durum: ad Çözümleyici hizmeti konak adı çözümlenemedi.
   * ErrorCode.CiSApplianceACSError – bu hizmet, bir kimlik doğrulama hatası döndürdü, ancak bağlantısı gösterir.
     
     Bir web özel durum oluşturmaz, ErrorCode.CiSApplianceFailure için denetleyin. Bu gereç başarısız olduğunu gösterir.
5. Bulut hizmeti bağlantısını denetleyin. Hizmetin web özel durum oluşturursa aşağıdaki hataları görebilirsiniz:
   
   * ErrorCode.CiSApplianceGateway – bu gösterir HttpStatusCode.BadGateway özel durum: bir ara proxy sunucusunun başka bir proxy veya özgün sunucudan hatalı bir istek aldı.
   * ErrorCode.CiSApplianceProxy – bu gösterir (HTTP durum kodu 407) HttpStatusCode.ProxyAuthenticationRequired özel durum: proxy sunucusu ile istemci kimliğini doğrulayamadı.
   * ErrorCode.CiSApplianceDNSError – bu gösterir WebExceptionStatus.NameResolutionFailure özel durum: ad Çözümleyici hizmeti konak adı çözümlenemedi.
   * ErrorCode.CiSApplianceACSError – bu hizmet, bir kimlik doğrulama hatası döndürdü, ancak bağlantısı gösterir.
     
     Bir web özel durum oluşturmaz, ErrorCode.CiSApplianceSaasServiceError için denetleyin. Bu, StorSimple cihaz Yöneticisi hizmeti ile bir sorun olduğunu gösterir.
6. Azure Service Bus bağlantısını denetleyin. Cihaz için Service Bus bağlantı kurulamıyor ErrorCode.CiSApplianceServiceBusError gösterir.

CiSCommandletLog0Curr.errlog günlük dosyaları ve özel durum ayrıntıları gibi daha fazla bilgi için CiSAgentsvc0Curr.errlog gerekir.

Bu cmdlet'in nasıl kullanılacağı hakkında daha fazla bilgi için Git [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) Windows PowerShell başvuru belgeleri.

> [!IMPORTANT]
> Hem etkin hem de edilgen denetleyici için bu cmdlet'i çalıştırabilirsiniz.

Çıktısı aşağıdaki örneklere bakın `Test-HcsmConnection` cmdlet'i.

**Örnek çıktı – başarıyla kayıtlı cihaz StorSimple güncelleştirme 3'ü çalıştırma**

      Controller1>Test-HcsmConnection

      Checking device registration state  ... Success
      Device registered successfully

      Checking primary NTP server [time.windows.com] ... Success

      Checking web proxy  ... NOT SET

      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET

      Checking device online  ... Success

      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success

      Checking connectivity from device to service  ... This will take a few minutes.

      Checking connectivity from device to service  ... Success

      Checking connectivity from service to device  ... Success

      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Örnek çıktı – çevrimdışı cihaz** 

Bu bir örnektir durumuna sahip bir CİHAZDAN **çevrimdışı** Azure portalında.

     Checking device registrationstate: Success
     Device is registered successfully
     Checking connectivity from device to SaaS.. Failure

Cihaz, geçerli web proxy yapılandırması'nı kullanarak bağlanamadı. Bu web proxy yapılandırmasını ya da bir ağ bağlantısı sorunu ile ilgili bir sorun olabilir. Bu durumda, web proxy ayarlarınızın doğru olduğundan ve web proxy sunucularınızın çevrimiçi ve ulaşılabilir olduğundan emin olmanız gerekir.

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Sync-HcsTime cmdlet'iyle sorun giderme
Cihaz saatini görüntülemek için bu cmdlet'i kullanın. Cihaz saatini NTP sunucusuyla bir uzaklık varsa, zorla-saatini NTP sunucunuzla eşitlemek için bu cmdlet'i kullanabilirsiniz.
- Cihaz ve NTP sunucusu arasındaki uzaklığı 5 dakikadan daha büyükse bir uyarı görürsünüz. 
- 15 dakika uzaklığı aşarsa, aygıtın çevrimdışı olarak geçer. Zaman eşitlemeye zorlamak için bu cmdlet'i kullanmaya devam edebilirsiniz. 
- 15 saat uzaklığı aşarsa, ancak daha sonra zorla saati ve bir hata iletisi gösterilir eşitleme için mümkün olmayacaktır.

**Örnek çıktı – Sync-HcsTime Kullanarak zorlamalı zaman eşitleme**

     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>HcsPing Enable ve Disable-HcsPing cmdlet'lerle ilgili sorunları giderme
Cihazınızdaki ağ arabirimleri ICMP ping isteklerine yanıt vermesini sağlamak için bu cmdlet'leri kullanın. Varsayılan olarak, StorSimple ağ arabirimleri ping isteklere yanıt vermez. Bu cmdlet kullanarak, Cihazınızı çevrimiçi ve erişilebilir olduğunu anlamak için en kolay yoludur.

**Örnek çıktı – HcsPing Enable ve Disable-HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>İzleme HcsRoute cmdlet'iyle sorun giderme
Bu cmdlet, bir rota izleme aracı kullanın. Bir süre son hedefine her yönlendiriciye paketleri gönderir ve ardından her adımdan döndürülen paketlerin göre sonuçları hesaplar. Cmdlet'i belirtilen bir yönlendirici veya bağlantı paket kaybı derecesini gösterir çünkü hangi yönlendiriciler saptayabilirler veya bağlantılar, ağ sorunlarına neden.

**İzleme HcsRoute olan bir paket rotası izleme gösteren örnek çıktı**

     Controller0>Trace-HcsRoute -Target 10.126.174.25

     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]

     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]

     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Get-HcsRoutingTable cmdlet'iyle sorun giderme
StorSimple cihazınız için yönlendirme tablosunu görüntülemek için bu cmdlet'i kullanın. Yönlendirme tablosunu bir veri paketi bir Internet Protokolü (IP) ağ üzerinden yolculuk burada yönlendirilirsiniz belirlemeye yardımcı olabilir bir kurallar kümesidir.

Yönlendirme tablosu, arabirimler ve ağ geçidi için belirtilen ağ verilerini yönlendirir gösterir. Ayrıca, belirli bir hedefe ulaşmak için geçen yoluna karar mercii olan Yönlendirme ölçüm sağlar. Alt yönlendirme ölçüm, daha yüksek öncelik.

Örneğin, 2 ağ arabirimi varsa, veri 2 ve DATA 3 Internet'e bağlı. 15 ve 261 veri 2 ve DATA 3 için yönlendirme ölçüleri sırasıyla olması halinde sonra veri 2 yönlendirme düşük ölçüye sahip İnternet'e erişmek için kullanılan tercih edilen arabirimdir.

DATA 0 ağ arabiriminiz, StorSimple Cihazınızda güncelleştirme 1 çalıştırıyorsanız, bulut trafiği için en yüksek tercih sahiptir. Bu, bulut trafiği olsa bile bulut özellikli diğer arabirimleri, 0 VERİLERİNE yönlendirilirdi anlamına gelir.

Çalıştırırsanız `Get-HcsRoutingTable` cmdlet (aşağıdaki örnekte gösterildiği) herhangi bir parametre belirtmeden cmdlet IPv4 ve IPv6 yönlendirme tablolarını çıkarır. Alternatif olarak, belirtebileceğiniz `Get-HcsRoutingTable -IPv4` veya `Get-HcsRoutingTable -IPv6` ilgili bir yönlendirme tablosu alınamıyor.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================

      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================

      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None

      Controller0>

## <a name="step-by-step-storsimple-troubleshooting-example"></a>StorSimple adım adım sorun giderme örneği
Aşağıdaki örnek, bir StorSimple dağıtımını adım adım sorun giderme gösterir. Örnek senaryoda, cihaz kaydı, bir ileti ağ ayarlarını veya DNS adı yanlış olduğunu belirten hata ile başarısız olur.

Döndürülen hata iletisi şudur:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Hata nedeni aşağıdakilerden biri olabilir:

* Yanlış donanım yükleme
* Hatalı ağ arabirimleri
* Hatalı IP adresi, alt ağ maskesi, ağ geçidi, birincil DNS sunucusu veya web Ara sunucusu
* Hatalı kayıt anahtarı
* Yanlış güvenlik duvarı ayarları

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Bulmak ve cihaz kaydı sorunu gidermek için
1. Cihaz yapılandırmanızı denetleyin: etkin denetleyicide çalıştırın `Invoke-HcsSetupWizard`.
   
   > [!NOTE]
   > Kurulum Sihirbazı, etkin denetleyicide çalıştırmanız gerekir. Etkin denetleyiciye bağlı olduğunu doğrulamak için seri konsol içinde sunulan başlığı bakın. Başlık, denetleyici 0 veya 1 denetleyicisine bağlı olduğunuzu ve denetleyici etkin veya Pasif olup gösterir. Daha fazla bilgi için Git [Cihazınızda etkin bir denetleyiciyi belirleme](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
   
2. Cihazın düzgün kablolu emin olun: cihaza geri düzlemi kablo ağı denetleyin. Kablolama, cihaz modeli için geçerlidir. Daha fazla bilgi için Git [StorSimple 8100 cihazınızın yükleme](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md).
   
   > [!NOTE]
   > 10 GbE ağ bağlantı noktası kullanıyorsanız, SFP kablolar ve sağlanan QSFP SFP bağdaştırıcıları'nı kullanmanız gerekir. Daha fazla bilgi için [kablolar ve anahtarlar vericilerinin 10 GbE bağlantı noktaları için önerilen listesini](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
  
3. Ağ arabirimi durumunu doğrulayın:
   
   * DATA 0 için ağ arabirimlerini durumunu algılamak için Get-NetAdapter cmdlet'ini kullanın. 
   * Bağlantı çalışmıyorsa **ifındex** durumu, arabirimin kapalı olduğunu gösterecektir. Gereç ve anahtar bağlantı noktasının ağ bağlantısını denetleyin ardından gerekecektir. Hatalı kablolar kural gerekecektir. 
   * DATA 0 üzerindeki bağlantı noktası şüpheleniyorsanız etkin denetleyiciyi başarısız oldu, bu Denetleyici 1 üzerinde 0 bağlantı noktası VERİLERE bağlanarak doğrulayın. Bunu doğrulamak için ağ cihazı arkasından denetleyici 0'dan kablosunu, denetleyici 1 kablo bağlayın ve sonra Get-NetAdapter cmdlet'ini yeniden çalıştırın.
     Veri 0 denetleyicisi üzerinde bağlantı noktası başarısız olur, [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) sonraki adımlar için. Denetleyici sisteminize değiştirmeniz gerekebilir.
4. Anahtar bağlantısını doğrulayın:
   
   * Denetleyici 0 ve denetleyici 1 birincil, muhafazada veri 0 ağ arabirimleri aynı alt ağda olduğundan emin olun. 
   * Yönlendirici ve hub bakın. Genellikle, her iki denetleyicilerinin aynı hub veya yönlendirici bağlanmanız gerekir. 
   * Bağlantı için kullandığınız anahtarları aynı vLAN her iki denetleyicilerinin için DATA 0 olduğundan emin olun.
5. Kullanıcı hataları kaldırın:
   
   * Kurulum Sihirbazı'nı yeniden çalıştırın (çalıştırma **Invoke-hcssetupwizard'ı**), yeniden hiçbir hata olmadığından emin olmak için değerleri girin. 
   * Kayıt doğrulama kullanılan anahtar. Aynı kayıt anahtarı, birden çok cihaz StorSimple cihaz Yöneticisi hizmetine bağlanmak için kullanılabilir. Yordamı kullanın [hizmet kayıt anahtarı alma](storsimple-8000-manage-service.md#get-the-service-registration-key) doğru kayıt anahtarı kullandığınızdan emin olmak için.
     
     > [!IMPORTANT]
     > Çalıştıran birden çok hizmeti varsa, cihazı kaydetmek için kayıt anahtarını uygun hizmet için kullanıldığından emin olmak gerekir. Yanlış StorSimple cihaz Yöneticisi hizmeti ile bir cihaz kayıtlıysanız, gerekecektir [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) sonraki adımlar için. Daha sonra hedef hizmete bağlanmak için (veri kaybına neden) cihazın Fabrika sıfırlaması gerçekleştirmek gerekebilir.
     > 
     > 
6. Dış ağ bağlantısına sahip olduğunuzu doğrulamak için Test-Connection cmdlet'ini kullanın. Daha fazla bilgi için Git [Test-Connection cmdlet'iyle sorun giderme](#troubleshoot-with-the-test-connection-cmdlet).
7. Güvenlik Duvarı Engelleme denetleyin. Doğruladığınız sanal IP (VIP), alt ağ, ağ geçidi ve DNS ayarları tüm doğru olduğundan ve bağlantı sorunları görmeye sonra güvenlik duvarınızı Cihazınızı dış ağ arasındaki iletişimi engelliyor olabilir. StorSimple Cihazınızda giden iletişim için 80 ve 443 bağlantı noktaları bulunduğundan emin olmanız gerekir. Daha fazla bilgi için [StorSimple cihazınız için ağ gereksinimleri](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).
8. Günlüklere bakın. Git [desteği paketleri ve cihaz günlükleri kullanılabilir sorun giderme için](#support-packages-and-device-logs-available-for-troubleshooting).
9. Yukarıdaki adımlar sorunu çözmezse [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) Yardım almak için.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple cihaz sorunlarını gidermek için Tanılama Aracı'nı kullanmayı öğrenin](storsimple-8000-diagnostics.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
