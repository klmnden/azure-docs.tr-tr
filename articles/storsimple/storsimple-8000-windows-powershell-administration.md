---
title: "StorSimple cihaz yönetimi için PowerShell | Microsoft Docs"
description: "StorSimple Cihazınızı yönetmek için StorSimple için Windows PowerShell'i kullanmayı öğrenin."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/03/2017
ms.author: alkohli@microsoft.com
ms.openlocfilehash: 89e1054117f19e787da5330932021351fb016209
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Cihazınızı yönetmek için StorSimple için Windows PowerShell kullanın

## <a name="overview"></a>Genel Bakış

StorSimple için Windows PowerShell, Microsoft Azure StorSimple Cihazınızı yönetmek için kullanabileceğiniz bir komut satırı arabirimi sağlar. Adı da anlaşılacağı gibi bir Windows PowerShell tabanlı komut satırı kısıtlı bir çalışma alanında yerleşik arabirimidir. Komut satırında kullanıcı açısından bakıldığında, kısıtlı bir çalışma alanı kısıtlanmış bir Windows PowerShell sürümü olarak görünür. Bazı Windows PowerShell temel özelliklerini korurken, bu arabirim Microsoft Azure StorSimple Cihazınızı yönetmek doğrultusunda sağlamıştır ek adanmış cmdlet'leri vardır.

Bu makalede bu arabirim için nasıl bağlanacağını dahil olmak üzere StorSimple özellikleri için Windows PowerShell ve adım adım yordamlar ya da bu arabirimi kullanarak gerçekleştirebilirsiniz iş akışları bağlantılar içerir. İş akışları Cihazınızı kaydedin, ağ arabiriminin aygıtınızda yapılandırın, Bakım modunda olması, aygıt durumunu değiştirin ve karşılaşabileceğiniz sorunları gidermek için cihazın gerektiren güncelleştirmeleri yüklemek nasıl içerir.

Bu makaleyi okuduktan sonra aşağıdakileri gerçekleştirebilirsiniz:

* StorSimple Cihazınızı StorSimple için Windows PowerShell kullanarak bağlanın.
* StorSimple Cihazınızı StorSimple için Windows PowerShell kullanarak yönetebilirsiniz.
* Yardım StorSimple için Windows PowerShell'de alın.

> [!NOTE]
> * StorSimple cmdlet'leri için Windows PowerShell bir seri konsoldan veya uzaktan Windows PowerShell uzaktan iletişimini aracılığıyla StorSimple Cihazınızı yönetmek olanak tanır. Her bir bu arabirimi kullanılabilir tek tek cmdlet'ler hakkında daha fazla bilgi için Git [StorSimple için Windows PowerShell için cmdlet başvurusu](https://technet.microsoft.com/library/dn688168.aspx).
> * Azure PowerShell StorSimple cmdlet'leri, StorSimple hizmet düzeyi otomatikleştirmenizi ve geçiş görevleri komut satırından izin cmdlet'lerinin farklı bir koleksiyonudur. StorSimple için Azure PowerShell cmdlet'leri hakkında daha fazla bilgi için Git [Azure StorSimple cmdlet başvurusu](https://docs.microsoft.com/powershell/servicemanagement/azure.storsimple/v3.1.0/azure.storsimple).


Aşağıdaki yöntemlerden birini kullanarak StorSimple için Windows PowerShell erişebilirsiniz:

* [StorSimple cihaz seri konsoluna bağlanmak](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
* [StorSimple için Windows PowerShell kullanarak uzaktan bağlanma](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Windows PowerShell için StorSimple cihaz seri konsoluna bağlanmak

Yapabilecekleriniz [PuTTY karşıdan](http://www.putty.org/) veya benzer StorSimple için Windows PowerShell'e bağlanmak için terminal öykünme yazılımı. Özellikle Microsoft Azure StorSimple cihaza erişim için PuTTY yapılandırmanız gerekir. Aşağıdaki konular, PuTTy yapılandırmak ve cihaza bağlanmak ayrıntılı adımlar içerir. Seri konsol içinde çeşitli menü seçeneklerini de açıklanmıştır.

### <a name="putty-settings"></a>PuTTY ayarları

Seri konsoldan Windows PowerShell arabirimine bağlamak için aşağıdaki PuTTY ayarları kullandığınızdan emin olun.

#### <a name="to-configure-putty"></a>PuTTY yapılandırmak için

1. PuTTY içinde **yeniden yapılandırma** iletişim kutusunda **kategori** bölmesinde, **klavye**.
2. Aşağıdaki seçenekleri'nin seçili olduğundan emin olun (yeni bir oturum başlattığınızda varsayılan ayarları bunlar).
   
   | Klavye öğesi | Şunu seçin: |
   | --- | --- |
   | Geri tuşu |Denetim-? (127) |
   | Giriş ve bitiş anahtarları |Standart |
   | İşlev tuşları ve tuş |ESC [n ~ |
   | İmleç anahtarların ilk durumu |Normal |
   | Sayısal tuş takımında başlangıç durumu |Normal |
   | Ek klavye özellikleri etkinleştirme |Denetim Alt AltGr farklı. |
   
    ![Desteklenen Putty ayarları](./media/storsimple-windows-powershell-administration/IC740877.png)
3. **Uygula**'ya tıklayın.
4. İçinde **kategori** bölmesinde, **çeviri**.
5. İçinde **uzak karakter kümesi** liste kutusunda **UTF-8**.
6. Altında **çizim karakterlerinden işleme**seçin **kullanım Unicode çizim kod noktaları**. Aşağıdaki ekran görüntüsünde doğru PuTTY seçimleri gösterir.
   
    ![UTF Putty ayarları](./media/storsimple-windows-powershell-administration/IC740878.png)
7. **Uygula**'ya tıklayın.

Artık, aşağıdaki adımları uygulayarak cihaz seri konsoluna bağlanmak için PuTTY kullanabilirsiniz.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="about-the-serial-console"></a>Seri Konsolu hakkında

StorSimple Cihazınızı bir başlık iletisi seri Konsolu aracılığıyla arabiriminin sunulan Windows PowerShell eriştiğinizde, menü seçenekleri tarafından izlenen.

Başlık iletisi modeli, ad, yüklü yazılım sürümü ve eriştiğiniz denetleyicisi durumunu gibi temel StorSimple cihaz bilgileri içerir. Aşağıdaki resimde bir başlık iletisi örneği gösterilmiştir.

![Seri tanıtıcı iletisi](./media/storsimple-windows-powershell-administration/IC741098.png)

> [!IMPORTANT]
> Başlık iletisi bağlı olduğunuzdan denetleyicisi olup olmadığını belirlemek için kullanabileceğiniz _etkin_ veya _pasif_.

Aşağıdaki resimde seri konsol menüsünde kullanılabilir çeşitli çalışma seçeneklerini gösterir.

![2 Cihazınızı kaydedin](./media/storsimple-windows-powershell-administration/IC740906.png)

Aşağıdaki ayarları arasından seçim yapabilirsiniz:

1. **Oturum oturum tam erişim** (uygun kimlik bilgileriyle) bağlanmak bu seçeneği sağlar **SSAdminConsole** yerel denetleyicisinde çalışma. (Yerel denetleyici şu anda StorSimple Cihazınızı seri konsol üzerinden eriştiğiniz denetleyicisi değil.) Bu seçenek, tüm olası cihaz sorunları gidermek için Kısıtlanmamış çalışma (Destek oturumu) erişmek için Microsoft Support izin vermek için de kullanılabilir. Oturum açmak için 1 seçeneğini kullandıktan sonra belirli bir cmdlet çalıştırarak Kısıtlanmamış çalışma erişmek Microsoft Support mühendisi izin verebilirsiniz. Ayrıntılar için başvurmak [destek oturum başlatma](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).
   
2. **Oturum açtığınızda tam erişimi olan eş denetleyicisi** (uygun kimlik bilgileriyle) bağlanabilirsiniz dışında bu seçenek seçenek 1, aynıdır **SSAdminConsole** eş denetleyicisinde çalışma. StorSimple cihazı bir Aktif-Pasif yapılandırmasında iki denetleyicisi yüksek oranda kullanılabilirlik aygıtla olduğundan, eş seri konsol üzerinden eriştiğiniz aygıt diğer denetleyicisi ifade eder).
   Seçenek 1 benzeyen, bu seçenek ayrıca eş denetleyicisinde Kısıtlanmamış çalışma erişmek için Microsoft Support izin vermek için kullanılabilir.

3. **Sınırlı erişimi bağlantı** bu seçenek Windows PowerShell arabirimi sınırlı modunda erişmek için kullanılır. Erişim kimlik bilgileri istenmez. Bu seçeneği 1 ve 2 seçeneklerine karşılaştırılan daha sınırlı bir çalışma alanı bağlanır.  Seçenek 1 kullanılabilir olan görevlerden bazıları, **olamaz* bu çalışma alanında gerçekleştirilebilir şunlardır:
   
   * Fabrika ayarlarına sıfırlama
   * Parola Değiştir
   * Etkinleştirmek veya destek erişimini devre dışı bırakma
   * Güncelleştirme uygulama
   * Düzeltmeleri yükleme

    > [!NOTE]
    > Cihaz Yöneticisi parolası unutmuş ve 1 veya 2 seçeneği üzerinden bağlanamıyorsa tercih edilen seçenek budur.

4. **Dili Değiştir** Windows PowerShell arabiriminde görüntüleme dilini değiştirmek bu seçeneği sağlar. Desteklenen dilleri İngilizce, Japonca, Rusça, Fransızca, Güney Kore dili, İspanyolca, İtalyanca, Almanca, Çince ve Portekizce (Brezilya) var.

## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>StorSimple StorSimple için Windows PowerShell kullanarak uzaktan bağlanmak

StorSimple Cihazınızı bağlamak için Windows PowerShell uzaktan iletişimi kullanabilirsiniz. Bu şekilde bağlandığınızda, menü görmezsiniz. (Yalnızca seri konsol cihazda bağlamak için kullanırsanız, bir menü görürsünüz. Uzaktan bağlanma sizi doğrudan "seçeneği 1 – tam erişim" seri konsol denk götürür.) Windows PowerShell uzaktan iletişimini ile belirli bir çalışma bağlayın. Görüntüleme dili de belirtebilirsiniz.

Görüntüleme dilini kullanarak ayarlamak dil bağımsızdır **Dili Değiştir** seri konsol menüsünde seçeneği. Uzak PowerShell belirtilmemişse içinden bağlanan yerel cihaz otomatik olarak seçer.

> [!NOTE]
> Microsoft Azure sanal konaklar ve StorSimple bulut cihazları ile çalışıyorsanız, bulut uygulaması bağlanmak için Windows PowerShell uzaktan iletişimi ve sanal ana bilgisayar kullanabilirsiniz. Ana bilgisayarda hangi Windows PowerShell oturumundan bilgileri kaydetmek bir paylaşım konumuna ayarladıysanız, dikkat etmeniz gerekir, _herkesin_ sorumlu kimliği doğrulanmış kullanıcılar yalnızca içerir. Bu nedenle, paylaşım tarafından erişime izin verecek şekilde ayarladıysanız _herkesin_ ve kimlik bilgilerini belirtmeden bağlanmak, kimliği doğrulanmamış anonim asıl kullanılacak ve bir hata görürsünüz. Bu sorunu gidermek için paylaşımında konak Konuk hesabı etkinleştirmeli ve ardından Konuk hesabı tam paylaşımına erişmenizi veya Windows PowerShell cmdlet'i ile birlikte geçerli kimlik bilgilerini belirtmeniz gerekir.


Windows PowerShell uzaktan iletişimini bağlanmak için HTTP veya HTTPS kullanın. Aşağıdaki öğreticilerde yönergeleri kullanın:

* [HTTP kullanarak uzaktan bağlanma](storsimple-remote-connect.md#connect-through-http)
* [HTTPS kullanarak uzaktan bağlanma](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Bağlantı güvenlik konuları

StorSimple için Windows PowerShell'e bağlanmak nasıl karar verirken aşağıdakileri göz önünde bulundurun:

* Doğrudan cihaz seri konsoluna bağlanmak güvenlidir, ancak seri konsol ağ anahtarları bağlanma değil. Cihaz seri ağ anahtarları bağlanırken güvenlik riski dikkatli olun.
* Bir HTTP oturumu aracılığıyla bağlanan ağ üzerinden seri konsol üzerinden bağlanma daha fazla güvenlik sunar. Bu en güvenli yöntemi olmamasına karşın, güvenilen ağlarda kabul edilebilir.
* HTTPS oturumu aracılığıyla bağlanan en güvenli ve önerilen seçenek olur.

## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>StorSimple Cihazınızı StorSimple için Windows PowerShell kullanarak yönetme

Aşağıdaki tabloda StorSimple Cihazınızı Windows PowerShell arabirimi içinde tüm genel yönetim görevleri ve gerçekleştirilebilir karmaşık iş akışları bir özetini gösterir. Her bir iş akışı hakkında daha fazla bilgi için tablodaki uygun girdiyi tıklatın.

#### <a name="windows-powershell-for-storsimple-workflows"></a>StorSimple iş akışları için Windows PowerShell

| Bunu yapmak istiyorsanız... | Bu yordamı kullanın. |
| --- | --- |
| Cihazınızı kaydedin |[StorSimple için Windows PowerShell kullanarak cihazı yapılandırma ve kaydetme](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
| Web proxy yapılandırma</br>Görünümü web proxy ayarları |[StorSimple cihazınız için Web Proxy'yi Yapılandır](storsimple-8000-configure-web-proxy.md) |
| Cihazınızda veri 0 ağ arabirimi ayarlarını değiştirme |[DATA 0 ağ arabirimindeki StorSimple cihazınız için değiştirme](storsimple-8000-modify-data-0.md) |
| Bir denetleyici Durdur </br> Yeniden başlatma veya kapatma denetleyicisi </br> Bir cihazı kapatmak</br>Cihazı fabrika varsayılan ayarlarına sıfırlama |[Cihaz Denetleyicilerini Yönet](storsimple-8000-manage-device-controller.md) |
| Bakım modu güncelleştirmeleri ve düzeltmeleri yüklemeniz |[Cihazınızı güncelleştirme](storsimple-update-device.md) |
| Bakım modu girin </br>Bakım modundan çık |[StorSimple cihaz modları](storsimple-8000-device-modes.md) |
| Bir destek paketi oluştur</br>Şifresini çözmek ve Destek paketini Düzenle |[Oluşturma ve Destek Paketi yönetme](storsimple-8000-create-manage-support-package.md) |
| Bir destek oturumu Başlat</br> |[StorSimple için Windows PowerShell içinde bir destek oturumu Başlat](storsimple-8000-create-manage-support-package.md#create-a-support-package) |

## <a name="get-help-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'de yardım alın

StorSimple için Windows PowerShell içinde cmdlet Yardım kullanılabilir. Bu Yardım çevrimiçi, güncel bir sürümünü sisteminizdeki Yardımı güncelleştirmek için kullanabileceğiniz, de kullanılabilir.

Windows PowerShell'de benzer bu arabirimde Yardım alma ve Yardım ile ilgili cmdlet'leri çoğunu çalışır. Yardım için Windows PowerShell çevrimiçi TechNet Kitaplığı'nda bulabilirsiniz: [ile Windows PowerShell komut dosyası](http://go.microsoft.com/fwlink/?LinkID=108518).

Yardım'ı güncelleştirme de dahil olmak üzere bu Windows PowerShell arabirimi Yardım türlerinde kısa bir açıklaması verilmiştir.

### <a name="to-get-help-for-a-cmdlet"></a>Bir cmdlet için Yardım almak için

* Herhangi bir cmdlet veya işlevi için Yardım almak için aşağıdaki komutu kullanın:`Get-Help <cmdlet-name>`
* Herhangi bir cmdlet'i için çevrimiçi Yardım almak için önceki cmdlet'iyle kullanmak `-Online` parametre:`Get-Help <cmdlet-name> -Online`
* Tam Yardım için kullandığınız `–Full` parametresi ve örnekler için `–Examples` parametresi.

### <a name="to-update-help"></a>Yardımı güncelleştirmek için

Windows PowerShell arabiriminde Yardım kolayca güncelleştirebilirsiniz. Sisteminizde Yardımı güncelleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-update-cmdlet-help"></a>Cmdlet Yardım güncelleştirmek için
1. Windows PowerShell ile başlangıç **yönetici olarak çalıştır** seçeneği.
2. Komut istemine yazın:`Update-Help`
3. Güncelleştirilmiş Yardım dosyalarını yüklenir.
4. Yardım dosyaları yüklendikten sonra yazın: `Get-Help Get-Command`. Bu Yardım kullanılabilir cmdlet'lerin bir listesi görüntülenir.

> [!NOTE]
> Bir çalışma alanında kullanılabilir tüm cmdlet'lerinin listesini almak için karşılık gelen menü seçeneğine oturum açın ve Çalıştır `Get-Command` cmdlet'i.


## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki iş akışları birini gerçekleştirirken, StorSimple cihazınızla herhangi bir sorunla karşılaşırsanız, başvurmak [StorSimple dağıtım sorunlarını giderme araçları](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

