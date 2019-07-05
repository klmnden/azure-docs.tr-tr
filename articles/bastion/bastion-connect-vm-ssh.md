---
title: Azure savunma kullanarak bir Linux VM'ye bağlanma | Microsoft Docs
description: Bu makalede, Azure savunma kullanarak Linux sanal makinesine bağlanmayı öğreneceksiniz.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 69548541d16db95f633400808f72aebaf59cff08
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477772"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion-preview"></a>Azure savunma (Önizleme) kullanarak bir Linux sanal makinesinde SSH kullanarak bağlanma

Bu makalede gösterilmektedir için güvenli ve sorunsuz bir şekilde bir Azure sanal ağına Linux VM'ler için SSH. Azure portalından doğrudan bir sanal makineye bağlanabilirsiniz. Azure Bastion’ı kullanırken sanal makineler için bir istemci, aracı veya ek yazılım gerekmez. Azure savunma hakkında daha fazla bilgi için bkz: [genel bakış](bastion-overview.md).

SSH kullanarak bir Linux sanal makineye bağlanmak için Azure savunma kullanabilirsiniz. Kimlik doğrulaması için hem kullanıcı adı/parola ve SSH anahtarlarını kullanabilirsiniz. Kullanarak, sanal Makinenize SSH anahtarlarıyla bağlanabilirsiniz:

* El ile girmeniz bir özel anahtar
* Özel anahtar bilgisi içeren bir dosya

SSH özel anahtarı ile başlayan bir biçimde olmalıdır `"-----BEGIN RSA PRIVATE KEY-----"` ve ile sona eren `"-----END RSA PRIVATE KEY-----"`.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Sanal Makinenin bulunduğu sanal ağ için bir Azure Burcu ana bilgisayarı'kurmak ayarladığınızdan emin olun. Daha fazla bilgi için [bir Azure Burcu ana bilgisayarı oluşturma](bastion-create-host-portal.md). Savunma hizmet sağlanması ve sanal ağınızda dağıtılan sonra bu sanal ağdaki herhangi bir VM bağlanmak için kullanabilirsiniz. Bağlanmak için savunma kullandığınızda, bu önizlemede, bir Windows VM ve SSH, Linux Vm'lere bağlanmak için bağlanmak için RDP kullandığınızı varsayar.

Bir bağlantı kurmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucusu rolü
* NIC sanal makinenin özel IP'si ile okuyucusu rolü
* Okuyucu rolü Azure savunma kaynağı

## <a name="username"></a>Bağlan: Kullanıcı adı ve parola kullanarak


1.  Kullanım [bu bağlantıyı](https://aka.ms/BastionHost) için savunma Azure Önizleme portalı sayfasını açın. Bağlanmak istediğiniz sanal makineye gidin, ardından tıklatın **Connect**. VM, bir SSH bağlantısı kullanılırken bir Linux sanal makinesi olmalıdır.
1. Bağlan'a tıklayın, sonra üç sekme – RDP ve SSH güvenlikli olan bir kenar çubuğu görünür. Sanal ağ için savunma sağlanması halinde, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız değilse [yapılandırma savunma](bastion-create-host-portal.md). Görmüyorsanız, **savunma** listelenen Önizleme portalı açtığınız değil. Portalı kullanarak açın [bu bağlantıyı](https://aka.ms/BastionHost).

      ![VM'ye bağlanın](./media/bastion-connect-vm-ssh/bastion.png)

1. Kullanıcı adı ve parola, sanal makinenize SSH için girin.
1. Tıklayın **Connect** anahtar girdikten sonra düğme.

## <a name="privatekey"></a>Bağlan: El ile bir özel anahtarı girin

1.  Kullanım [bu bağlantıyı](https://aka.ms/BastionHost) için savunma Azure Önizleme portalı sayfasını açın. Bağlanmak istediğiniz sanal makineye gidin, ardından tıklatın **Connect**. VM, bir SSH bağlantısı kullanılırken bir Linux sanal makinesi olmalıdır.
1. Bağlan'a tıklayın, sonra üç sekme – RDP ve SSH güvenlikli olan bir kenar çubuğu görünür. Sanal ağ için savunma sağlanması halinde, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız değilse [yapılandırma savunma](bastion-create-host-portal.md). Görmüyorsanız, **savunma** listelenen Önizleme portalı açtığınız değil. Portalı kullanarak açın [bu bağlantıyı](https://aka.ms/BastionHost).

      ![VM'ye bağlanın](./media/bastion-connect-vm-ssh/bastion.png)

1. Kullanıcı adı girin ve seçin **SSH özel anahtarı**.
1. Metin alanına özel anahtarınızı girin **SSH özel anahtarı** (veya doğrudan yapıştırın).
1. Tıklayın **Connect** anahtar girdikten sonra düğme.

## <a name="ssh"></a>Bağlan: Bir özel anahtar dosyası kullanma

1.  Kullanım [bu bağlantıyı](https://aka.ms/BastionHost) için savunma Azure Önizleme portalı sayfasını açın. Bağlanmak istediğiniz sanal makineye gidin, ardından tıklatın **Connect**. VM, bir SSH bağlantısı kullanılırken bir Linux sanal makinesi olmalıdır.

    ![VM'ye bağlanın](./media/bastion-connect-vm-ssh/connect.png)

1. Bağlan'a tıklayın, sonra üç sekme – RDP ve SSH güvenlikli olan bir kenar çubuğu görünür. Sanal ağ için savunma sağlanması halinde, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız değilse [yapılandırma savunma](bastion-create-host-portal.md). Görmüyorsanız, **savunma** listelenen Önizleme portalı açtığınız değil. Portalı kullanarak açın [bu bağlantıyı](https://aka.ms/BastionHost).

    ![VM'ye bağlanın](./media/bastion-connect-vm-ssh/bastion.png)

1. Kullanıcı adı girin ve seçin **SSH özel anahtarı yerel dosyadan**.
1. Tıklayın **Gözat** düğmesine (klasör simgesi yerel dosyada).
1. Dosya için Gözat'a tıklayın **açık**.
1. Tıklayın **Connect** VM'ye bağlanma. Doğrudan bağlan, bu sanal makineye SSH tıkladığınızda Azure Portalı'nda açılır. Bu bağlantı HTML5 olduğunda savunma hizmet bağlantı noktası 443 üzerinden sanal makinenizin özel IP kullanan.

## <a name="next-steps"></a>Sonraki adımlar

Okuma [savunma SSS](bastion-faq.md)
