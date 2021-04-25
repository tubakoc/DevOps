1.CASE
1.1
Hypervisor işlemi için VMbox'ı seçildi. Nedeni ise Vmware de centos kurulumu yaptıktan sonra vmwarenın networke bağlanmadığını görüldü. Gerekli olan güncellemeler yapılmıyordu. Ens33 deaktifti. Manuel olarak aktifleştirmek için terminalde 
#sudo nmtui 
komutu yazıldı. Bu işlemde networkmanager tuı penceresinden aktifleştirmeye çalışıldı bu işlem işe yaramadı.  
#vi /etc/sysconfig/network-scripts/ifcfg-ens33 
komutu ile network arayüzü manuel olarak yapılandırıldı. default olarak
ONBOOT=no geliyordu. Yes ile değiştirildi. Vmware de bu çözümler işe yaramadığı için VmBox a kurulumu yapıldı. VmBox ta centos sürümü olarak Centos7 kurulumunu yapıldı.
# yum update
komutu ile güncellemeler yapıldı. Yapılan değişikliğin işlemleri githubta paylaşmak için 
#sudo yum install git 
komutu ile git indirildi. Gerekli configuration terminalden 
#git config  --global  user.name “tubakoc”
#git config  --global  user.email “testuser@example.com”
komutları ile yapıldı. 


1.2
Yapılacak olan case adımlarını #tuba.koca user ile yapmak için yeni bir user eklendi
#adduser tuba.koca 
ile yeni bir user oluşturuldu.  
#passwd  tuba.koca 
komutu ile oluşturulan user a şifre ataması yapıldı. Root kullanıcısından tuba.koca kullanıcısına geçiş yapmak için  
#su tuba.koca 
komutu yazıldı.
Kullanıcı ve şifre tanımlama işlemi tamamlandıktan sonra, # visudo komutu ile sudoers dosyasında kullanıcıya root yetkisi tanımlandı. Karşımıza gelen ekranda
 ## Allow root to run any commands anywhere bölümüne gelerek root kullanıcısının altına “tuba.koca” isimli kullanıcıyı tanımlayıp “ALL” yetkisi verilir ve dosya kayıt edildi.
tuba.koca ALL=(ALL)    ALL


1.3
Disk ekleme ve mount işlemini yapmadan önce 
#sudo fdisk –l 
veya 
#lsblk
komutu ile diskin Centos 7 nin disk mevcut yapısı incelendi bulunan diskler listelendi.

![01](https://user-images.githubusercontent.com/28953086/116008780-eff56a80-a61e-11eb-931e-2dd66afff889.png)
#df -h 
![02](https://user-images.githubusercontent.com/28953086/116008788-fb489600-a61e-11eb-8675-a1c336a7b7fd.png)
komutu sistemdeki disklerin kullanım oranlarını içeren bir rapor oluşturur.  

Disk ekleme işlemi sırayla böyle gerçekleşti. Vmbox ta ayarlar sekmesinden Stroge(Depolama) tıklandı.
Depolama aygıtları kısmında yeni disk ekleme işlemi yapıldı. Ekrana gelen yönlendirmeler ile disk başarılı bir şekilde eklendi.
![03](https://user-images.githubusercontent.com/28953086/116008796-056a9480-a61f-11eb-889e-29da71bf0860.png)

Eklenen disk boyutu istenilen boyutta belirlenebilir.
![04](https://user-images.githubusercontent.com/28953086/116008812-24692680-a61f-11eb-8bb2-fe32ec663d7e.png)

Disk ekleme işleminden sonra mevcut sabit disk sürücülerini listelemek için 
#ls /dev/sd* 
koumutu yazıldı. Ve eklenen disk artık listeleniyor.(yeni eklenen disk sdb)
![05](https://user-images.githubusercontent.com/28953086/116008820-2df28e80-a61f-11eb-929f-deb9f28be13d.png)
![06](https://user-images.githubusercontent.com/28953086/116008834-3945ba00-a61f-11eb-8647-08252b5cf74a.png)

#sudo fdisk –l 
komutu ile mevcut diskler görüntülendi. Yeni eklenen disk listede görünmüyordu nedeni disk üzerinde henüz mount işlemi yapmamaktı. 
![07](https://user-images.githubusercontent.com/28953086/116008838-406cc800-a61f-11eb-9c08-fe73e36351a7.png)

Disk yönetimi için fdisk komutu kullanıldı.
# sudo fdisk /dev/sdb  
ardından p ye basılarak var olan disk ayrıntılar disk üzerinde yapılan partitionlar görüntülendi.
n ne tıklanarak yeni bir partition oluşturuldu. Oluşturulan partition sdb altında sdb olarak kaydedildi.
![08](https://user-images.githubusercontent.com/28953086/116008845-49f63000-a61f-11eb-8140-3f183b58b9e7.png)

#mkdir –p /opt/bootcamp 
komutu ile opt altında bootcamp dizini oluşturuldu.
Eklenen disk 
#mkfs.ext4 /dev/sdb1 
komutu ile formatlandı. Formatlama işleminden sonra mount işlemine geçildi. 
#mount /dev/sdb1 /opt/bootcamp/ 
komutu ile  mount işlemi yapıldı. Disk botcamp dizinine bağlandı. 
Mount işleminin kalıcı olması için  
# vi /etc/fstab 
komutu ile fstab arayüzüne girilir ve ek olarak mount şlemi yapılan disk bilgileri yazılır. 
/dev/sdb1  /opt/bootcamp     ext4  defaults 0 0 
Fstab da yapılan değişikliğin kaydedilmesi için 
#mount –a komutu yazılır.
Yapılan işlemler disk ekleme ve “bootcamp” olarak mount etme başarıyla gerçekleşti.

![09](https://user-images.githubusercontent.com/28953086/116008849-51b5d480-a61f-11eb-8e0c-d7bb35d22ebf.png)


1.4
#mkdir –p /opt/bootcamp/ 
komutu ile bootcamp dizini  oluşturuldu.
# cd /opt/bootcamp 
komutu ile bootcamp dizinine gidildi.
#touch bootcamp.txt 
komutu ile bootcamp dosyası oluşturuldu.
# echo “Merhaba Trendyol” > bootcamp.txt 
ile bootcamp.txt metin editörüne “Merhaba Trendyol” yazdırıldı.


1.5
[tuba.koca@ localhost]#sudo find / -name "bootcamp.txt" -exec mv {} /bootcamp/ \  komutu ile bootcamp.txt bulup bootcamp diskine taşıma işlemi yapıldı.






 
 
  
  
  
  
 



