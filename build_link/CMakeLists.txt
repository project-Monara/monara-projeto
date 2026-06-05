cmake_minimum_required(VERSION 3.10)
project(monara_linker)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BINARY_DIR "/home/iury-lee/Downloads/monara-projeto/build_final")

# Encontra o Boost de forma nativa
find_package(Boost REQUIRED COMPONENTS filesystem thread date_time chrono serialization program_options locale)
find_package(OpenSSL REQUIRED)

# Encontra as bibliotecas de sistema restantes
find_library(ZMQ_LIB zmq REQUIRED)
find_library(SODIUM_LIB sodium REQUIRED)
find_library(LMDB_LIB lmdb REQUIRED)

include_directories(/home/iury-lee/Downloads/monara-projeto/src /home/iury-lee/Downloads/monara-projeto/src/crypto /home/iury-lee/Downloads/monara-projeto/contrib/epee/include /home/iury-lee/Downloads/monara-projeto/external/easylogging++)

# PASSADA CIRÚRGICA: Aponta para o arquivo .cc real do Easylogging para compilar junto
add_executable(monerod
  ${CMAKE_BINARY_DIR}/src/daemon/CMakeFiles/daemon.dir/main.cpp.o
  ${CMAKE_BINARY_DIR}/src/daemon/CMakeFiles/daemon.dir/daemon.cpp.o
  ${CMAKE_BINARY_DIR}/src/daemon/CMakeFiles/daemon.dir/command_server.cpp.o
  ${CMAKE_BINARY_DIR}/src/daemon/CMakeFiles/daemon.dir/command_parser_executor.cpp.o
  ${CMAKE_BINARY_DIR}/src/daemon/CMakeFiles/daemon.dir/executor.cpp.o
  ${CMAKE_BINARY_DIR}/src/daemon/CMakeFiles/daemon.dir/rpc_command_executor.cpp.o
  /home/iury-lee/Downloads/monara-projeto/external/easylogging++/easylogging++.cc
)

set_target_properties(monerod PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(monerod PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")

# Linka usando os caminhos absolutos resolvidos pelo CMake
target_link_libraries(monerod
  PRIVATE
  ${CMAKE_BINARY_DIR}/lib/libdaemonizer.a
  ${CMAKE_BINARY_DIR}/lib/libcryptonote_core.a
  ${CMAKE_BINARY_DIR}/lib/libblockchain_db.a
  ${CMAKE_BINARY_DIR}/lib/libcryptonote_protocol.a
  ${CMAKE_BINARY_DIR}/lib/libdaemon_rpc_server.a
  ${CMAKE_BINARY_DIR}/lib/libnet.a
  ${CMAKE_BINARY_DIR}/lib/libcncrypto.a
  ${CMAKE_BINARY_DIR}/lib/libringct.a
  ${CMAKE_BINARY_DIR}/lib/libringct_basic.a
  ${CMAKE_BINARY_DIR}/lib/libdevice.a
  ${CMAKE_BINARY_DIR}/lib/libcommon.a
  ${CMAKE_BINARY_DIR}/lib/libblocks.a
  ${CMAKE_BINARY_DIR}/lib/libepee.a
  ${CMAKE_BINARY_DIR}/lib/libversion.a
  Boost::filesystem
  Boost::thread
  Boost::date_time
  Boost::chrono
  Boost::serialization
  Boost::program_options
  Boost::locale
  OpenSSL::Crypto
  OpenSSL::SSL
  ${ZMQ_LIB}
  ${SODIUM_LIB}
  ${LMDB_LIB}
  pthread
  dl
)
