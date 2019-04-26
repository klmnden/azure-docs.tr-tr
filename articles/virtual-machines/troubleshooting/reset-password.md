---
title: Azure sanal makinelerinde yerel Linux parola sıfırlama | Microsoft Docs
description: Azure sanal makinesinde yerel Linux parola sıfırlama adım Ekle
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: d96d75f4f2623476f7af4e6eea930c1f2c503e3a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306960"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Azure sanal makinelerinde yerel Linux parola sıfırlama

Bu makalede, yerel Linux sanal makine (VM) parola sıfırlama için çeşitli yöntemler sunar. Kullanıcı hesabının süresi doldu veya yalnızca yeni bir hesap oluşturmak istiyorsanız, yeni bir yerel yönetici hesabı oluşturmak ve VM erişimi yeniden kazanmak için aşağıdaki yöntemleri kullanabilirsiniz.

## <a name="symptoms"></a>Belirtiler

VM'de oturum açamaz ve parolayı yanlış olduğunu belirten bir ileti alırsınız. Ayrıca, Azure portalında parola sıfırlama için VMAgent kullanamazsınız.

## <a name="manual-password-reset-procedure"></a>Yordam el ile parola sıfırlama

1.  VM'yi silin ve kullanıma açılan diskler tutun.

2.  İşletim sistemi sürücüsünü aynı konumdaki başka bir geçici sanal makineye veri diski olarak ekleyin.

3.  Süper kullanıcı olacak zamana bağlı sanal makinede aşağıdaki SSH komutunu çalıştırın.

    ```bash
    sudo su
    ```

4.  Çalıştırma **fdisk -l** ya da yeni eklenen diski bulmak için sistem günlüklerine bakın. Bağlama için sürücü adını bulun. Ardından zamana bağlı sanal makinede, ilgili günlük dosyasına bakın.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    Grep komut örnek çıktısı aşağıdaki gibidir:

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  Adlı bir bağlama noktası oluşturma **tempmount**.

    ```bash
    mkdir /tempmount
    ```

6.  İşletim sistemi diskini bağlama noktasında bağlayın. Genellikle bağlama gerek *sdc1* veya *sdc2*. Bu bölümde barındırma bağlıdır */etc* bozuk makine diskten dizin.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Çekirdek kopyasını, herhangi bir değişiklik yapmadan önce kimlik bilgisi dosyaları oluşturun:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  İhtiyacınız olan kullanıcının parolasını sıfırlama:

    ```bash
    passwd <<USER>> 
    ```

9.  Değiştirilen dosyaları bozuk makinenin diskte doğru konuma taşıyın.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Kök geri dönün ve diski çıkarın.

    ```bash
    cd /
    umount /tempmount
    ```

11. Yönetim Portalı'ndan diski çıkarın.

12. VM yeniden oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makinesinin işletim sistemi diskini başka bir Azure sanal makineye ekleyerek sorunlarını giderme](https://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: Silme ve VHD bir VM'den yeniden dağıtma](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
