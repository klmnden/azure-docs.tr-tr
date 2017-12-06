# <a name="support-matrix"></a>Destek matrisi

| | **Vmware’den Azure’a** |**Hyper-V'den Azure'a**|**Azure'dan Azure'a**|**Hyper-V ikincil siteye**|**İkincil site için VMware**
--|--|--|--|--|--
Desteklenen senaryolar |Evet|Evet|Hayır|Evet*|Hayır
Desteklenen sürüm | vCenter 6.5, 6.0 veya 5.5| Windows Server 2016, Windows Server 2012 R2 | NA |Windows Server 2016, Windows Server 2012 R2|NA
Desteklenen yapılandırma|vCenter, ESXi| Hyper-V kümesi, Hyper-V konağı|NA|Hyper-V kümesi, Hyper-V konağı|NA|
Azure Site Recovery dağıtımı Planlayıcısı örneğini çalıştıran her profili sunucularının sayısı |Tek (bir vCenter sunucusu veya bir ESXi sunucusunda ait VM'ler profili aynı anda)|Birden çok (birden çok konak veya konak kümeleri VM'ler profili bir aynı anda olabilir)| NA |Birden çok (birden çok konak veya konak kümeleri VM'ler profili bir aynı anda olabilir)| NA

* Azure olağanüstü durum kurtarma senaryosuna Hyper-V için öncelikle bir araçtır. Kaynak anlamak için gerekli ağ bant genişliği, her biri kaynak Hyper-V sunucuları ve ilk çoğaltma numaraları ve toplu toplu işleme gerekli boş depolama alanı yan önerileri ister yalnızca Hyper-V için ikincil site olağanüstü durum kurtarma için kullanılabilir tanımları.  Azure önerileri ve maliyetleri raporundan yoksayar. Ayrıca, işleme alma işlemi için ikincil site olağanüstü durum kurtarma senaryosunda Hyper-V için geçerli değildir.
