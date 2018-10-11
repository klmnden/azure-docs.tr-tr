---
title: Öğretici - Traffic Manager ile etki alanı tepe adlarını desteklemek için Azure DNS diğer ad kaydı oluşturma
description: Bu öğreticide Traffic Manager ile etki alanı tepe adının kullanılmasını desteklemek için Azure DNS diğer ad kaydı yapılandırma adımları gösterilmektedir.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 4a5d8968861f6f2938e605d2f7106529ca401df4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967447"
---
# <a name="tutorial-configure-an-alias-record-to-support-apex-domain-names-with-traffic-manager"></a>Öğretici: Traffic Manager ile tepe etki alanı adlarını desteklemek için diğer ad kaydı yapılandırma 

Bir Traffic Manager profiline başvurmak için etki alanı tepe adı (contoso.com gibi) için diğer ad kaydı oluşturabilirsiniz. Bu sayede bu işlem için bir yönlendirme hizmeti kullanmak yerine Azure DNS yapılandırması ile Traffic Manager profiline doğrudan bölgenizden başvurabilirsiniz. 


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ana bilgisayar VM'si ve ağ altyapısı oluşturma
> * Traffic Manager profili oluşturma
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
2. Portalın sol üst köşesinde **Kaynak oluştur**’a tıklayın, arama kutusuna *kaynak grubu* yazın ve **RG-DNS-Alias-TM** adlı bir kaynak grubu oluşturun.
3. **Kaynak grubu oluştur**’a, **Ağ**’a ve sonra **Sanal ağ**’a tıklayın.
4. **VNet-Servers** adlı bir sanal ağ oluşturun, **RG-DNS-Alias-TM** kaynak grubunun içine yerleştirin ve alt ağı **SN-Web** olarak adlandırın.

## <a name="create-two-web-server-virtual-machines"></a>İki web sunucusu sanal makinesi oluşturma
1. **Kaynak grubu oluştur** ve **Windows Server 2016 VM**’ye tıklayın.
2. Ad için **Web-01** yazın ve VM’yi **RG-DNS-Alias-TM** kaynak grubuna yerleştirin. Bir kullanıcı adı ve parola girip **Tamam**’a tıklayın.
3. **Boyut** için 8 GB RAM’e sahip bir SKU seçin.
4. **Ayarlar** için **VNet-Servers** sanal ağını ve **SN-Web** alt ağını seçin.
5. **Genel IP adresi**'ne tıklayın ve **Atama** bölümünde **Statik** seçeneğine ve ardından **Tamam**'a tıklayın.
6. Genel gelen bağlantı noktaları için **HTTP**, **HTTPS** ve **RDP (3389)** öğelerini seçip **Tamam**’a tıklayın.
7. Özet sayfasında **Oluştur**’a tıklayın.

   Bu işlemin tamamlanması birkaç dakika sürer.
6. Bu yordamı tekrarlayarak **Web-02** adlı başka bir sanal makine oluşturun.

### <a name="add-a-dns-label"></a>DNS etiketi ekleme
Genel IP adreslerinin Traffic Manager ile çalışabilmesi için bir DNS etiketi kullanılması gerekir.
1. **RG-DNS-Alias-TM** kaynak grubunda **Web-01-ip** genel IP adresine tıklayın.
2. **Ayarlar** altında **Yapılandırma**'ya tıklayın.
3. DNS adı metin kutusuna **web01pip** yazın.
4. **Kaydet**’e tıklayın.

Bu yordamı **Web-02-ip** genel IP adresi için tekrarlayın ve DNS ad etiketi için **web02pip** değerini kullanın.

### <a name="install-iis"></a>IIS yükleme

**Web-01** ve **Web-02** üzerine IIS yükleyin.

1. **Web-01**’e bağlanıp oturum açın.
2. Sunucu Yöneticisi Panosunda **Rol ve özellik ekle**’ye tıklayın.
3. **İleri**’ye üç kez tıklayın ve **Sunucu Rolleri** sayfasında **Web Sunucusu (IIS)** öğesini seçin.
4. **Özellik Ekle** ve **İleri**’ye tıklayın.
5. **İleri**'ye dört kez tıklayın ve **Yükle**'ye tıklayın.

   İşlemin tamamlanması birkaç dakika sürebilir.
6. Yükleme tamamlandığında **Kapat**'a tıklayın.
7. Bir web tarayıcısı açın ve varsayılan IIS web sayfasının göründüğünü doğrulamak için **localhost** sayfasına göz atın.

Bu işlemleri tekrarlayarak **Web-02** üzerine de IIS yükleyin.


## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Bitiş tarihi 

1. **RG-DNS-Alias-TM** kaynak grubunu açın ve **Web-01-ip** Genel IP adresine tıklayın. Daha sonra kullanmak için IP adresini not edin. Bu işlemi **Web-02-ip** Genel IP adresi için tekrarlayın.
1. **Kaynak grubu oluştur**’a, **Ağ**’a ve sonra **Traffic Manager profili**’ne tıklayın.
2. Ad alanına **TM-alias-test** yazın ve **RG-DNS-Alias-TM** kaynak grubuna yerleştirin.
3. **Oluştur**’a tıklayın.
4. Dağıtım tamamlandıktan sonra **Kaynağa git**'e tıklayın.
5. Traffic Manager profili sayfasının **Ayarlar** bölümünde **Uç noktalar**'a tıklayın.
6. **Ekle**'ye tıklayın.
7. **Tür** olarak **Dış uç nokta**'yı seçin ve **Ad** alanına **EP-Web01** yazın.
8. **Tam etki alanı adı (FQDN) veya IP** metin kutusuna önceden not ettiğiniz **Web-01-ip** IP adresini yazın.
9. Diğer kaynaklarınızla aynı **Konum** değerini seçin ve **Tamam**'a tıklayın.

Bu yordamı tekrarlayarak **Web-02** uç noktasını ekleyin ve önceden not ettiğiniz **Web-02-ip** IP adresini kullanın.

## <a name="create-an-alias-record"></a>Diğer ad kaydı oluşturma

Traffic Manager profiline işaret eden bir diğer ad kaydı oluşturun.

1. Azure DNS bölgenizi açmak için bölgeye tıklayın.
2. **Kayıt kümesi**’ne tıklayın.
3. Etki alanı tepe adını (örneğin, contoso.com) kullanmak için **Ad** metin kutusunu boş bırakın.
4. **Tür** alanını bir **A** kaydı olarak bırakın.
5. **Diğer Ad Kayıt Kümesi** onay kutusuna tıklayın.
6. **Azure hizmeti seçin**'e tıklayıp **TM-alias-test** Traffic Manager profilini seçin.

## <a name="test-the-alias-record"></a>Diğer ad kaydını test etme

1. Web tarayıcısından etki alanı tepe adına gidin (örneğin, contoso.com). Varsayılan IIS web sayfasını görmeniz gerekir. Web tarayıcısını kapatın.
2. **Web-01** sanal makinesini kapatın ve tamamen kapandığından emin olmak için birkaç dakika bekleyin.
3. Yeni bir web tarayıcısı açın ve tekrar etki alanınızın tepe adına gidin.
4. Traffic Manager durumla ilgilenip trafiği **Web-02**'ye yönlendirdiğinden yine varsayılan IIS sayfasını görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde **RG-DNS-Alias-TM** kaynak grubunu silerek bu öğreticide oluşturulan tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Traffic Manager profiline başvurmak üzere etki alanınızın tepe adını kullanmak için bir diğer ad kaydı oluşturdunuz. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
