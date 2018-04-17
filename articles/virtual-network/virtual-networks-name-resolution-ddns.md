---
title: Dinamik DNS ana bilgisayar adları kaydolmak için kullanma
description: Bu sayfa, ana bilgisayar adları kendi DNS sunucularınızı kaydetmek için dinamik DNS ayarlamalısınız konusunda ayrıntılarını verir.
services: dns
documentationcenter: na
author: GarethBradshawMSFT
manager: timlt
editor: ''
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: garbrad
ms.openlocfilehash: 0539736f4b7294a613ffd42c28ff6bb29cf9e2bf
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Dinamik DNS ana bilgisayar adları kendi DNS sunucusu kaydolmak için kullanma
[Azure ad çözümlemesi sağlar](virtual-networks-name-resolution-for-vms-and-role-instances.md) sanal makineleri (VM'ler) ve rol örnekleri için. Ancak, ad çözümlemesi Azure tarafından sağlanan ötesinde olduğunuzda, kendi DNS sunucularınızı sağlayabilir. Bu DNS çözümünüzü belirli gereksinimlerinize uyacak şekilde uyarlamak için güç sağlar. Örneğin, Active Directory etki alanı denetleyicinizi aracılığıyla şirket içi kaynaklara erişmek gerekebilir.

Azure VM'ler, özel DNS sunucularınızın barındırıldığında ana bilgisayar adları çözümlemek için Azure'a aynı sanal ağ için ana bilgisayar adı sorguları iletebilir. Bu yol kullanmak istemiyorsanız, VM ana bilgisayar adları DNS sunucunuzun dinamik DNS kullanarak kaydedebilirsiniz.  Azure alternatif düzenlemeleri genellikle gerektiği şekilde kayıtları, DNS sunucularınızın doğrudan oluşturmak için (örneğin, kimlik bilgileri) yeteneğine sahip değil. Burada, Alternatiflerle birlikte ortak bazı senaryolar vardır.

## <a name="windows-clients"></a>Windows istemcileri
Olmayan etki alanına katılmış Windows istemcileri güvenli olmayan dinamik DNS (DDNS) güncelleştirmeleri bunlar önyüklediğinizde veya IP adreslerini değiştiğinde çalışır. DNS ana bilgisayar adı ve birincil DNS sonekine adıdır. Azure birincil DNS soneki boş bırakır, ancak bu VM'de aracılığıyla ayarlayabileceğiniz [kullanıcı arabirimi](https://technet.microsoft.com/library/cc794784.aspx) veya [PowerShell](/powershell/module/dnsclient/set-dnsclient).

Etki alanına katılmış Windows istemcileri güvenli dinamik DNS kullanarak IP adresleri etki alanı denetleyicisi ile kaydedin. Etki alanına katılma işlemi istemcide birincil DNS soneki ayarlar ve oluşturur ve güven ilişkisini korur.

## <a name="linux-clients"></a>Linux istemcileri
Linux istemcileri genellikle başlangıçta DNS sunucusuyla kendilerini yok, DHCP sunucusu mevcut varsayalım. Azure'nın DHCP sunucuları, özelliği veya kimlik bilgilerini kayıtları, DNS sunucusu kaydetmek için yok.  Adlı bir aracı kullanabilirsiniz *nsupdate*, BIND pakete dahil, dinamik DNS göndermek için güncelleştirir. Dinamik DNS protokolü standartlaştırılmış için kullanabileceğiniz *nsupdate* bile zaman BIND DNS sunucusunda kullanmakta olduğunuz değil.

Oluşturmak ve DNS sunucusu ana bilgisayar adı girişi korumak için DHCP istemcisi tarafından sağlanan kancaları kullanabilirsiniz. DHCP döngüsü sırasında istemci komut yürütür */etc/dhcp/dhclient-exit-hooks.d/*. Bu, yeni IP adresini kullanarak kaydetmek için kullanılabilir *nsupdate*. Örneğin:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        
        

Aynı zamanda *nsupdate* güvenli dinamik DNS güncelleştirmeleri gerçekleştirmek için komutu. Örneğin, bir BIND DNS sunucusu kullanırken, bir genel-özel anahtar çifti [oluşturulan](http://linux.yyz.us/nsupdate/).  DNS sunucusu [yapılandırılmış](http://linux.yyz.us/dns/ddns-server.html) ortak olan istek imza doğrulayabilmeniz için anahtar parçası olan. Kullanmalısınız *-k* için anahtar çifti sağlamak için seçeneği *nsupdate* sırada dinamik DNS güncelleştirme isteği imzalanması gerekir.

Bir Windows DNS sunucusu kullanırken, Kerberos kimlik doğrulaması ile kullanabileceğiniz *-g* parametresinde *nsupdate* (Windows sürümünde kullanılabilir *nsupdate*). Bunu yapmak için kullanın *kinit* kimlik bilgileri yüklemek için (örneğin bir [keytab dosya](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Ardından *nsupdate -g* önbellekten kimlik bilgileri çeker.

Gerekli olursa, DNS Arama soneki Vm'leriniz için ekleyebilirsiniz. DNS soneki belirtilen */etc/resolv.conf* dosya. Çoğu Linux distro'lar içeriği otomatik olarak yönetir bu dosya, bu nedenle genellikle düzenleyemezsiniz. Ancak, DHCP istemci kullanarak soneki geçersiz kılabilirsiniz *yerine geçen* komutu. Bunu yapmak için */etc/dhcp/dhclient.conf*, ekleyin:

        supersede domain-name <required-dns-suffix>;

