cmake_minimum_required(VERSION 3.10)
project(hkex_gateway)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find required packages
find_package(OpenSSL REQUIRED)
find_package(Boost REQUIRED COMPONENTS system thread)
find_package(PkgConfig REQUIRED)
pkg_check_modules(JSONCPP REQUIRED jsoncpp)

# Find OMNet package
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
find_package(OMNet REQUIRED)

# Add external dependencies
include_directories(
    ${CMAKE_SOURCE_DIR}/external/omnet
    ${CMAKE_SOURCE_DIR}/include
)

# Source files
set(SOURCES
    src/Logger.cpp
    src/ErrorHandler.cpp
    src/SessionHandler.cpp
    src/OrderManager.cpp
    src/MarketDataHandler.cpp
    src/TradeManager.cpp
    src/HKEXGateway.cpp
)

# Header files
set(HEADERS
    include/Logger.hpp
    include/ErrorHandler.hpp
    include/SessionHandler.hpp
    include/OrderManager.hpp
    include/MarketDataHandler.hpp
    include/TradeManager.hpp
    include/HKEXGateway.hpp
)

# Create the library
add_library(hkex_gateway STATIC ${SOURCES} ${HEADERS})
target_include_directories(hkex_gateway 
    PUBLIC 
        ${CMAKE_SOURCE_DIR}/include
    PRIVATE
        ${CMAKE_SOURCE_DIR}/external/omnet
)

# Create the main executable
add_executable(gateway_main src/main.cpp)
target_link_libraries(gateway_main PRIVATE hkex_gateway)
target_include_directories(gateway_main PRIVATE ${CMAKE_SOURCE_DIR}/include)

# Add test executable if testing is enabled
option(BUILD_TESTS "Build test cases" OFF)
if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

# Include directories
target_include_directories(hkex_gateway
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
        ${OMNET_INCLUDE_DIRS}
        ${OPENSSL_INCLUDE_DIR}
        ${JSONCPP_INCLUDE_DIRS}
        ${Boost_INCLUDE_DIRS}
)

# Link against libraries
target_link_libraries(hkex_gateway
    PRIVATE
        ${OMNET_LIBRARIES}
        OpenSSL::SSL
        OpenSSL::Crypto
        ${JSONCPP_LIBRARIES}
        Boost::system
        Boost::thread
        pthread
)

# Install rules
install(TARGETS hkex_gateway
    RUNTIME DESTINATION bin
)

# Copy configuration template during build
configure_file(
    ${CMAKE_SOURCE_DIR}/config.ini.template
    ${CMAKE_BINARY_DIR}/config.ini
    COPYONLY
) 