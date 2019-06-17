---
title: Azure ile Chef kullanma
description: Chef ve Azure altyapınıza test yapılandırmak için kullanmaya giriş
ms.service: virtual-machines-linux
keywords: Azure, chef, devops, sanal makineler, genel bakış, otomatikleştirin
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 5c4e20177c1b334a34f7ce9328dfaa7bd8c66d3d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60656583"
---
# <a name="using-chef-with-azure"></a>Azure ile Chef kullanma
[Chef](https://www.chef.io) azure'da sanal makine altyapısı koduna dönüştüren bir güçlü Otomasyon platformudur. Chef nasıl altyapı yapılandırılmış, dağıtılan ve boyutu ne olursa olsun, ağ üzerinden yönetilen otomatikleştirir.

Bu makalede, Azure altyapı yönetmek için Chef kullanmanın avantajları anlatılmaktadır.

## <a name="chef-extension-on-azure"></a>Azure üzerinde Chef uzantısı
Chef ile bir arka plan hizmeti olarak çalışan istemci ile bir sanal makine sağlama [Chef uzantısı](https://docs.microsoft.com/azure/chef/chef-extension-portal) Azure Portalı'nda. Sağlanan sonra bu sanal makineler Chef sunucusu tarafından yönetilmeye hazırdır.

## <a name="chef-cloud-shell"></a>Chef Cloud Shell
Azure Cloud shell'de doğrudan iş istasyonu kullanmak Chef! Tüm Chef yardımcı programları ve Inspec doğru Cloud Shell'den çalıştırın. Chef komutları kullanabilir:

* [Chef](https://docs.chef.io/ctl_chef.html)
* [mutfak](https://docs.chef.io/ctl_kitchen.html)
* [inspec](https://www.inspec.io/docs/reference/cli/)
* [Bıçak](https://docs.chef.io/knife.html)
* [cookstyle](https://docs.chef.io/cookstyle.html)
* [foodcritic](https://docs.chef.io/foodcritic.html)
* [Chef çalıştırma](https://www.chef.sh/docs/chef-workstation/getting-started/)

Gibi bizim komut yardımcı programları Cloud Shell'de kullanılabilen diğer Araçları'yla birleştirerek `git`, `az-cli`, ve `terraform`ve altyapı ve uyumluluk otomasyonunuzu tarayıcıdan yazma.

## <a name="automate-infrastructure-apps-and-compliance-with-one-platform"></a>Altyapı, uygulamalar ve uyumluluk tek bir platform ile otomatikleştirme
Şirketler, hız, hız ve dijital Market'te mücadele etmek için güvenliği gerektirir. Chef ve Microsoft birlikte kişiler, takımlar ve kurumlar tüm bu işlemleri gerçekleştirmenize yardımcı olur. Bir platform ile Chef Automate, artık hale getirebilir ve altyapı, uygulamalarınızın ve uyumluluk, Microsoft Emlak arasında sürekli olarak sunun.

## <a name="test-drive-chef-automate-on-azure"></a>Test sürüşü Chef Automate Azure üzerinde
Chef tarafından desteklenen [Chef otomatikleştirmek Azure Market çözüm](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/chef-software.chef-automate) oluşturmanızı, dağıtmanızı ve altyapınızı ve uygulamalarınızı işbirliğine dayalı bir şekilde yönetmenize olanak sağlar. Tek bir tıklamayla Chef Automate'e dahil edilen tüm ticari özelliklere anında erişim alır; filonuzun tamamı uçtan uca görünürlük elde edin, sürekli uyumluluk sağlayabilir ve birleşik bir iş akışıyla tüm değişiklik yönetin.

## <a name="next-steps"></a>Sonraki adımlar

* [Chef kullanarak Azure'da Windows sanal makine oluşturma](/azure/virtual-machines/windows/chef-automation)
