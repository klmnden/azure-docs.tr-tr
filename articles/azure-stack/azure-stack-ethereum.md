---
title: Azure yığın Ethereum çözüm şablonu
description: Azure yığında Konsorsiyumu Ethereum ağ yapılandırmak ve dağıtmak özel çözüm şablonlarını kullanma
services: azure-stack
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 6/28/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: 4c2b0cda2d4144cde733f7f57ac6311e1a69f547
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114738"
---
# <a name="azure-stack-ethereum-solution-templates"></a>Azure yığın Ethereum çözüm şablonları

Ethereum çözüm şablonu, kolay ve hızlı bir çok üye Konsorsiyumu Ethereum ağ ile minimum Azure ve Ethereum bilgi yapılandırmak ve dağıtmak için tasarlanmıştır.

Kullanıcı girişleri ve Azure yığın Yönetici portalı üzerinden bir tek tıklamalı dağıtım sayıda ile üyelerin kendi ağ kaplama alanını sağlayabilirsiniz. Yük dengeli işlem düğümleri kümesi her üyenin ağ kaplama alanını oluşur, bir uygulama ya da kullanıcı işlemleri, kayıt işlemleri için araştırma düğümleri kümesi ve bir ağ sanal Gereci (NVA) göndermek için etkileşim kurabilen ile. Bir sonraki bağlantı adımı tam olarak yapılandırılmış birden çok üye blockchain ağ oluşturmak üzere NVAs bağlanır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki karşıdan [marketten](azure-stack-download-azure-marketplace-item.md):

* Ubuntu Server 16.04 LTS sürüm 16.04.201802220
* Windows Server 2016 
* Linux 2.0 için özel bir komut dosyası 
* Özel Betik Uzantısı 

Azure blockchain senaryoları hakkında daha fazla bilgi için bkz: [Ethereum iş kanıt Konsorsiyumu çözüm şablonu](../blockchain-workbench/ethereum-deployment-guide.md).

Birden fazla sanal makine dağıtımı destekleyen bir Azure aboneliği gereklidir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Bu çözüm şablonu tek veya birden çok üye Ethereum Konsorsiyumu ağ dağıtabilirsiniz. Sanal ağ ağ sanal gereç ve bağlantı kaynaklarını kullanan bir zincir topolojisinde bağlanır. 

## <a name="deployment-use-cases"></a>Dağıtım kullanım örnekleri

Şablon Ethereum Konsorsiyumu öncüsü ve çeşitli şekillerde üye birleştirme için dağıtabilirsiniz, Test ettiğimiz olanları şunlardır:
- Azure AD veya AD FS ile bir çok düğümlü Azure yığında sağlama ve aynı aboneliği kullanarak üye dağıtmak veya farklı Aboneliklerde ile.
- (Azure AD ile) tek bir düğüm Azure yığında sağlama ve üye aynı aboneliği kullanarak dağıtın.

### <a name="standalone-and-consortium-leader-deployment"></a>Tek başına ve Konsorsiyumu öncü dağıtımı

Konsorsiyumu öncü şablonu ilk üyenin ayak izini ağ yapılandırır. 

1. Karşıdan [öncü şablonunu github'dan](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/ConsortiumLeader/mainTemplate.json)
2. Azure yığın Yönetim Portalı'nda seçin **yeni > şablon dağıtımı** özel bir şablonu dağıtmak için.
3. Seçin **Düzen şablonu** yeni özel şablonu düzenlemek için.
4. Sağdaki düzenleme bölmesinde kopyalayın ve daha önce indirdiğiniz JSON öncü şablon yapıştırın.
    
    ![Öncü şablonunu düzenleyin.](media/azure-stack-ethereum/edit-leader-template.png)

5. **Kaydet**’i seçin.
6. Seçin **Düzenle parametreleri** ve dağıtımınız için şablon parametreleri tamamlayın.
    
    ![Öncü şablon parametreleri Düzenle](media/azure-stack-ethereum/edit-leader-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Temel olarak dağıtılan kaynakları adlandırmak için kullanılan dize. | Alfasayısal karakter uzunluğu 1 ile 6 | Eth
    KİMLİK | Sanal makine kimliğini doğrulamak için yöntem. | Parola veya SSH ortak anahtarı | Parola
    ADMINUSERNAME | Dağıtılan her VM yönetici kullanıcı adı | 1 - 64 karakter | gethadmin
    ADMINPASSWORD (kimlik doğrulama türü = parola)| Dağıtılan sanal makinelerin her biri için yönetici hesabı için parola. Parola aşağıdaki gereksinimleri 3 içermelidir: 1 büyük harf, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm sanal makineleri aynı parolayı başlangıçta olmakla birlikte, sağlama işlemi sonrası parolasını değiştirebilirsiniz.|12 - 72 karakter|
    ADMINSSHKEY (kimlik doğrulama türü sshPublicKey =) | Uzak oturum açma için kullanılan güvenli Kabuk anahtarı. | |
    GENESISBLOCK | Özel genesis bloğu temsil eden JSON dize. | |
    ETHEREUMACCOUNTPSSWD | Ethereum hesabını güvenli hale getirmek için kullanılan yönetici parolası. | |
    ETHEREUMACCOUNTPASSPHRASE | Ethereum hesapla ilişkili özel anahtarı oluşturmak için kullanılan parola. | |
    ETHEREUMNETWORKID | Konsorsiyumu ağ kimliği. | 999,999,999 ile 5 arasındaki herhangi bir değer kullanın | 72
    CONSORTIUMMEMBERID | Her Konsorsiyumu ağ üyesiyle ilişkili kimliği.   | Bu kimlik ağ içinde benzersiz olmalıdır. | 0
    NUMMININGNODES | Araştırma düğüm sayısı. | 2-15 arasında. | 2
    MNNODEVMSIZE | Araştırma düğümlerin VM boyutu. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Depolama performansı araştırma düğümlerinin. | | Standard_LRS
    NUMTXNODES | İşlem düğümleri sayısı. | 1 ile 5 arasında. | 1
    TXNODEVMSIZE | İşlem düğümlerinin VM boyutu. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | Depolama performansı işlem düğümlerinin. | | Standard_LRS
    BASEURL | Bağlı olarak şablonlardan almak için temel URL. | Dağıtım şablonları özelleştirmek istediğiniz sürece varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. İçinde **özel dağıtım**, belirtin **abonelik**, **kaynak grubu**, ve **kaynak grubu konumu**.
    
    ![Öncü dağıtım parametreleri](media/azure-stack-ethereum/leader-deployment-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Abonelik | Abonelik Konsorsiyumu ağ dağıtılacağı | | Tüketim abonelik
    Kaynak Grubu | Kaynak grubu Konsorsiyumu ağ dağıtılacağı. | | EthereumResources
    Konum | Kaynak grubu için Azure bölgesi. | | yerel

8. **Oluştur**’u seçin.

Dağıtım, 20 dakika alabilir veya tamamlamak için daha uzun.

Dağıtım tamamlandıktan sonra dağıtım için özeti gözden geçirebilirsiniz **Microsoft. Şablon** kaynak grubu dağıtım bölümünde. Özet Konsorsiyumu üyeleri katılmak için kullanılan çıkış değerlerini içerir.

Öncü'nın dağıtımını doğrulama öncü'nın yönetici sitesi göz atın. Yönetici sitesi adresi çıktı bölümünde bulabilirsiniz **Microsoft.Template** dağıtım.  

![Öncü dağıtım özeti](media/azure-stack-ethereum/ethereum-node-status.png)

### <a name="joining-consortium-member-deployment"></a>Konsorsiyumu üye dağıtım birleştirme

1. Karşıdan [Konsorsiyumu üye şablonunu github'dan](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/JoiningMember/mainTemplate.json)
2. Azure yığın Yönetim Portalı'nda seçin **yeni > şablon dağıtımı** özel bir şablonu dağıtmak için.
3. Seçin **Düzen şablonu** yeni özel şablonu düzenlemek için.
4. Sağdaki düzenleme bölmesinde kopyalayın ve daha önce indirdiğiniz JSON öncü şablon yapıştırın.
5. **Kaydet**’i seçin.
6. Seçin **Düzenle parametreleri** ve dağıtımınız için şablon parametreleri tamamlayın.

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Temel olarak dağıtılan kaynakları adlandırmak için kullanılan dize. | Alfasayısal karakter uzunluğu 1 ile 6 | Eth
    KİMLİK | Sanal makine kimliğini doğrulamak için yöntem. | Parola veya SSH ortak anahtarı | Parola
    ADMINUSERNAME | Dağıtılan her VM yönetici kullanıcı adı | 1 - 64 karakter | gethadmin
    ADMINPASSWORD (kimlik doğrulama türü = parola)| Dağıtılan sanal makinelerin her biri için yönetici hesabı için parola. Parola aşağıdaki gereksinimleri 3 içermelidir: 1 büyük harf, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm sanal makineleri aynı parolayı başlangıçta olmakla birlikte, sağlama işlemi sonrası parolasını değiştirebilirsiniz.|12 - 72 karakter|
    ADMINSSHKEY (kimlik doğrulama türü sshPublicKey =) | Uzak oturum açma için kullanılan güvenli Kabuk anahtarı. | |
    CONSORTIUMMEMBERID | Her Konsorsiyumu ağ üyesiyle ilişkili kimliği.   | Bu kimlik ağ içinde benzersiz olmalıdır. | 0
    NUMMININGNODES | Araştırma düğüm sayısı. | 2-15 arasında. | 2
    MNNODEVMSIZE | Araştırma düğümlerin VM boyutu. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Depolama performansı araştırma düğümlerinin. | | Standard_LRS
    NUMTXNODES | İşlem düğümleri sayısı. | 1 ile 5 arasında. | 1
    TXNODEVMSIZE | İşlem düğümlerinin VM boyutu. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | Depolama performansı işlem düğümlerinin. | | Standard_LRS
    CONSORTIUMDATA | Başka bir üyenin dağıtım tarafından sağlanan ilgili Konsorsiyumu yapılandırma verilerini işaret eden URL. Bu değer öncü'nın dağıtım Çıkışta bulunabilir. | |
    REMOTEMEMBERVNETADDRESSSPACE | Öncü NVA IP adresi. Bu değer öncü'nın dağıtım Çıkışta bulunabilir. | | 
    REMOTEMEMBERNVAPUBLICIP | Öncü NVA IP adresi. Bu değer öncü'nın dağıtım Çıkışta bulunabilir. | | 
    CONNECTIONSHAREDKEY | Bir bağlantı kurarak Konsorsiyumu ağ üyeleri arasında önceden oluşturulmuş bir gizli. | |
    BASEURL | Şablon için temel URL. | Dağıtım şablonları özelleştirmek istediğiniz sürece varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. İçinde **özel dağıtım**, belirtin **abonelik**, **kaynak grubu**, ve **kaynak grubu konumu**.

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Abonelik | Abonelik Konsorsiyumu ağ dağıtılacağı | | Tüketim abonelik
    Kaynak Grubu | Kaynak grubu Konsorsiyumu ağ dağıtılacağı. | | MemberResources
    Konum | Kaynak grubu için Azure bölgesi. | | yerel

8. **Oluştur**’u seçin.

Dağıtım, 20 dakika alabilir veya tamamlamak için daha uzun.

Dağıtım tamamlandıktan sonra dağıtım için özeti gözden geçirebilirsiniz **Microsoft.Template** kaynak grubu dağıtım bölümünde. Özet Konsorsiyumu üyeleri bağlanmak için kullanılan çıkış değerlerini içerir.

Üyenin dağıtım doğrulamak için üyenin yönetici sitesi göz atın. Yönetici sitesi adresi Microsoft.Template dağıtım çıktı bölümünde bulabilirsiniz.

![Üye dağıtım özeti](media/azure-stack-ethereum/ethereum-node-status-2.png)

Aşağıdaki resimde gösterildiği gibi üyenin düğümleri durumudur **çalışmıyor**. Üye öncü arasındaki bağlantı kurulmadı olmasıdır. Üye ve öncü arasındaki bağlantıyı iki yönlü bir bağlantıdır. Üye dağıttığınızda, şablon otomatik olarak bağlantı üyeden öncü oluşturur. Üye öncü arasında bağlantı oluşturmak için sonraki adıma gidin.

### <a name="connect-member-and-leader"></a>Üye ve öncü Bağlan

Bu şablon, uzak üyesi öncü bir bağlantı oluşturur. 

1. Karşıdan [üye ve öncü şablonunu Github'dan Bağlan](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/Connection/mainTemplate.json)
2. Azure yığın Yönetim Portalı'nda seçin **yeni > şablon dağıtımı** özel bir şablonu dağıtmak için.
3. Seçin **Düzen şablonu** yeni özel şablonu düzenlemek için.
4. Sağdaki düzenleme bölmesinde kopyalayın ve daha önce indirdiğiniz JSON öncü şablon yapıştırın.
    
    ![Bağlanma Şablonu Düzenle](media/azure-stack-ethereum/edit-connect-template.png)

5. **Kaydet**’i seçin.
6. Seçin **Düzenle parametreleri** ve dağıtımınız için şablon parametreleri tamamlayın.
    
    ![Şablon parametreleri bağlanma Düzenle](media/azure-stack-ethereum/edit-connect-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    MEMBERNAMEPREFIX | Öncü'nın adı öneki. Bu değer öncü'nın dağıtım Çıkışta bulunabilir.  | Alfasayısal karakter uzunluğu 1 ile 6 | |
    MEMBERROUTETABLENAME | Öncü 's yol tablosu adı. Bu değer öncü'nın dağıtım Çıkışta bulunabilir. |  | 
    REMOTEMEMBERVNETADDRESSSPACE | Adres alanı üyesi. Bu değer üyenin dağıtım Çıkışta bulunabilir. | |
    CONNECTIONSHAREDKEY | Bir bağlantı kurarak Konsorsiyumu ağ üyeleri arasında önceden oluşturulmuş bir gizli.  | |
    REMOTEMEMBERNVAPUBLICIP | Üye NVA IP adresi. Bu değer üyenin dağıtım Çıkışta bulunabilir. | |
    MEMBERNVAPRIVATEIP | Öncü'nın özel NVA IP adresi. Bu değer öncü'nın dağıtım Çıkışta bulunabilir. | |
    KONUM | Azure yığın ortamınızı konumu. | | yerel
    BASEURL | Şablon için temel URL. | Dağıtım şablonları özelleştirmek istediğiniz sürece varsayılan değeri kullanın. | 

7. **Tamam**’ı seçin.
8. İçinde **özel dağıtım**, belirtin **abonelik**, **kaynak grubu**, ve **kaynak grubu konumu**.
    
    ![Dağıtım parametreleri Bağlan](media/azure-stack-ethereum/connect-deployment-parameters.png)

    Parametre Adı | Açıklama | İzin Verilen Değerler | Örnek değer
    ---------------|-------------|----------------|-------------
    Abonelik | Öncü 's abonelik. | | Tüketim abonelik
    Kaynak Grubu | Öncü ait kaynak grubu. | | EthereumResources
    Konum | Kaynak grubu için Azure bölgesi. | | yerel

8. **Oluştur**’u seçin.

Dağıtım tamamlandıktan sonra öncüsü ve üye iletişim başlatmak birkaç dakika sürer. Dağıtımı doğrulamak için üyenin yönetici sitesi yenileyin. Üyenin düğümleri durumunu çalıştırıyor olması gerekir. 

![Dağıtımı doğrulama](media/azure-stack-ethereum/ethererum-node-status-3.png)

## <a name="next-steps"></a>Sonraki adımlar

- Ethereum ve Azure hakkında daha fazla bilgi için bkz: [Blockchain teknolojisi ve uygulamaları | Microsoft Azure](https://azure.microsoft.com/solutions/blockchain/).
- Azure blockchain senaryoları hakkında daha fazla bilgi için bkz: [Ethereum iş kanıt Konsorsiyumu çözüm şablonu](../blockchain-workbench/ethereum-deployment-guide.md).