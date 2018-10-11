---
title: Öğretici - Bölgedeki kaynak kaydına başvurmak için bir Azure DNS diğer ad kaydı oluşturma.
description: Bu öğreticide, bölgedeki kaynak kaydına başvurmak için bir Azure DNS diğer ad kaydını yapılandırma işlemi gösterilir.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 09c1768602fede51d7ff2b23f48278a1d0b0cd2a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990850"
---
# <a name="tutorial-create-an-alias-record-to-refer-to-a-zone-resource-record"></a>Öğretici: Bölge kaynak kaydına başvurmak için diğer ad kaydı oluşturma

Diğer ad kayıtları aynı türdeki diğer kayıt kümelerine başvurabilir. Örneğin, bir DNS CNAME kayıt kümesinin aynı türdeki başka bir CNAME kayıt kümesine diğer ad olmasını sağlayabilirsiniz. Bazı kayıt kümelerinin diğer ad gibi davranmasını diğerlerinin diğer ad değilmiş gibi davranmasını istiyorsanız, bu yararlı olacaktır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bölgedeki kaynak kaydı için diğer ad oluşturma
> * Diğer ad kaydını test etme


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağı dahil olmak üzere bu etki alanı üzerinde tam denetiminiz olmalıdır.

Azure DNS’te etki alanınızı barındırma yönergeleri için bkz. [Öğretici: Azure DNS’te etki alanınızı barındırma](dns-delegate-domain-azure-dns.md).


## <a name="create-an-alias-record"></a>Diğer ad kaydı oluşturma

Bölgedeki kaynak kaydına işaret eden bir diğer ad kaydı oluşturun.

### <a name="create-the-target-resource-record"></a>Hedef kaynak kaydını oluşturma
1. Azure DNS bölgenizi açmak için bölgeye tıklayın.
2. **Kayıt kümesi**’ne tıklayın.
3. **Ad** metin kutusuna **server** yazın.
4. **Tür** olarak **A** seçin.
5. **IP ADRESİ** metin kutusuna **10.10.10.10** yazın.
6. **Tamam** düğmesine tıklayın.

### <a name="create-the-alias-record"></a>Diğer ad kaydını oluşturma
1. Azure DNS bölgenizi açmak için bölgeye tıklayın.
2. **Kayıt kümesi**’ne tıklayın.
3. **Ad** metin kutusuna **test** yazın.
4. **Tür** olarak **A** seçin.
5. **Diğer Ad Kayıt Kümesi** onay kutusunda **Evet**'e tıklayın ve **Bölge kayıt kümesi** seçeneğini belirtin.
6. **Bölge kayıt kümesi** için **server** kaydını seçin.
7. **Tamam** düğmesine tıklayın.

## <a name="test-the-alias-record"></a>Diğer ad kaydını test etme

1. Sık kullandığınız nslookup aracını başlatın. Bir seçenek de [https://network-tools.com/nslook](https://network-tools.com/nslook) adresine göz atmaktır.
2. A kayıtları için sorgu türünü ayarlayın ve **test.\<etki alanınızın adı\>** için arama yapın. Yanıt olarak **10.10.10.10** almalısınız.
3. Azure portalında **server** A kaydını **10.11.11.11** olarak değiştirin.
4. Birkaç dakika bekleyin ve ardından **test** kaydı için nslookup aracını yeniden kullanın.
5. Yanıt olarak **10.11.11.11** almalısınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmiyorsa bölgenizdeki **server** ve **test** kayıt kaynaklarını silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bölgedeki kaynak kaydına başvurmak için bir diğer ad kaydı oluşturdunuz. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
