CMAKE_MINIMUM_REQUIRED(VERSION 2.6 FATAL_ERROR)

# Use clang if available
IF(EXISTS /usr/bin/clang++)
  set(CMAKE_CXX_COMPILER /usr/bin/clang++)
ENDIF(EXISTS /usr/bin/clang++)

Project (jts_common)

INCLUDE_DIRECTORIES(include)

# add compiler flag for tinyxml compilation
ADD_DEFINITIONS(-DTIXML_USE_STL) # tell tinyxml to use the STL

SET(COMMONLIB_SRCS
    src/InternalEvents/InternalEventHandler.cpp 
    src/Service.cpp 
    src/SimpleThread.cpp 
    src/EventReceiver.cpp
    src/Transport/Address.cpp 
    src/Transport/JausAddress.cpp 
    src/Transport/JausTransport.cpp 
    src/jFixedLengthString.cpp
    src/Transport/JuniorAPI.cpp
    src/Transport/JrSockets.cpp
    src/Transport/JuniorMgr.cpp
    src/Transport/JuniorRTE.cpp
    src/Transport/OS.cpp
    src/Transport/JrLogger.cpp
    src/Transport/XmlConfig.cpp
    src/Transport/JUDPTransport.cpp
    src/Transport/JUDPTransportLB.cpp
    src/Transport/JSerial.cpp
    src/Transport/ChecksumCRC.cpp
    src/Transport/TCPConnection.cpp
    src/Transport/JTCPTransport.cpp
    src/Transport/tinyxml/tinyxml.cpp
    src/Transport/tinyxml/tinyxmlerror.cpp
    src/Transport/tinyxml/tinyxmlparser.cpp
    src/InternalEvents/Send_1_0.cpp
    src/InternalEvents/Receive_1_0.cpp
    src/InternalEvents/Send_1_1.cpp
    src/InternalEvents/Receive_1_1.cpp    
)

SET(COMMONLIB_HEADERS
    include/InternalEvents/InternalEventHandler.h 
    include/Service.h 
    include/SimpleThread.h 
    include/EventReceiver.h
    include/Transport/Address.h 
    include/Transport/JausAddress.h 
    include/Transport/JausTransport.h 
    include/jFixedLengthString.h
    include/Transport/JuniorAPI.h
    include/Transport/JrSockets.h
    include/Transport/JuniorMgr.h
    include/Transport/OS.h
    include/Transport/JrLogger.h
    include/Transport/XmlConfig.h
    include/Transport/JUDPTransport.h
    include/Transport/JUDPTransportLB.h
    include/Transport/JSerial.h
    include/Transport/ChecksumCRC.h
    include/Transport/TCPConnection.h
    include/Transport/JTCPTransport.h
    include/InternalEvents/Send_1_0.h
    include/InternalEvents/Receive_1_0.h
    include/InternalEvents/Send_1_1.h
    include/InternalEvents/Receive_1_1.h    
)

# Build static library
ADD_LIBRARY(JAUS_common ${COMMONLIB_SRCS})

## Setup installation 
# Configure make install
IF(NOT CMAKE_INSTALL_PREFIX)
SET(CMAKE_INSTALL_PREFIX /usr/local)
ENDIF(NOT CMAKE_INSTALL_PREFIX)

# install headers
INSTALL( FILES include/Service.h include/SimpleThread.h include/EventReceiver.h include/jFixedLengthString.h include/JausUtils.h include/statemap.h include/JTSStateMachine.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/jts_common)
INSTALL( DIRECTORY include/Transport include/InternalEvents include/Messages DESTINATION ${CMAKE_INSTALL_PREFIX}/include/jts_common)


#install libraries
INSTALL(TARGETS JAUS_common
RUNTIME DESTINATION bin
LIBRARY DESTINATION lib
ARCHIVE DESTINATION lib
)

IF(NOT CMAKE_FIND_INSTALL_PATH)
set(CMAKE_FIND_INSTALL_PATH ${CMAKE_ROOT})
ENDIF(NOT CMAKE_FIND_INSTALL_PATH)

#INSTALL(FILES ../../Findjaus.cmake DESTINATION ${CMAKE_FIND_INSTALL_PATH}/Modules/)

ADD_CUSTOM_TARGET(uninstall @echo uninstall package)

IF (UNIX)
ADD_CUSTOM_COMMAND(
COMMENT "uninstall package"
COMMAND xargs ARGS rm < install_manifest.txt

TARGET  uninstall
)
ELSE(UNIX)
ADD_CUSTOM_COMMAND(
COMMENT "uninstall only implemented in unix"
TARGET  uninstall
)
ENDIF(UNIX)
