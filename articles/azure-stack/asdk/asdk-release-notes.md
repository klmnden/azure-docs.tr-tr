---
title: Microsoft Azure Stack Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeleri, düzeltmeleri ve bilinen sorunlar için Azure Stack Geliştirme Seti.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 04/04/2019
ms.openlocfilehash: ee98aee0873796c2a06db73d3365e3ff9ef87ccf
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58915892"
---
# <a name="asdk-release-notes"></a>ASDK sürüm notları

Bu makalede, değişiklikler, düzeltmeleri ve bilinen sorunlar Azure Stack geliştirme Seti'ni (ASDK) hakkında bilgi sağlar. Hangi sürümü çalıştırdığınızdan emin değilseniz yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](../azure-stack-updates.md#determine-the-current-version).

Abone olarak ASDK yenilikler ile güncel kalın [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [akış](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-1903"></a>Derleme 1903

1903 yükü ASDK yayın içermez.

## <a name="build-11902069"></a>Derleme 1.1902.0.69

### <a name="new-features"></a>Yeni Özellikler

- 1902 derleme planlar, teklifler, kotalar ve eklenti planı oluşturmak için Azure Stack Yönetici portalında yeni bir kullanıcı arabirimi sunar. Ekran görüntüleri de dahil daha fazla bilgi için bkz. [planlar, teklifler ve kotalar oluşturma](../azure-stack-create-plan.md).

- Diğer değişiklikler ve geliştirmeler bu sürümde bir listesi için bkz. [Bu bölümde](../azure-stack-update-1902.md#improvements) Azure Stack'te sürüm notları.

<!-- ### New features

- For a list of new features in this release, see [this section](../azure-stack-update-1902.md#new-features) of the Azure Stack release notes.

### Fixed and known issues

- For a list of issues fixed in this release, see [this section](../azure-stack-update-1902.md#fixed-issues) of the Azure Stack release notes. For a list of known issues, see [this section](../azure-stack-update-1902.md#known-issues-post-installation).
- Note that [available Azure Stack hotfixes](../azure-stack-update-1902.md#azure-stack-hotfixes) are not applicable to the Azure Stack ASDK. -->

### <a name="known-issues"></a>Bilinen sorunlar

- Soruna hangi paketlerin 1450 bayt bir iç yük dengeleyici (ILB) için bırakılan belirlenmiştir. Sorunu 1901 itibarıyla konağa taşınamaz rolü, geçiş yapan kapsüllenmiş VXLAN paket uyum sağlamak için çok düşük olan konak üzerindeki MTU ayarı kaynaklanır. İçinde bu sorunu kendisini bildirim gördük karşılaşabileceğiniz en az iki senaryo vardır:

  - Bir iç yük dengeleyici (ILB) arkasında ve 660 bayt üzerinde olduğundan SQL her zaman açık SQL sorguları.
  - Birden çok ana sunucu etkinleştirmeye Kubernetes dağıtımları başarısız.  

  Aynı sanal ağda ancak farklı alt ağlarda bir ILB ile bir VM arasındaki iletişimi varsa sorun oluşur. Bu sorunu geçici olarak ASDK ana bilgisayarda yükseltilmiş komut isteminde aşağıdaki komutları çalıştırarak çalışabilirsiniz:

  ```shell
  netsh interface ipv4 set sub "hostnic" mtu=1660
  netsh interface ipv4 set sub "management" mtu=1660
  ```

## <a name="build-11901095"></a>Derleme 1.1901.0.95

Bkz: [önemli yapı bilgilerini Azure Stack sürüm notlarında](../azure-stack-update-1901.md#build-reference).

### <a name="changes"></a>Değişiklikler

Bu derleme Azure Stack için aşağıdaki geliştirmeleri içerir:

- BGP ve NAT bileşenleri artık fiziksel ana bilgisayarda dağıtılır. Bu ASDK dağıtmak için iki ortak ya da Kurumsal IP adreslerine sahip gereğini ortadan kaldırır ve ayrıca dağıtım basitleştirir.
- Azure Stack tümleşik sistemleri artık yedeklemeleri [doğrulanmış](asdk-validate-backup.md) kullanarak **asdk installer.ps1** PowerShell Betiği.

### <a name="new-features"></a>Yeni Özellikler

- Bu sürümdeki yeni özellikler listesi için bkz. [Bu bölümde](../azure-stack-update-1901.md#new-features) Azure yığını sürüm notları.

### <a name="fixed-and-known-issues"></a>Sabit ve bilinen sorunlar

- Bu sürümde giderilen sorunların bir listesi için bkz. [Bu bölümde](../azure-stack-update-1901.md#fixed-issues) Azure yığını sürüm notları. Bilinen sorunların bir listesi için bkz. [Bu bölümde](../azure-stack-update-1901.md#known-issues-post-installation).
- Unutmayın [kullanılabilir Azure Stack düzeltmelerin](../azure-stack-update-1901.md#azure-stack-hotfixes) Azure Stack ASDK için geçerli değildir.

## <a name="build-118110101"></a>Derleme 1.1811.0.101

### <a name="changes"></a>Değişiklikler

Bu derleme Azure Stack için aşağıdaki geliştirmeleri içerir:  

- Bir dizi yeni en düşük ve önerilen donanım ve yazılım gereksinimleri ASDK yoktur. Özellikleri önerilen yeni bu bölümünde belgelendirilen [Azure Stack dağıtım planlama konuları](asdk-deploy-considerations.md). Azure Stack platform geliştirildiğini gibi diğer hizmetler kullanıma sunulmuştur ve daha fazla kaynak gerekli olabilir. Bu yeniden düzenlenen öneriler artan özellikleri yansıtır.

### <a name="new-features"></a>Yeni Özellikler

Bu sürümdeki yeni özellikler listesi için bkz. [Bu bölümde](../azure-stack-update-1811.md#new-features) Azure yığını sürüm notları.

### <a name="fixed-and-known-issues"></a>Sabit ve bilinen sorunlar

Bu sürümde giderilen sorunların bir listesi için bkz. [Bu bölümde](../azure-stack-update-1811.md#fixed-issues) Azure yığını sürüm notları. Bilinen sorunların bir listesi için bkz. [Bu bölümde](../azure-stack-update-1811.md#known-issues-post-installation).
