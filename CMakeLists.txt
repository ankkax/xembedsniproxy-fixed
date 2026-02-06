cmake_minimum_required(VERSION 3.16)

project(xembedsniproxy)

find_package(ECM 5.14.0 REQUIRED NO_MODULE)
include(FeatureSummary)
include(WriteBasicConfigVersionFile)
include(GenerateExportHeader)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

find_package(Qt5 REQUIRED COMPONENTS
    Core
    DBus
    Gui
    X11Extras
)

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings)
include(ECMInstallIcons)
include(ECMOptionalAddSubdirectory)
include(ECMQtDeclareLoggingCategory)

# KDE Frameworks 5 components
find_package(KF5 REQUIRED COMPONENTS WindowSystem)

# XCB components
find_package(XCB REQUIRED COMPONENTS
    XCB
    XFIXES
    DAMAGE
    COMPOSITE
    RANDR
    SHM
    UTIL
    IMAGE
)

add_definitions(-Wall -std=c++11)
add_definitions(
    -DQT_NO_CAST_TO_ASCII
    -DQT_NO_CAST_FROM_ASCII
    -DQT_NO_URL_CAST_FROM_STRING
    -DQT_NO_CAST_FROM_BYTEARRAY
)

# Source files
set(XEMBED_SNI_PROXY_SOURCES
    main.cpp
    fdoselectionmanager.cpp
    snidbus.cpp
    sniproxy.cpp
)

# DBus interfaces
qt5_add_dbus_adaptor(XEMBED_SNI_PROXY_SOURCES org.kde.StatusNotifierItem.xml sniproxy.h SNIProxy)
qt5_add_dbus_interface(XEMBED_SNI_PROXY_SOURCES org.kde.StatusNotifierWatcher.xml statusnotifierwatcher_interface)

# Executable
add_executable(xembedsniproxy ${XEMBED_SNI_PROXY_SOURCES})

# XCB libraries
set(XCB_LIBS
    XCB::XCB
    XCB::XFIXES
    XCB::DAMAGE
    XCB::COMPOSITE
    XCB::RANDR
    XCB::SHM
    XCB::UTIL
    XCB::IMAGE
)

# Logging category
ecm_qt_declare_logging_category(xembedsniproxy HEADER debug.h
    IDENTIFIER SNIPROXY
    CATEGORY_NAME kde.xembedsniproxy
    DEFAULT_SEVERITY Debug
)

# Link libraries
target_link_libraries(xembedsniproxy
    PRIVATE
        Qt5::Core
        Qt5::X11Extras
        Qt5::DBus
        KF5::WindowSystem
        ${XCB_LIBS}
        xcb-ewmh         # xcb-util-wm
        xcb-icccm        # xcb-util-wm/ICCCM support
)

# Install
install(TARGETS xembedsniproxy ${KDE_INSTALL_TARGETS_DEFAULT_ARGS})
install(FILES xembedsniproxy.desktop DESTINATION ${KDE_INSTALL_AUTOSTARTDIR})

