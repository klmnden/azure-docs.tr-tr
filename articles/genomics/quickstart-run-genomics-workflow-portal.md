---
title: 'Hızlı Başlangıç: Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma | Microsoft Docs'
description: Bu hızlı başlangıçta giriş verilerini Azure Blob Depolama'ya yükleme ve Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma adımları gösterilir.
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: quickstart
ms.date: 12/07/2017
ms.openlocfilehash: 1436ad54eb13052aa87ccfd5adc371c8d7d5a100
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31403799"
---
# <a name="quickstart-run-a-workflow-through-the-microsoft-genomics-service"></a>Hızlı Başlangıç: Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma

Microsoft Genomiks, ham okumalardan başlayarak hizalanmış okumalar ve varyant ilanları üreten, bir genomu hızlı bir şekilde işleyebilen ikincil analize yönelik ölçeklenebilir ve güvenli bir hizmettir. Birkaç adımda kullanmaya başlayabilirsiniz: 
1.  Kurulum: Azure portalından bir Microsoft Genomiks hesabı oluşturup Microsoft Genomiks Genomics Python istemcisini yerel ortamınıza yükleyin. 
2.  Giriş verilerini yükleme: Azure portalından bir Microsoft Azure depolama hesabı yükleyip giriş dosyalarını yükleyin. Giriş dosyalarının uç okumalarında eşleştirilmesi gerekir (fastq veya bam dosyaları).
3.  Çalıştırma: Microsoft Genomiks komut satırı arabirimini kullanarak Microsoft Genomiks hizmeti üzerinden iş akışlarını çalıştırabilirsiniz. 

Microsoft Genomiks hakkında daha fazla bilgi için bkz. [Microsoft Genomiks nedir?](overview-what-is-genomics.md)

## <a name="set-up-create-a-microsoft-genomics-account-in-the-azure-portal"></a>Kurulum: Azure portalında bir Microsoft Genomiks hesabı oluşturma

Microsoft Genomiks hesabı oluşturmak için [Azure portalına](https://portal.azure.com/#create/Microsoft.Genomics) gidin. Azure aboneliğiniz yoksa Microsoft Genomiks hesabı oluşturmadan bir hesap açın. 

![Azure portalında Microsoft Genomiks](./media/quickstart-run-genomics-workflow-portal/genomics-create-blade.png "Azure portalında Microsoft Genomiks")



Genomiks hesabınızı bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. 

 |**Ayar**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------       |:-------------         |:----------            |
 |Hesap adı         | MyGenomicsAccount     |Benzersiz bir hesap tanımlayıcı seçin. Geçerli adlar için bkz. [Adlandırma Kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Abonelik         | Aboneliğinizin adı|Bu, Azure hizmetleriniz için faturalandırma birimidir. Aboneliğiniz hakkında ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Kaynak grupları kolay yönetim için birden fazla Azure kaynağını (depolama hesabı, genomiks hesabı vs.) tek bir grupta toplamanızı sağlar. Daha fazla bilgi için bkz. [Kaynak Grupları] (https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups). Geçerli kaynak grubu adları için bkz. [Adlandırma Kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Konum                   | Batı ABD 2                    |    Bu hizmet Batı ABD 2, Batı Avrupa ve Güneydoğu Asya konumlarında kullanılabilir |




Dağıtım işlemini izlemek için üst menü çubuğundaki Bildirimler'e tıklayabilirsiniz.
![Microsoft Genomiks Bildirimleri](./media/quickstart-run-genomics-workflow-portal/genomics-notifications-box.png "Microsoft Genomiks Bildirimleri")



## <a name="set-up-install-the-microsoft-genomics-python-client"></a>Kurulum: Microsoft Genomiks Python istemcisini yükleme

Kullanıcıların hem Python hem de Microsoft Genomiks Python istemcisini yerel ortamlarına yüklemesi gerekir. 

### <a name="install-python"></a>Python'ı yükleme

Microsoft Genomiks Python istemcisi Python 2.7 ile uyumludur. Sürüm 2.7.12 veya üzerini kullanmanızı öneririz, 2.7.14 sürümü tercih edilir. Dosyayı [buradan](https://www.python.org/downloads/) indirebilirsiniz. 


### <a name="install-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini yükleme

Microsoft Genomiks istemcisini `msgen` yüklemek için Python pip kullanın. Aşağıdaki talimatlarda Python uygulamasının sistem yolunuzda olduğu kabul edilmektedir. pip yüklemesinin tanınmaması nedenli sorunlarla karşılaşırsanız Python uygulamasını ve betik alt klasörünü sistem yolunuza eklemeniz gerekir.


```
pip install --upgrade --no-deps msgen
pip install msgen
```


`msgen` yüklemesini sistem çapında ikili dosya olarak yüklemek ve sistem çapındaki Python paketlerini değiştirmek istemezseniz `–-user` bayrağını `pip` ile kullanın.
Paket tabanlı yükleme veya setup.py dosyasını kullandığınızda gerekli tüm paketler yüklenir. Yüklenmezse msgen için gerekli temel paketler 

 * [Azure-storage](https://pypi.python.org/pypi/azure-storage). 
 * [İstekler](https://pypi.python.org/pypi/requests). 


Bu paketleri `pip`, `easy_install` veya standart `setup.py` yordamlarını kullanarak yükleyebilirsiniz. 





### <a name="test-the-microsoft-genomics-client"></a>Microsoft Genomiks istemcisini test etme
Microsoft Genomiks istemcisini test etmek için genomiks hesabınızdan yapılandırma dosyasını indirin. Sol üstte bulunan **Tüm hizmetler**'e tıklayıp, filtreleyip genomiks hesaplarını seçerek, genomiks hesabınıza gidebilirsiniz.


![Azure portalında Microsoft Genomiks hesaplarını filtreleme](./media/quickstart-run-genomics-workflow-portal/genomics-filter-box.png "Azure portalında Microsoft Genomiks hesaplarını filtreleme")



En son oluşturduğunuz genomiks hesabını seçin, **Erişim Anahtarları**'na gidin ve yapılandırma dosyasını indirin.

![Microsoft Genomiks yapılandırma dosyasını indirme](./media/quickstart-run-genomics-workflow-portal/genomics-mygenomicsaccount-box.png "Microsoft Genomiks yapılandırma dosyasını indirme")


Aşağıdaki komutu kullanarak Microsoft Genomiks Python istemcisinin çalışıp çalışmadığını test edin


```
msgen list -f “<full path where you saved the config file>”
```

## <a name="create-a-microsoft-azure-storage-account"></a>Microsoft Azure Depolama Hesabı oluşturma 
Microsoft Genomiks hizmeti girişlerinin Azure depolama hesabında blok blobları olarak depolanmasını bekler. Ayrıca çıkış dosyalarını Azure depolama hesabında kullanıcı tarafından belirtilen bir kapsayıcıya blok blobları olarak yazar. Girişler ve çıkışlar farklı depolama hesaplarında tutulabilir.
Azure depolama hesabınızda veri varsa Genomiks hesabınızla aynı konumda olduğundan emin olmanız gerekir. Aksi halde Genomiks hizmeti çalıştırıldığında çıkış ücretleri alınabilir. Microsoft Azure Depolama hesabınız yoksa hesap oluşturup verilerinizi yüklemeniz gerekir. Depolama hesabı ve sunduğu hizmetler dahil olmak üzere Azure Depolama hesapları hakkında daha fazla bilgiye [buradan](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) ulaşabilirsiniz. Microsoft Azure Depolama hesabı oluşturmak için [Azure portalına](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM ) gidin.  

![Depolama oluşturma dikey penceresi](./media/quickstart-run-genomics-workflow-portal/genomics-storage-create-blade.png "Depolama oluşturma dikey penceresi")

Depolama hesabınızı bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle yapılandırın. Depolama hesabı için standart seçenekleri kullanın ve yalnızca hesabın genel amaçlı değil blob depolama için olduğunu belirtin. Blob depolama indirme ve yükleme işlemlerinde 2-5 kat daha yüksek hız sunabilir. 


 |**Ayar**          |  **Önerilen değer**  | **Alan açıklaması** |
 |:-------------------------       |:-------------         |:----------            |
 |Adı         | MyStorageAccount     |Benzersiz bir hesap tanımlayıcı seçin. Geçerli adlar için bkz. [Adlandırma Kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Dağıtım Modeli         | Resource Manager| Önerilen dağıtım modeli Resource Manager'dır. Daha fazla bilgi için bkz. [Resource Manager dağıtımını anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) |      
 |Hesap türü       | Blob depolama       |  Blob depolama indirme ve yükleme işlemlerinde genel amaçlı depolama alanından 2-5 kat daha yüksek hız sunabilir. |
 |Performans                  | Standart                   | Varsayılan olarak standart seçeneği kullanılır. Standart ve premium depolama hesapları hakkında daha fazla bilgi için bkz. [Microsoft Azure Depolama'ya giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction)    |
 |Çoğaltma                  | Yerel olarak yedekli depolama                  | Yerel olarak yedekli depolama, verilerinizi depolama hesabınızı oluşturduğunuz bölgedeki veri merkezi içinde çoğaltır. Daha fazla bilgi için bkz. [Azure Depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy)    |
 |Güvenli aktarım gerekir                  | Devre dışı                 | Varsayılan olarak devre dışı seçeneği kullanılır. Veri aktarımı güvenliği hakkında daha fazla bilgi için bkz. [Güvenli aktarım gerektir](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer)    |
 |Erişim katmanı                  | Sık Erişimli                   | Sık erişimli seçeneği, depolama hesabındaki nesnelere erişimin daha sık olduğunu belirtir.    |
 |Abonelik         | Azure aboneliğiniz |Aboneliğiniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions) |      
 |Kaynak grubu       | MyResourceGroup       |  Genomiks hesabınızla aynı kaynak grubunu seçebilirsiniz. Geçerli kaynak grubu adları için bkz. [Adlandırma Kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) |
 |Konum                  | Batı ABD 2                  | Çıkış ücretlerini ve gecikme süresini azaltmak için genomiks hesabınızla aynı konumu kullanın. Genomiks hizmeti Batı ABD 2, Batı Avrupa ve Güneydoğu Asya konumlarında kullanılabilir    |
 |Sanal ağlar                | Devre dışı                   | Varsayılan olarak devre dışı seçeneği kullanılır. Daha fazla bilgi için bkz. [Azure Sanal Ağları](https://docs.microsoft.com/azure/storage/common/storage-network-security)    |





Ardından depolama hesabınızı oluşturmak için Oluştur'a tıklayın. Genomiks Hesabınızı oluşturma işlemlerinde olduğu gibi üst menü çubuğundaki Bildirimler'e tıklayarak dağıtım işlemini izleyebilirsiniz. 


## <a name="upload-input-data-to-your-storage-account"></a>Giriş verilerini depolama hesabınıza yükleyin

Microsoft Genomiks hizmeti giriş dosyası olarak eşleştirilmiş uç okuma bekler. Kendi verilerinizi yükleyebilir veya sunulan genel kullanıma açık örnek verileri kullanarak hizmeti keşfedebilirsiniz. Genel kullanıma açık örnek verileri kullanmak isterseniz buradan ulaşabilirsiniz:


[https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz)
[https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz](https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz)


Depolama hesabınızda biri giriş verileriniz, biri de çıkış verileriniz için olmak üzere iki blob kapsayıcısı oluşturmanız gerekir.  Giriş verilerini giriş blob kapsayıcısına yükleyin. Bunu yapmak için [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), [blobporter](https://github.com/Azure/blobporter) veya [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi farklı araçları kullanabilirsiniz. 



## <a name="run-a-workflow-through-the-microsoft-genomics-service-using-the-python-client"></a>Python istemcisini kullanarak Microsoft Genomiks hizmeti üzerinden iş akışı çalıştırma 

Microsoft Genomiks hizmeti üzerinden bir iş akışı çalıştırmak için config.txt dosyasını verilerinizin giriş ve çıkış depolama kapsayıcısını belirtecek şekilde düzenleyin.
Genomiks hesabınızdan indirdiğiniz config.txt dosyasını açın. Belirtmeniz gereken bölümler abonelik anahtarınız ve en alttaki altı öğe, depolama hesabınızın adı, anahtar ve hem giriş hem de çıkış için kapsayıcı adı. Bu bilgiye portala gidip depolama hesabınızın **Erişim anahtarları** sayfasından veya doğrudan Azure Depolama Gezgini'nden ulaşabilirsiniz.  


![Genomiks yapılandırması](./media/quickstart-run-genomics-workflow-portal/genomics-config.png "Genomiks yapılandırması")

### <a name="submit-your-workflow-to-the-microsoft-genomics-service-the-microsoft-genomics-client"></a>İş akışınızı Microsoft Genomiks hizmetine ve Microsoft Genomiks istemcisine gönderme

Aşağıdaki komutu kullanarak Microsoft Genomiks Python istemcisiyle iş akışınızı gönderin:


```python
msgen submit -f [full path to your config file] -b1 [name of your first paired end read] -b2 [name of your second paired end read]
```


İş akışlarınızın durumunu görüntülemek için aşağıdaki komutu kullanabilirsiniz: 
```python
msgen list -f c:\temp\config.txt 
```


İş akışınız tamamlandıktan sonra yapılandırdığınız Azure Depolama Hesabı çıkış kapsayıcısında çıkış dosyalarını görüntüleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede örnek giriş verilerini Azure Depolama'ya yükleyip `msgen` Python istemcisi üzerinden Microsoft Genomiks hizmetine bir iş akışı gönderdiniz. Microsoft Genomiks hizmeti ile kullanılabilecek diğer giriş dosya türleri hakkında daha fazla bilgi için şu sayfalara bakın: [eşleştirilmiş FASTQ](quickstart-input-pair-FASTQ.md) | [BAM](quickstart-input-BAM.md) | [Çoklu FASTQ veya BAM](quickstart-input-multiple.md). Bu öğreticiyi [Azure not defteri öğreticimizi](http://aka.ms/genomicsnotebook) kullanarak da keşfedebilirsiniz.