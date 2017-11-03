---
title: "Azure VM'ler için belirli RDP hata iletileri | Microsoft Docs"
description: "Azure'da Windows sanal makine için Uzak Masaüstü Bağlantısı çalışırken karşılaşabileceğiniz belirli hata iletilerinin kullanımını anlamanıza"
keywords: "Uzak Masaüstü hata, Uzak Masaüstü Bağlantısı hatası, VM için bağlantı kuramıyor Uzak Masaüstü sorunlarını giderme"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 803ca6cb9e7c5633920ab44e45cf211eca1517a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Bir Windows VM Azure belirli RDP hata iletileri sorunlarını giderme
Azure'da Windows sanal makine (VM) için Uzak Masaüstü bağlantısı kullanırken, belirli bir hata iletisi alabilirsiniz. Bu makalede bazı karşılaştı, sorun giderme bunları gidermek için adımları yanı sıra daha sık karşılaşılan hata iletileri ayrıntılarını verir. VM'nize bağlantı sorunları yaşıyorsanız RDP kullanmayan belirli bir hata iletisi karşılaşırsanız bkz [Uzak Masaüstü için sorun giderme kılavuzu](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Özel hata iletileri hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Uzak Masaüstü lisans sunucusu lisans sağlamak kullanılabilir olmadığından uzak oturum kesildi](#rdplicense).
* [Uzak Masaüstü bilgisayar "name" bulamıyor](#rdpname).
* [Bir kimlik doğrulama hatası oluştu. Yerel Güvenlik Yetkilisi kurulamıyor](#rdpauth).
* [Windows güvenlik hatası: kimlik bilgilerinizi çalışmama](#wincred).
* [Bu bilgisayar uzak bilgisayara bağlanamıyor](#rdpconnect).

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Uzak Masaüstü lisans sunucusu lisans sağlamak kullanılabilir olmadığından uzak oturum bağlantısı kesildi.
Neden: Uzak Masaüstü Sunucusu rolü için 120 gün lisans yetkisiz kullanım süresi dolmuştur ve lisansları yüklemeniz gerekir.

Geçici bir çözüm olarak portaldan RDP dosyasını yerel bir kopyasını kaydedin ve bağlanmak için PowerShell komut isteminde şu komutu çalıştırın. Bu adım yalnızca o bağlantı için lisans devre dışı bırakır:

        mstsc <File name>.RDP /admin

VM ikiden fazla eşzamanlı uzak masaüstü bağlantıları gerçekten gerek duymuyorsanız, Uzak Masaüstü sunucu rolünü kaldırmak için Sunucu Yöneticisi'ni kullanabilirsiniz.

Daha fazla bilgi için blog gönderisine bakın [Azure VM başarısız "Hiçbir Uzak Masaüstü lisans sunucusu ile"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a>Uzak Masaüstü bilgisayar "name" bulunamıyor.
Neden: Uzak Masaüstü İstemcisi bilgisayarınızda RDP dosyasını ayarlarında bilgisayarın adını çözümleyemiyor.

Olası çözümler:

* Bir kuruluşun intranet üzerinde değilseniz, bilgisayarınızı proxy sunucusu erişimi ve HTTPS trafiğinin gönderebilir emin olun.
* Yerel olarak saklanan bir RDP dosyası kullanıyorsanız, portal tarafından oluşturulan bir kullanmayı deneyin. Bu adım sanal makinenin veya Bulut hizmeti ve uç nokta bağlantı noktası VM için doğru bir DNS adı sahip olmasını sağlar. Portal tarafından oluşturulan örnek bir RDP dosyası şöyledir:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Bu RDP dosyası adres bölümü vardır:

* ("Tailspin-azdatatier.cloudapp.net" Bu örnekte) VM içeren bulut hizmeti tam olarak nitelenmiş etki alanı adı.
* Dış TCP bağlantı noktasını uç nokta için Uzak Masaüstü trafiği (55919).

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Bir kimlik doğrulama hatası oluştu. Yerel Güvenlik Yetkilisi iletişim kurulamıyor.
Neden: Hedef VM kimlik bilgilerinizi kullanıcı adı kısmını güvenlik yetkilisi bulamıyor.

Kullanıcı adınızı biçiminde olduğunda *SecurityAuthority*\\*kullanıcıadı* (örnek: CORP\User1), *SecurityAuthority* bölümüdür VM'in bilgisayar adı (yerel güvenlik yetkilisi) veya bir Active Directory etki alanı adı.

Olası çözümler:

* Hesabı VM yerel ise, VM adının doğru yazıldığından emin olun.
* Hesap bir Active Directory etki alanındaysa, etki alanı adının yazımını denetleyin.
* Bir Active Directory etki alanı hesabı ise ve etki alanı adının doğru yazıldığından, bu etki alanında etki alanı denetleyicisinin kullanılabilir olduğunu doğrulayın. Başlatılan kurmadı için bir etki alanı denetleyicisi kullanılamıyor etki alanı denetleyicileri içeren Azure sanal ağlarda yaygın bir sorundur. Geçici bir çözüm olarak, bir etki alanı hesabı yerine yerel yönetici hesabı kullanabilirsiniz.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows güvenlik hatası: kimlik bilgilerinizi çalışmaz.
Neden: Hedef VM hesap adı ve parola doğrulanamıyor.

Windows tabanlı bir bilgisayarda yerel bir hesap veya bir etki alanı hesabının kimlik bilgilerini doğrulayabilir.

* Yerel hesaplar için kullanmak *ComputerName*\\*kullanıcıadı* sözdizimi (örnek: SQL1\Admin4798).
* Etki alanı hesapları için *DomainName*\\*kullanıcıadı* sözdizimi (örnek: CONTOSO\peterodman).

Yeni bir Active Directory ormanındaki etki alanı denetleyicisi, VM yükselttiyseniz, yeni orman ve etki alanında aynı parolayı eşdeğer bir hesapla oturum yerel yönetici hesabı dönüştürülür. Yerel hesap sonra silinir.

Örneğin, DC1\DCAdmin yerel hesabıyla oturum açtığınızdan ve bir etki alanı denetleyicisi corp.contoso.com etki alanı için yeni bir orman olarak sanal makine tanıtılması, DC1\DCAdmin yerel hesap silinir ve yeni bir etki alanı hesabı (CORP\DCAdmin) aynı parola ile oluşturulur.

Hesap adı sanal makine geçerli bir hesap doğrulayabilirsiniz bir adı olduğunu ve parolanın doğru olduğundan emin olun.

Yerel yönetici hesabının parolasını değiştirmeniz gerekiyorsa, bkz: [bir parola veya Windows sanal makineler için Uzak Masaüstü hizmetini sıfırlayın nasıl](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Bu bilgisayar, uzak bilgisayara bağlanamıyor.
Neden: bağlanmak için kullanılan hesabın Uzak Masaüstü Oturum açma hakları yok.

Her Windows bilgisayar hesapları ve içine uzaktan oturum grupları içeren bir Uzak Masaüstü Kullanıcıları yerel grubu vardır. Bu hesaplar Uzak Masaüstü Kullanıcıları yerel Grup listelenmeyen olsa bile yerel Yöneticiler grubunun üyeleri, ayrıca, erişimi. Etki alanına katılan makineler için yerel Yöneticiler grubunun da etki alanı için etki alanı yöneticileri içerir.

Bağlanmak için kullandığınız hesabın Uzak Masaüstü Oturum açma hakları olduğundan emin olun. Geçici bir çözüm olarak, Uzak Masaüstü üzerinden bağlanmak için bir etki alanı veya yerel yönetici hesabı kullanın. İstenen hesabı Uzak Masaüstü Kullanıcıları yerel grubuna eklemek için Microsoft Yönetim Konsolu ek bileşenini kullanın (**Sistem Araçları > yerel kullanıcılar ve Gruplar > Gruplar > Uzak Masaüstü Kullanıcıları**).

## <a name="next-steps"></a>Sonraki adımlar
Hiçbiri bu hatalar oluştu ve RDP kullanarak bağlanma ile bkz bilinmeyen varsa [Uzak Masaüstü için sorun giderme kılavuzu](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Bir VM üzerinde çalışan uygulamalar erişimde adımları sorun giderme için bkz: [bir Azure VM üzerinde çalışan bir uygulama için erişim sorunlarını giderme](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Güvenli Kabuk (SSH) kullanarak azure'da bir Linux VM bağlanmak için bkz: sorunları yaşıyorsanız [sorun giderme SSH bağlantıları azure'da bir Linux VM](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

