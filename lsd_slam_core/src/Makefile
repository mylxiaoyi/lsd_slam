CXX = g++ -std=c++11 -Wl,--copy-dt-needed-entries

all: dataset

dataset: main_on_images.cpp
	$(CXX) main_on_images.cpp -o dataset -I. -I../cfg/cpp -I../thirdparty/Sophus $(shell pkg-config --cflags --libs eigen3) $(shell pkg-config --libs roslib rosconsole) -lpthread -lboost_system -L../lib -llsdslam $(shell pkg-config --libs opencv) /home/mylxiaoyi/source/ros_catkin_ws/install_isolated/lib/libdynamic_reconfigure_config_init_mutex.so /home/mylxiaoyi/source/ros_catkin_ws/install_isolated/lib/libroscpp.so -L/home/mylxiaoyi/study/g2o/lib -lg2o_csparse_extension -lg2o_ext_csparse -lg2o_core -lg2o_solver_cholmod -lg2o_solver_csparse
