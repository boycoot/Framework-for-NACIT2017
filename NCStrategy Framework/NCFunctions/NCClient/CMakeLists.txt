# CMake最低版本要求
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

# 系统中寻找OpenCV的安装路径
FIND_PACKAGE(OpenCV REQUIRED)

# 添加源文件中的#include路径
INCLUDE_DIRECTORIES(./)
INCLUDE_DIRECTORIES(../)
INCLUDE_DIRECTORIES(../../CustomizeFunctions/SupportFunctions)
INCLUDE_DIRECTORIES(../../CustomizeFunctions/CustomizeStructs)
INCLUDE_DIRECTORIES(../../CustomizeFunctions/GeneralImageProcess)
INCLUDE_DIRECTORIES(${OpenCV_INCLUDE_DIRS})

# 添加依赖库路径
LINK_DIRECTORIES(${PROJECT_BINARY_DIR}/lib)

# 设置路径变量
SET(LIB_CLIENT_SOURCE
	./ncclient.cpp
	../../CustomizeFunctions/SupportFunctions/string_fun.cpp)
SET(LIB_IMAGE_SOURCE
	${LIB_CLIENT_SOURCE}
	./ncclient_image.cpp)
SET(LIB_SEND_SOURCE
	${LIB_CLIENT_SOURCE}
	./ncclient_send.cpp)
SET(LIB_SURF_SOURCE
	${LIB_CLIENT_SOURCE}
	./ncclient_surface.cpp)

# 生成库文件
ADD_LIBRARY(ncclient SHARED ${LIB_CLIENT_SOURCE})
ADD_LIBRARY(ncclient_image SHARED ${LIB_IMAGE_SOURCE})	
# 连接库文件，以这些库文件为基础生成目标文件
TARGET_LINK_LIBRARIES(ncclient_image 
	${OpenCV_LIBS}
	imageprocessing_fun)

ADD_LIBRARY(ncclient_send SHARED ${LIB_SEND_SOURCE})
ADD_LIBRARY(ncclient_surf SHARED ${LIB_SURF_SOURCE})
