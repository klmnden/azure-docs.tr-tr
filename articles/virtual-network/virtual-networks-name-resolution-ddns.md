---
title: Azure'da ana bilgisayar adlarını kaydetmek için dinamik DNS kullanma | Microsoft Docs
description: Kendi DNS sunucularınızı ana bilgisayar adlarını kaydetmek için dinamik DNS ayarlamayı öğrenin.
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
ms.openlocfilehash: c2ef842fd62ef060f06536d66387c3facd0627b5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60640387"
---
# <a name="use-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Kendi DNS sunucusu ana bilgisayar adlarını kaydetmek için dinamik DNS kullanma

[Azure ad çözümlemesi sağlar](virtual-networks-name-resolution-for-vms-and-role-instances.md) sanal makineler (VM) ve rol örnekleri için. Azure'nın varsayılan DNS tarafından sağlanan özellikler aşan, ad çözümlemesi gerektiğinde, kendi DNS sunucularınızı sağlayabilirsiniz. Kendi DNS sunucularınızı kullanarak belirli ihtiyaçlarınıza uyacak şekilde DNS çözümünüzü uyarlama olanağı sağlar. Örneğin, Active Directory etki alanı denetleyicinizi aracılığıyla şirket içi kaynaklara erişim gerekebilir.

Özel DNS sunucularınızın Azure Vm'leri olarak barındırıldığında, ana bilgisayar adlarını çözümlemek için Azure için aynı sanal ağ için ana bilgisayar adı sorguları iletebilirsiniz. Bu seçeneği kullanmak istemiyorsanız, VM ana bilgisayar adları DNS sunucunuzun dinamik DNS (DDNS) kullanarak kaydedebilirsiniz. Azure, alternatif düzenlemeleri genellikle gereken şekilde DNS sunucularınızın doğrudan kayıtları oluşturmak için kimlik bilgilerine sahip değil. Alternatif bazı yaygın senaryolar izleyin:

## <a name="windows-clients"></a>Windows istemcileri
Olmayan etki alanı ile birleşik Windows istemcileri, bunlar önyükleme veya IP adreslerini değiştiğinde güvenli olmayan DDNS güncelleştirmeleri çalışır. DNS ana bilgisayar adının yanı sıra birincil DNS soneki adıdır. Azure birincil DNS soneki boş bırakır, ancak VM soneki aracılığıyla ayarlayabileceğiniz [kullanıcı arabirimi](https://technet.microsoft.com/library/cc794784.aspx) veya [PowerShell](/powershell/module/dnsclient/set-dnsclient).

Etki alanına katılmış Windows istemcileri IP adreslerini, etki alanı denetleyicisi ile güvenli DDNS kullanarak kaydedin. Etki alanına katılma işlemi birincil DNS soneki istemci üzerinde ayarlar ve oluşturur ve güven ilişkisini korur.

## <a name="linux-clients"></a>Linux istemcileri
Linux istemcileri genellikle kendi başlangıç üzerindeki DNS sunucusu ile kayıt ettirmezseniz, DHCP sunucusu yaptığını varsayalım. Azure'nın DHCP sunucuları kayıtlarını DNS sunucunuzun kaydetmek üzere kimlik bilgileri yok. Adında bir araç kullanabilirsiniz `nsupdate`bağlama pakete dahil, DDNS göndermek için güncelleştirir. DDNS Protokolü standartlaştırılmıştır çünkü kullanabileceğiniz `nsupdate` bile ne zaman, bağlama DNS sunucusunda kullanmıyorsunuz demektir.

Oluşturmak ve DNS sunucusunun ana bilgisayar adı girişi korumak için DHCP istemcisi tarafından sağlanan kancaları kullanabilirsiniz. DHCP döngüsü sırasında istemci betiklerini yürütür */etc/dhcp/dhclient-exit-hooks.d/* . Kancaları yeni IP adresini kullanarak kaydetmek için kullanabileceğiniz `nsupdate`. Örneğin:

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

Ayrıca `nsupdate` güvenli DDNS gerçekleştirmek için komut güncelleştirir. Örneğin, bir bağlama DNS sunucusu kullanırken, bir ortak-özel anahtar çifti olan [oluşturulan](http://linux.yyz.us/nsupdate/). DNS sunucusu [yapılandırılmış](http://linux.yyz.us/dns/ddns-server.html) ortak anahtarın parçası bu nedenle, BT kontrol edebilirsiniz istek imza. Anahtar çifti sağlamak `nsupdate`, kullanın `-k` DDNS güncelleştirme isteği imzalanması için seçenek.

Bir Windows DNS sunucusu kullanırken, Kerberos kimlik doğrulaması ile kullanabileceğiniz `-g` parametresinde `nsupdate`, ancak Windows sürümünde kullanılabilir değil `nsupdate`. Kerberos kullanmak için `kinit` kimlik bilgileri yüklenemiyor. Kimlik bilgilerinden for example, yükleme bir [anahtar tablosu dosya](https://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)), ardından `nsupdate -g` önbellekten kimlik bilgilerini alır.

Gerekirse, sanal makineleriniz için bir DNS Arama soneki ekleyebilirsiniz. DNS son eki içinde belirtilen */etc/resolv.conf* dosya. Çoğu Linux dağıtım paketlerini içeriği otomatik olarak yönetir. Bu dosya, bu nedenle genellikle düzenleyemezsiniz. Ancak DHCP istemcisinin kullanarak soneki geçersiz kılabilirsiniz `supersede` komutu. Son eki geçersiz kılmak için aşağıdaki satırı ekleyin */etc/dhcp/dhclient.conf* dosyası:

```
supersede domain-name <required-dns-suffix>;
```
