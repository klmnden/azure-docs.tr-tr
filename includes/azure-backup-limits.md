
Aşağıdaki sınırları Azure yedekleme hizmeti için geçerlidir.

| Sınır tanımlayıcısı | Varsayılan Sınır |
| --- | --- |
| Her kasa için kaydedilebilen kayıtlı sunucuları/makine sayısı |Windows Server/istemci/SCDPM için 50 <br/> Iaas VM'ler için 200 |
| Azure kasası depolamada depolanan veriler için bir veri kaynağı boyutu |54400 GB max<sup>1</sup> |
| Her Azure aboneliğinizde oluşturduğunuz yedek kasalarını sayısı |25 (Yedekleme kasaları) <br/> Her bölge 25 kurtarma Hizmetleri kasaları |
| Yedekleme günde zamanlanabilir sayısı |Windows Server/istemcisi için günde 3 <br/> SCDPM için gün başına 2 <br/> Iaas VM'ler için günde bir kez |
| Yedekleme için bir Azure sanal makinesine bağlı veri diskleri |16 |
| Yedekleme için bir Azure sanal makineye bağlı tek bir veri diski boyutu| 1023 GB <sup>2</sup>|

* <sup>1</sup>54400 GB sınırını Iaas VM yedekleme için geçerli değildir.
* <sup>2</sup> sahip olduğumuz bir [özel Önizleme](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a?redir=0) yönetilmeyen diskleri fazla 4 TB desteklemek için. 

