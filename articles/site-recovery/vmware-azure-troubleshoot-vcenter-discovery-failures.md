---
title: Azure Site Recovery ile azure'a VMware VM'LERİNDE olağanüstü durum kurtarma sırasında şirket içine yeniden çalışma sorunlarını giderme | Microsoft Docs
description: Bu makalede, azure'da Azure Site Recovery ile VMware VM'LERİNDE olağanüstü durum kurtarma sırasında yeniden çalışma ve yeniden koruma sorunlarını gidermenin yolları açıklanır.
author: vDonGlover
manager: JarrettRenshaw
ms.service: site-recovery
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: v-doglov
ms.openlocfilehash: c598c5e238458c010500579c5371622b85e71de0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60565200"
---
# <a name="troubleshoot-vcenter-discovery-failures"></a>VCenter bulma sorunlarını giderme

Bu makalede, VMware vCenter bulma hataları nedeniyle oluşan sorunları gidermenize yardımcı olur.

## <a name="non-numeric-values-in-the-maxsnapshots-property"></a>Sayısal olmayan değerleri maxSnapShots özelliği

Özellik için bir sayısal olmayan değer aldığında 9.20 önceki sürümlerinde, vCenter bağlantısını keser `snapshot.maxSnapShots` bir VM'de özelliği.

Bu sorun, hata kimliği 95126 tarafından tanımlanır.

    ERROR :: Hit an exception while fetching the required informationfrom vCenter/vSphere.Exception details:
    System.FormatException: Input string was not in a correct format.
       at System.Number.StringToNumber(String str, NumberStyles options, NumberBuffer& number, NumberFormatInfo info, Boolean parseDecimal)
       at System.Number.ParseInt32(String s, NumberStyles style, NumberFormatInfo info)
       at VMware.VSphere.Management.InfraContracts.VirtualMachineInfo.get_MaxSnapshots()
    
Bu sorunu çözmek için:

- Sanal makine belirleyin ve değeri bir sayısal değer (vcenter ayarlarını VM Düzenle) ayarlayın.

Veya

- Yapılandırma sunucunuzda 9.20 veya üzeri sürüme yükseltin.

## <a name="proxy-configuration-issues-for-vcenter-connectivity"></a>VCenter bağlantısı için proxy yapılandırma sorunları

vCenter bulma sistem kullanıcı tarafından yapılandırılan sistem varsayılan proxy ayarlarını korur. DRA hizmeti birleşik Kurulum Yükleyici veya OVA şablonunu kullanarak yapılandırma sunucusu yüklemesi sırasında kullanıcı tarafından sağlanan proxy ayarlarını korur. 

Genel olarak, proxy ortak ağlara iletişim kurmak için kullanılır; Azure ile iletişim kurma gibi. Proxy yapılandırılır ve yerel bir ortamda vCenter ise, DRA ile iletişim kurmak mümkün olmayacaktır.

Bu sorun oluştuğunda aşağıdaki durumlarda oluşur:

- VCenter server \<vCenter > hata nedeniyle erişilebilir değil: Uzak sunucu hata döndürdü: (503) sunucusu kullanılamıyor
- VCenter server \<vCenter > hata nedeniyle erişilebilir değil: Uzak sunucu hata döndürdü: Uzak sunucuya bağlanılamıyor.
- VCenter/ESXi sunucusuna bağlanılamıyor.

Bu sorunu çözmek için:

İndirme [PsExec aracı](https://aka.ms/PsExec). 

Sistem kullanıcı bağlamı erişmek ve proxy adresi yapılandırılıp yapılandırılmadığını belirlemek için PsExec aracını kullanın. Bu gibi durumlarda, vCenter ardından aşağıdaki yordamları kullanarak atlama listesine ekleyebilirsiniz.

Keşif proxy yapılandırması için:

1. PsExec aracını kullanarak sistem kullanıcı bağlamı IE'de Aç.
    
    psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"

2. Internet Explorer'ı vCenter IP adresini atlamak için Proxy'yi değiştirin.
3. Tmanssvc hizmetini yeniden başlatın.

DRA proxy yapılandırması için:

1. Bir komut istemi açın ve Microsoft Azure Site Recovery sağlayıcısı klasörü açın.
 
    **CD C:\Program Files\Microsoft Azure Site Recovery sağlayıcısı**

3. Komut isteminden aşağıdaki komutu çalıştırın.
   
   **DRCONFIGURATOR. EXE / /AddBypassUrls configure [IP adresini/FQDN'yi Server vCenter ekleme zamanında sağlanan vCenter'ın]**

4. DRA Sağlayıcı hizmetini yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM'LERİNDE olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-manage-configuration-server#refresh-configuration-server) 
