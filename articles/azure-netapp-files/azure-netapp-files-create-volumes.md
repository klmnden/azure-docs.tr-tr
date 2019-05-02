---
title: Azure NetApp Files için birim oluşturma | Microsoft Docs
description: Azure NetApp Files için birim oluşturma işlemi açıklanır.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 4/23/2019
ms.author: b-juche
ms.openlocfilehash: 53b2742cf92f3a3df346ba3557c718b8d7a11a4e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719428"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Azure NetApp Files için birim oluşturma

En fazla 500 birimlerin her kapasitesi havuzu olabilir. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. Azure NetApp dosyaları, NFS ve SMBv3 birimleri destekler. 

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasitesi havuzu oluşturmak](azure-netapp-files-set-up-capacity-pool.md)   
Bir alt ağ, Azure için NetApp dosyaları temsilci gerekir.  
[Temsilci bir alt ağ Azure NetApp dosyaları](azure-netapp-files-delegate-subnet.md)

## <a name="create-an-nfs-volume"></a>NFS birimi oluşturma

1.  Tıklayın **birimleri** kapasitesi havuzu dikey penceresinden dikey. 

    ![Birimlere gidin](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Oluşturma bir birim penceresi görüntülenir.

3.  Bir birim penceresi Oluştur'u tıklatın **Oluştur** ve aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Birim adı her kapasitesi havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır. Herhangi bir alfasayısal karakter kullanabilirsiniz.

    * **Kapasitesi havuzu**  
        Birimin oluşturulması için istediğiniz kapasitesi havuzu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  

        Belirttiğiniz sanal ağ, Azure için NetApp dosyaları temsilci bir alt ağ olması gerekir. Azure NetApp dosyaları hizmeti, yalnızca aynı sanal ağda veya bir sanal ağ aynı bölgedeyse Vnet eşlemesi aracılığıyla toplu olarak erişilebilir. Express Route üzerinden şirket içi ağınızdan birimi de erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağ belirtin.  
        Belirttiğiniz alt ağ, Azure için NetApp dosyaları temsilci gerekir. 
        
        Bir alt ağ temsilcisi yok, tıklayabilirsiniz **Yeni Oluştur** sayfasında bir birim oluşturun. Ardından alt ağ oluşturma sayfası alt ağ bilgileri belirtin ve seçin **Microsoft.NetApp/volumes** alt ağın Azure NetApp dosyaları için temsilci. Her bir sanal ağda yalnızca bir alt ağ, Azure için NetApp dosyaları atanabilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Tıklayın **Protokolü**, ardından **NFS** protokol türü için toplu olarak.   
    * Belirtin **dosya yolu** yeni birim dışarı aktarma yolu oluşturmak için kullanılır. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.

        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır. 

        Dosya yolu, her abonelik ve her bölge içinde benzersiz olmalıdır. 

    * İsteğe bağlı olarak, [NFS birimini için verme ilkesi yapılandırma](azure-netapp-files-configure-export-policy.md)

    ![NFS protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Tıklayın **gözden geçir + Oluştur** birim ayrıntılarını gözden geçirmek için.  Ardından **Oluştur** NFS birimini oluşturmak için.

    Oluşturduğunuz birim birimler sayfasında görüntülenir. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="create-an-smb-volume"></a>SMB birim oluşturun

Azure NetApp dosyaları SMBv3 birimleri destekler. SMB birim eklemeden önce Active Directory bağlantıları oluşturmanız gerekir. 

### <a name="create-an-active-directory-connection"></a>Bir Active Directory bağlantısı oluşturun

1. Aşağıdaki requiements karşıladığından emin olun: 

    * Kullandığınız Yönetici hesap kuruluş birimi (OU) yolun, belirttiğiniz makine hesapları oluşturma olanağına olması gerekir.
    * Gerekli bağlantı noktaları geçerli bir Windows Active Directory (AD) sunucusunda açık olmalıdır.  
        Gerekli bağlantı noktaları aşağıdaki gibidir: 

        |     Hizmet           |     Bağlantı noktası     |     Protokol     |
        |-----------------------|--------------|------------------|
        |    AD Web Hizmetleri    |    9389      |    TCP           |
        |    DNS                |    53        |    TCP           |
        |    DNS                |    53        |    UDP           |
        |    ICMPv4             |    Yok       |    Yankı Yanıtı    |
        |    Kerberos           |    464       |    TCP           |
        |    Kerberos           |    464       |    UDP           |
        |    Kerberos           |    88        |    TCP           |
        |    Kerberos           |    88        |    UDP           |
        |    LDAP               |    389       |    TCP           |
        |    LDAP               |    389       |    UDP           |
        |    LDAP               |    3268      |    TCP           |
        |    NetBIOS adı       |    138       |    UDP           |
        |    SAM/LSA            |    445       |    TCP           |
        |    SAM/LSA            |    445       |    UDP           |
        |    Güvenli LDAP        |    636       |    TCP           |
        |    Güvenli LDAP        |    3269      |    TCP           |
        |    W32time            |    123       |    UDP           |


1. NetApp hesabınızdan tıklayın **Active Directory bağlantıları**, ardından **katılın**.  

    ![Active Directory bağlantıları](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. Active Directory katılın penceresinde aşağıdaki bilgileri sağlayın:

    * **Birincil DNS**   
        Tercih edilen Active Directory Domain Services kullanımı için Azure NetApp dosyaları ile etki alanı denetleyicisi IP adresini budur. 
    * **İkincil DNS**  
        İkincil Active Directory Domain Services kullanımı için Azure NetApp dosyaları ile etki alanı denetleyicisi IP adresini budur. 
    * **Etki alanı**  
        Bu, Active Directory etki alanına katılmak için istediğiniz hizmetleri etki alanı adıdır.
    * **SMB sunucusu (bilgisayar hesabı) öneki**  
        Bu makine hesabının Active Directory'de Azure NetApp dosyaları için yeni hesaplar oluşturulmasını kullanacağı adlandırma önekidir.

        NAS-01, NAS-02..., dosya sunucuları için kuruluşunuzun kullandığı adlandırma standardı ise, örneğin, NAS 045 ve ardından "NAS" ön eki girin. 

        Hizmet ek makine hesaplarını gerektiği gibi Active Directory'de oluşturur.

    * **Kuruluş birimi yolu**  
        SMB server makinesi hesaplarının oluşturulacağı kuruluş birimi (OU) için LDAP yolu budur. Diğer bir deyişle, OU ikinci düzey, OU = ilk düzeyi =. 
    * Kimlik bilgileri de dahil olmak üzere, **kullanıcıadı** ve **parola**

    ![Active Directory ekleyin](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. **Katıl**’a tıklayın.  

    Oluşturduğunuz Active Directory bağlantısı görüntülenir.

    ![Active Directory bağlantıları](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

### <a name="add-an-smb-volume"></a>SMB birim ekleme

1. Tıklayın **birimleri** kapasitesi havuzu dikey penceresinden dikey. 

    ![Birimlere gidin](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Oluşturma bir birim penceresi görüntülenir.

3. Bir birim penceresi Oluştur'u tıklatın **Oluştur** ve aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Birim adı her kapasitesi havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır. Herhangi bir alfasayısal karakter kullanabilirsiniz.

    * **Kapasitesi havuzu**  
        Birimin oluşturulması için istediğiniz kapasitesi havuzu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  

        Belirttiğiniz sanal ağ, Azure için NetApp dosyaları temsilci bir alt ağ olması gerekir. Azure NetApp dosyaları hizmeti, yalnızca aynı sanal ağda veya bir sanal ağ aynı bölgedeyse Vnet eşlemesi aracılığıyla toplu olarak erişilebilir. Express Route üzerinden şirket içi ağınızdan birimi de erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağ belirtin.  
        Belirttiğiniz alt ağ, Azure için NetApp dosyaları temsilci gerekir. 
        
        Bir alt ağ temsilcisi yok, tıklayabilirsiniz **Yeni Oluştur** sayfasında bir birim oluşturun. Ardından alt ağ oluşturma sayfası alt ağ bilgileri belirtin ve seçin **Microsoft.NetApp/volumes** alt ağın Azure NetApp dosyaları için temsilci. Her bir sanal ağda yalnızca bir alt ağ, Azure için NetApp dosyaları atanabilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Tıklayın **Protokolü** ve aşağıdaki bilgileri doldurun:  
    * Seçin **SMB** protokol türü için toplu olarak. 
    * Seçin, **Active Directory** aşağı açılan listeden bağlantıyı.
    * Paylaşılan biriminde adını **paylaşım adı**.

    ![SMB protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Tıklayın **gözden geçir + Oluştur** birim ayrıntılarını gözden geçirmek için.  Ardından **Oluştur** SMB birim oluşturmak için.

    Oluşturduğunuz birim birimler sayfasında görüntülenir. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar  

* [Bağlamak veya bir birimi Windows veya Linux sanal makineleri için çıkarma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [NFS birimi için verme ilkesi yapılandırma](azure-netapp-files-configure-export-policy.md)
* [Azure Hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
