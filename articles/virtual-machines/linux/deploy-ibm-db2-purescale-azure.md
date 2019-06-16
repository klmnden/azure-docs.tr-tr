---
title: IBM DB2 pureScale azure'da dağıtma
description: IBM DB2 ortamın Azure üzerinde IBM DB2 pureScale z/OS üzerinde çalışan bir kuruluş geçirmek için kullanılan bir örnek mimari dağıtmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: fba6b5308b380b374611c09747302dbf8305dd9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60716056"
---
# <a name="deploy-ibm-db2-purescale-on-azure"></a>IBM DB2 pureScale azure'da dağıtma

Bu makalede nasıl dağıtılacağı bir [örnek mimarisi](ibm-db2-purescale-azure.md) Kurumsal müşteri z/OS üzerinde IBM DB2 pureScale Azure üzerinde çalışan, IBM DB2 ortamdan geçiş yapmak için kısa bir süre önce kullanılan.

Geçiş için kullanılan adımları için yükleme komut bkz [DB2onAzure](https://aka.ms/db2onazure) github deposu. Bu komut dosyalarını normal, orta ölçekli çevrimiçi işlem gerçekleştirme (OLTP) iş yükü mimarisini temel alır.

## <a name="get-started"></a>başlarken

Bu mimariyi dağıtmak için bulunan deploy.sh betik indirip çalıştırmak [DB2onAzure](https://aka.ms/db2onazure) github deposu.

Depo bir Grafana Pano ayarlamak için komut dosyalarını da vardır. Pano Prometheus, açık kaynaklı izleme ve uyarı sistemi ile DB2 dahil sorgulamak için kullanabilirsiniz.

> [!NOTE]
> İstemci üzerinde deploy.sh betik, özel SSH anahtarlarını oluşturur ve bunları dağıtım şablonu için HTTPS üzerinden geçirir. Daha fazla güvenlik için kullanılması önerilir [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview) gizli anahtarları ve parolaları saklamak için.

## <a name="how-the-deployment-script-works"></a>Dağıtım betiği nasıl çalışır?

Deploy.sh betiği oluşturur ve bu mimarinin Azure kaynakları yapılandırır. Komut dosyası hedef ortamda kullanılan sanal makinelerin ve Azure aboneliği için ister ve sonra aşağıdaki işlemleri gerçekleştirir:

-   Kaynak grubu, sanal ağ ve alt ağları Azure üzerinde yükleme için ayarlar

-   Ortam için ağ güvenlik gruplarını ve SSH ayarlar

-   NIC GlusterFS hem DB2 pureScale sanal makineleri ayarlar

-   GlusterFS depolama sanal makineleri oluşturur

-   Sıçrama kutusu sanal makine oluşturur

-   DB2 pureScale sanal makineleri oluşturur

-   Bu DB2 pureScale ping Tanık sanal makine oluşturur

-   Test etmek için kullanılacak bir Windows sanal makinesi oluşturur, ancak herhangi bir şey üzerinde yüklemez

Ardından, bir iSCSI sanal depolama alanı ağı (vSAN) Azure üzerinde paylaşılan depolama alanı için dağıtım betikleri ayarlayın. Bu örnekte, GlusterFS için iSCSI bağlanır. Bu çözüm, iSCSI hedefleri Windows tek düğüm olarak yüklemek için bir seçenek sağlar. iSCSI, paylaşılan depolama alanına bağlanmak için bir cihaz arabirimi kullanılacak DB2 pureScale Kurulum yordamı izin veren TCP/IP üzerinde paylaşılan blok depolama arabirimi sağlar. GlusterFS temel bilgilerini görmek [mimarisi: Birim türleri](https://docs.gluster.org/en/latest/Quick-Start-Guide/Architecture/) Gluster Docs konuda.

Dağıtım betikleri genel adımları çalıştırın:

1.  Azure üzerinde paylaşılan depolama kümesi ayarlama için GlusterFS kullanın. Bu adım, en az iki Linux düğümleri içerir. Kurulum Ayrıntılar için bkz [Microsoft azure'da Red Hat Gluster Storage ayarlama](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.1/html/deployment_guide_for_public_cloud/chap-documentation-deployment_guide_for_public_cloud-azure-setting_up_rhgs_azure) Red Hat Gluster belgelerinde.

2.  Bir iSCSI hedef Linux sunucuları doğrudan arabirim için GlusterFS ayarlayın. Kurulum Ayrıntılar için bkz [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/) GlusterFS Yönetim Kılavuzu'nda.

3.  Linux sanal makinelerinde iSCSI başlatıcısı ayarlayın. Başlatıcı GlusterFS kümedeki bir iSCSI hedef kullanarak erişir. Kurulum Ayrıntılar için bkz [bir iSCSI hedefi ve başlatıcısını de Linux için nasıl yapılandırılacağını](https://www.rootusers.com/how-to-configure-an-iscsi-target-and-initiator-in-linux/) RootUsers belgelerinde.

4.  İSCSI arabirimi için depolama katmanı olarak GlusterFS yükleyin.

Betikleri iSCSI cihazı oluşturmanızın ardından son adım DB2 pureScale yüklemektir. DB2 pureScale kurulumunun parçası olarak [IBM Spektrumun ölçek](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0057167.html) (eski adıyla GPFS da bilinir) derlenir ve GlusterFS kümeye yüklü. Bu kümelenmiş dosya sistemi DB2 pureScale DB2 pureScale altyapısı çalıştıran sanal makineler arasında veri paylaşmasına izin verir. Daha fazla bilgi için [IBM Spektrumun ölçek](https://www.ibm.com/support/knowledgecenter/en/STXKQY_4.2.0/ibmspectrumscale42_welcome.html) IBM Web sitesinde belge.

## <a name="db2-purescale-response-file"></a>DB2 pureScale yanıt dosyası

GitHub deposunu DB2server.rsp, DB2 pureScale yükleme için otomatik bir betik oluşturmanıza olanak sağlayan bir yanıt (.rsp) dosyasını içerir. Aşağıdaki tabloda kurulum için yanıt dosyasını kullanan DB2 pureScale seçenekleri listeler. Yanıt dosyası, ortamınız için gerektiği şekilde özelleştirebilirsiniz.

> [!NOTE]
> DB2server.rsp, örnek bir yanıt dosyası dahil [DB2onAzure](https://aka.ms/db2onazure) github deposu. Bu dosya kullanmak istiyorsanız, ortamınızda çalışmadan önce onu düzenlemeniz gerekir.

| Ekran adı               | Alan                                        | Değer                                                                                                 |
|---------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Hoş Geldiniz                   |                                              | Yeni yükleme                                                                                           |
| Bir ürün seçin          |                                              | DB2 sürüm 11.1.3.3. DB2 pureScale ile Server sürümleri                                              |
| Yapılandırma             | Dizin                                    | /Data1/OPT/ibm/DB2/V11.1                                                                              |
|                           | Yükleme türünü seçin                 | Tipik                                                                                               |
|                           | IBM koşullarını kabul ediyorum                     | İşaretli                                                                                               |
| Örnek sahibi            | Mevcut kullanıcı için örneği, kullanıcı adı        | DB2sdin1                                                                                              |
| Çevrelenmiş kullanıcı               | Varolan bir kullanıcı, kullanıcı adı                     | DB2sdfe1                                                                                              |
| Küme dosya sistemi       | Paylaşılan disk bölümü cihaz yolu            | /dev/DM-2                                                                                             |
|                           | Bağlama noktası                                  | /DB2sd\_1804a                                                                                         |
|                           | Veri diski paylaşılan                         | /dev/DM-1                                                                                             |
|                           | Bağlama noktası (veriler)                           | / DB2fs/datafs1                                                                                        |
|                           | Paylaşılan disk günlüğü                          | /dev/DM-0                                                                                             |
|                           | Bağlama noktası (günlük)                            | / DB2fs/logfs1                                                                                         |
|                           | DB2 Küme Hizmetleri Tiebreaker. Cihaz yolu | /dev/DM-3                                                                                             |
| Ana bilgisayar listesi                 | D1 [eth1] d2 [eth1] cf1 [eth1] cf2 [eth1] |                                                                                                       |
|                           | Tercih edilen birincil CF                         | cf1                                                                                                   |
|                           | Tercih edilen ikincil CF                       | cf2                                                                                                   |
| Yanıt dosyası ve özeti | İlk seçenek                                 | IBM DB2 pureScale özelliğiyle DB2 Server Edition'ı yükleyin ve bir yanıt dosyasında Ayarlarımı Kaydet |
|                           | Yanıt dosyası adı                           | /root/DB2server.rsp                                                                                   |

### <a name="notes-about-this-deployment"></a>Bu dağıtım ile ilgili notlar

- Burada Kurulumu gerçekleşir sanal makinede yeniden başlatma sonrasında /dev-dm0, /dev-dm1 /dev-dm2 ve /dev-dm3 değerlerini değiştirebilirsiniz (d0 otomatik komut dosyasında). Doğru değerleri bulmak için Kurulum'u çalıştıracağınız yanıt dosyası sunucuda tamamlamadan önce aşağıdaki komutu kullanabilirsiniz:

   ```
   [root\@d0 rhel]\# ls -als /dev/mapper
   total 0
   0 drwxr-xr-x 2 root root 140 May 30 11:07 .
   0 drwxr-xr-x 19 root root 4060 May 30 11:31 ..
   0 crw------- 1 root root 10, 236 May 30 11:04 control
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2data1 -\> ../dm-1
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2log1 -\> ../dm-0
   0 lrwxrwxrwx 1 root root 7 May 30 11:26 db2shared -\> ../dm-2
   0 lrwxrwxrwx 1 root root 7 May 30 11:08 db2tieb -\> ../dm-3
   ```

- Ayar betikleri, diğer adlar için iSCSI disklerini kullanın gerçek adlarını bir kolayca bulunabilir.

- Kurulum betiğini d0 üzerinde çalıştırdığınızda **/dev/dm -\***  değerleri d1, cf0 ve cf1 farklı olabilir. Değerleri fark DB2 pureScale Kurulum etkilemez.

## <a name="troubleshooting-and-known-issues"></a>Sorun çözümü ve bilinen sorunlar

GitHub deposunu yazarlar bakımını yapan bir Bilgi Bankası içerir. Bu, sahip olduğunuz olası sorunlar ve çözümleri deneyebilirsiniz listeler. Örneğin, sorunları oluşabilir bilinen olduğunda:

-   Ağ geçidi IP adresini erişmeye çalışıyoruz.

-   Genel Kamu Lisansı (GPL) derleme.

-   Konaklar arasında güvenlik el sıkışması başarısız olur.

-   DB2 yükleyici, var olan bir dosya sistemi algılar.

-   El ile IBM Spektrumun ölçek güvenilirliğinden.

-   IBM Spektrumun ölçek zaten oluşturulduğunda DB2 pureScale güvenilirliğinden.

-   DB2 pureScale ve IBM Spektrumun ölçek kaldırırsınız.

Bu ve diğer bilinen sorunlar hakkında daha fazla bilgi için bkz: kb.md dosyasında [DB2onAzure](https://aka.ms/DB2onAzure) depo.

## <a name="next-steps"></a>Sonraki adımlar

-   [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)

-   [Bir DB2 pureScale özelliği yükleme için gerekli kullanıcı oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)

-   [DB2icrt - örnek bir komut oluşturun](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)

-   [DB2 pureScale kümeleri veri çözümü](https://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)

-   [IBM veri Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

-   [Platform Modernizasyonu Alliance: Azure üzerinde IBM DB2](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)

-   [Azure sanal veri merkezi Lift- and -Shift Kılavuzu](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
