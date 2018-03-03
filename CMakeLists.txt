cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB xeracoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    xeracoin/external
    xeracoin/include
    xeracoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("xeracoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  xeracoin/external/miniupnpc/connecthostport.c
  xeracoin/external/miniupnpc/igd_desc_parse.c
  xeracoin/external/miniupnpc/minisoap.c
  xeracoin/external/miniupnpc/miniupnpc.c
  xeracoin/external/miniupnpc/miniwget.c
  xeracoin/external/miniupnpc/minixml.c
  xeracoin/external/miniupnpc/portlistingparse.c
  xeracoin/external/miniupnpc/receivedata.c
  xeracoin/external/miniupnpc/upnpcommands.c
  xeracoin/external/miniupnpc/upnpreplyparse.c
  xeracoin/src/Common/Base58.cpp
  xeracoin/src/Common/CommandLine.cpp
  xeracoin/src/Common/Util.cpp
  xeracoin/src/Common/StringTools.cpp
  xeracoin/src/Common/JsonValue.cpp
  xeracoin/src/Common/ConsoleTools.cpp
  xeracoin/src/Common/MemoryInputStream.cpp
  xeracoin/src/Common/PathTools.cpp
  xeracoin/src/Common/StdInputStream.cpp
  xeracoin/src/Common/StdOutputStream.cpp
  xeracoin/src/Common/StreamTools.cpp
  xeracoin/src/Common/StringOutputStream.cpp
  xeracoin/src/Common/StringView.cpp
  xeracoin/src/Common/VectorOutputStream.cpp
  xeracoin/src/crypto/blake256.c
  xeracoin/src/crypto/chacha8.c
  xeracoin/src/crypto/crypto-ops-data.c
  xeracoin/src/crypto/crypto-ops.c
  xeracoin/src/crypto/crypto.cpp
  xeracoin/src/crypto/groestl.c
  xeracoin/src/crypto/hash-extra-blake.c
  xeracoin/src/crypto/hash-extra-groestl.c
  xeracoin/src/crypto/hash-extra-jh.c
  xeracoin/src/crypto/hash-extra-skein.c
  xeracoin/src/crypto/hash.c
  xeracoin/src/crypto/jh.c
  xeracoin/src/crypto/keccak.c
  xeracoin/src/crypto/oaes_lib.c
  xeracoin/src/crypto/random.c
  xeracoin/src/crypto/skein.c
  xeracoin/src/crypto/slow-hash.c
  xeracoin/src/crypto/slow-hash.cpp
  xeracoin/src/crypto/tree-hash.c
  xeracoin/src/CryptoNoteCore/BlockchainIndices.cpp
  xeracoin/src/CryptoNoteCore/BlockchainMessages.cpp
  xeracoin/src/CryptoNoteCore/BlockIndex.cpp
  xeracoin/src/CryptoNoteCore/CoreConfig.cpp
  xeracoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  xeracoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  xeracoin/src/CryptoNoteCore/Currency.cpp
  xeracoin/src/CryptoNoteCore/MinerConfig.cpp
  xeracoin/src/CryptoNoteCore/Transaction.cpp
  xeracoin/src/CryptoNoteCore/Account.cpp
  xeracoin/src/CryptoNoteCore/Blockchain.cpp
  xeracoin/src/CryptoNoteCore/Checkpoints.cpp
  xeracoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  xeracoin/src/CryptoNoteCore/Core.cpp
  xeracoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  xeracoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  xeracoin/src/CryptoNoteCore/Difficulty.cpp
  xeracoin/src/CryptoNoteCore/IBlock.cpp
  xeracoin/src/CryptoNoteCore/Miner.cpp
  xeracoin/src/CryptoNoteCore/TransactionExtra.cpp
  xeracoin/src/CryptoNoteCore/TransactionPool.cpp
  xeracoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  xeracoin/src/CryptoNoteCore/TransactionUtils.cpp
  xeracoin/src/InProcessNode/InProcessNode.cpp
  xeracoin/src/InProcessNode/InProcessNodeErrors.cpp
  xeracoin/src/NodeRpcProxy/NodeErrors.cpp
  xeracoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  xeracoin/src/P2p/NetNodeConfig.cpp
  xeracoin/src/Serialization/BinaryInputStreamSerializer.cpp
  xeracoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  xeracoin/src/Serialization/JsonInputValueSerializer.cpp
  xeracoin/src/Serialization/JsonOutputStreamSerializer.cpp
  xeracoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  xeracoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  xeracoin/src/Serialization/SerializationOverloads.cpp
  xeracoin/src/Transfers/BlockchainSynchronizer.cpp
  xeracoin/src/Transfers/SynchronizationState.cpp
  xeracoin/src/Transfers/TransfersConsumer.cpp
  xeracoin/src/Transfers/TransfersContainer.cpp
  xeracoin/src/Transfers/TransfersSubscription.cpp
  xeracoin/src/Transfers/TransfersSynchronizer.cpp
  xeracoin/src/Wallet/LegacyKeysImporter.cpp
  xeracoin/src/Wallet/WalletAsyncContextCounter.cpp
  xeracoin/src/Wallet/WalletErrors.cpp
  xeracoin/src/WalletLegacy/KeysStorage.cpp
  xeracoin/src/WalletLegacy/WalletLegacy.cpp
  xeracoin/src/WalletLegacy/WalletHelper.cpp
  xeracoin/src/WalletLegacy/WalletLegacySerializer.cpp
  xeracoin/src/WalletLegacy/WalletLegacySerialization.cpp
  xeracoin/src/WalletLegacy/WalletTransactionSender.cpp
  xeracoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  xeracoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  xeracoin/src/System/ContextGroup.cpp
  xeracoin/src/System/Event.cpp
  xeracoin/src/System/EventLock.cpp
  xeracoin/src/System/InterruptedException.cpp
  xeracoin/src/System/Ipv4Address.cpp
  xeracoin/src/System/TcpStream.cpp
  xeracoin/src/HTTP/HttpRequest.cpp
  xeracoin/src/HTTP/HttpParser.cpp
  xeracoin/src/HTTP/HttpParserErrorCodes.cpp
  xeracoin/src/HTTP/HttpResponse.cpp
  xeracoin/src/Rpc/HttpClient.cpp
  xeracoin/src/Rpc/JsonRpc.cpp
  xeracoin/src/P2p/NetNode.cpp
  xeracoin/src/P2p/LevinProtocol.cpp
  xeracoin/src/P2p/NetNodeConfig.cpp
  xeracoin/src/P2p/PeerListManager.cpp
  xeracoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  xeracoin/src/Logging/ILogger.cpp
  xeracoin/src/Logging/LoggerGroup.cpp
  xeracoin/src/Logging/CommonLogger.cpp
  xeracoin/src/Logging/LoggerManager.cpp
  xeracoin/src/Logging/FileLogger.cpp
  xeracoin/src/Logging/StreamLogger.cpp
  xeracoin/src/Logging/ConsoleLogger.cpp
  xeracoin/src/Logging/LoggerMessage.cpp
  xeracoin/src/Logging/LoggerRef.cpp
  xeracoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  xeracoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  xeracoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(xeracoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} xeracoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(xeracoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES xeracoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
