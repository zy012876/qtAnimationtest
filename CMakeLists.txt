cmake_minimum_required(VERSION 3.16)

project(animation VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 REQUIRED COMPONENTS Quick)

qt_standard_project_setup(REQUIRES 6.8)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
    if(${CMAKE_SYSTEM_PROCESSOR} MATCHES "aarch64")
        link_directories("d:/work/code/sdk/lib/linux/aarch64/")
    else()
        link_directories("d:/work/code/sdk/lib/linux/")
    endif()
    set(QML_IMPORT_PATH "/opt/QtPlugins/;${CMAKE_CURRENT_SOURCE_DIR}/res/modules" CACHE STRING "" FORCE)
else()
    link_directories("d:/work/code/sdk/lib/windows/")
    set(QML_IMPORT_PATH "e:/Qt/Plugins/;${CMAKE_CURRENT_SOURCE_DIR}/res/modules" CACHE STRING "" FORCE)
endif()

qt_add_executable(appanimation
    main.cpp
)

qt_add_qml_module(appanimation
    URI animation
    QML_FILES
        Main.qml
)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
set_target_properties(appanimation PROPERTIES
#    MACOSX_BUNDLE_GUI_IDENTIFIER com.example.appanimation
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

target_link_libraries(appanimation
    PRIVATE Qt6::Quick
)

include(GNUInstallDirs)
install(TARGETS appanimation
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
