pkgdesc="ROS - 3D visualization tool for ROS."
url='https://wiki.ros.org/rviz'

pkgname='ros-noetic-rviz'
pkgver='1.14.25'
arch=('i686' 'x86_64' 'aarch64' 'armv7h' 'armv6h')
pkgrel=2
license=('BSD, Creative Commons')

ros_makedepends=(
    ros-noetic-catkin
    ros-noetic-cmake-modules
    ros-noetic-message-generation
)

makedepends=(
    cmake
    ros-build-tools
    ${ros_makedepends[@]}
    eigen
    qt5-base
    urdfdom
    urdfdom-headers
)

ros_depends=(
    ros-noetic-geometry-msgs
    ros-noetic-image-transport
    ros-noetic-interactive-markers
    ros-noetic-laser-geometry
    ros-noetic-map-msgs
    ros-noetic-message-filters
    ros-noetic-pluginlib
    ros-noetic-python-qt-binding
    ros-noetic-resource-retriever
    ros-noetic-rosbag
    ros-noetic-rosconsole
    ros-noetic-roscpp
    ros-noetic-roslib
    ros-noetic-rospy
    ros-noetic-sensor-msgs
    ros-noetic-std-msgs
    ros-noetic-std-srvs
    ros-noetic-tf2-ros
    ros-noetic-tf2-geometry-msgs
    ros-noetic-tf2-eigen
    ros-noetic-urdf
    ros-noetic-visualization-msgs
    ros-noetic-media-export
    ros-noetic-message-runtime
)

depends=(
    ${ros_depends[@]}
    assimp
    ogre=1.9
    tinyxml2
    yaml-cpp
    mesa
    qt5-base
    sip4
    python-sip4
)

_dir="rviz-${pkgver}/"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/ros-visualization/rviz/archive/${pkgver}.tar.gz"
"mesh_loader.patch"
"robot_link.patch")
sha256sums=('d0eb3e9522d106bdf6ae5e8991895ce3408400cba437fe295d4d2fa916bfb852'
'384e0fcfe9f39b25a40cc9c87501c37f9ac49b0af69453d2120a0994730dd273'
'2c57ef710f7e70bc8db547342a50d9ebbc912def8840f38c6575946511bb9458')

prepare() {
cd "$srcdir/rviz-$pkgver"
patch -p1 < "$srcdir/mesh_loader.patch"
patch -p1 < "$srcdir/robot_link.patch"
}
build() {
    # Use ROS environment variables.
    source /usr/share/ros-build-tools/clear-ros-env.sh
    [ -f /opt/ros/noetic/setup.bash ] && source /opt/ros/noetic/setup.bash

    # Build the project.
    cmake -B build -S ${_dir} \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/noetic \
        -DCMAKE_CXX_STANDARD=17 \
        -DPYTHON_EXECUTABLE=/usr/bin/python \
        -DSETUPTOOLS_DEB_LAYOUT=OFF

    make -sC build
}

package() {
    cd build
    make DESTDIR="${pkgdir}/" install
}
