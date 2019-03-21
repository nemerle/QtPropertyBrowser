# Tell CMake to run moc when necessary:
set(CMAKE_AUTOMOC ON)

# As moc files are generated in the binary dir, tell CMake
# to always look for includes there:
set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(_SRCS
  qtbuttonpropertybrowser.cpp
  qteditorfactory.cpp
  qtgroupboxpropertybrowser.cpp
  qtpropertybrowser.cpp
  qtpropertybrowserutils.cpp
  qtpropertymanager.cpp
  qttreepropertybrowser.cpp
  qtvariantproperty.cpp
  )

file(GLOB _IMPL_HDRS *.h)
file(GLOB _PUBLIC_HDRS Qt*)

set(_UI_FORMS
  )

set(_RESOURCES
  qtpropertybrowser.qrc
  )
  
QT5_WRAP_UI(_UI_SRCS ${_UI_FORMS})
QT5_ADD_RESOURCES(_QRC_SRCS ${_RESOURCES})

set(TARGET_NAME ${PROJECT_NAME})

add_library(${TARGET_NAME}
  ${_SRCS}
  ${_UI_SRCS}
  ${_QRC_SRCS}
  ${_IMPL_HDRS}
  )

target_link_libraries(${TARGET_NAME} PUBLIC Qt5::Widgets)
target_compile_features(${TARGET_NAME} PUBLIC cxx_std_11)
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    target_compile_options(${TARGET_NAME} PRIVATE -Wall)
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    target_compile_options(${TARGET_NAME} PRIVATE -Wall -pedantic -Wno-long-long -Wno-variadic-macros)
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel")
  # using Intel C++
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
    target_compile_options(${TARGET_NAME} PRIVATE /W3)
endif()

######################### Installation Stuff ###########################
include(GenerateExportHeader)
generate_export_header(${TARGET_NAME})

install(TARGETS ${TARGET_NAME}
  EXPORT ${TARGET_NAME}Targets
  RUNTIME DESTINATION ${INSTALL_BIN_DIR}
  LIBRARY DESTINATION ${INSTALL_LIB_DIR}
  ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
  INCLUDES DESTINATION ${INSTALL_INCLUDE_DIR}
)

install(
  FILES
    ${_PUBLIC_HDRS}
    ${_IMPL_HDRS}
  DESTINATION
    ${INSTALL_INCLUDE_DIR}
  COMPONENT
    Devel
)

