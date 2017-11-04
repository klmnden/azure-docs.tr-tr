---
title: "StorSimple dağıtım sorunlarını giderme | Microsoft Docs"
description: "Tanılama ve StorSimple ilk kez dağıttığınızda oluşan hataları düzeltme açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f8352eaa-193c-42d1-8818-0b8e02d8485d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: 230a652ceca8b4643d1984d81383c6628b8e1f5f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-storsimple-device-deployment-issues"></a>StorSimple cihaz dağıtım sorunlarını giderme
## <a name="overview"></a>Genel Bakış
Bu makalede, Microsoft Azure StorSimple dağıtımınız için yararlı sorun giderme kılavuzu verilmiştir. Sık karşılaşılan sorunları, olası nedenler ve önerilen adımları StorSimple yapılandırdığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olması için açıklar. Bu bilgiler, StorSimple şirket içi fiziksel cihazı ve StorSimple sanal cihazı için geçerlidir.

> [!NOTE]
> Yüz aygıt yapılandırması ile ilgili sorunlar, cihaz ilk kez dağıtırken veya aygıt çalışır durumda bunlar daha sonra ortaya çıkabilir ortaya çıkabilir. Bu makalede, ilk kez dağıtım sorunlarını giderme odaklanır. İşletimsel bir aygıtı sorun giderme için şu adrese gidin [işletimsel aygıt sorunlarını giderme](storsimple-troubleshoot-operational-device.md).
> 
> 

Bu makalede ayrıca StorSimple dağıtım sorunlarını giderme araçları açıklar ve adım adım sorun giderme örnek sağlar.

## <a name="first-time-deployment-issues"></a>İlk kez dağıtım sorunları
Cihazınızı ilk kez dağıtırken bir sorunu yaşayıp çalıştırırsanız, aşağıdakileri göz önünde bulundurun:

* Fiziksel bir aygıtı gideriyorsanız donanım yüklendi ve açıklandığı şekilde yapılandırılmış olduğundan emin olun [StorSimple 8100 cihazınız yüklemek](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md).
* Dağıtım önkoşulları denetleyin. Açıklanan tüm bilgilere sahip olduğunuzdan emin olun [dağıtım yapılandırma denetim listesi](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
* Sorun açıklanan görmek için StorSimple sürüm notlarını gözden geçirin. Sürüm Notları bilinen yükleme sorunları için geçici çözümler içerir. 

Aygıt dağıtımı sırasında en yaygın yüz ortaya kullanıcıların Kurulum Sihirbazı'nı çalıştırdığınızda ve bunlar StorSimple için Windows PowerShell aracılığıyla aygıtın ne zaman kaydını verir. (StorSimple için Windows PowerShell kaydetmek ve StorSimple Cihazınızı yapılandırmak için kullandığınız. Cihaz kaydı hakkında daha fazla bilgi için bkz: [adım 3: yapılandırmak ve storsimple için Windows PowerShell aracılığıyla cihazınızın](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Aşağıdaki bölümlerde, StorSimple cihaz ilk defa yapılandırdığınızda karşılaştığınız sorunları çözmenize yardımcı olabilir.

## <a name="first-time-setup-wizard-process"></a>İlk kez Kurulum Sihirbazı işlemi
Kurulum Sihirbazı işlemi aşağıdaki adımları özetler. Ayrıntılı kurulum bilgi için bkz: [şirket içi StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough.md).

1. Çalıştırma [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) kalan aracılığıyla yönlendirecek Kurulum Sihirbazı'nı başlatmak için cmdlet adımları. 
2. Ağ yapılandırma: Kurulum Sihirbazı, StorSimple Cihazınızda veri 0 ağ arabirimi için ağ ayarlarını yapılandırmanıza olanak sağlar. Bu ayarlar aşağıdakileri içerir:
   * Sanal IP (VIP), alt ağ maskesi ve ağ geçidi – [kümesi HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) cmdlet arka planda gerçekleştirilir. StorSimple Cihazınızda IP adresi, alt ağ maskesi ve ağ geçidi veri 0 ağ arabirimi için yapılandırır.
   * Birincil DNS sunucusunu – [kümesi HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) cmdlet arka planda gerçekleştirilir. StorSimple çözümünüzün için DNS ayarlarını yapılandırır.
   * NTP sunucusu – [kümesi HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet arka planda gerçekleştirilir. StorSimple çözümünüzün NTP sunucusu ayarlarını yapılandırır.
   * İsteğe bağlı web proxy – [kümesi HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) cmdlet arka planda gerçekleştirilir. Ayarlar ve StorSimple çözümünüzün için web proxy yapılandırması sağlar.
3. Parolaları ayarlamanız: Aygıt Yöneticisi ve StorSimple Snapshot Manager parolalar ayarlamak için sonraki adımdır. Güncelleştirme 1 çalıştırıyorsanız, daha sonra StorSimple Snapshot Manager parolasını ayarla gerekecektir değil.
   
   * Cihaz Yöneticisi parolası aygıtınıza oturum açmak için kullanılır. Varsayılan cihaz parolası **Password1**’dir.
   * StorSimple Snapshot Manager parolasını StorSimple Snapshot Manager kullanmak için bir aygıt yapılandırdığınızda gereklidir. İlk Kurulum Sihirbazı'nda parola ayarlamanız gerekir ve ardından ayarlamak ve StorSimple Yöneticisi hizmeti değiştirin. Bu parolayı, cihazı ile StorSimple Snapshot Manager kimliğini doğrular.
     
     > [!IMPORTANT]
     > Parolalar önce kayıt toplanan, ancak yalnızca aygıt başarıyla kaydettikten sonra uygulanır. Bir parola uygulamak için bir hata olduğunda (karmaşıklık gereksinimlerini karşılayan) gerekli parolalar toplanan kadar parola tekrar girmeniz istenir.
     > 
     > 
4. Kaydedilecek: Microsoft Azure'da çalışan StorSimple Yöneticisi hizmetiyle cihazı kaydetmek için son adımdır. Kayıt gerektirir [hizmet kayıt anahtarını alın](storsimple-manage-service.md#get-the-service-registration-key) Azure Klasik portalından ve Kurulum Sihirbazı'nda sağlayın. Cihaz sorunsuz kaydedildikten sonra hizmet verileri şifreleme anahtarı olanak sağlanır. Tüm sonraki cihazlar hizmete kaydolmak için gerekli olması gerektiğinden bu şifreleme anahtarı güvenli bir yerde sakladığınızdan emin olun.

## <a name="common-errors-during-device-deployment"></a>Aygıt dağıtımı sırasında ortak hataları
Aşağıdaki tablolar, ne zaman karşılaşabileceğiniz sık karşılaşılan hataları listeler:

* Gerekli ağ ayarlarını yapılandırın.
* İsteğe bağlı web proxy ayarlarını yapılandırın.
* Aygıt Yöneticisi ve StorSimple Snapshot Manager parolaları ayarlayın. 
* Cihazı kaydedin. 

## <a name="errors-during-the-required-network-settings"></a>Gerekli ağ ayarlarını sırasında hatalar
| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Bu komut yalnızca etkin denetleyicisinde çalıştırılabilir. |Yapılandırma pasif denetleyicisinde gerçekleştirildi. |Bu komut etkin denetleyicisinden çalıştırın. Daha fazla bilgi için bkz: [Cihazınızı etkin bir denetleyicisindeki tanımlamak](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| 2 |Invoke-HcsSetupWizard: Cihaz hazır değil. |Veri 0 ağ bağlantısı sorunları vardır. |Veri 0 üzerinde fiziksel ağ bağlantısını denetleyin. |
| 3 |Invoke-HcsSetupWizard: Bir IP adresi çakışması ağdaki başka bir sistemle yoktur (HRESULT özel durumu: 0x80070263). |DATA 0 için sağlanan IP zaten başka bir sistem tarafından kullanılıyor oluştu. |Kullanımda olmadığından yeni bir IP sağlar. |
| 4 |Invoke-HcsSetupWizard: Küme kaynağı başarısız oldu. (HRESULT özel durumu: 0x800713AE). |Yinelenen VIP. Sağlanan IP zaten kullanımda. |Kullanımda olmadığından yeni bir IP sağlar. |
| 5 |Invoke-HcsSetupWizard: Geçersiz bir IPv4 adresi. |IP adresi hatalı bir biçimde sağlanır. |Biçimini denetleyin ve IP adresiniz yeniden sağlayın. Daha fazla bilgi için bkz: [IPv4 adresleme][1]. |
| 6 |Invoke-HcsSetupWizard: Geçersiz IPv6 adresi. |IP adresi hatalı bir biçimde sağlanır. |Biçimini denetleyin ve IP adresiniz yeniden sağlayın. Daha fazla bilgi için bkz: [IPv6 adresleme][2]. |
| 7 |Invoke-HcsSetupWizard: Bitiş Noktası Eşleştiricisi kullanılabilir daha fazla uç nokta yok. (HRESULT özel durumu: 0x800706D9) |Küme işlevselliğini çalışmıyor. |[Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sonraki adımlar için. |

## <a name="errors-during-the-optional-web-proxy-settings"></a>İsteğe bağlı web proxy ayarları sırasında hatalar
| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Invoke-HcsSetupWizard: Geçersiz parametre (HRESULT özel durumu: 0x80070057) |İçin proxy ayarlarını sağlanan parametrelerden biri geçerli değil. |URI doğru biçimde sağlanmadı. Aşağıdaki biçimi kullanın: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 |Invoke-HcsSetupWizard: RPC sunucusu kullanılamıyor (HRESULT özel durumu: 0x800706ba) |Kök nedeni aşağıdakilerden biridir:<ol><li>Kümenin en fazla değil.</li><li>Pasif denetleyiciyi etkin denetleyicisi ile iletişim kuramıyor ve Pasif denetleyicisinden komutunu çalıştırın.</li></ol> |Bağlı olarak kök neden:<ol><li>[Microsoft Support başvurun](storsimple-contact-microsoft-support.md) kümenin çalışır durumda olduğundan emin olmak için.</li><li>Komutu etkin denetleyicisinden çalıştırın. Pasif denetleyicisinden komutu çalıştırmak istiyorsanız, pasif denetleyiciyi etkin denetleyicisi ile iletişim kurabildiğinden emin olmak gerekir. Etmeniz [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) Bu bağlantı bozuk ise.</li></ol> |
| 3 |Invoke-HcsSetupWizard: RPC çağrısı başarısız oldu (HRESULT özel durumu: 0x800706be) |Küme çalışmıyor. |[Microsoft Support başvurun](storsimple-contact-microsoft-support.md) kümenin çalışır durumda olduğundan emin olmak için. |
| 4 |Invoke-HcsSetupWizard: Küme kaynağı bulunamadı (HRESULT özel durumu: 0x8007138f) |Küme kaynağı bulunamadı. Bu durum yükleme doğru değildi ortaya çıkar. |Cihazı fabrika varsayılan ayarlarına sıfırlama gerekebilir. [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) küme kaynağı oluşturulamadı. |
| 5 |Invoke-HcsSetupWizard: Küme kaynağı çevrimiçi değil (HRESULT özel durumu: 0x8007138c) |Küme kaynaklarının çevrimiçi değildir. |[Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sonraki adımlar için. |

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Aygıt Yöneticisi ve StorSimple Snapshot Manager parolaları ilgili hatalar
Varsayılan cihaz Yöneticisi parolası **Parola1**. Bu parola süresi dolduktan sonra ilk oturum açma; Bu nedenle, değiştirmek için Kurulum Sihirbazı'nı kullanmanız gerekir. Cihaz ilk kez kaydederken yeni bir cihaz Yöneticisi parolasını sağlamanız gerekir. 

Aygıtı yönetmek için Windows Server ana bilgisayarında çalışan StorSimple Snapshot Manager yazılımı kullanıyorsanız, aynı zamanda bir StorSimple Snapshot Manager parolası ilk kez kayıt sırasında sağlamalısınız. 

Parolalarınızı aşağıdaki gereksinimleri karşıladığından emin olun:

* Cihaz Yöneticisi parolası 8 ile 15 karakter uzunluğunda olmalıdır.
* StorSimple Snapshot Manager parolası 14 veya 15 karakter uzunluğunda olmalıdır.
* Parolalar, 3 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harf, sayısal ve özel. 
* Parolanızı son 24 parola ile aynı olamaz.

Ayrıca, parolaları her yıl süresi dolacak ve yalnızca aygıt başarıyla kaydettikten sonra değiştirilebilir göz önünde bulundurun. Kayıt herhangi bir nedenle başarısız olursa, parolalar değiştirilmez. Aygıt Yöneticisi ve StorSimple Snapshot Manager parolaları hakkında daha fazla bilgi için Git [StorSimple parolalarını değiştirmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-change-passwords.md).

Aygıt Yöneticisi ve StorSimple Snapshot Manager parolalarını ayarlarken bir veya daha fazla şu hatalarla karşılaşabilirsiniz.

| Hayır. | Hata iletisi | Önerilen eylem |
| --- | --- | --- |
| 1 |Parola en fazla uzunluğu aşıyor. |Bu gereksinimleri karşılayan bir parola kullanın:<ul><li>Cihaz Yöneticisi parolası 8-15 karakter uzunluğunda olmalıdır.</li><li>StorSimple Snapshot Manager parolası 14 veya 15 karakter uzunluğunda olmalıdır.</li></ul> |
| 2 |Parola gerekli uzunluğa karşılamıyor. |Bu gereksinimleri karşılayan bir parola kullanın:<ul><li>Cihaz Yöneticisi parolası 8-15 karakter uzunluğunda olmalıdır.</li><li>StorSimple Snapshot Manager parolası 14 veya 15 karakter uzunluğunda olmalıdır.</lu></ul> |
| 3 |Parola küçük harf karakterler içermelidir. |Parolalar, 3 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harf, sayısal ve özel. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 4 |Parola sayısal karakterler içermelidir. |Parolalar, 3 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harf, sayısal ve özel. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 5 |Parola özel karakterler içermelidir. |Parolalar, 3 4 karakter türlerinden birini içermelidir: küçük harfler, büyük harf, sayısal ve özel. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 6 |Parola 3 4 karakter türlerinden birini içermelidir: büyük harf, küçük harfler, sayısal ve özel. |Parolanız karakter gerekli türlerini içermiyor. Parolanız şu gereksinimleri karşıladığından emin olun. |
| 7 |Parametre onay eşleşmiyor. |Parolanızı tüm gereksinimlerini karşıladığından ve bunu doğru girdiğinizden emin olun. |
| 8 |Parolanızı varsayılan aynı olamaz. |Varsayılan parola *Parola1*. İlk kez oturum açtıktan sonra bu parolayı değiştirmeniz gerekir. |
| 9 |Girdiğiniz parola aygıt parolası eşleşmiyor. Lütfen parolayı yeniden yazın. |Parolayı denetleyin ve yeniden yazın. |

Cihaz kayıtlı değil, ancak yalnızca başarılı kayıttan sonra uygulanan önce parolaları toplanır. Parola kurtarma iş akışı cihazın kaydedilmesi gerekir. 

> [!IMPORTANT]
> Bir parola uygulamak için bir deneme başarısız olursa, genel olarak, daha sonra yazılım art arda başarılı olana kadar parola toplamak çalışır. Nadir durumlarda, parola uygulanamaz. Bu durumda, kaydedilecek ve devam edin, ancak parolalar değiştirilmeyecektir. Hangi parola değiştirilmedi bir gösterge olmadan alacak — cihaz Yöneticisi parolası veya StorSimple Snapshot Manager parolası. Bu durum oluşursa, parolalar değiştirmenizi öneririz.
> 
> 

StorSimple Yöneticisi hizmeti üzerinden klasik Azure portalındaki parolaları sıfırlayabilirsiniz. Daha fazla bilgi için gidin: 

* [Cihaz Yöneticisi parolasını değiştirme](storsimple-change-passwords.md#change-the-device-administrator-password).
* [StorSimple Snapshot Manager parolasını değiştirme](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Cihaz kaydı sırasında hatalar
Microsoft Azure'da çalışan StorSimple Yöneticisi hizmeti, cihaz kaydetmek için kullanın. Cihaz kaydı sırasında bir veya daha fazla aşağıdaki sorunlardan biriyle karşılaşırsanız.

| Hayır. | Hata iletisi | Olası nedenler | Önerilen eylem |
| --- | --- | --- | --- |
| 1 |Hata 350027: cihaz StorSimple Yöneticisi ile kaydedilemedi. | |Birkaç dakika bekleyin ve sonra işlemi yeniden deneyin. Sorun devam ederse [Microsoft Support başvurun](storsimple-contact-microsoft-support.md). |
| 2 |Hata 350013: Cihaz kaydedilirken bir hata oluştu. Bu, hatalı hizmet kayıt anahtarını nedeniyle olabilir. | |Lütfen cihazı doğru hizmet kayıt anahtarıyla yeniden kaydedin. Daha fazla bilgi için bkz: [hizmet kayıt anahtarını alın.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 |Hata 350063: StorSimple Yöneticisi hizmeti geçirilen ancak kayıt için kimlik doğrulaması başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. |Bu hata, ACS ile kimlik doğrulaması başarılı ancak hizmetine yapılan kayıt çağrısı başarısız oldu gösterir. Bu aralıklı ağ sistemimizde sorun sonucu olabilir. |Sorunu ederse [Microsoft Support başvurun](storsimple-contact-microsoft-support.md). |
| 4 |Hata 350049: Hizmet kayıt sırasında erişilemedi. |Hizmete çağrısı yapıldığında, bir web özel durumu aldı. Bazı durumlarda, bu işlemi daha sonra yeniden deneniyor ile sabit. |Lütfen IP adresi ve DNS adı denetleyin ve işlemi yeniden deneyin. Sorun devam ederse [Microsoft Support başvurun.](storsimple-contact-microsoft-support.md) |
| 5 |Hata 350031: Aygıt zaten kayıtlı. | |Kullanılabilir eylem gerekli. |
| 6 |Hata 350016: Cihaz kaydı başarısız oldu. | |Kayıt anahtarını doğru olduğundan emin olun. |
| 7 |Invoke-HcsSetupWizard: Cihazınız kaydedilirken bir hata oluştu; Bu, yanlış IP adresi veya DNS adı nedeniyle olabilir. Lütfen ağ ayarlarınızı denetleyin ve yeniden deneyin. Sorun devam ederse [Microsoft Support başvurun](storsimple-contact-microsoft-support.md). (Hata 350050) |Cihazınızı Ağ dışından ping atabilir emin olun. Kayıt, dış ağ bağlantısı yoksa bu hata ile başarısız olabilir. Bu hata, aşağıdakilerden birini veya birkaçını birleşimi olabilir:<ul><li>Yanlış IP</li><li>Hatalı alt ağ</li><li>Yanlış ağ geçidi</li><li>DNS ayarları yanlış</li></ul> |Açıklanan adımları izleyerek [adım adım sorun giderme örnek](#step-by-step-storsimple-troubleshooting-example). |
| 8 |Invoke-HcsSetupWizard: Geçerli işlem bir iç hata nedeniyle [0x1FBE2] başarısız oldu. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft Support başvurun. |Bu, hizmet veya aracı için tüm kullanıcı görünmez hataların durum genel bir hatadır. En yaygın nedeni, ACS kimlik doğrulaması başarısız oldu olabilir. Hatanın olası bir neden NTP sunucusu yapılandırma sorunları ve cihazda saati düzgün ayarlanmamış olmasıdır. |(Sorunları varsa) saati düzeltin ve kayıt işlemi yeniden deneyin. Saat dilimini ayarlamak için Set-HcsSystem - saat dilimi komutunu kullanırsanız, her sözcüğün saat dilimi (örneğin "Pasifik Standart Saati") büyük harfe çevirme.  Bu sorun devam ederse [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sonraki adımlar için. |
| 9 |Uyarı: cihaz etkinleştirilemedi. Cihaz yöneticinize ve StorSimple Snapshot Manager parolaları değiştirilmedi. |Kayıt başarısız olursa, Aygıt Yöneticisi ve StorSimple Snapshot Manager parolaları değiştirilmez. | |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>StorSimple dağıtım sorunlarını giderme araçları
StorSimple StorSimple çözümünüzün gidermek için kullanabileceğiniz çeşitli araçlar içerir. Bunlar:

* Destek paketler ve cihaz günlükleri 
* Sorun giderme için özellikle tasarlanmış cmdlet'leri 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Paketler ve cihaz günlükleri kullanılabilir sorun giderme için destek
Bir destek paketi Microsoft Support takım aygıt sorunları giderme konusunda yardımcı olabilecek ilgili günlükleri içerir. StorSimple için Windows PowerShell ile destek personeli sonra paylaşabilirsiniz bir şifrelenmiş destek paketi oluşturmak için kullanabilirsiniz.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Günlükleri veya destek paketinin içeriğini görüntülemek için
1. Bölümünde açıklandığı gibi destek paketi oluşturmak için StorSimple için Windows PowerShell kullanın [oluşturma ve Destek Paketi yönetmek](storsimple-create-manage-support-package.md).
2. Karşıdan [şifre çözme betik](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) , istemci bilgisayarda yerel olarak.
3. Bu [adım adım yordam](storsimple-create-manage-support-package.md#edit-a-support-package) açın ve Destek Paketi şifresini çözmek için.
4. Şifresi çözülmüş destek paketi günlükleri etw/etvx biçimindedir. Windows Olay Görüntüleyicisi'nde bu dosyaları görüntülemek için aşağıdaki adımları gerçekleştirebilirsiniz:
   
   1. Çalıştırma **eventvwr** Windows istemciniz üzerinde komutu. Bu Olay Görüntüleyicisi'ni başlatır.
   2. İçinde **Eylemler** bölmesinde, tıklatın **açık kaydedilmiş günlük** ve etvx/etw biçimi (Destek Paketi) günlük dosyalarında in üzerine gelin. Dosya artık görüntüleyebilirsiniz. Dosyayı açtıktan sonra sağ tıklayın ve dosyayı metin olarak kaydedin.
      
      > [!IMPORTANT]
      > Aynı zamanda **Get-WinEvent** cmdlet'ini bu dosyalar Windows PowerShell'i açın. Daha fazla bilgi için bkz: [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) Windows PowerShell cmdlet başvuru belgelerinde.
      > 
      > 
5. Günlükleri Olay Görüntüleyicisi'nde açtığınızda, aygıt yapılandırması ile ilgili sorunları içeren günlükleri arayın:
   
   * hcs_pfconfig/Operational günlük
   * hcs_pfconfig/Config
6. Kurulum Sihirbazı'nı adlı cmdlet'leri ilgili dizeleri günlük dosyalarını arayın. Bkz: [ilk kez Kurulum Sihirbazı işlemi](#first-time-setup-wizard-process) bu cmdlet'leri listesi. 
7. Sorunun nedenini anlamak mümkün değilse, yapabilecekleriniz [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sonraki adımlar için. İçindeki adımları kullanın [bir destek isteği oluşturmak](storsimple-contact-microsoft-support.md#create-a-support-request) olduğunda, Microsoft Yardım için Support başvurun.

## <a name="cmdlets-available-for-troubleshooting"></a>Sorun giderme için uygun cmdlet'leri
Bağlantı hataları algılamak için aşağıdaki Windows PowerShell cmdlet'lerini kullanın.

* `Get-NetAdapter`: Ağ arabirimlerinin sağlığını algılamak için bu cmdlet'i kullanın. 
* `Test-Connection`: Ağ bağlantısı içinde ve dışında ağ denetlemek için bu cmdlet'i kullanın.
* `Test-HcsmConnection`: Başarıyla kayıtlı bir cihazla bağlantısını denetlemek için bu cmdlet'i kullanın.

StorSimple Cihazınızda güncelleştirme 1 çalıştırıyorsanız, aşağıdaki tanı cmdlet'leri de kullanılabilir.

* `Sync-HcsTime`: Cihaz zaman görüntülemek ve bir saat eşitlemesi NTP sunucusu ile zorlamak için bu cmdlet'i kullanın.
* `Enable-HcsPing`ve `Disable-HcsPing`: StorSimple Cihazınızı ağ arabirimlerindeki ping işlemi yapmak konakları izin vermek için bu cmdlet'leri kullanın. Varsayılan olarak, StorSimple ağ arabirimleri ping isteklere yanıt vermez.
* `Trace-HcsRoute`: Bu cmdlet bir rota izleme aracı olarak kullanın. Bir süre boyunca son hedefine şekilde her yönlendiriciye paketleri gönderir ve her atlama dönen paketlere dayalı olarak sonuçları hesaplar. Bu yana `Trace-HcsRoute` hangi yönlendiriciler saptayabilirler veya bağlantıları ağ sorunlara neden verilen yönlendirici veya bağlantı, paket kayıplarının derecesini gösterir. 
* `Get-HcsRoutingTable`: Yerel IP yönlendirme tablosunu görüntülemek için bu cmdlet'i kullanın.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Get-NetAdapter cmdlet ile ilgili sorunları giderme
Ağ arabirimleri ilk kez aygıt dağıtımı için yapılandırdığınızda, cihaz hizmetiyle henüz kaydedilmediğinden donanım durum StorSimple Yöneticisi hizmeti kullanıcı Arabirimi kullanılabilir değil. Ayrıca, özellikle hizmet eşitleme etkileyen bir sorun varsa donanım durum sayfasını cihaz durumu her zaman doğru yansıtmayabilir. Bu durumlarda, kullandığınız `Get-NetAdapter` ve sistem durumunu ağ arabirimlerinin belirlemek için cmdlet.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Cihazınızda tüm ağ bağdaştırıcılarının bir listesini görmek için
1. StorSimple için Windows PowerShell'i başlatın ve ardından `Get-NetAdapter`. 
2. Çıktısını kullanmak `Get-NetAdapter` cmdlet'i ve ağ arabiriminin durumunu anlamak için aşağıdaki yönergeleri.
   
   * Arabirim sağlıklı ve etkinleştirilmiş ise **İfındex** durum gösterilir olarak **yukarı**.
   * Arabirim sağlıklı olduğundan ancak (bir ağ kablosu), fiziksel olarak bağlı değil **İfındex** olarak gösterilen **devre dışı**.
   * Arabirim sağlıklı ancak etkin değil ise **İfındex** durum gösterilir olarak **NotPresent**.
   * Arabirim mevcut değilse, bu listede görünmez. StorSimple Yöneticisi hizmeti kullanıcı Arabirimi hala bu arabirimi başarısız durumda gösterir.

Bu cmdlet'in nasıl kullanılacağı hakkında daha fazla bilgi için Git [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) Windows PowerShell cmdlet başvurusu. 

Aşağıdaki bölümlerde çıktısını örnekleri Göster `Get-NetAdapter` cmdlet'i. 

 Bu örnekte, denetleyici 0 pasif denetleyiciyi olduğu ve aşağıdaki gibi yapılandırılmış:

* Veri 0, DATA 1, veri 2 ve veri 3 ağ arabirimleri cihazda vardı.
* Veri 4 ve verileri 5 ağ arabirim kartları mevcut değil; Bu nedenle, çıktıda listelenmemiştir.
* Veri 0 etkinleştirildi.

Denetleyici 1 etkin denetleyicisi olduğundan ve aşağıdaki gibi yapılandırılmış:

* Veri 0, veri 1, veri 2, veri 3, veri 4 ve verileri 5 ağ arabirimleri cihazda vardı.
* Veri 0 etkinleştirildi.

**Örnek çıktı – denetleyici 0**

Denetleyici 0 (pasif denetleyiciyi) çıktısını verilmiştir. Veri 1, veri 2 ve veri 3 bağlı değil. Aygıtta olmadıklarından veri 4 ve verileri 5 listelenmemiştir. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Örnek çıktı – Denetleyici 1**

Denetleyici 1 (etkin denetleyicisi) çıktısını verilmiştir. Yalnızca aygıt ağ arabiriminde yapılandırılan veri 0 ve çalışma.

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


## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Bağlantıyı Sına cmdlet ile ilgili sorunları giderme
Kullanabileceğiniz `Test-Connection` StorSimple Cihazınızı dış ağa bağlanıp bağlanmadığını belirlemek için cmdlet. DNS dahil olmak üzere tüm ağ parametreleri, Kurulum Sihirbazı'nda doğru yapılandırılmışsa kullanabileceğiniz `Test-Connection` bilinen bir adresi outlook.com gibi ağ dışında ping işlemi yapmak için cmdlet. 

Ping devre dışıysa bu cmdlet ile ilgili bağlantı sorunlarını gidermek ping etkinleştirmeniz gerekir.

Çıktı aşağıdaki örnekleri görmek `Test-Connection` cmdlet'i. 

> [!NOTE]
> İlk örnek cihaz yanlış DNS ile yapılandırılır. İkinci örnekte, DNS doğrudur.
> 
> 

**Örnek çıktı – yanlış DNS**

Aşağıdaki örnekte, DNS güvenilmedi gösteren hiçbir çıktı IPv4 ve IPv6 adresleri için yoktur. Bunun anlamı dış ağ bağlantısı yok ve doğru DNS sağlanmalıdır. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Örnek çıktı – doğru DNS**

Aşağıdaki örnekte, DNS düzgün yapılandırılmış olduğunu belirten, IPv4 adresi DNS döndürür. Bu, dış ağ bağlantısı olduğunu doğrular. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Test HcsmConnection cmdlet ile ilgili sorunları giderme
Kullanım `Test-HcsmConnection` cmdlet'i zaten bağlı olan ve StorSimple Yöneticisi hizmetiniz ile kayıtlı bir cihaz için. Bu cmdlet, kayıtlı bir cihazla ve karşılık gelen StorSimple Yöneticisi hizmetiniz arasındaki bağlantıyı doğrulamanıza yardımcı olur. StorSimple için Windows PowerShell üzerinde bu komutu çalıştırabilirsiniz. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Test HcsmConnection cmdlet'i çalıştırmak için
1. Cihaz kayıtlı olduğundan emin olun.
2. Cihaz durumunu kontrol edin. Cihaz, Bakım modunda veya çevrimdışı devre dışı bırakılmışsa aşağıdaki hatalar görebilirsiniz: 
   
   * ErrorCode.CiSDeviceDecommissioned – bu cihaz devre dışı olduğunu belirtir.
   * ErrorCode.DeviceNotReady – bu aygıtın bakım modunda olduğunu gösterir.
   * ErrorCode.DeviceNotReady – bu aygıtın çevrimiçi olduğunu gösterir.
3. StorSimple Yöneticisi hizmetinin çalıştığını doğrulayın (kullanmak [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) cmdlet'i). Hizmet çalışmıyorsa, aşağıdaki hatalar görebilirsiniz:
   
   * ErrorCode.CiSApplianceAgentNotOnline
   * ErrorCode.CisPowershellScriptHcsError – Bu, Get-ClusterResource çalıştırdığınızda istisna olduğunu gösterir.
4. Erişim denetimi Hizmeti'nden (ACS) belirteci denetleyin. Bir web özel durum oluşturursa, bunun nedeni bir ağ geçidi sorun, eksik bir proxy kimlik doğrulaması, yanlış bir DNS veya kimlik doğrulama hatası olabilir. Aşağıdaki hatalar görebilirsiniz:
   
   * ErrorCode.CiSApplianceGateway – bu gösterir HttpStatusCode.BadGateway özel durum: ad Çözümleyici hizmeti ana bilgisayar adı çözümlenemedi. 
   * ErrorCode.CiSApplianceProxy – bu gösterir (HTTP durum kodu 407) HttpStatusCode.ProxyAuthenticationRequired özel durum: istemci proxy sunucusu ile kimlik doğrulaması yapamıyor. 
   * ErrorCode.CiSApplianceDNSError – bu gösterir WebExceptionStatus.NameResolutionFailure özel durum: ad Çözümleyici hizmeti ana bilgisayar adı çözümlenemedi.
   * ErrorCode.CiSApplianceACSError – bu hizmet kimlik doğrulama hatası döndürdü, ancak bağlantısı olduğunu gösterir.
     
     Bir web özel oluşturmadığını ErrorCode.CiSApplianceFailure için kontrol edin. Bu Gereci başarısız olduğunu gösterir.
5. Bulut hizmeti bağlantısını denetleyin. Hizmet bir web özel durum oluşturursa, aşağıdaki hatalar görebilirsiniz:
   
   * ErrorCode.CiSApplianceGateway – bu gösterir HttpStatusCode.BadGateway özel durum: başka bir proxy veya özgün sunucudan bir ara proxy sunucusunun bozuk bir isteği aldı.
   * ErrorCode.CiSApplianceProxy – bu gösterir (HTTP durum kodu 407) HttpStatusCode.ProxyAuthenticationRequired özel durum: istemci proxy sunucusu ile kimlik doğrulaması yapamıyor. 
   * ErrorCode.CiSApplianceDNSError – bu gösterir WebExceptionStatus.NameResolutionFailure özel durum: ad Çözümleyici hizmeti ana bilgisayar adı çözümlenemedi.
   * ErrorCode.CiSApplianceACSError – bu hizmet kimlik doğrulama hatası döndürdü, ancak bağlantısı olduğunu gösterir.
     
     Bir web özel oluşturmadığını ErrorCode.CiSApplianceSaasServiceError için kontrol edin. Bu, StorSimple Yöneticisi hizmeti ile ilgili bir sorun olduğunu gösterir.
6. Azure Service Bus bağlantısını denetleyin. Aygıt için hizmet veri yolu bağlanamıyor ErrorCode.CiSApplianceServiceBusError gösterir.

CiSCommandletLog0Curr.errlog günlük dosyaları ve CiSAgentsvc0Curr.errlog özel durum ayrıntıları gibi daha fazla bilgi bulunur. 

Cmdlet'inin nasıl kullanılacağı hakkında daha fazla bilgi için Git [Test HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) Windows PowerShell'de başvuru belgelerini.

> [!IMPORTANT]
> Aktif ve Pasif denetleyiciyi için bu cmdlet'i çalıştırabilirsiniz. 
> 
> 

Çıktı aşağıdaki örnekleri görmek `Test-HcsmConnection` cmdlet'i. 

**Örnek çıktı – başarıyla kayıtlı cihaz StorSimple sürüm çalıştıran (Temmuz 2014)**

İlk StorSimple Yöneticisi hizmetiyle başarıyla kaydedildi ve hiçbir bağlantı sorunları olan bir aygıttan örnektir. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Örnek çıktı – başarıyla kayıtlı cihaz StorSimple güncelleştirme 1'çalışan**

StorSimple Cihazınızda güncelleştirme 1 çalıştırıyorsanız, ayrıntılı anahtar ile çalıştırmak gerekmez.

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

**Örnek çıktı – çevrimdışı cihaz StorSimple sürüm çalıştıran (Temmuz 2014)**

Bu örnek durumuna sahip bir aygıttan olan **çevrimdışı** Klasik Azure portalındaki.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Aygıt, geçerli web proxy yapılandırması'nı kullanarak bağlanamadı. Bu web proxy yapılandırmasını ya da bir ağ bağlantısı sorunu ile ilgili bir sorun olabilir. Bu durumda, web proxy ayarlarının doğru olduğundan ve web proxy sunucularınızın çevrimiçi ve erişilebilir olduğundan emin olmanız gerekir. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Eşitleme HcsTime cmdlet ile ilgili sorunları giderme
Aygıt zaman görüntülemek için bu cmdlet'i kullanın. Aygıt zaman NTP sunucusu ile bir uzaklık varsa, zorla-zaman, NTP sunucusu ile eşitlemek için bu cmdlet'i kullanabilirsiniz. Cihaz ve NTP sunucusu arasındaki uzaklığı 5 dakikadan daha büyükse bir uyarı görürsünüz. Uzaklık 15 dakika aşarsa, aygıtın çevrimdışı olarak geçer. Zaman eşitleme zorlamak için bu cmdlet'i kullanmaya devam edebilirsiniz. Uzaklık 15 saat aşarsa, ancak, daha sonra zorla-saati ve bir hata iletisi gösterilir eşitleme yapamıyor olmayacaktır.

**Örnek çıktı – eşitleme HcsTime kullanarak zorlanmış zaman eşitleme**

     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.

     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>HcsPing etkinleştirme ve devre dışı bırakma HcsPing cmdlet'lerle ilgili sorunları giderme
Cihazınızı ağ arabirimlerindeki ICMP ping isteklerine yanıt vermesini sağlamak için bu cmdlet'leri kullanın. Varsayılan olarak StorSimple ağ arabirimleri ping isteklere yanıt vermez. Bu cmdlet kullanılarak, Cihazınızı çevrimiçi ve erişilebilir olup olmadığını bilmek için kolay bir yoludur.  

**Örnek çıktı – HcsPing etkinleştirme ve devre dışı bırakma HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>İzleme HcsRoute cmdlet ile ilgili sorunları giderme
Bu cmdlet, bir rota izleme aracı olarak kullanın. Bir süre boyunca son hedefine şekilde her yönlendiriciye paketleri gönderir ve her atlama dönen paketlere dayalı olarak sonuçları hesaplar. Cmdlet'i belirtilen herhangi bir yönlendiricide veya bağlantı paket kaybı derecesini gösterildiğinden hangi yönlendiriciler saptayabilirler veya bağlantıları ağ sorunlarına neden.

**İzleme HcsRoute sahip bir paket yolu izlemek nasıl gösteren bir örnek çıktı**

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

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Get-HcsRoutingTable cmdlet ile ilgili sorunları giderme
StorSimple cihazınız için yönlendirme tablosunu görüntülemek için bu cmdlet'i kullanın. Bir yönlendirme tablosu, veri paketlerinin Internet Protokolü (IP) ağ üzerinden yolculuk burada yönlendirilirsiniz belirlemenize yardımcı olabilir kurallar kümesidir. 

Yönlendirme tablosu arabirimleri ve belirtilen ağ verilerini yönlendirir ağ geçidi gösterir. Ayrıca, belirli bir hedefe ulaşmak için geçen yolu için karar veren olan Yönlendirme ölçüsünü verir. Daha düşük bir yönlendirme metriği, daha yüksek öncelik. 

Örneğin, 2 ağ arabirimi varsa, veri 2 ve veri 3, Internet'e bağlı. Veri 2 ve veri 3 için yönlendirme ölçümleri 15 ve 261 sırasıyla olduğunda yönlendirme düşük ölçüye sahip veri 2 Internet'e erişmek için kullanılan tercih edilen arabirim olur.

StorSimple Cihazınızda güncelleştirme 1 çalıştırıyorsanız, DATA 0 ağ arabirimindeki bulut trafiği için en yüksek öncelik vardır. Bu, bulut trafiği olsa bile diğer bulut etkin arabirimleri, 0 VERİLERİNE yönlendirilecektir anlamına gelir. 

Çalıştırırsanız `Get-HcsRoutingTable` cmdlet (aşağıdaki örnekte gösterildiği gibi) herhangi bir parametre belirtmeden cmdlet IPv4 ve IPv6 yönlendirme tablolarını çıkarır. Alternatif olarak, belirtebilirsiniz `Get-HcsRoutingTable -IPv4` veya `Get-HcsRoutingTable -IPv6` ilgili bir yönlendirme tablosu alma.

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

## <a name="step-by-step-storsimple-troubleshooting-example"></a>Adım adım StorSimple sorun giderme örneği
Aşağıdaki örnek, StorSimple dağıtımı adım adım sorun giderme gösterir. Örnek senaryoda, cihaz kaydı, bir ağ ayarlarını veya DNS adı yanlış olduğunu belirten hata iletisi ile başarısız olur.

Döndürülen hata iletisi şudur:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Hata nedeni aşağıdakilerden biri olabilir:

* Yanlış donanım yükleme
* Hatalı ağ arabirimi
* Hatalı IP adresi, alt ağ maskesi, ağ geçidi, birincil DNS sunucusu veya web proxy
* Hatalı kayıt anahtarı
* Yanlış güvenlik duvarı ayarları

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Bulun ve cihazın kayıt sorunu düzeltmek için
1. Cihaz yapılandırmanızı denetleyin: etkin denetleyicisinde çalıştırın `Invoke-HcsSetupWizard`.
   
   > [!NOTE]
   > Kurulum Sihirbazı'nı etkin denetleyicisinde çalıştırmanız gerekir. Etkin denetleyiciye bağlı olduğunu doğrulamak için seri konsoldan sunulan başlık bakın. Başlık, denetleyici 0 veya 1 denetleyicisine bağlı olduğunuzu ve denetleyici etkin veya Pasif olduğunu gösterir. Daha fazla bilgi için Git [Cihazınızı etkin bir denetleyicisindeki tanımlamak](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
   > 
   > 
2. Cihaz düzgün kablolu emin olun: geri düzlemi aygıtta kablo ağ kontrol edin. Kablolama, aygıt modeline özgüdür. Daha fazla bilgi için Git [StorSimple 8100 cihazınız yüklemek](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md).
   
   > [!NOTE]
   > 10 GbE ağ bağlantı noktası kullanıyorsanız, sağlanan QSFP SFP bağdaştırıcısı ve SFP kablolar kullanmanız gerekecektir. Daha fazla bilgi için bkz: [kablo, anahtarlar ve 10 GbE bağlantı noktaları için önerilen ileticileri listesi](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
   > 
   > 
3. Ağ arabirimi durumunu doğrulayın:
   
   * DATA 0 ağ arabirimlerinin sağlığını algılamak için Get-NetAdapter cmdlet'ini kullanın. 
   * Bağlantıyı çalışmıyorsa **ifındex** durumu, arabirimin kapalı olduğunu olmadığını gösterir. Uygulamasına ve anahtarı için bağlantı noktası ağ bağlantısını denetlemek gerekir. Out hatalı kabloları kural gerekecektir. 
   * Veri 0 üzerinde bağlantı noktası şüpheleniyorsanız etkin denetleyicisi başarısız oldu, bu verileri 0 bağlantı noktası 1 denetleyicisinde bağlanarak onaylayın. Bunu doğrulamak için ağ aygıtı arkasından 0 denetleyicisinden kablosunu, denetleyici 1 kabloyu bağlayın ve Get-NetAdapter cmdlet'i yeniden çalıştırın. 
     Veri 0 denetleyicisi üzerinde bağlantı noktası başarısız olur, [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sonraki adımlar için. Sisteminizin denetleyicisinde değiştirmeniz gerekebilir.
4. Anahtar bağlanabildiğini doğrulayın:
   
   * Veri 0 ağ arabirimlerindeki denetleyici 0 ve denetleyici 1, birincil muhafazada aynı alt ağda olduğundan emin olun. 
   * Hub veya yönlendirici denetleyin. Genellikle, her iki denetleyicileri aynı hub veya yönlendirici bağlanmanız gerekir. 
   * Bağlantı için kullandığınız anahtarları, hem de aynı vLAN denetleyicileri veri 0 olduğundan emin olun.
5. Kullanıcı hataları kaldırın:
   
   * Kurulum Sihirbazı'nı yeniden çalıştırın (çalıştırma **Invoke-HcsSetupWizard**) ve yeniden herhangi bir hata olduğundan emin olmak için değerleri girin. 
   * Kayıt doğrulama kullanılan anahtarı. Aynı kayıt anahtarı, birden çok aygıt bir StorSimple Yöneticisi hizmetine bağlanmak için kullanılabilir. Yordamı kullanın [hizmet kayıt anahtarını alın](storsimple-manage-service.md#get-the-service-registration-key) doğru kayıt anahtarını kullandığınızdan emin olmak için.
     
     > [!IMPORTANT]
     > Çalıştıran birden çok hizmetleriniz varsa, uygun hizmet için kayıt anahtarını cihazı kaydetmek için kullanıldığından emin olmak gerekir. Bir aygıt yanlış StorSimple Yöneticisi hizmetiyle kaydolduysanız, gerekecek [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) sonraki adımlar için. Daha sonra hedeflenen hizmete bağlanmak için (veri kaybına neden olabilir) cihazı fabrika ayarlarına gerçekleştirmek zorunda kalabilirsiniz.
     > 
     > 
6. Dış ağ bağlantınız olduğunu doğrulamak için Bağlantıyı Sına cmdlet'ini kullanın. Daha fazla bilgi için Git [Bağlantıyı Sına cmdlet ile ilgili sorunları giderme](#troubleshoot-with-the-test-connection-cmdlet).
7. Güvenlik Duvarı Engelleme denetleyin. Doğruladıysanız sanal IP (VIP), alt ağ, ağ geçidi ve DNS ayarları doğru ve bağlantı sorunları görmeye devam sonra güvenlik duvarınızın Cihazınızı dış ağ arasındaki iletişimi engelliyor olabilir. 80 ve 443 numaralı bağlantı noktalarını, StorSimple Cihazınızda giden iletişim için kullanılabilir olduğundan emin olmanız gerekir. Daha fazla bilgi için bkz: [StorSimple cihazınız için ağ gereksinimleri](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
8. Günlüklerine bakın. Git [desteği paketleri ve cihaz günlükleri kullanılabilir sorun giderme için](#support-packages-and-device-logs-available-for-troubleshooting).
9. Yukarıdaki adımlar sorunu çözmezse [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) Yardım için.

## <a name="next-steps"></a>Sonraki adımlar
[İşletimsel bir aygıtı sorun giderme öğrenin](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
