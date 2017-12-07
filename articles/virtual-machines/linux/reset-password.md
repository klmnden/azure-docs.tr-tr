---
title: "Azure vm'lerinde yerel Linux parola sıfırlama | Microsoft Docs"
description: "Azure VM'de yerel Linux parola sıfırlama adım tanıtır"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: delhan
ms.openlocfilehash: 2591436b576580f51129b9dadbfe3814f23ac2cc
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Azure vm'lerinde yerel Linux parola sıfırlama

Bu makalede, yerel Linux sanal makine (VM) parola sıfırlama için çeşitli yöntemler sunar. Kullanıcı hesabının süresinin dolduğunu veya yalnızca yeni bir hesap oluşturmak istiyorsanız, yeni bir yerel yönetici hesabı oluşturun ve VM erişimi yeniden kazanmak için aşağıdaki yöntemleri kullanabilirsiniz.

## <a name="symptoms"></a>Belirtiler

VM oturum açamıyor ve kullandığınız parola yanlış olduğunu belirten bir ileti alırsınız. Ayrıca, Azure Portal'da, parolanızı sıfırlamak için VMAgent kullanamazsınız. 

## <a name="manual-password-reset-procedure"></a>El ile parola sıfırlama yordamı

1.  VM silme ve kullanıma açılan diskler tutun.

2.  İşletim sistemi sürücüsü zamana bağlı başka bir VM ile aynı konumda bir veri diski iliştirin.

3.  Aşağıdaki SSH komutu süper kullanıcı olmasını zamana bağlı VM üzerinde çalıştırın.


    ~~~~
    sudo su
    ~~~~

4.  Çalıştırma **fdisk -l** ya da yeni eklenen disk bulmak için sistem günlüklerine bakın. Bağlama için sürücü adını bulun. Ardından zamana bağlı VM ilgili günlük dosyasına bakın.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    Örnek çıktı grep komut verilmiştir:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Adlı bir bağlama noktası oluşturma **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  İşletim sistemi diski bağlama noktasında bağlayın. Bağlama sdc1 veya sdc2 için genellikle olması gerekir. Bu barındırma/etc dizin bozuk makine diskten bölümünde bağlıdır.

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  Herhangi bir değişiklik yapmadan önce bir yedekleme gerçekleştirin:

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  Gereksinim duyduğunuz kullanıcının parolasını sıfırlama:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  Değiştirilen Dosyalar bozuk makinenin diskte doğru konuma taşıyın.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ~~~~

10. Kök geri dönün ve disk çıkarın.

    ~~~~
    cd /
    umount /tempmount
    ~~~~

11. Yönetim Portalı diski kullanımdan çıkarın.

12. VM yeniden oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* [Başka bir Azure VM için işletim sistemi diski ekleyerek Azure VM sorun giderme](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: silin ve yeniden VHD'den VM dağıtmak nasıl](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
