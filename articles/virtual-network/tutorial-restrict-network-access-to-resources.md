---
title: PaaS kaynaklarına - Azure portalında ağ erişimi kısıtlama | Microsoft Docs
description: Sınırlayabilir ve Azure Storage ve Azure SQL veritabanı gibi Azure kaynakları için Azure Portalı'nı kullanarak sanal ağ hizmet uç noktaları ile ağ erişimini kısıtlayabilir öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 9a64a5c1f63dc05cba6fdfa310b694e34bdba7d1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sanal ağ hizmet uç noktaları ile PaaS kaynaklarına ağ erişimi kısıtla

Sanal ağ hizmet uç noktaları bazı Azure hizmeti kaynaklar sanal ağ alt ağına ağ erişimini sınırlamak etkinleştirin. Internet kaynaklarına erişim de kaldırabilirsiniz. Hizmet uç noktaları desteklenen Azure Hizmetleri Azure hizmetlerine erişmek için sanal ağınızın özel adres alanı kullanmanıza olanak sağlayan, sanal ağ üzerinden doğrudan bağlantı sağlar. Hizmet uç noktaları üzerinden Azure kaynaklarına her zaman hedefleyen trafiğe, Microsoft Azure omurga ağı üzerinde kalır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir alt ağ ile bir sanal ağ oluşturma
> * Bir alt ağ ekleyin ve bir hizmet uç noktası etkinleştirin
> * Bir Azure kaynağı oluşturun ve ağ erişimi için yalnızca bir alt ağdan izin verin
> * Her alt ağda bir sanal makine (VM) dağıtma
> * Bir alt ağdan bir kaynağa erişimi onaylayın
> * Bir alt ağ ve Internet bağlantısını bir kaynağa erişimi reddedildi onaylayın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

Oturum açtığınızda Azure portalında http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Girin ya da, aşağıdaki bilgileri seçin ve ardından **oluşturma**:

    |Ayar|Değer|
    |----|----|
    |Ad| myVirtualNetwork |
    |Adres alanı| 10.0.0.0/16|
    |Abonelik| Aboneliğinizi seçin|
    |Kaynak grubu | Seçin **Yeni Oluştur** ve girin *myResourceGroup*.|
    |Konum| Seçin **Doğu ABD** |
    |Alt Ağ Adı| Genel|
    |Alt ağ adres aralığı| 10.0.0.0/24|
    |Hizmet uç noktaları| Devre dışı|

    ![Sanal ağınız hakkında temel bilgileri girin](./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png)


## <a name="enable-a-service-endpoint"></a>Hizmet uç noktası etkinleştirme

1. İçinde **arama kaynakları, hizmetleri ve belgeleri** kutusuna portalın en üstünde *myVirtualNetwork.* Zaman **myVirtualNetwork** arama sonuçlarında görünür.
2. Bir alt ağ sanal ağa ekleyin. Altında **ayarları**seçin **alt ağlar**ve ardından **+ alt**, aşağıdaki resimde gösterildiği gibi:

    ![Alt ağ ekleme](./media/tutorial-restrict-network-access-to-resources/add-subnet.png) 

3. Altında **alt ağ Ekle**seçin veya aşağıdaki bilgileri girin ve ardından **Tamam**:

    |Ayar|Değer|
    |----|----|
    |Ad| Özel |
    |Adres aralığı| 10.0.1.0/24|
    |Hizmet uç noktaları| Seçin **Microsoft.Storage** altında **Hizmetleri**|

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Bir alt ağa gelen ve giden ağ erişimi kısıtlama

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **ağ güvenlik grubu**.
Altında **bir ağ güvenlik grubu oluşturun**, girin ya da, aşağıdaki bilgileri seçin ve ardından **oluşturma**:

    |Ayar|Değer|
    |----|----|
    |Ad| myNsgPrivate |
    |Abonelik| Aboneliğinizi seçin|
    |Kaynak grubu | Seçin **var olanı kullan** seçip *myResourceGroup*.|
    |Konum| Seçin **Doğu ABD** |

4. Ağ güvenlik grubu oluşturulduktan sonra girin *myNsgPrivate*, **arama kaynakları, hizmetleri ve belgeleri** portalı üstündeki kutusu. Zaman **myNsgPrivate** arama sonuçlarında görünür.
5. Altında **ayarları**seçin **giden güvenlik kurallarında**.
6. Seçin **+ Ekle**.
7. Azure depolama hizmeti için atanan ortak IP adreslerine giden erişim sağlayan bir kural oluşturun. Girin ya da, aşağıdaki bilgileri seçin ve ardından **Tamam**:

    |Ayar|Değer|
    |----|----|
    |Kaynak| Seçin **VirtualNetwork** |
    |Kaynak bağlantı noktası aralıkları| * |
    |Hedef | Seçin **hizmet etiketi**|
    |Hedef hizmet etiketi | Seçin **depolama**|
    |Hedef bağlantı noktası aralıkları| * |
    |Protokol|Herhangi biri|
    |Eylem|İzin Ver|
    |Öncelik|100|
    |Ad|İzin ver-Storage-tüm|
8. Tüm genel IP adreslerine giden erişime izin veren varsayılan güvenlik kuralı geçersiz kılan bir kural oluşturun. 6. ve 7. adımları aşağıdaki değerleri kullanarak yeniden tamamlayın:

    |Ayar|Değer|
    |----|----|
    |Kaynak| Seçin **VirtualNetwork** |
    |Kaynak bağlantı noktası aralıkları| * |
    |Hedef | Seçin **hizmet etiketi**|
    |Hedef hizmet etiketi| Seçin **Internet**|
    |Hedef bağlantı noktası aralıkları| * |
    |Protokol|Herhangi biri|
    |Eylem|Reddet|
    |Öncelik|110|
    |Ad|Deny-Internet-All|

9. Altında **ayarları**seçin **gelen güvenlik kuralları**.
10. Seçin **+ Ekle**.
11. Uzak Masaüstü Protokolü (RDP) trafiğe izin veren bir kural oluşturmak için alt ağ yerden gelen. Kural internet'ten gelen tüm trafiği engelleyen bir varsayılan güvenlik kuralı geçersiz kılar. Bağlantı bir sonraki adımda test edilebilir böylece Uzak Masaüstü bağlantıları alt ağına izin verilir. 6. ve 7. adımları aşağıdaki değerleri kullanarak yeniden tamamlayın:

    |Ayar|Değer|
    |----|----|
    |Kaynak| Herhangi biri |
    |Kaynak bağlantı noktası aralıkları| * |
    |Hedef | Seçin **hizmet etiketi**|
    |Hedef hizmet etiketi| Seçin **VirtualNetwork**|
    |Hedef bağlantı noktası aralıkları| 3389 |
    |Protokol|Herhangi biri|
    |Eylem|İzin Ver|
    |Öncelik|120|
    |Ad|İzin ver-RDP-tüm|

12. Altında **ayarları**seçin **alt ağlar**.
13. Seçin **+ ilişkilendir**
14. Altında **ilişkilendirmek alt**seçin **sanal ağ** ve ardından **myVirtualNetwork** altında **sanal ağ seçin**.
15. Altında **alt ağ seçin**seçin **özel**ve ardından **Tamam**.

## <a name="restrict-network-access-to-a-resource"></a>Bir kaynağa ağ erişimi kısıtlama

Hizmet uç noktaları için etkin Azure Hizmetleri aracılığıyla oluşturulan kaynaklarına ağ erişimi kısıtlamak için gerekli adımları hizmetleri arasında değişir. Her hizmet için belirli adımlar için tek tek Hizmetleri belgelerine bakın. Bu makalenin sonraki bölümlerinde, örnek bir Azure Storage hesabı ağ erişimini kısıtlamak için adımları içerir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **depolama**ve ardından **depolama hesabı - blob, dosya, tablo, kuyruk**.
3. Girin veya seçin, aşağıdaki bilgileri, kalan Varsayılanları kabul edin ve ardından **oluşturma**:

    |Ayar|Değer|
    |----|----|
    |Ad| 3-24 karakter uzunluğunda, yalnızca sayılar ve küçük harfler kullanarak tüm Azure konumları arasında benzersiz bir ad girin.|
    |Hesap türü|StorageV2 (genel amaçlı v2)|
    |Çoğaltma| Yerel olarak yedekli depolama (LRS)|
    |Abonelik| Aboneliğinizi seçin|
    |Kaynak grubu | Seçin **var olanı kullan** seçip *myResourceGroup*.|
    |Konum| Seçin **Doğu ABD** |

### <a name="create-a-file-share-in-the-storage-account"></a>Depolama hesabında bir dosya paylaşımı oluşturma

1. Depolama hesabı oluşturulduktan sonra depolama hesabı adı girin **arama kaynakları, hizmetleri ve belgeleri** portalı üstündeki kutusu. Depolama hesabınızın adını arama sonuçlarında görüntülendiğinde, onu seçin.
2. Seçin **dosyaları**, aşağıdaki resimde gösterildiği gibi:

    ![Depolama hesabı](./media/tutorial-restrict-network-access-to-resources/storage-account.png) 
3. Seçin **+ dosya paylaşımı**altında **dosya hizmeti**.
4. Girin *dosya paylaşımı my* altında **adı**ve ardından **Tamam**.
5. Kapat **dosya hizmeti** kutusu.

### <a name="enable-network-access-from-a-subnet"></a>Bir alt ağdaki ağ erişimini etkinleştir

Varsayılan olarak, depolama hesapları herhangi bir ağ istemcilerinden gelen ağ bağlantılarını kabul eder. Yalnızca belirli bir alt izni ve diğer tüm ağlardan ağ erişimini engellemek için aşağıdaki adımları tamamlayın:

1. Altında **ayarları** için depolama hesabı seçmeniz **güvenlik duvarları ve sanal ağlar**.
2. Altında **sanal ağlar**seçin **seçili ağları**.
3. Seçin **var olan sanal ağ ekleme**.
4. Altında **eklemek ağları**, aşağıdaki değerleri seçin ve ardından **Ekle**:

    |Ayar|Değer|
    |----|----|
    |Abonelik| Aboneliğinizi seçin.|
    |Sanal ağlar|Seçin **myVirtualNetwork**altında **sanal ağlar**|
    |Alt ağlar| Seçin **özel**altında **alt ağlar**|

    ![Güvenlik duvarları ve sanal ağlar](./media/tutorial-restrict-network-access-to-resources/storage-firewalls-and-virtual-networks.png) 

5. **Kaydet**’i seçin.
6. Kapat **güvenlik duvarları ve sanal ağlar** kutusu.
7. Altında **ayarları** için depolama hesabı seçmeniz **erişim anahtarlarını**, aşağıdaki resimde gösterildiği gibi:

      ![Güvenlik duvarları ve sanal ağlar](./media/tutorial-restrict-network-access-to-resources/storage-access-key.png)

8. Not **anahtar** değeri olarak el ile bir sonraki adımda dosya paylaşımı için bir sürücü harfi bir VM'de eşlerken girmeniz gerekecek.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Ağ erişimi bir depolama hesabına sınamak için her alt ağ için bir VM dağıtın.

### <a name="create-the-first-virtual-machine"></a>İlk sanal makine oluşturma

1. Seçin **+ kaynak oluşturma** üst, sol üst köşesinde Azure portalı üzerinde bulunamadı.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.
3. Girin ya da, aşağıdaki bilgileri seçin ve ardından **Tamam**:

    |Ayar|Değer|
    |----|----|
    |Ad| myVmPublic|
    |Kullanıcı adı|Seçtiğiniz kullanıcı adı girin.|
    |Parola| Seçtiğiniz bir parola girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| Seçin **var olanı kullan** seçip **myResourceGroup**.|
    |Konum| Seçin **Doğu ABD**.|

    ![Bir sanal makine hakkında temel bilgileri girin](./media/tutorial-restrict-network-access-to-resources/virtual-machine-basics.png)
4. Sanal makine için bir boyut seçin ve ardından **seçin**.
5. Altında **ayarları**seçin **ağ** ve ardından **myVirtualNetwork**. Ardından **alt**seçip **ortak**, aşağıdaki resimde gösterildiği gibi:

    ![Sanal ağ seçin](./media/tutorial-restrict-network-access-to-resources/virtual-machine-settings.png)
6. Üzerinde **Özet** sayfasında, **oluşturma** sanal makine dağıtımı başlatmak için. VM dağıtmak için birkaç dakika sürer ancak VM oluşturma sırasında bir sonraki adıma devam edebilirsiniz.

### <a name="create-the-second-virtual-machine"></a>İkinci sanal makine oluşturma

Tüm adımları 1-6 yeniden, ancak adım 3, sanal makine adı *myVmPrivate* ve 5. adımda seçin **özel** alt ağ.

VM dağıtmak için birkaç dakika sürer. Sonraki adıma oluşturmayı tamamladığında ve portalda ayarlarını açın kadar devam etmeyin.

## <a name="confirm-access-to-storage-account"></a>Depolama hesabı erişim Onayla

1. Bir kez *myVmPrivate* VM bittikten oluşturma, Azure ayarlar açar. Seçerek VM Bağlan **Bağlan** düğmesi, aşağıdaki resimde gösterildiği gibi:

    ![Bir sanal makineye bağlanma](./media/tutorial-restrict-network-access-to-resources/connect-to-virtual-machine.png)

2. Seçtikten sonra **Bağlan** düğmesi, bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilmeden.  
3. İndirilen rdp dosyasını açın. İstenirse, seçin **Bağlan**. Kullanıcı adı ve VM oluştururken belirttiğiniz parolayı girin. Seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, seçin **Evet** veya **devam**, bağlantı ile devam etmek için.
6. Üzerinde *myVmPrivate* VM, Azure dosya paylaşımı PowerShell kullanarak Z sürücü eşleme. İzleyin komutları çalıştırmadan önce Değiştir `<storage-account-key>` ve `<storage-account-name>` sağlanan ve içinde alınan değerlerle [depolama hesabı oluşturma](#create-a-storage-account).

    ```powershell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
    ```
    
    PowerShell çıkışı aşağıdaki örnek çıktıya benzer döndürür:

    ```powershell
    Name           Used (GB)     Free (GB) Provider      Root
    ----           ---------     --------- --------      ----
    Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
    ```

    Z sürücüsüne başarıyla eşlenen Azure dosya paylaşımı.

7. VM yok giden tüm diğer ortak IP adresleri için bir komut isteminden bağlanabildiğini doğrulayın:

    ```
    ping bing.com
    ```
    
    Ağ güvenlik grubu ile ilişkili olduğundan hiç yanıt alırsınız *özel* alt ağ Azure Storage hizmetine atanan adresler dışında genel IP adreslerine giden erişim izin vermez.

8. Uzak Masaüstü oturumu kapatmak *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Depolama hesabı için erişim reddedildi onaylayın

1. Girin *myVmPublic* içinde **arama kaynakları, hizmetleri ve belgeleri** portalı üstündeki kutusu.
2. Zaman **myVmPublic** arama sonuçlarında görünür.
3. Adım 1-6'tamamlamak [depolama hesabına erişim izni onaylamak](#confirm-access-to-storage-account) için *myVmPublic* VM.

    Erişim reddedildi ve aldığınız bir `New-PSDrive : Access is denied` hata. Nedeniyle erişim reddedildi *myVmPublic* VM dağıtıldığı *ortak* alt ağ. *Ortak* alt Azure Storage için etkin bir hizmet uç noktası yok ve depolama hesabı yalnızca gelen ağ erişimi sağlayan *özel* alt değil *ortak*alt ağ.

4. Uzak Masaüstü oturumu kapatmak *myVmPublic* VM.

5. Azure, bilgisayarınızdan Gözat [portal](https://portal.azure.com).
6. Oluşturduğunuz depolama hesabının adını girin **arama kaynakları, hizmetleri ve belgeleri** kutusu. Depolama hesabınızın adını arama sonuçlarında görüntülendiğinde, onu seçin.
7. **Dosyalar**’ı seçin.
8. Aşağıdaki resimde gösterilen hatasını alıyorsunuz:

    ![Erişim reddedildi hatası](./media/tutorial-restrict-network-access-to-resources/access-denied-error.png)

    Erişim engellendi, bilgisayarınızın içinde olmadığından *özel* alt *MyVirtualNetwork* sanal ağ.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği tüm kaynaklar Sil:

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağ alt ağı için bir hizmet uç noktası etkin. Hizmet uç noktaları birden çok Azure services ile dağıtılan kaynaklar için etkinleştirilebilir öğrendiniz. Bir Azure Storage hesabını ve sınırlı ağ erişimi yalnızca bir sanal ağ alt ağ içindeki kaynaklar için depolama hesabı oluşturuldu. Sanal ağlar üretimde hizmet uç noktaları oluşturmadan önce baştan sona ile öğrenmeniz olduğunu önerilir [hizmet uç noktaları](virtual-network-service-endpoints-overview.md).

Hesabınızı birden çok sanal ağlarınız varsa, her sanal ağ içindeki kaynaklara birbirleri ile iletişim kurabilmesi iki sanal ağları birbirine bağlamak isteyebilirsiniz. Sanal ağlara bağlanma hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Sanal ağlara bağlanabilir](./tutorial-connect-virtual-networks-portal.md)
