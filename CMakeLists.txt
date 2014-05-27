# How To Use:
# Alle Felder die angepasst werden müssen, sind mit "[TOCHANGE]" markiert.
# Wichtig: Die Dateien cmake_uninstall.cmake.in und 
#                      cmake_uninstall_dirs.cmake.in 
#          müssen vorhanden sein.


# 1. header
cmake_minimum_required (VERSION 2.6)
project ( splitster )


# 2. Metadata (e.g. Version information, custom variables, definitions, ...)

#  Full Project Name, e.g. "Foo-Bar Iterator"
set (FULL_PROJECT_NAME "splitster" )

# Version information
set (VER_MAJOR "0")
set (VER_MINOR "0")
set (VER_PATCH "0")

set (AUTHOR       "Daniel Wegemer")
set (MAINTAINER   "${AUTHOR}" )    

# Executable file (without "")
set (EXECUTABLE_NAME         splitster)


#configure_file (
#               "${PROJECT_SOURCE_DIR}/src/config.h.in"
#               "${PROJECT_BINARY_DIR}/include/config.h"
#)

#configure_file (
#               "${PROJECT_SOURCE_DIR}/INSTALL.in"
#               "${PROJECT_BINARY_DIR}/INSTALL"
#)


# 3. Needed paths (for un-/installing)
#    (e.g. share_dir, doc_dir, ...)
# (To add a path, add it to the list at the bottom of this section, too. 
#  Otherwise, it will not be deleted at uninstalling.)
# IMPORTANT NOTICE: Don't rename list DIR_LIST!
# - share_dir, example:
#        $PREFIX/share/foobar-0.02/
#set (share_dir share/foobar-${VER_MAJOR}.${VER_MINOR})
# !! add a new path after dir_list !!
#set (DIR_LIST ${share_dir})


# 4. Needed packages/libraries
# Examples list:
# - standard c math routines
# - GTK2
#FIND_PACKAGE (GTK2 REQUIRED)
#IF(GTK2_FOUND) 
#              include_directories(${GTK2_INCLUDE_DIRS})
#              link_libraries(foobar ${GTK2_LIBRARIES})
#ENDIF(GTK2_FOUND)
#link_libraries(foobar "m")



# 5. Build statements
include_directories ("${PROJECT_BINARY_DIR}/include")
#add_executable (${EXECUTABLE_NAME} src/foobar.c)


# 6. Files to install
#install (TARGETS ${EXECUTABLE_NAME} DESTINATION bin)
#install (FILES   AUTHORS  DESTINATION ${share_dir})
#install (FILES   README   DESTINATION ${share_dir})
#install (FILES   LICENSE  DESTINATION ${share_dir})
#install (FILES   TODO     DESTINATION ${share_dir})


# 7. Uninstall target
########### Add uninstall target for files ###############
CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake-modules/cmake_uninstall.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/cmake-modules/cmake_uninstall.cmake"
  IMMEDIATE @ONLY)

ADD_CUSTOM_TARGET(uninstall_files
  "${CMAKE_COMMAND}" -P "${CMAKE_CURRENT_BINARY_DIR}/cmake-modules/cmake_uninstall.cmake") 

########### Add uninstall target for dirs ##################
CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake-modules/cmake_uninstall_dirs.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/cmake-modules/cmake_uninstall_dirs.cmake"
  IMMEDIATE @ONLY)

ADD_CUSTOM_TARGET(uninstall_dirs
  "${CMAKE_COMMAND}" -P "${CMAKE_CURRENT_BINARY_DIR}/cmake-modules/cmake_uninstall_dirs.cmake") 
# The target "uninstall_dirs" depends on target "uninstall_files", because
# it doesn't make sense to uninstall/remove program dirs without the files
# (it would lead to an inconsistent installation)
ADD_DEPENDENCIES(uninstall_dirs uninstall_files)

############ Add general uninstall target ###################
# Because of the dependency of target uninstall_dirs, the target uninstall
# has synonymous meaning to uninstall_dirs.
ADD_CUSTOM_TARGET(uninstall)
ADD_DEPENDENCIES(uninstall uninstall_dirs)


# 8 CPack Rules

SET(CPACK_CMAKE_GENERATOR "Unix Makefiles")
SET(CPACK_GENERATOR "TGZ;TBZ2;RPM")
SET(CPACK_INSTALL_CMAKE_PROJECTS ".;${FULL_PROJECT_NAME} ${VER_MAJOR}.${VER_MINOR};ALL;/")
SET(CPACK_NSIS_DISPLAY_NAME "${FULL_PROJECT_NAME} ${VER_MAJOR}.${VER_MINOR}")
SET(CPACK_OUTPUT_CONFIG_FILE "${PROJECT_BINARY_DIR}/CPackConfig-bin.cmake")
SET(CPACK_PACKAGE_DESCRIPTION_FILE "${PROJECT_SOURCE_DIR}/README")
SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "${PROJECT_SOURCE_DIR}/README")
SET(CPACK_PACKAGE_EXECUTABLES "${PROJECT_SOURCE_DIR}/${EXECUTABLE_NAME}")
SET(CPACK_PACKAGE_FILE_NAME "${EXECUTABLE}-${VER_MAJOR}.${VER_MINOR}-Linux-x86_64")
SET(CPACK_PACKAGE_INSTALL_DIRECTORY "${FULL_PROJECT_NAME} ${VER_MAJOR}.${VER_MINOR}")
SET(CPACK_PACKAGE_NAME          "${FULL_PROJECT_NAME}-${VER_MAJOR}.${VER_MINOR}")
SET(CPACK_PACKAGE_VENDOR        "${AUTHOR}")
SET(CPACK_PACKAGE_VERSION       "${VER_MAJOR}.${VER_MINOR}")
SET(CPACK_PACKAGE_VERSION_MAJOR "${VER_MAJOR}")
SET(CPACK_PACKAGE_VERSION_MINOR "${VER_MINOR}")
SET(CPACK_PACKAGE_VERSION_PATCH "${VER_PATCH}")
SET(CPACK_RESOURCE_FILE_LICENSE "${PROJECT_SOURCE_DIR}/LICENSE")
SET(CPACK_RESOURCE_FILE_README  "${PROJECT_SOURCE_DIR}/README.md")
SET(CPACK_RESOURCE_FILE_WELCOME "${PROJECT_SOURCE_DIR}/README.md")
SET(CPACK_SOURCE_GENERATOR "TGZ;TZ")
SET(CPACK_SOURCE_OUTPUT_CONFIG_FILE "${PROJECT_BINARY_DIR}/CPackConfig-src.cmake")
SET(CPACK_SOURCE_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}")
SET(CPACK_SOURCE_STRIP_FILES "")
SET(CPACK_STRIP_FILES "${EXECUTABLE_NAME}")
SET(CPACK_SYSTEM_NAME "Linux-x86_64")
SET(CPACK_TOPLEVEL_TAG "Linux-x86_64")
SET(CPACK_SOURCE_IGNORE_FILES "/CVS/;/.svn/;/.bzr/;/.hg/;/.git/;/.swp$;/.#;/#;/build/CMakeFiles/;/build/CMakeCache.txt;/*.cmake$;/SConstruct;/.sconsign.dblite;/.gitignore;/*.*~$;/build/config.h;/build/Makefile;/*.o$")
SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "${MAINTAINER}")

include(CPack)
