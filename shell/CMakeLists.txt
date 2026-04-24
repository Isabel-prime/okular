
include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}/..
  ${CMAKE_CURRENT_BINARY_DIR}/../
)

# okular

set(okular_SRCS
   main.cpp
   okular_main.cpp
   shell.cpp
   shellutils.cpp
   welcomescreen.cpp

   ../gui/recentitemsmodel.cpp

   shell.qrc
)

ki18n_wrap_ui(okular_SRCS
    welcomescreen.ui)

file(GLOB ICONS_SRCS "${CMAKE_CURRENT_SOURCE_DIR}/../icons/*-apps-okular.png")
ecm_add_app_icon(okular_SRCS ICONS ${ICONS_SRCS})

add_executable(ockular ${okular_SRCS})

target_link_libraries(ockular KF6::I18n KF6::Parts KF6::WindowSystem KF6::Crash KF6::IconThemes KF6::KIOGui)
if (TARGET Qt6::DBus)
    target_link_libraries(ockular Qt6::DBus)
endif()
if (HAVE_X11)
    target_link_libraries(ockular Qt6::GuiPrivate)
endif()
if (WIN32)
    # QLocalServer / QLocalSocket (Windows named-pipe IPC for single-instance routing)
    target_link_libraries(ockular Qt6::Network)
endif()

install(TARGETS ockular ${KDE_INSTALL_TARGETS_DEFAULT_ARGS})

if(APPLE)
    set_target_properties(ockular PROPERTIES
        MACOSX_BUNDLE_DISPLAY_NAME "Ockular"
        MACOSX_BUNDLE_BUNDLE_NAME "Ockular"
        MACOSX_BUNDLE_ICON_FILE "okular_SRCS"
        MACOSX_BUNDLE_LONG_VERSION_STRING "Okular ${RELEASE_SERVICE_VERSION}"
        MACOSX_BUNDLE_SHORT_VERSION_STRING "${RELEASE_SERVICE_VERSION_MAJOR}.${RELEASE_SERVICE_VERSION_MINOR}"
        MACOSX_BUNDLE_BUNDLE_VERSION "${RELEASE_SERVICE_VERSION}"
        MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/MacOSXBundleInfo.plist.in"
        MACOSX_BUNDLE_GUI_IDENTIFIER "org.kde.okular"
        MACOSX_BUNDLE_COPYRIGHT "2002-2026 The Okular Developers")
endif()

# okular shell data files

install( PROGRAMS org.kde.okular.desktop  DESTINATION  ${KDE_INSTALL_APPDIR} )
install( FILES org.kde.okular.appdata.xml DESTINATION  ${KDE_INSTALL_METAINFODIR} )
