project( viewerPy VERSION 1.38.0 )

	add_library( ${PROJECT_NAME} STATIC )

set_property(TARGET ${PROJECT_NAME} PROPERTY AUTOMOC ON)

target_sources( ${PROJECT_NAME}
    PRIVATE
    viewerPy.h
    viewerPy.cpp
    viewerPyApplication.h
    viewerPyApplication.cpp
    )

target_include_directories( ${PROJECT_NAME} PRIVATE
    ${CC_FBO_LIB_SOURCE_DIR}/include
    ${CCCoreLib_SOURCE_DIR}/include
    ${QCC_DB_LIB_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ) 
 
file( GLOB ui_list ${CMAKE_CURRENT_SOURCE_DIR}/../CloudCompare/ccViewer/ui_templates/*.ui )
file( GLOB qrc_list ${CMAKE_CURRENT_SOURCE_DIR}/../CloudCompare/qCC/*.qrc )
file( GLOB rc_list ${CMAKE_CURRENT_SOURCE_DIR}/../CloudCompare/qCC/*.rc )

if (OPTION_USE_QT6) 
    qt6_wrap_ui( generated_ui_list ${ui_list} )
    qt5_add_resources( generated_qrc_list ${qrc_list} )
else()
    qt5_wrap_ui( generated_ui_list ${ui_list} )
    qt5_add_resources( generated_qrc_list ${qrc_list} )
endif()

target_sources( ${PROJECT_NAME} PRIVATE ${generated_ui_list} ${generated_qrc_list} ${rc_list} )

target_link_libraries( ${PROJECT_NAME}
    CCAppCommon
    )
    
# Qt libraries

if (OPTION_USE_QT6) 
    target_link_libraries( ${PROJECT_NAME}
        Qt6::Core
        Qt6::Gui
        Qt6::Widgets
        Qt6::OpenGL
        Qt6::PrintSupport
        )

    if (WIN32)
    target_link_libraries( ${PROJECT_NAME} Qt6::WinMain)
    endif()
else()
    target_link_libraries( ${PROJECT_NAME}
        Qt5::Core
        Qt5::Gui
        Qt5::Widgets
        Qt5::OpenGL
        Qt5::PrintSupport
        )

    if (WIN32)
    target_link_libraries( ${PROJECT_NAME} Qt5::WinMain)
    endif()
endif()    
