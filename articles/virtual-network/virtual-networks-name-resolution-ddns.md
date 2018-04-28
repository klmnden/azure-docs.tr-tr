---
title: Dinamik DNS ana bilgisayar adları Azure'a kaydetme kullanmayı | Microsoft Docs
description: Dinamik DNS ana bilgisayar adları kendi DNS sunucularınızı kaydetmek için kurulumu öğrenin.
services: dns
documentationcenter: na
author: subsarma
manager: vitinnan
editor: ''
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: subsarma
ms.openlocfilehash: bbbce45b7c321fd4934374c76f2a4421b125d46f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="use-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Dinamik DNS ana bilgisayar adları kendi DNS sunucusu kaydetmek için kullanın

[Azure ad çözümlemesi sağlar](virtual-networks-name-resolution-for-vms-and-role-instances.md) sanal makineler (VM) ve rol örnekleri için. Ad çözümlemesi Azure'nın varsayılan DNS tarafından sağlanan özellikler aşan gerektiğinde kendi DNS sunucularınızı sağlayabilir. Kendi DNS sunucularınızı kullanarak belirli gereksinimlerinize uygun olarak, DNS çözümünüzün uyarlama olanağı sağlar. Örneğin, Active Directory etki alanı denetleyicinizi aracılığıyla şirket içi kaynaklara erişmek gerekebilir.

Azure VM'ler, özel DNS sunucularınızın barındırıldığında, ana bilgisayar adları çözümlemek için Azure'a aynı sanal ağ için ana bilgisayar adı sorguları iletebilir. Bu seçeneği kullanmak istemiyorsanız, VM ana bilgisayar adları DNS sunucunuzun dinamik DNS (DDNS) kullanarak kaydedebilirsiniz. Azure alternatif düzenlemeleri genellikle gerektiği şekilde kayıtları, DNS sunucularınızın doğrudan oluşturmak için kimlik bilgilerine sahip değil. Alternatif bazı yaygın senaryolar izleyin:

## <a name="windows-clients"></a>Windows istemcileri
Olmayan etki alanına katılmış Windows istemcileri güvenli DDNS güncelleştirmeleri bunlar önyüklediğinizde ya da kendi IP adresini değiştirse çalışır. DNS ana bilgisayar adı ve birincil DNS sonekine adıdır. Azure birincil DNS soneki boş bırakır, ancak aracılığıyla VM soneki ayarlayabilirsiniz [kullanıcı arabirimi](https://technet.microsoft.com/library/cc794784.aspx) veya [PowerShell](/powershell/module/dnsclient/set-dnsclient).

Etki alanına katılmış Windows istemcileri IP adresleri, etki alanı denetleyicisiyle güvenli DDNS kullanarak kaydedin. Etki alanına katılma işlemi istemcide birincil DNS soneki ayarlar ve oluşturur ve güven ilişkisini korur.

## <a name="linux-clients"></a>Linux istemcileri
Linux istemcileri genellikle başlangıçta DNS sunucusuyla kendilerini yok, DHCP sunucusu mevcut varsayalım. Azure'nın DHCP sunucuları, DNS sunucunuzun kayıtlarını kaydetmek için kimlik bilgilerini gerekmez. Adlı bir aracı kullanabilirsiniz `nsupdate`, BIND pakete dahil, DDNS göndermek için güncelleştirir. DDNS Protokolü standartlaştırılmış için kullanabileceğiniz `nsupdate` bile zaman BIND DNS sunucusunda kullanmakta olduğunuz değil.

Oluşturmak ve DNS sunucusu ana bilgisayar adı girişi korumak için DHCP istemcisi tarafından sağlanan kancaları kullanabilirsiniz. DHCP döngüsü sırasında istemci komut yürütür */etc/dhcp/dhclient-exit-hooks.d/*. Yeni IP adresini kullanarak kaydetmek için kancaları kullanabilirsiniz `nsupdate`. Örneğin:

```bash
#!/bin/sh
requireddomain=mydomain.local

# only execute on the primary nic
if [ "$interface" != "eth0" ]
then
    return
fi

# When you have a new IP, perform nsupdate
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
```

Aynı zamanda `nsupdate` güvenli DDNS gerçekleştirmek için komut güncelleştirir. Örneğin, bir BIND DNS sunucusu kullanırken, bir genel-özel anahtar çifti [oluşturulan](http://linux.yyz.us/nsupdate/). DNS sunucusu [yapılandırılmış](http://linux.yyz.us/dns/ddns-server.html) ortak anahtarının bir parçası, bu nedenle, BT'nin doğrulayabilir isteği imza. Anahtar çiftine sağlamak için `nsupdate`, kullanın `-k` seçeneği DDNS imzalanacak isteği güncelleştirmek için.

Bir Windows DNS sunucusu kullanırken, Kerberos kimlik doğrulaması ile kullanabileceğiniz `-g` parametresinde `nsupdate`, ancak Windows sürümünde kullanılabilir değil `nsupdate`. Kerberos kullanmak için `kinit` kimlik bilgileri yüklenemedi. Kimlik bilgilerini for example, yükleme bir [keytab dosya](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)), ardından `nsupdate -g` önbellekten kimlik bilgilerini kullanır.

Gerekli olursa, DNS Arama soneki Vm'leriniz için ekleyebilirsiniz. DNS soneki belirtilen */etc/resolv.conf* dosya. Çoğu Linux distro'lar içeriği otomatik olarak yönetir bu dosya, bu nedenle genellikle düzenleyemezsiniz. Ancak, DHCP istemci kullanarak soneki geçersiz kılabilirsiniz `supersede` komutu. Son ekini geçersiz kılmak için aşağıdaki satırı ekleyin */etc/dhcp/dhclient.conf* dosyası:

```
supersede domain-name <required-dns-suffix>;
```
