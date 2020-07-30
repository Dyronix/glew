# --------------------------
# Setup sources
# --------------------------
message(CHECK_START "Generating ${GLEW_LIBRARY_NAME}")

file(GLOB_RECURSE TARGET_INC    "${GLEW_INCLUDE_DIRECTORY}/*.h")
file(GLOB_RECURSE TARGET_SRC    "${GLEW_SOURCE_DIRECTORY}/glew.c")

if(MSVC)
    GROUPSOURCES("include")
    GROUPSOURCES("src")
endif()

# --------------------------
# Setup library
# --------------------------
# Compiler flag /MD
SETUP_MULTITHREADING_MD()

ADD_LIBRARY(${GLEW_LIBRARY_NAME} STATIC
            ${TARGET_INC}
            ${TARGET_SRC}) 

add_definitions(-DGLEW_STATIC)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)

target_compile_options(${GLEW_LIBRARY_NAME} PRIVATE /W3 /WX /Zi)

# --------------------------
# Target properties
# --------------------------
set_target_properties( ${GLEW_LIBRARY_NAME} PROPERTIES      VS_DEBUGGER_WORKING_DIRECTORY    "${OUTPUT_BINDIR}"             # Working directory
                                                            FOLDER                           "0. External Libraries"        # Project filter location
                                                            DEFINE_SYMBOL                    "" )                           # Library symbol

# --------------------------
# Target include directories.
# --------------------------
target_include_directories(${GLEW_LIBRARY_NAME} PUBLIC     
                                                $<BUILD_INTERFACE:${GLEW_INCLUDE_DIRECTORY}>
                                                $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${GLEW_LIBRARY_NAME}>)

# --------------------------
# Install.
# --------------------------
SET(EXPORT_TARGETS ${GLEW_LIBRARY_NAME})
SET(EXPORT_TARGETS_NAME ${GLEW_LIBRARY_NAME}_targets_name)

INSTAll_TARGETS(${EXPORT_TARGETS} ${EXPORT_TARGETS_NAME})
INSTALL_HEADERS(${GLEW_INCLUDE_DIRECTORY} ${CMAKE_INSTALL_INCLUDEDIR}/${GLEW_LIBRARY_NAME})

message(CHECK_PASS "Done")