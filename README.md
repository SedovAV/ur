## 0. <a href="https://drive.google.com/file/d/1tyhFtulMPRBldcNUsnddQIRv0HSOLLNl/view?usp=sharing">Скачать дистрибутив ubuntu</a>
# 1. Установка ROS

Рекомендуется использовать ROS Noetic для Ubuntu 20.04 или ROS Melodic для Ubuntu 18.04.
Пример для Ubuntu 20.04 (ROS Noetic):

### Добавление репозитория ROS
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

### Обновление и установка ROS
sudo apt update

sudo apt install ros-noetic-desktop-full

echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc

source ~/.bashrc

# 2. Установка Gazebo

Gazebo обычно устанавливается вместе с ROS. Если нет:

sudo apt install gazebo11 libgazebo11-dev

# 3. Создание рабочего пространства ROS

mkdir ~/catkin_ws

cd ~/catkin_ws

mkdir src

cd ~/catkin_ws

sudo catkin_make

exit

cd ~/catkin_ws

source devel/setup.bash

# 4. Установка пакетов Universal Robots для Gazebo

cd ~/catkin_ws/src

### Основные пакеты Universal Robots
git clone -b noetic-devel https://github.com/ros-industrial/universal_robot.git

### Драйверы и утилиты
git clone https://github.com/UniversalRobots/Universal_Robots_ROS_Driver.git

git clone -b noetic-devel https://github.com/ros-industrial/ur_msgs.git

### Пакет для интеграции с Gazebo
git clone https://github.com/ros-simulation/gazebo_ros_pkgs.git

# 5. Установка зависимостей
sudo apt install ros-noetic-moveit ros-noetic-joint-state-controller ros-noetic-effort-controllers

sudo apt install ros-noetic-gazebo-ros-control ros-noetic-ros-controllers

sudo rosdep update

cd ~/catkin_ws

sudo rosdep install --from-paths src --ignore-src -r -y

# 6. Сборка рабочего пространства
sudo cd ~/catkin_ws

sudo catkin_make

exit

cd ~/catkin_ws

source devel/setup.bash

# 7. Запуск симуляции в Gazebo

### Для запуска модели UR5 в Gazebo:
Эта команда запустит Gazebo. Загрузит модель UR5. Активирует контроллеры для управления роботом.

roslaunch ur_gazebo ur5_bringup.launch

# 8. Управление роботом через MoveIt (!!! следующие команды необходмо проверить согласно именования папок и файлов !!!)
### Пример управления роботом с помощью MoveIt (планирование движений):

roslaunch ur5_moveit_config ur5_moveit_planning_execution.launch sim:=true

# 9. Пример тестового скрипта

Отправьте тестовую цель для движения:

roslaunch ur5_moveit_config moveit_planning_execution.launch sim:=true

rosrun ur5_moveit_config move_group_interface_demo.py

### Возможные проблемы и решения
Ошибки при запуске Gazebo:

Убедитесь, что все зависимости установлены (ros-noetic-gazebo-ros-pkgs).

Проверьте, что модели роботов загружены (иногда Gazebo требует скачивания ассетов).

### Отсутствие контроллеров:
Убедитесь, что пакеты ros-noetic-ros-controllers установлены.

### Ошибки MoveIt:
Пересоберите рабочее пространство: 

catkin_make clean && catkin_make.

### Полезные команды

#### Просмотр топиков:
rostopic list

#### Ручное управление через GUI:
rosrun rqt_joint_trajectory_controller rqt_joint_trajectory_controller
