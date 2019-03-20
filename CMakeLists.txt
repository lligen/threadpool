
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
INCLUDE(cppbuild_common)
PROJECT(threadpool)

CPPBUILD_DEPEND()
CPPBUILD_ENABLE_CXX11()


INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})

MESSAGE(STATUS "CPPBUILD_TARGET_OS: " ${CPPBUILD_TARGET_OS})
#set(Prebuild_Boost_Path "/data/tools/Boost-for-Android-Prebuilt/boost_1_68_0-clang")
set(Prebuild_Boost_Path "/data/tools/Boost-for-Android-Prebuilt/boost_1_64_0")
set(Prebuild_Boost_Lib)

if(${CPPBUILD_TARGET_OS} STREQUAL "LINUX")
	set(Boost_USE_STATIC_LIBS        ON)  # only find static libs
	set(Boost_USE_DEBUG_LIBS         OFF) # ignore debug libs and
	set(Boost_USE_RELEASE_LIBS       ON)  # only find release libs
	set(Boost_USE_MULTITHREADED      ON)
	set(Boost_USE_STATIC_RUNTIME    OFF)
#	add_definitions(-DBOOST_ERROR_CODE_HEADER_ONLY)
	FIND_PACKAGE(Boost REQUIRED COMPONENTS filesystem system thread)
	include_directories(${Boost_INCLUDE_DIRS})
	message(STATUS "--Boost_LIBRARIES : " ${Boost_LIBRARIES})
elseif(${CPPBUILD_TARGET_OS} STREQUAL "ANDROID")
	INCLUDE_DIRECTORIES(${Prebuild_Boost_Path}/include)
#	FILE(GLOB Prebuild_Boost_Lib ${Prebuild_Boost_Path}/x86_64/lib/*.a)
	FILE(GLOB Prebuild_Boost_Lib ${Prebuild_Boost_Path}/armeabi-v7a/lib/*.a)
#	LINK_DIRECTORIES(${Prebuild_Boost_Path}/armeabi-v7a/lib)
	message(STATUS "--Prebuild_Boost_Lib : " ${Prebuild_Boost_Lib})
endif()

#if(IOS)
#CPPBUILD_MANGLELIB_TARGET(st_pose ${ALL_SRC})
#else()
#CPPBUILD_DYNLIB_TARGET(st_pose ${ALL_SRC})
#endif()
#CPPBUILD_INSTALL_LIB(st_pose)

#install(DIRECTORY include DESTINATION .)
#install(DIRECTORY samples DESTINATION ../..)

add_executable(tutorial libs/threadpool/tutorial/tutorial.cpp)
target_link_libraries(tutorial ${Prebuild_Boost_Lib} ${Boost_LIBRARIES} ${CPPBUILD_LINK_LIBS})

add_executable(quickstart libs/threadpool/quickstart/quickstart.cpp)
target_link_libraries(quickstart ${Prebuild_Boost_Lib} ${Boost_LIBRARIES} ${CPPBUILD_LINK_LIBS})

add_executable(mergesort libs/threadpool/example/mergesort/mergesort.cpp)
target_link_libraries(mergesort ${Prebuild_Boost_Lib} ${Boost_LIBRARIES} ${CPPBUILD_LINK_LIBS})

add_executable(compile_all libs/threadpool/test/compile_all/compile_all.cpp)
target_link_libraries(compile_all ${Prebuild_Boost_Lib} ${Boost_LIBRARIES} ${CPPBUILD_LINK_LIBS})

if(${CPPBUILD_TARGET_OS} STREQUAL "LINUX")
target_link_libraries(tutorial -lpthread)
target_link_libraries(quickstart -lpthread)
target_link_libraries(mergesort -lpthread)
target_link_libraries(compile_all -lpthread)
endif()

CPPBUILD_INSTALL_LIB(tutorial)
CPPBUILD_INSTALL_LIB(quickstart)
CPPBUILD_INSTALL_LIB(mergesort)
CPPBUILD_INSTALL_LIB(compile_all)

