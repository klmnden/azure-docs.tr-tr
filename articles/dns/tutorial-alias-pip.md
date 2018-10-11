---
title: Öğretici - Azure Genel IP adresine başvurmak için bir Azure DNS diğer ad kaydı oluşturma.
description: Bu öğreticide Azure Genel IP adresine başvurmak için bir Azure DNS diğer ad kaydını yapılandırma işlemi gösterilmektedir.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 842015d853790e3ac78214cca6a70becb7f9fadf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991242"
---
# <a name="tutorial-configure-an-alias-record-to-refer-to-an-azure-public-ip-address"></a>Öğretici: Azure Genel IP adresine başvurmak için diğer ad kaydı yapılandırma 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağ altyapısı oluşturma
> * Web sunucusu sanal makinesi oluşturma
> * Diğer ad kaydı oluşturma
> * Diğer ad kaydını test etme


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağı dahil olmak üzere bu etki alanı üzerinde tam denetiminiz olmalıdır.

Azure DNS’te etki alanınızı barındırma yönergeleri için bkz. [Öğretici: Azure DNS’te etki alanınızı barındırma](dns-delegate-domain-azure-dns.md).

Bu öğreticide örnek olarak contoso.com etki alanı kullanılmaktadır, ancak sizin kendi etki alanı adınızı kullanmanız gerekir.

## <a name="create-the-network-infrastructure"></a>Ağ altyapısını oluşturma
İlk olarak, web sunucularınızı içine yerleştirmek için bir sanal ağ ve alt ağ oluşturun.
1. http://portal.azure.com adresinden Azure portalında oturum açın.
2. Portalın sol üst köşesinde **Kaynak oluştur**’a tıklayın, arama kutusuna *kaynak grubu* yazın ve **RG-DNS-Alias-pip** adlı bir kaynak grubu oluşturun.
3. **Kaynak grubu oluştur**’a, **Ağ**’a ve sonra **Sanal ağ**’a tıklayın.
4. **VNet-Server** adlı bir sanal ağ oluşturun, **RG-DNS-Alias-pip** kaynak grubunun içine yerleştirin ve alt ağı **SN-Web** olarak adlandırın.

## <a name="create-a-web-server-virtual-machine"></a>Web sunucusu sanal makinesi oluşturma
1. **Kaynak grubu oluştur** ve **Windows Server 2016 VM**’ye tıklayın.
2. Ad için **Web-01** yazın ve VM’yi **RG-DNS-Alias-TM** kaynak grubuna yerleştirin. Bir kullanıcı adı ve parola girip **Tamam**’a tıklayın.
3. **Boyut** için 8 GB RAM’e sahip bir SKU seçin.
4. **Ayarlar** için **VNet-Servers** sanal ağını ve **SN-Web** alt ağını seçin. Genel gelen bağlantı noktaları için **HTTP**, **HTTPS** ve **RDP (3389)** öğelerini seçip **Tamam**’a tıklayın.
5. Özet sayfasında **Oluştur**’a tıklayın.

   Bu işlemin tamamlanması birkaç dakika sürer.

### <a name="install-iis"></a>IIS yükleme

**Web-01**’e IIS yükleyin.

1. **Web-01**’e bağlanıp oturum açın.
2. Sunucu Yöneticisi Panosunda **Rol ve özellik ekle**’ye tıklayın.
3. **İleri**’ye üç kez tıklayın ve **Sunucu Rolleri** sayfasında **Web Sunucusu (IIS)** öğesini seçin.
4. **Özellik Ekle** ve **İleri**’ye tıklayın.
5. **İleri**'ye dört kez tıklayın ve **Yükle**'ye tıklayın.

   İşlemin tamamlanması birkaç dakika sürebilir.
6. Yükleme tamamlandığında **Kapat**'a tıklayın.
7. Bir web tarayıcısı açın ve varsayılan IIS web sayfasının göründüğünü doğrulamak için **localhost** sayfasına göz atın.

## <a name="create-an-alias-record"></a>Diğer ad kaydı oluşturma

Genel IP adresine işaret eden bir diğer ad kaydı oluşturun.

1. Azure DNS bölgenizi açmak için bölgeye tıklayın.
2. **Kayıt kümesi**’ne tıklayın.
3. **Ad** metin kutusuna **web01** yazın.
4. **Tür** alanını bir **A** kaydı olarak bırakın.
5. **Diğer Ad Kayıt Kümesi** onay kutusuna tıklayın.
6. **Azure hizmeti seçin**’e tıklayın ve **Web-01-ip** genel IP adresini seçin.

## <a name="test-the-alias-record"></a>Diğer ad kaydını test etme

1. **RG-DNS-Alias-pip** kaynak grubunda **Web-01** sanal makinesine tıklayın. Genel IP adresini not edin.
1. Bir web tarayıcısından Web01-01 sanal makinesinin tam etki alanı adına göz atın. Örnek: **web01.contoso.com**. IIS varsayılan web sayfasını görmeniz gerekir.
2. Web tarayıcısını kapatın.
3. **Web-01** sanal makinesini durdurun ve sonra yeniden başlatın.
4. Yeniden başlatıldığında, sanal makinenin yeni genel IP adresini not edin.
5. Yeni bir tarayıcı açın ve tekrar Web01-01 sanal makinesinin tam etki alanı adına göz atın. Örnek: **web01.contoso.com**.
6. Standart bir A kaydı yerine genel IP adresi kaynağına işaret eden bir diğer ad kaydı kullandığınız için bu işlem başarılı olmalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde **RG-DNS-Alias-pip** kaynak grubunu silerek bu öğreticide oluşturulan tüm kaynakları silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Genel IP adresine başvurmak için bir diğer ad kaydı oluşturdunuz. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
