# 220601002_ros2_env2
# ROS 2 Mobile Robot Simulation – Assignment 2

Bu repository, ROS 2 Humble kullanılarak geliştirilen mobil robot simülasyonuna ait Ödev 2 çalışmasını içermektedir. Çalışma kapsamında diferansiyel sürüş (diff drive) modeli Gazebo ortamında etkinleştirilmiş, robotun kinematik yapısı, odometri üretimi ve temel doğrulamalar gerçekleştirilmiştir.

## Kullanılan Teknolojiler

- İşletim Sistemi: Ubuntu 22.04 LTS  
- ROS Dağıtımı: ROS 2 Humble  
- Simülasyon Ortamı: Gazebo (gz sim)  
- Derleme Aracı: colcon  
- Workspace: `~/robotik2`

## Paket Yapısı

```text
robotik2/
├── src/
│   ├── my_robot_description
│   │   ├── urdf
│   │   └── models
│   │       └── my_robot
│   │           ├── my_robot.urdf
│   │           └── my_robot.sdf
│   ├── my_robot_bringup
│   │   ├── launch
│   │   │   └── bringup.launch.py
│   │   └── worlds
│   │       └── empty_sensors.sdf
│   └── ball_chaser
```

---

## Robot Modeli

Mobil robot modeli URDF/Xacro formatında tanımlanmış ve SDF formatına dönüştürülerek Gazebo ortamında çalıştırılmıştır. Robot gövdesi, sol ve sağ tekerlekler ve sabit bir caster yapıdan oluşmaktadır. Model, Gazebo ortamında doğru şekilde yüklendiği ve sahnede görünür olduğu doğrulanmıştır.

---

## Diff Drive Entegrasyonu

Robotun diferansiyel sürüş modeli Gazebo üzerinde **gz-sim-diff-drive-system** eklentisi kullanılarak etkinleştirilmiştir. Sol ve sağ tekerlek eklemleri **left_wheel_joint** ve **right_wheel_joint** olarak tanımlanmıştır. Robotun kinematik parametreleri olarak tekerlek mesafesi **0.22 m** ve tekerlek yarıçapı **0.05 m** kullanılmıştır.

Hareket kontrolü için **/cmd_vel** topic’i atanmış, odometri çıktısı için **/odom** topic’i etkinleştirilmiştir. Gazebo tarafından üretilen odometri verisi okunarak diferansiyel sürüş entegrasyonu doğrulanmıştır.

---

## Kamera ve RViz Doğrulaması

Robot modeli RViz ortamında **RobotModel** gösterimi kullanılarak doğrulanmış ve TF ağı **tf2_tools view_frames** aracı ile çıkarılmıştır. Kamera sensörü robot modeline eklenmiş ve simülasyon ortamında test edilmiştir. Ancak mevcut yapılandırmada **/camera/image_raw** topic’inin kararlı şekilde yayınlandığı doğrulanamamıştır. Bu nedenle kamera çıktısı kesin kanıt olarak sunulmamış, doğrulama RViz ve TF ağacı üzerinden yapılmıştır.

---

## Ball Chaser

Bu ödev kapsamında **Ball Chaser uygulaması gerçekleştirilmemiştir**. Bu nedenle top takibi davranışı, algoritma açıklaması ve ilgili topic akışları rapora dahil edilmemiştir.

---

## Deney ve Gözlemler

Simülasyon sürecinde robotun Gazebo ortamında doğru şekilde yüklendiği, diferansiyel sürüş eklentisi ile **/cmd_vel** ve **/odom** topic’lerinin oluştuğu gözlemlenmiştir. Odometri verisinin Gazebo tarafından üretildiği doğrulanmıştır. Ball Chaser uygulanmadığı için davranış temelli takip senaryoları değerlendirilmemiştir.

---

## Çalıştırma Adımları

Workspace’in derlenmesi ve simülasyonun başlatılması için aşağıdaki adımlar izlenmiştir:

```bash
cd ~/robotik2
colcon build --symlink-install
source install/setup.bash
ros2 launch my_robot_bringup bringup.launch.py
```
Doğrulama amacıyla kullanılan örnek komutlar aşağıda verilmiştir:

```bash
gz model --list
gz topic -l | grep -E "cmd_vel|odom"
gz topic -e -n 1 -t /odom
```

## Sonuç

Bu çalışma ile ROS 2 tabanlı bir mobil robot modeli oluşturulmuş, diferansiyel sürüş kontrolü Gazebo ortamında başarıyla entegre edilmiştir. Robotun odometri üretimi, TF ağacı ve temel hareket kontrol altyapısı doğrulanmıştır. Gelecek çalışmalarda sensör entegrasyonunun kararlı hale getirilmesi ve davranış tabanlı kontrol katmanlarının eklenmesi hedeflenmektedir.
