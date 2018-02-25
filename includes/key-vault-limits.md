Anahtar işlemleri (10 saniye içinde başına izin verilen en fazla işlemler kasa bölge<sup>1</sup>):

|Anahtar türü|HSM-Key<br>Anahtar oluşturma|HSM anahtarı<br>Diğer tüm işlemler|Yazılım anahtarı<br>Anahtar oluşturma|Yazılım anahtarı<br>Diğer tüm işlemler|
|:---|---:|---:|---:|---:|
|RSA 2048 bit|5|1000|10|2000|
|RSA 3072-bit|5|250|10|500|
|RSA 4096 bit|5|125|10|250|
|

Parolaları, yönetilen depolama hesabı anahtarlarını ve kasa işlemleri:
| İşlem türü | 10 saniye içinde başına izin verilen en fazla işlemler kasa bölge<sup>1</sup> |
| --- | --- |
| Tüm işlemleri |2000 |
|

Bkz: [Azure anahtar kasası Kılavuzu azaltma](../key-vault/key-vault-ovw-throttling.md) bu sınırları aşıldığında azaltma nasıl ele alınacağını hakkında bilgi için.

<sup>1</sup> tüm işlem türleri için anahtar kasası başına 5 x, bir abonelik genelinde sınırı yoktur. Abonelik başına 10 saniye içinde diğer işlemleri abonelik başına 5000 işlemleri için sınırlı Örneğin, HSM -.
