---
title: Azure Vm'leri için belirli RDP hata iletileri | Microsoft Docs
description: Azure'da Windows sanal makinesine Uzak Masaüstü Bağlantısı çalışırken karşılaşabileceğiniz belirli hata iletileri kullanın anlama
keywords: Uzak Masaüstü hatası, Uzak Masaüstü bağlantısı hata, VM'ye bağlanamıyor Uzak Masaüstü sorunlarını giderme
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: f4d733e29d2ba8213e1832f2c604b726283ab3e1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60318706"
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Azure'da Windows VM için belirli RDP hata iletileri sorunları giderme
Azure'da Windows sanal makinesi (VM) için Uzak Masaüstü Bağlantısı kullanılırken, belirli bir hata iletisi alabilirsiniz. Bu makalede karşılaştı, sorun giderme yanı sıra bunları gidermek için adımları daha genel hata iletileri bazıları ayrıntılı olarak açıklanmaktadır. Sanal makinenizde bağlanma sorunu yaşıyorsanız RDP kullanmayan belirli hata iletisiyle karşılaştığınız bkz [Uzak Masaüstü için sorun giderme kılavuzu](troubleshoot-rdp-connection.md).

Belirli hata iletileri hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Lisans sağlanabilecek Uzak Masaüstü lisans sunucusu olmadığından uzak oturumun bağlantısı kesildi](#rdplicense).
* [Uzak Masaüstü bilgisayar "name" bulamıyor](#rdpname).
* [Bir kimlik doğrulama hatası oluştu. Yerel Güvenlik Yetkilisi temas kurulamıyor](#rdpauth).
* [Windows güvenlik hatası: Kimlik bilgilerinizi çalışmama](#wincred).
* [Bu bilgisayar, uzak bilgisayara bağlanamıyor](#rdpconnect).

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Lisans sağlanabilecek Uzak Masaüstü lisans sunucusu olmadığından uzak oturumun bağlantısı kesildi.
Neden: Uzak Masaüstü Sunucusu rolü için 120 günlük lisansının yetkisiz kullanım süresi doldu ve lisansları yüklemeniz gerekir.

Geçici bir çözüm olarak portaldan RDP dosyasının yerel bir kopyasını kaydedin ve bağlanmak için PowerShell komut isteminde şu komutu çalıştırın. Bu adım yalnızca bu bağlantı için lisanslama devre dışı bırakır:

        mstsc <File name>.RDP /admin

VM'nin ikiden fazla eş zamanlı Uzak Masaüstü bağlantılarında gerçekten ihtiyacınız yoksa, Uzak Masaüstü sunucu rolünü kaldırmak için Sunucu Yöneticisi'ni kullanabilirsiniz.

Daha fazla bilgi için bkz. blog gönderisine [Azure VM başarısız "Hiçbir Uzak Masaüstü lisans sunucusu ile"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a>Uzak Masaüstü bilgisayar "name" bulunamıyor.
Neden: Uzak Masaüstü İstemcisi bilgisayarınızda ayarlarında ve RDP dosyasını bilgisayarın adı çözümlenemiyor.

Olası çözümler:

* Bir kuruluşun intranet üzerinde kullanıyorsanız, bilgisayarınız proxy sunucusu erişimi olan ve HTTPS trafiğini gönderebilir emin olun.
* Yerel olarak saklanan bir RDP dosyası kullanıyorsanız, portal tarafından oluşturulan bir kullanarak deneyin. Bu adım, sanal makine veya Bulut hizmeti ve VM'nin uç nokta bağlantı noktası doğru DNS adı sahip olmasını sağlar. Portal tarafından oluşturulan örnek bir RDP dosyası aşağıda verilmiştir:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Bu RDP dosyası adresi bölümü vardır:

* ("Tailspin-azdatatier.cloudapp.net" Bu örnekte) VM içeren bulut hizmetinin tam etki alanı adı.
* Dış TCP bağlantı Uzak Masaüstü trafiğine (55919) için uç nokta.

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Bir kimlik doğrulama hatası oluştu. Yerel Güvenlik Yetkilisi iletişim kurulamıyor.
Neden: ' % S'hedef sanal makine güvenlik yetkilisi kimlik bilgilerinizi kullanıcı adı kısmı bulunamıyor.

Kullanıcı adınızı biçimde olduğunda *SecurityAuthority*\\*kullanıcıadı* (örnek: CORP\User1) *SecurityAuthority* bölümüdür (için yerel güvenlik yetkilisi) sanal makinenin bilgisayar adı ya da bir Active Directory etki alanı adı.

Olası çözümler:

* VM yerel hesapsa, VM adının doğru yazıldığından emin olun.
* Hesabın bir Active Directory etki alanında ise, etki alanı adının yazımını kontrol edin.
* Bir Active Directory etki alanı hesabı ve etki alanı adının doğru yazıldığından, bu etki alanında bir etki alanı denetleyicisinin kullanılabilir olduğunu doğrulayın. Etki alanı denetleyicileri, başlamadı nedeni bir etki alanı denetleyicisi kullanılamıyorsa içeren Azure sanal ağlarda bulunan sık karşılaşılan bir sorundur. Geçici çözüm olarak, bir yerel yönetici hesabı yerine bir etki alanı hesabı kullanabilirsiniz.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows güvenlik hatası: Kimlik bilgilerinizi çalışmadı.
Neden: ' % S'hedef sanal makine hesap adını ve parolasını doğrulayamıyor.

Windows tabanlı bir bilgisayarda yerel bir hesap veya bir etki alanı hesabının kimlik bilgilerini doğrulayabilirsiniz.

* Yerel hesaplar için kullanmak *ComputerName*\\*kullanıcıadı* sözdizimi (örnek: SQL1\Admin4798).
* Etki alanı hesapları için kullanıyorsanız *DomainName*\\*kullanıcıadı* sözdizimi (örnek: CONTOSO\peterodman).

VM'nize yeni bir Active Directory ormanındaki bir etki alanı denetleyicisi yükselttiyseniz, yeni ormanın ve etki alanı aynı parolayı eşdeğer bir hesapla oturum açtığınız yerel yönetici hesabı dönüştürülür. Yerel hesap ardından silinir.

Örneğin, eğer DC1\DCAdmin yerel hesapla oturum açmış ve sanal makine bir etki alanı denetleyicisi corp.contoso.com etki alanında yeni bir ormanda tanıtılma biçimini DC1\DCAdmin yerel hesap silindiğinde ve yeni bir etki alanı hesabı (CORP\DCAdmin) aynı parola ile oluşturuldu.

Hesap adı geçerli bir hesap sanal makine doğrulayabilirsiniz bir adı olduğunu ve parolanın doğru olduğundan emin olun.

Yerel yönetici hesabının parolasını değiştirmeniz gerekiyorsa, bkz. [bir parola veya Windows sanal makineler için Uzak Masaüstü hizmetini sıfırlama](reset-rdp.md).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Bu bilgisayar, uzak bilgisayara bağlanamıyor.
Neden: Bağlanmak için kullanılan hesap, Uzak Masaüstü Oturum açma hakları yok.

İçine uzaktan oturum grupları ve hesapları içeren bir Uzak Masaüstü users yerel grubu, her Windows bilgisayarı vardır. Bu hesaplar Uzak Masaüstü Kullanıcıları yerel Grup listelenmeyen olsa bile yerel Yöneticiler grubunun üyesi erişimi de. Yerel Yöneticiler grubunun, etki alanına katılan makineler için etki alanı için etki alanı yöneticileri de içerir.

Bağlanmak için kullandığınız hesabın Uzak Masaüstü Oturum açma hakları olduğundan emin olun. Geçici bir çözüm olarak, Uzak Masaüstü aracılığıyla bağlanmak için bir etki alanı veya yerel yönetici hesabı kullanın. İstenen hesabı Uzak Masaüstü Kullanıcıları yerel grubuna eklemek için Microsoft Yönetim Konsolu ek bileşenini kullanın (**Sistem Araçları > yerel kullanıcılar ve Gruplar > Gruplar > Uzak Masaüstü Kullanıcıları**).

## <a name="next-steps"></a>Sonraki adımlar
Bu hatalar hiçbiri oluştu ve sorunu RDP kullanarak bağlantı kurulduğunda, bkz: Bilinmeyen bir varsa [Uzak Masaüstü için sorun giderme kılavuzu](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Bir VM'de çalışan uygulamalara erişme adımlar sorun giderme için bkz: [bir Azure sanal makinesinde çalışan bir uygulamaya erişim sorunlarını giderme](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Güvenli Kabuk (SSH) kullanarak azure'da bir Linux VM bağlanmak için bkz sorunları yaşıyorsanız [sorun giderme SSH bağlantıları için azure'da bir Linux sanal makinesi](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

