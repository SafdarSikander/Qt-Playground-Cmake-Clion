cmake_minimum_required(VERSION 3.15.3)
project(Qt-Playground VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_PREFIX_PATH "C:/Qt/Qt5.14.2/5.14.2/msvc2017_64/lib/cmake")
set(QT_DLLS "C:/Qt/Qt5.14.2/5.14.2/msvc2017_64/bin")

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

file(GLOB SRC
        *.h
        *.cpp
        *.ui
    )

add_executable(${PROJECT_NAME} ${SRC})

#Qt Bindings
set(QT_DLLS_LINK Qt5Widgets Qt5Core Qt5Gui)
foreach (i ${QT_DLLS_LINK})
    find_package(${i} REQUIRED)
endforeach ()

target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Widgets)

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(ext d.dll)
else()
    set(ext .dll)
endif()

# Qt5 copy dll
foreach (i ${QT_DLLS_LINK})
    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_if_different "${QT_DLLS}/${i}${ext}" $<TARGET_FILE_DIR:${PROJECT_NAME}>)
endforeach ()

# install dependencies
set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/Install)
install(TARGETS ${PROJECT_NAME} DESTINATION deb CONFIGURATIONS Debug)
install(TARGETS ${PROJECT_NAME} DESTINATION rel CONFIGURATIONS Release)

foreach (i ${QT_LIBS_LINK})
    install(FILES "${QT_LIBS_DLL}/${i}${ext}" DESTINATION ${des})
endforeach ()

